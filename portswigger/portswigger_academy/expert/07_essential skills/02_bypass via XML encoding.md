
![[Pasted image 20230521061254.png]]

apagamos el intercept y hago una verificacion de cualquier request:

![[Pasted image 20230521063937.png]]

![[Pasted image 20230521064000.png]]

identifico este endpoint y lo paso al repeater:

![[Pasted image 20230521064041.png]]

![[Pasted image 20230521064108.png]]

verificamos si puede aceptar un SQLI:

![[Pasted image 20230521064151.png]]

con Hackvertor ya instalado en el burp, hago click derecho -previamente sombreando el payload- luego : extensions -> hackvertor -> encode -> hex entities

![[Pasted image 20230521064405.png]]

el payload queda asi:

![[Pasted image 20230521064521.png]]

y al enviar el request otengo esto:

![[Pasted image 20230521064555.png]]

ahora modificaré el payload para obtener las centas de usuarios:

UNION SELECT username || '~' || password FROM users

![[Pasted image 20230521064705.png]]

![[Pasted image 20230521064725.png]]

usando las creds de admin para logearme:

![[Pasted image 20230521064815.png]]

