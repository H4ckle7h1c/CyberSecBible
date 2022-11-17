## [subdomains ]
---

- dnsrecon :
```
dnsrecon -d [domain]
```
- sublist3r :
```
sublist3r.py -d [domain]
```
- ffuf : 
```
ffuf -w [wordlist] -H "Host: FUZZ.test.com" -u http://MACHINE_IP -fs {size}
# -w for WL and -fs for the filtering of unsuccessfull results
```
https://dnsdumpster.com/

## [usernames]
--- 
- ffuf : 
```
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.246.97/customers/signup -mr "username already exists"'
```

## [Websites]
- ### gobuster
```bash
gobuster dir -u http://10.10.130.131 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt 

```
## [Network scanning]
---
- ### nmap
classic
```cmd 
sudo nmap -sC -sV <IP_ADDRESS> -oN out.txt
```
arp-scan

[enum4linux]

## [Linux System]
---
```bash
# Additionnal informations about the kernel
$ uname -a

# Information about target sys file system processes 
$ cat /proc/version

# Informations about the os, can be customized
$ cat /etc/issue
$ cat /etc/os-release

# Environment variables 
$ env

# Processes
$ ps -A //all processes
$ ps axjf //processes tree

# User permissions 
$ sudo -l
$ id
$ cat /etc/passwd

# Historic of commands
$ history

# Network config
$ ifconfig
$ ip route
$ netstat

# Files
$ find
$ find / -perm -o x -type d 2>/dev/null : Find world executable
$ find / -writable -type d 2>/dev/null  : Find world-writeable folders
$ find / -perm -222 -type d 2>/dev/null : Find world-writeable folders
$ find / -perm -o w -type d 2>/dev/null : Find world-writeable folders
$ find / -perm -o x -type d 2>/dev/null : Find world-executable folders
$ $find / -name perl*
$ find / -name python*
$ find / -name gcc*
```