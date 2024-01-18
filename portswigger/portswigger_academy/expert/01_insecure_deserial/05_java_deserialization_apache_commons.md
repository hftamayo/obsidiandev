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

Siguiendo el metodo de bug bounty espana:

Totalmente pelado lo que me paso:

1. descargue el jar desde aca:  https://github.com/frohoff/ysoserial/releases/tag/v0.0.6
2. lo pegue en esta direccion: 
![[Pasted image 20230627202911.png]]

3. sin estar en tmux ejecute el script y luego copie la salida con el mouse

![[Pasted image 20230627203051.png]]

4. lo pase al encoder de burp, en la segunda ventana le di Text y en la primera seleccione Encode As Url, el resultado fue este:

![[Pasted image 20230627203214.png]]


5. inicie el lab, puse las credenciales y el Home lo mande al repeater, borre la cookie session y le pegue el resultado del Decoder:

![[Pasted image 20230627203307.png]]


6.  primero pase el request a 1.1 y al darle send me salio un 404, luego lo pase a Http 2 y me salio error 500:

![[Pasted image 20230627203404.png]]

....

![[Pasted image 20230627203440.png]]

7. buscando el error estaba en el navegador cuando me regrese al lab y vi que se habia resuelto, creo que debo esperar algunos minutos para que el cache se refreque:

![[Pasted image 20230627203536.png]]