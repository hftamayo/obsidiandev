![[portswigger_academy/expert/08_prototype_pollution/images/Pasted image 20220929174022.png]]

![[portswigger_academy/expert/08_prototype_pollution/images/Pasted image 20220929175332.png]]

selecciono la cookie y se activa el inspector:

![[portswigger_academy/expert/08_prototype_pollution/images/Pasted image 20220929175425.png]]

la codificacion es Base64 con un SHA1-HMAC . Inspector decodifico la cookie:

{"token":"Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJveHZxbGgwMnlubHB0czZnMXR5eWNqeTU0ZWI0ZHhoeiI7fQ==","sig_hmac_sha1":"ae88ab33c1f1c76c72c1b600ba6ec81ed51b1c57"}

la paso al decoder:

Selecciono unicamente el token y selecciono Decode as Base64:

![[portswigger_academy/expert/08_prototype_pollution/images/Pasted image 20220929175815.png]]

el resultado es este: {"token":"O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"oxvqlh02ynlpts6g1tyycjy54eb4dxhz";}","sig_hmac_sha1":"ae88ab33c1f1c76c72c1b600ba6ec81ed51b1c57"}

el cual es un objeto PHP deserializado.

Siguiendo el metodo de Bug Bounty Espana: https://www.youtube.com/watch?v=JwN8oWH2TZc

Payload original:

![[Pasted image 20230628203234.png]]

herramienta para deserializar:

![[Pasted image 20230628203501.png]]

generando el payload:

![[Pasted image 20230628203640.png]]

como supimos que es para symfony?

iniciamos el lab:

![[Pasted image 20230628204037.png]]

![[Pasted image 20230628204058.png]]

![[Pasted image 20230628204118.png]]

![[Pasted image 20230628204145.png]]

si cambio algun caracter del payload:

![[Pasted image 20230628204211.png]]

de aca tomamos que el payload debe ser para simfony.

copio el payload y lo paso a mi script exploit y lo pongo en una linea:

![[Pasted image 20230628204331.png]]

![[Pasted image 20230628204406.png]]

para obtener el secretkey:

![[Pasted image 20230628204519.png]]

el script lo mando al repeater y le doy send:

![[Pasted image 20230628204546.png]]

![[Pasted image 20230628204626.png]]

![[Pasted image 20230628204656.png]]

notese en el script de arriba que a la var payload le falta el simbolo de dolar al inicio, corregido ese error ejecuto el script:

![[Pasted image 20230628205015.png]]

este payload lo sustituyo en el repeater y aunque recibi un 500 el lab se resolvio:

![[Pasted image 20230628205210.png]]

![[Pasted image 20230628205225.png]]
