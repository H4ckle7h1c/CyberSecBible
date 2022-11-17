## Basics
--- 
### [Ressource Monitor]
resmon.exe
--> overview of system ressources

### [System Information]
msinfo32.msc

--> -   **Hardware Resources**
-   **Components**
-   **Software Environment**

### [System Configuration]
MSConfig.msc

--> Advance troobleshooting

### [Computer Management]
compmgmt.msc

### [User Access Control]
UserAccountControlSettings.exe

--> Access control management

### [Internet Protocol Configuration]
C:\Windows\System32\cmd.exe /k %windir%\system32\ipconfig.exe

--> Ip configuration

[Registry Editor]
regedt32.exe

--> registry editor


## [Permissions in Windows]
--- 
- ### Granting permission 
```cmd 
icacls <file> /grant <towhom>:<privilege>
```
- ### Checking file permissions
```cmd
icacls <file>
```

## [Privileges in Windows]
---
### - Checking user privileges
```cmd
whoami /priv
```

## [Services in Windows]
---
- ###  Checking configuration of a service
```cmd
sc qc <SERVICE_NAME>
```
- ### Start/Stop service
```cmd
sc <START/STOP> <SERVICE_NAME> 
```
- ### Configure a service 
```cmd 
sc config <SERVICE> binPath=<PATH>" obj=LocalSystem
```

## [Backup in Windows]
---
- ### Backuping System
```cmd
reg save hklm\system C:\Users\THMBackup\system.hive
```

- ### Backuping Sam
```cmd
reg save hklm\sam C:\Users\THMBackup\sam.hive
```

## [Remote Desktop Protocol]
- ### Using xfreerdp on kali linux
```bash
xfreerdp /u:admin /p:password /cert:ignore /v:MACHINE_IP /workarea
```