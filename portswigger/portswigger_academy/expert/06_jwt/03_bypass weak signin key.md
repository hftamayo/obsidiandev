![[Pasted image 20230422170008.png]]

![[Pasted image 20230422172155.png]]

![[Pasted image 20230422172226.png]]

![[Pasted image 20230422172252.png]]

![[Pasted image 20230422172322.png]]

![[Pasted image 20230422172354.png]]

copio el hash completo y utilizo hashcat para crakear la palabra clave:

![[Pasted image 20230422172504.png]]

de la siguiente direccion bajo el diccionario de tokens jwt: https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list

![[Pasted image 20230422173157.png]]

hashcat -a 0 -m 16500 eyJraWQiOiJkMzY4ODczMC00ODYyLTRmMDItYWZhNi05YWM0ZGRhNzRjO
DYiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY4MjIwOTMyMH0.uJdbygjbHwN-7F3ca
3o-MNUds-uUE84Ldkiy0frx5IY jwt.secrets.list 

![[Pasted image 20230422173410.png]]

...

![[Pasted image 20230422173449.png]]

secret1 es la key

esto lo pego en el Decoder y lo voy a codificar como base64:

![[Pasted image 20230422173616.png]]

luego me voy al JWT Editor Key y creo una llave de 128 bits:

![[Pasted image 20230422173728.png]]

en el valor de k lo sustituyo por la representación de secret1 en base64:

![[Pasted image 20230422173838.png]]

me vuelvo al repeater y esta vez le doy click a la pestaña de JSON Web Token:

![[Pasted image 20230422181227.png]]

en el payload le cambio a administrator y lo firmo con la key que hicimos:

![[Pasted image 20230422181436.png]]

ahora vuelvo a mandar el request:

![[Pasted image 20230422182018.png]]

es bien importante que en el signin key me asegure que no tengo espacios de mas porque eso afecta la codificacion al base84, de hecho, eso me paso y me dio error al enviar el request:

![[Pasted image 20230422182146.png]]

![[Pasted image 20230422182212.png]]

![[Pasted image 20230422182239.png]]