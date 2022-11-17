It has been created by OWASP

## Topics covered

- Injection

- Broken Authentication

- Sensitive Data Exposure

- Broken Access Control

- Cross-Site Scripting XSS

## STEP  1 - Reconnaissance
--- 
Walk through the application !

![[Pasted image 20221103091846.png]]

[emails]
admin@juice-sh.op
bender@juice-sh.op
uvogin@juice-sh.op
jim@juice-sh.op

![[Pasted image 20221103092055.png]]

search parameter **q**

let's check the login
![[Pasted image 20221103092359.png]]

## STEP 2 - Focus on injection vulnerabilities
---

I tried a **'** to see if vulnerable to sql injection, and apparently yes.
![[Pasted image 20221103092556.png]]

In the meantime I also discovered a flag : 169940f83378cc420ae4fdeb9c1f73631a2baee6

Let's try to SQL injection.

Capture a request with BURP
![[Pasted image 20221103092415.png]]
![[Pasted image 20221103093214.png]]


![[Pasted image 20221103093132.png]]