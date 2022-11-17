## Commons
We rarely hand up on a system with administrator privileges which allow us to :

-    Reset passwords  
-    Bypass access controls to compromise protected data
-    Edit software configurations
-    Enable persistence, so you can access the machine again later.
-    Change privilege of users
-    Get that cheeky root flag ;)
![[Pasted image 20221107145736.png]]

### [Linenum]
Simple bash script which enumerates commands related to linux privesc.
```bash
# To download the linenum script the the victim machine
python3 -m http.sever 8080
```
Otherwise if we can't download it and that we have W rights we can copy the script to the machine and execute it. 

### [SUID/GUID]
Manual find  :
```bash
find / -perm -u=s -type f 2>/dev/null
find / -user root -perm -4000 -exec ls -ldb {} \;
```

### [Escape VIM]
https://gtfobins.github.io/

escape => :!sh
```bash
# List all sudo commands
sudo -l
``` 

### [Crontab]
Use msfvenom


[Kernel exploit]
		https://github.com/jondonas/linux-exploit-suggester-2


## [POST]
--- 
```shell
post/multi/manage/shell_to_meterpreter
```


