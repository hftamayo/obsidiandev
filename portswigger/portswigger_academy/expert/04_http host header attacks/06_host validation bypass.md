![[Pasted image 20230209122405.png]]

La resolucion de este lab es por medio de un autor desconocido: https://www.youtube.com/watch?v=PuwzBfkX4EQ

la investigacion de PortSwigger sobre esta vuln: https://portswigger.net/research/browser-powered-desync-attacks#state

por algun motivo cuando quice abrir el lab me daba timeout.

![[Pasted image 20230209124743.png]]

![[Pasted image 20230209124804.png]]

mando al repeater el root

![[Pasted image 20230209124841.png]]

esta misma traza la duplico y la envío al repeater:

![[Pasted image 20230209124922.png]]

hago los siguientes cambios:

![[Pasted image 20230209125004.png]]

me devuelve esto:
![[Pasted image 20230209125024.png]]

agrupo ambas trazas con una opcion que entiendo aparece de la version de burp 2022, en esta version se puede enviar un grupo de trazas repeater como un mismo request. 

Para resolver este lab necesito actualizar a esta version:

![[Pasted image 20230209125821.png]]

![[Pasted image 20230209125840.png]]

siguiendo la solucion de bug bounty espana y con el burp 2022:

mando esta traza al repeater 2 veces:

![[Pasted image 20230626205949.png]]


![[Pasted image 20230626210056.png]]

en el signo mas creo el grupo:

![[Pasted image 20230626210138.png]]\

![[Pasted image 20230626210209.png]]

click derecho y change request method:

![[Pasted image 20230626210248.png]]


agrego el endpoint objetivo y una ip al azar:

y la propiedad Connection la dejo como "keep alive"

![[Pasted image 20230626210436.png]]

luego en el boton Send selecciono single connection:

![[Pasted image 20230626210529.png]]

en la pestana 2 y 1 presiono el boton Send y debo obtener un 200 en la pestana1:
![[Pasted image 20230626210728.png]]

![[Pasted image 20230626210744.png]]


en la pestana 1 cambio connection a keep-alive y le doy enviar, notese que en la pestana 2 aparece respuesta y el endpoint de admin:

![[Pasted image 20230626210933.png]]

copio el csrf y le doy enviar en la pestana 2:

08D5d5Suh0hKqKKel3b9r0HO8bBuGLwX


vuelvo a copiar el csrf que aparecio:

![[Pasted image 20230626211132.png]]

08D5d5Suh0hKqKKel3b9r0HO8bBuGLwX

Ahora este payload:

POST /admin/delete HTTP/1.1
Host 192.168.0.1
Cookie: 
Content-Type: x-www-form-urlencoded
Content-Length: CORRECT

csrf:08D5d5Suh0hKqKKel3b9r0HO8bBuGLwX&username=carlos

lo sustituyo en el request de la pestana 2:

![[Pasted image 20230626211423.png]]

y queda asi:

![[Pasted image 20230626211530.png]]

tengo un error en la imagen de arriba, debe ser csrf =

![[Pasted image 20230626211646.png]]

presiono el boton enviar single connection:

![[Pasted image 20230626211710.png]]

![[Pasted image 20230626211728.png]]