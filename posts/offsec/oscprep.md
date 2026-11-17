# OSCP Preparation Guide - OffSec

## Overview
This comprehensive guide covers the preparation strategy for the Offensive Security Certified Professional (OSCP) certification exam.

## Exam Format
- **Duration**: 24 hours (exam) + 24 hours (report)
- **Targets**: 5 machines (3 standalone + 1 AD set)
- **Points**: 100 total (70 points to pass)
- **Report**: 24 hours to submit

## Preparation Strategy

### Phase 1: Foundation (Months 1-2)
1. **Linux Fundamentals**
   - Command line proficiency
   - File system navigation
   - Process management
   - Network configuration

2. **Networking Basics**
   - TCP/IP stack
   - Common protocols (HTTP, HTTPS, FTP, SSH, SMB)
   - Network troubleshooting

3. **Web Application Security**
   - OWASP Top 10
   - Common vulnerabilities (SQLi, XSS, CSRF)
   - Web application testing methodology

### Phase 2: Core Skills (Months 2-4)
1. **Reconnaissance**
   - Passive information gathering
   - Active reconnaissance techniques
   - OSINT tools and techniques

2. **Enumeration**
   - Port scanning (Nmap, Masscan)
   - Service enumeration
   - Directory/file enumeration
   - SMB enumeration
   - DNS enumeration

3. **Vulnerability Assessment**
   - Manual testing techniques
   - Automated scanning tools
   - Vulnerability databases (CVE, Exploit-DB)

### Phase 3: Exploitation (Months 4-6)
1. **Buffer Overflows**
   - Stack-based buffer overflows
   - EIP control and shellcode injection
   - Bypassing protections (ASLR, DEP, Stack Canaries)

2. **Web Application Exploitation**
   - SQL injection (manual and automated)
   - Cross-site scripting (XSS)
   - File upload vulnerabilities
   - Authentication bypass

3. **Network Service Exploitation**
   - SMB exploitation
   - FTP exploitation
   - SSH key exploitation
   - Database exploitation

### Phase 4: Post-Exploitation (Months 6-8)
1. **Privilege Escalation**
   - Linux privilege escalation
   - Windows privilege escalation
   - Kernel exploits
   - Service exploitation

2. **Lateral Movement**
   - Pass-the-hash techniques
   - Token impersonation
   - Credential dumping
   - Network pivoting

## Essential Tools

### Reconnaissance
- **Nmap**: Port scanning and service detection
- **Masscan**: Fast port scanning
- **Gobuster/Dirb**: Directory enumeration
- **Sublist3r**: Subdomain enumeration

### Exploitation
- **Metasploit**: Exploit framework
- **Burp Suite**: Web application testing
- **SQLmap**: SQL injection testing
- **John the Ripper**: Password cracking

### Post-Exploitation
- **LinPEAS**: Linux privilege escalation
- **WinPEAS**: Windows privilege escalation
- **Mimikatz**: Credential dumping
- **BloodHound**: Active Directory enumeration

## Practice Platforms

### Primary Practice
1. **PWK Labs** (Official)
   - 30+ lab machines
   - Covers all exam topics
   - Realistic scenarios

2. **HackTheBox**
   - Active machines
   - Retired machines
   - Challenges and CTFs

3. **VulnHub**
   - Vulnerable VMs
   - Various difficulty levels
   - Real-world scenarios

### Additional Practice
1. **TryHackMe**
   - Beginner to advanced rooms
   - Structured learning paths
   - Community support

2. **OverTheWire**
   - Bandit (Linux basics)
   - Natas (Web security)
   - Leviathan (Privilege escalation)

## Study Resources

### Books
- **The Web Application Hacker's Handbook**
- **Black Hat Python**
- **The Hacker Playbook 3**
- **Metasploit: The Penetration Tester's Guide**

### Online Resources
- **OWASP Testing Guide**
- **NIST Cybersecurity Framework**
- **SANS Reading Room**
- **Exploit-DB**

## Exam Day Strategy

### Time Management
- **Hours 1-2**: Reconnaissance and enumeration
- **Hours 3-8**: Initial exploitation
- **Hours 9-16**: Privilege escalation
- **Hours 17-20**: Documentation and reporting
- **Hours 21-24**: Final review and submission

### Documentation
- **Screenshots**: Every step documented
- **Commands**: All commands recorded
- **Proof**: User and root flags captured
- **Methodology**: Clear step-by-step process

### Report Structure
1. **Executive Summary**
2. **Methodology**
3. **Findings**
4. **Exploitation Steps**
5. **Recommendations**
6. **Appendices**

## Common Mistakes to Avoid

### Technical Mistakes
- Not taking enough screenshots
- Poor documentation
- Rushing through enumeration
- Not checking all services
- Missing privilege escalation vectors

### Strategic Mistakes
- Spending too much time on one machine
- Not managing time effectively
- Not reading questions carefully
- Not following the methodology
- Poor report quality

## Final Tips

### Before the Exam
- Get adequate sleep
- Prepare your environment
- Test all tools
- Review your notes
- Stay calm and focused

### During the Exam
- Read all questions first
- Start with easier machines
- Take breaks regularly
- Document everything
- Don't panic if stuck

### After the Exam
- Review your report
- Check all screenshots
- Verify all flags
- Submit on time
- Wait for results

## Conclusion
OSCP preparation requires dedication, practice, and a systematic approach. Focus on understanding the methodology rather than memorizing exploits. Practice regularly and document everything. The key to success is consistent practice and thorough documentation.

---
*This guide provides a comprehensive roadmap for OSCP preparation. Adjust the timeline based on your experience level and available time.*
