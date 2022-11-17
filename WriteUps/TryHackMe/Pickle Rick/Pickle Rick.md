![[Pasted image 20221105221455.png]]
- 1st step enumeration
Looking at the source code of the page

![[Pasted image 20221105221521.png]]
We find a note with the Username *R1ckRul3s*

![[Pasted image 20221105221826.png]]

We got 2 services ssh and http which seems to be logica as it is a web server. And we have the versions of the services.

![[Pasted image 20221105221922.png]]

![[Pasted image 20221105222709.png]]

Robots.txt is interesting: it's content 'Wubbalubbadubdub'

![[Pasted image 20221105222734.png]]

![[Pasted image 20221105222107.png]]

For now I think we should try to go more in depth. A thing would be to hydra bruteforce the ssh with the username but will be very long.

- 2nd step try to exploit information
In the login page :
username = R1ckRul3s
let's try password = Wubbalubbadubdub

![[Pasted image 20221105223026.png]]

youpi

![[Pasted image 20221105223041.png]]

![[Pasted image 20221105223116.png]]

![[Pasted image 20221105223130.png]]

Let's try an other way :
![[Pasted image 20221105223238.png]]
We found the first ingredient
mr. meseek hair

![[Pasted image 20221105223359.png]]

Reverse shell method with mkfifo
![[Pasted image 20221105224833.png]]

![[Pasted image 20221105224915.png]]

1. What is the first ingredient Rick needs?

3. Whats the second ingredient Rick needs?
1 jerry tear

5. Whats the final ingredient Rick needs?
sudo bash

![[Pasted image 20221105231822.png]]

terminé