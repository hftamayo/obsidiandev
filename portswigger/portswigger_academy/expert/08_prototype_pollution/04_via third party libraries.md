![[Pasted image 20230529182330.png]]

este lab sera resuelto usando casi en su totalidad el DOM invader que ya viene cargado en el burp browser, carog el lab, copio la direccion y me paso al burp browser, previamente para aprender a usar DOM Invader hice la consulta en twitter y recibi este link:
https://t.co/bIXbqAiB3M

![[Pasted image 20230530195033.png]]

aunque efectivamente funciono pero con el lab de portswigger no encuentro como hacerlo funcionar:

![[Pasted image 20230530202102.png]]


leyendo la descripcion de la solucion oficial, DOM Invader debe tener activada la opcion de Prototype Pollution, de inicio la herramienta esta asi:

![[Pasted image 20230530202608.png]]



despues de activarla y darle update:

![[Pasted image 20230530202737.png]]

ya se activa todo


Se encuentran 2 vulnerabilidades y le doy click en el boton "scan for targets" de la primera coincidencia:

![[Pasted image 20230530202941.png]]

se abre otra pestana y el Dom invader escanea el sitio:

![[Pasted image 20230530203018.png]]

checkando el Proxy History vemos varios hits:

![[Pasted image 20230530203116.png]]

si me regreso a la segunda pestana y despliego el dom invader:

![[Pasted image 20230530203321.png]]

lo que hice fue cerrar la pestana, regresarme a la primera y darle click nuevamente a la primera vuln, cuando estaba escaneando aprete F12 y me pase al Dom Invader y justo esta el exploit:

![[Pasted image 20230530203534.png]]

si le doy click en el boton de exploit muestra el alert:

![[Pasted image 20230530203637.png]]

entonces copio este URL y me paso al navegador normal, paso al exploit server e inicialmente puedo hacer este script:

![[Pasted image 20230530203913.png]]

modificando el payload para que muestre la cookie del sitio:

![[Pasted image 20230530204007.png]]

luego de esto presiono Store y luego Deliver to the Victim:

![[Pasted image 20230530204139.png]]

