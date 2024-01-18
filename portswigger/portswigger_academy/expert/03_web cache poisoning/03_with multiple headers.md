![[Pasted image 20230106120926.png]]

![[Pasted image 20230106120957.png]]

regresando al intercept para buscar el request hacia el archivo .js

![[Pasted image 20230106121210.png]]


verificando si soporta redireccion:

![[Pasted image 20230106121242.png]]

GET /resources/js/tracking.js?cb=1234 HTTP/1.1
X-Forwarded-Host: example.com

![[Pasted image 20230106121421.png]]

Ahora probaremos con :  X-Forwarded-Scheme: http hasta que de un 302, a veces se tarda y hay que hacer pausa entre requests:

![[Pasted image 20230106121820.png]]

![[Pasted image 20230106121839.png]]

Ahora probamos agregando un Forwarded-Host:

![[Pasted image 20230106121956.png]]

![[Pasted image 20230106122013.png]]

notese que en la location aparece el link que arme en el payload

usando el exploit server hago el siguiente payload, lo guardo y luego copio la direccion del exploit server:

![[Pasted image 20230106122241.png]]

https://exploit-0aab009503f2998bec0af84f01ff00b2.exploit-server.net/

lo pego en el payload que tenemos el repeater:

![[Pasted image 20230106122427.png]]

![[Pasted image 20230106122446.png]]

si quiero verificar el contenido del archivo js envenenado uso la direccion del exploit server:

http://exploit-0aab009503f2998bec0af84f01ff00b2.exploit-server.net/resources/js/tracking.js

![[Pasted image 20230106122626.png]]

finalmente limpiamos el cb=1234 en el GET y envio el request nuevamente

![[Pasted image 20230106122737.png]]

es importante que en el respose de un 302 y no un 200

![[Pasted image 20230106122819.png]]

![[Pasted image 20230106122847.png]]


