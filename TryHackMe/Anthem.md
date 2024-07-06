```
Machine: Anthem
Platform: TryHackMe
Difficulty: Easy
```

## WALKTHROUGH:
The machine can be found [here](https://tryhackme.com/r/room/anthem)

### Reconnaissance:
Let start with the Nmap Vulnerability Scan using following command `sudo nmap -sV -sC -vv --script vuln $IP`

```console
┌──(kali㉿kali)-[~]
└─$ nmap -sV -vv --script vuln 10.10.117.31
Starting Nmap 7.94 ( https://nmap.org ) at 2024-07-06 12:58 EDT
NSE: Loaded 150 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 12:58
Completed NSE at 12:58, 10.02s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 12:58
Completed NSE at 12:58, 0.00s elapsed
Initiating Ping Scan at 12:58
Scanning 10.10.117.31 [2 ports]
Completed Ping Scan at 12:58, 0.41s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 12:58
Completed Parallel DNS resolution of 1 host. at 12:58, 0.00s elapsed
Initiating Connect Scan at 12:58
Scanning 10.10.117.31 [1000 ports]
Discovered open port 3389/tcp on 10.10.117.31
Discovered open port 80/tcp on 10.10.117.31
Completed Connect Scan at 12:58, 22.50s elapsed (1000 total ports)
Initiating Service scan at 12:58
Scanning 2 services on 10.10.117.31
Completed Service scan at 12:58, 8.92s elapsed (2 services on 1 host)
NSE: Script scanning 10.10.117.31.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 12:58
NSE: [firewall-bypass 10.10.117.31] lacks privileges.
NSE Timing: About 97.44% done; ETC: 12:59 (0:00:01 remaining)
NSE Timing: About 98.17% done; ETC: 13:00 (0:00:01 remaining)
NSE Timing: About 98.53% done; ETC: 13:00 (0:00:01 remaining)
NSE Timing: About 98.53% done; ETC: 13:01 (0:00:02 remaining)
NSE Timing: About 99.63% done; ETC: 13:01 (0:00:01 remaining)
NSE Timing: About 99.63% done; ETC: 13:02 (0:00:01 remaining)
NSE Timing: About 99.63% done; ETC: 13:02 (0:00:01 remaining)
NSE Timing: About 99.63% done; ETC: 13:03 (0:00:01 remaining)
NSE Timing: About 99.63% done; ETC: 13:03 (0:00:01 remaining)
NSE Timing: About 99.63% done; ETC: 13:04 (0:00:01 remaining)
NSE Timing: About 99.63% done; ETC: 13:04 (0:00:01 remaining)
Completed NSE at 13:04, 341.16s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 13:04
NSE: [tls-ticketbleed 10.10.117.31:3389] Not running due to lack of privileges.
Completed NSE at 13:04, 15.41s elapsed
Nmap scan report for 10.10.117.31
Host is up, received syn-ack (0.45s latency).
Scanned at 2024-07-06 12:58:27 EDT for 388s
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON  VERSION
80/tcp   open  http          syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-litespeed-sourcecode-download: Request with null byte did not work. This web server might not be vulnerable
|_http-jsonp-detection: Couldn't find any JSONP endpoints.
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=10.10.117.31
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: http://10.10.117.31:80/
|     Form id: 
|     Form action: /search
|     
|     Path: http://10.10.117.31:80/categories
|     Form id: 
|     Form action: /search
|     
|     Path: http://10.10.117.31:80/archive/a-cheers-to-our-it-department/
|     Form id: 
|     Form action: /search
|     
|     Path: http://10.10.117.31:80/search
|     Form id: 
|     Form action: /search
|     
|     Path: http://10.10.117.31:80/archive/we-are-hiring/
|     Form id: 
|     Form action: /search
|     
|     Path: http://10.10.117.31:80/tags
|     Form id: 
|_    Form action: /search
|_http-wordpress-users: [Error] Wordpress installation was not found. We couldn't find wp-login.php
| http-enum: 
|   /blog/: Blog
|   /rss/: RSS or Atom feed
|   /robots.txt: Robots file
|   /categories/viewcategory.aspx: MS Sharepoint
|_  /categories/allcategories.aspx: MS Sharepoint
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
3389/tcp open  ms-wbt-server syn-ack Microsoft Terminal Services
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 13:04
Completed NSE at 13:04, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 13:04
Completed NSE at 13:04, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 399.15 seconds
```

This scan depicts that the machine has a directory of `/robots.txt`

```console
| http-enum: 
|   /blog/: Blog
|   /rss/: RSS or Atom feed
|   /robots.txt: Robots file
|   /categories/viewcategory.aspx: MS Sharepoint
|_  /categories/allcategories.aspx: MS Sharepoint
```

Now Let's access the `robots.txt` from the browser `http://10.10.117.31/robots.txt` 

![Screenshot 2024-07-06 212115](https://github.com/mishqatabid/WriteUps/assets/145700715/c8040845-35bc-4f00-b9c5-3846feedcf5b)

The directory shows the further directories and the `password` at the top i.e. `UmbracoIsTheBest!`

Now after directory hunting and analyzing it, I have came across a directory in a which a 

---
## Happy Hacking ;)
---

