para instalar dependencias en modo testing:

![[Pasted image 20230907130817.png]]

![[Pasted image 20230907130921.png]]


testing del controller todo: https://www.digitalocean.com/community/tutorials/test-a-node-restful-api-with-mocha-and-chai

https://buddy.works/tutorials/unit-testing-jwt-secured-node-and-express-restful-api-with-chai-and-mocha

testing del controller user: 

tengo 2 scripts:

![[Pasted image 20230814132307.png]]

para correrlo actualmente he encontrado esta opcion:

npm run testrpt (genera un xml)

npm test (genera salida en pantalla)

sin embargo no funcionan con la conexion a mongodb si no uso proxychains y previamente tengo un tunnel con el 10.0.0.65:

![[Pasted image 20230815121357.png]]

entonces el comando es:
proxychains4 -f /etc/proxychains4.conf npm test

sin embargo estoy teniendo un timeout entre el proxy y el server:

![[Pasted image 20230817123959.png]]


por eso obtengo que el res esta vacío:

![[Pasted image 20230817124116.png]]

![[Pasted image 20230907122244.png]]

Probando conexion con mi celular:

![[Pasted image 20230907122753.png]]

![[Pasted image 20230907122905.png]]

MEjorando el codigo y corriendo el test con mi celular:

![[Pasted image 20230908091825.png]]

haciendo otro cambio al codigo y corriendolo con proxychains:


