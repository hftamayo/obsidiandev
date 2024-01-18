![[Pasted image 20230118121854.png]]

![[Pasted image 20230118124810.png]]

![[Pasted image 20230118124834.png]]

![[Pasted image 20230118124932.png]]

voy al exploit server y en los botones de abajo esta check email client

![[Pasted image 20230118125025.png]]

![[Pasted image 20230118125058.png]]

reseteamos password:

![[Pasted image 20230118125154.png]]

me voy al http history y mando el post forgot password al repeater:

![[Pasted image 20230118125508.png]]

envio el response y verificamos que es un 200:

![[Pasted image 20230118125632.png]]

![[Pasted image 20230118125652.png]]

puesto que ya envie nuevamente esta peticion, en el correo deberia tener otro link:

![[Pasted image 20230118125802.png]]

ahora, necesitamos generar un link para un host que no exista, para eso modificamos el request asi y luego lo enviamos:

![[Pasted image 20230118130050.png]]

![[Pasted image 20230118130108.png]]

chequeamos el correo:

![[Pasted image 20230118130144.png]]

notese que el dominio es distinto. Ahora en el repeater quito el dominio falso y le pondré la url del exploit server:

![[Pasted image 20230118130318.png]]

sin el exploit lo pego en el request del repeater y cambio el usuario de wiener a carlos:

![[Pasted image 20230118130459.png]]

en la imagen de arriba lo pegue con el protocolo y obtuve un error, lo correcto es:

![[Pasted image 20230118130614.png]]

![[Pasted image 20230118130632.png]]

en el exploit server vamos al access log:

![[Pasted image 20230118130719.png]]

si todo anda bien deberia ver un get request con un token:

![[Pasted image 20230118130844.png]]

note que la ip es del backend. Con este token voy a construir una url:

Usy9fD63SNTQJsjvtQwTlS47tPgs4XVr

voy al correo y puedo escoger una url de link valido para cambiar password, en mi caso, seleccionare el primero que utilice:

![[Pasted image 20230118131128.png]]

https://0a15002204feba8fc002ae5e003f000e.web-security-academy.net/forgot-password?temp-forgot-password-token=6hootMdpvZ58jNe7UIAgHWRAA8rczYFp

cambiamos password

![[Pasted image 20230118131329.png]]

probamos las credenciales de carlos


![[Pasted image 20230118131531.png]]
