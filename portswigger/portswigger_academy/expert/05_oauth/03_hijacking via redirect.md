
![[Pasted image 20230213124012.png]]

![[Pasted image 20230213124103.png]]

![[Pasted image 20230213124142.png]]

![[Pasted image 20230213124209.png]]

![[Pasted image 20230213124236.png]]

![[Pasted image 20230213124258.png]]

despues de hacer el logout y vuelvo a entrar con social media creds. En este momento ya me puedo pasar al burp y ordeno el history de manera descendente, la idea es buscar despues de mi ultimo logout un Get Request id y mandarlo al repeater:

![[Pasted image 20230213124836.png]]

![[Pasted image 20230213124902.png]]

cambio el nombre del redirect uri:

![[Pasted image 20230213125024.png]]

para verificar nuevamente que redirige, me voy al exploit server y copio esa misma direccion para pegarla en este endpoint:

![[Pasted image 20230213125133.png]]

![[Pasted image 20230213125206.png]]

si hago click en el follow redirection del repeater y luego voy al log del exploit server, deberia ver el request:

![[Pasted image 20230213125402.png]]

me vuelvo al repeater y esta vez le doy la fecha atras para regresar al payload anterior:

![[Pasted image 20230213125510.png]]

![[Pasted image 20230213125528.png]]

aca le doy click derecho y selecciono "copy url":

![[Pasted image 20230213125804.png]]

presiono guardar y luego view exploit:

![[Pasted image 20230213125921.png]]

aca veo como se esta iterando algo, al ver el log encontramos:

![[Pasted image 20230213130114.png]]

se supone que el ultimo GET es el de la credencial de administrador, me vuelvo al exploit server y presiono "deliver to the victim". Me paso a la pestaña del lab y hago un logout. Aca es importante tener el log del exploit server en la pantalla pues utilizaré el ultimo GET code, que es este: 
pYPuDx9UiiQhrYG4Jt53PEhA4xv8bHe66pWYsjI-1Fx

pues bien, luego me voy al history del intercept y busco el callback que esta justo despues del auth id code que usé en el repeater, aca le doy copy url y lo pego en el navegador web:

![[Pasted image 20230213130722.png]]

le borro el code y lo reemplazo por el ultimo GET, asi:

https://0a3800130393ba9bc19de45e00a9002d.web-security-academy.net/oauth-callback?code=kA4yghiVBxkzrgjxN-wRXYulbno-AUWh6CW2cWHOsVZ
https://0a3800130393ba9bc19de45e00a9002d.web-security-academy.net/oauth-callback?code=

https://0a3800130393ba9bc19de45e00a9002d.web-security-academy.net/oauth-callback?code=PONER EL ULTIMO CODE DEL GET

si todo va bien cuando de enter deberia ver el admin panel

![[Pasted image 20230213130841.png]]

y puedo borrar la cuenta:

![[Pasted image 20230213130859.png]]

![[Pasted image 20230213130924.png]]

