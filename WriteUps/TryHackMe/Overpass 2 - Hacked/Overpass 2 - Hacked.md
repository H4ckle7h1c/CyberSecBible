I pass for now on the pcap analysis which is trivial.

How to connect to the backdoor ?

```
┌──(kali㉿kali)-[~/TryHackMe/Rooms/BufferOverflow]
└─$ ssh 10.10.228.17 -p 2222   
Unable to negotiate with 10.10.228.17 port 2222: no matching host key type found. Their offer: ssh-rsa
```


So a new option :
```
ssh james@10.10.228.17 -p 2222 -oHostKeyAlgorithms=+ssh-rsa
```

As we saw in the pcap james user and the password november16.

Found a suid_bash hidden in the home directory of james
used it with -p option to have root rights and saw the flag