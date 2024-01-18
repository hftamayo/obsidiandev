![[Pasted image 20230630212823.png]]

me logeo con credenciales, pongo un comentario con un correo bueno y luego con un correo malo, todo lo cacho estando el intercept en off

![[Pasted image 20230630213022.png]]

![[Pasted image 20230630213117.png]]

![[Pasted image 20230630213137.png]]

![[Pasted image 20230630213223.png]]

![[Pasted image 20230630213247.png]]

el correo invalido no se guardo en los comentarios.

En el history busco el post comment y el get con el id de la entrada del blog, ambos los mando al repeater:

![[Pasted image 20230630213443.png]]

el post sera el encrypted (cuando el post fue exitoso) y el get el decrypted:

![[Pasted image 20230630213635.png]]

![[Pasted image 20230630214041.png]]

EL PRIMer paso es que en el decrypted el stay-logged-in lo copio y lo pego 

![[Pasted image 20230630214154.png]]

ACA OBTENGO UNA ESPECIE DE TOKEN ASOCIADO A WIENER:

![[Pasted image 20230630214303.png]]

                   wiener:1688182227361

el numero lo voy a pegar en el enrypted en el lugar del correo y bajo el asocio de la cuenta de administrator:

![[Pasted image 20230630214546.png]]

![[Pasted image 20230630214603.png]]

recibo un 302  y la notificacion la copio:

4HX8oJlnVI4pBprRmHOO%2bzs9gbiwCuPX1rlk%2f1gatcbe8JjFZElkcbgtisKrzBOs2jAw6jW3EKc6VLA1pH7eOg%3d%3d

Esta notificacion me voy al decrypted y la sustituyo:

![[Pasted image 20230630214722.png]]

![[Pasted image 20230630214818.png]]

obengo un 200 y otro token del admin:

![[Pasted image 20230630214847.png]]

administrator:1688182227361

puesto que obtuve info del admin quiere decir que la notificacion es un asset valido al admin y lo voy a decodear como URL:

![[Pasted image 20230630215107.png]]

el segundo resultado lo decodeo como Base64:

![[Pasted image 20230630215150.png]]

click derecho en el tercer resultado y le voy a borrar 23 bytes:

