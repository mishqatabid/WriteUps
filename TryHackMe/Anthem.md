```
Machine: Anthem
Platform: TryHackMe
Difficulty: Easy
```

## WALKTHROUGH:
The machine can be found [here](https://tryhackme.com/r/room/anthem)

### Web Analysis:
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

The scan gives the answer to 2nd and 3rd questions.
Port 80 is open which shoes that there is a webpage. Upon accessing it, domain is shown in the front. 

![Screenshot 2024-07-07 171237](https://github.com/mishqatabid/WriteUps/assets/145700715/5867b6a4-cac0-44eb-a2bc-c0f006bef302)

_Q. 4: What is a possible password in one of the pages web crawlers check for?_

Analyzing the nmap scan results, we came across a directory of `/robots.txt`

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

As we saw the `robots.txt`, the CMS used by the website is `umbraco`.

In the website, there are 2 blogs written by user Jane Doe. In second one, there was a poem written. I searched it on Google and came across a nursery rhyme. There, the name was visible that was of Admin.

![Screenshot 2024-07-06 220306](https://github.com/mishqatabid/WriteUps/assets/145700715/2bc1226d-0786-429b-8864-c692da6c0791)

In the other article, an email was given. Using its format and the name of the admin, I got to know the email address of admin.

![Screenshot 2024-07-06 221021](https://github.com/mishqatabid/WriteUps/assets/145700715/31886020-5cb7-4108-9497-ed180babc5ed)

**Admin Email:** `SG@anthem.com`

### Spot the Flags:

I was able to get 3 of  the flags by analyzing the source code of the html pages.

![Screenshot 2024-07-06 215903](https://github.com/mishqatabid/WriteUps/assets/145700715/e2868e98-1653-4df5-80d1-69de7af1f966)

![Screenshot 2024-07-06 220222](https://github.com/mishqatabid/WriteUps/assets/145700715/554b8bfa-fedd-46a3-b36f-f247b2be2f42)

![Screenshot 2024-07-06 215957](https://github.com/mishqatabid/WriteUps/assets/145700715/ea776994-7916-44a9-ad31-f4444f541efd)

Now to find the final flag, I opened the profile of the author of the blog. The flag was written on that page.

![Screenshot 2024-07-06 220908](https://github.com/mishqatabid/WriteUps/assets/145700715/7031cd59-7cee-4f8d-b44b-9b463bc8d643)

### Final Stage:

Now let's get into the box using the information that I have collected. I accessed the windows with SG as user using rdesktop command. Then provided password found earlier.

```console
rdesktop $IP
```
**User:** SG
**Password:** UmbracoIsTheBest!

![Screenshot 2024-07-06 224231](https://github.com/mishqatabid/WriteUps/assets/145700715/62171d7e-a685-4024-b59d-be89777349ea)

There is a `user.txt` present. Upon opening it we get the flag.

**User Flag:** `THM{N00T_NO0T}`

For admin password, I changed the view setting to display `hidden files/directories`, and it displayed the `backup folder` that contains `restore.txt` but it requires privelege access to open. So. I changed the security permissions and gave read & write access, and got the password.

![Screenshot 2024-07-06 223853](https://github.com/mishqatabid/WriteUps/assets/145700715/317cc6f4-c301-4fe7-89d6-8d82fd0cc06c)

**Admin Password:** `ChangeMeBaby1MoreTime`

Now login into to windows using the admin password.

![Screenshot 2024-07-06 224332](https://github.com/mishqatabid/WriteUps/assets/145700715/e16add0e-c7a3-430e-bb0b-82d2d822a71b)

After escalating privileges, I got the `root.txt` file and retrieved the final flag.

![Screenshot 2024-07-06 224435](https://github.com/mishqatabid/WriteUps/assets/145700715/529e2f4c-6273-4ba7-97f8-c4b3376010ab)

**Root Flag:** `THM{Y0U_4R3_1337}`

---
## Happy Hacking ;)
---

