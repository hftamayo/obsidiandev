
00- Setting up de proxychains

Hay 2 archivos relacionados:
proxychains.conf y proxychains4.conf, la diferencia es que la version 4 es una mejorada en cuanto estabilidad , peformance y compatibilidad,

En PopOS uso proxychains.conf y tengo el siguiente agregado al final:

#socks4 	127.0.0.1 9050
socks4		127.0.0.1 8080

En Kali usaré proxychains4.conf asi:

![[Pasted image 20230811134855.png]]

![[Pasted image 20230811135645.png]]

recordemos que proxychains va a salir via la conexion SSH que establezco en el OJ por medio del server 10.0.0.65 a través del puerto 8080

01 - Setting up git behind a proxy:

![[Pasted image 20230606090025.png]]

![[Pasted image 20230606090057.png]]

02 - maven e intellij:

sudo apt install maven

carpetas relacionadas:
/usr/share/maven/bin/mvn y /usr/bin/mvn

revisando la version de maven:

![[Pasted image 20230818101402.png]]

Maven:

en el HOME de Maven 3.x se encuentra el settings.xml:

![[Pasted image 20230818102121.png]]

![[Pasted image 20230818102149.png]]

modificando el /etc/maven/settings.xml:

![[Pasted image 20230818102324.png]]

haciendo los cambios:

![[Pasted image 20230818102538.png]]

para verificar que no interfiere en nada moveré el settings que tengo en HOME:

![[Pasted image 20230818102648.png]]

verificando si maven se puede conectar a los repos aun y con estas configuraciones no me puedo conectar. ASi que verifique en la documentación oficial y apunta a que estos cambios se hacen en el HOME:

![[Pasted image 20230822092141.png]]

ASi que estos son los pasos para hacer efectivos estos cambios:


![[Pasted image 20230606090405.png]]


![[Pasted image 20230606090324.png]]


probando si da bola esta configuracion de Maven con mi RestAPI de ToDo:

- Desde el IntelliJ busca actualizaciones en el pom.xml y efectivamente había y las descargo.
- Probando desde la terminal:

![[Pasted image 20230822093732.png]]

....

![[Pasted image 20230822093648.png]]

indudablemente que el xml que funciona es el del HOME. Haciendo cambios:

1. eliminando la data del proxy en el /etc/maven

![[Pasted image 20230822094249.png]]

notese que esta comentariado, por eso nunca se leyo esta configuracion, la quite o no la data no tiene ningun impacto sobre el servicio.

2. corrigiendo el settings.xml de mi HOME

antes:

![[Pasted image 20230822094457.png]]

despues:

![[Pasted image 20230822094614.png]]
probando de nuevo con el comando:  mvn dependency:copy-dependencies


![[Pasted image 20230822095639.png]]

....

![[Pasted image 20230822095534.png]]

efectivamente las dependencias se descargaron en la carpeta dependecy (antes del comando copy depencies no existia):

![[Pasted image 20230822095837.png]]


intellij y android studio:

![[Pasted image 20230606090502.png]]

![[Pasted image 20230606091123.png]]

![[Pasted image 20230606091158.png]]

esta es para estar seguro que se podrá conectar a cualquier repo:

![[Pasted image 20230606093008.png]]

a pesar que si puedo hacer un test de conexion pero la dependencia no se descarga:

![[Pasted image 20230606093717.png]]

Probando con Android Studio:

![[Pasted image 20230612101859.png]]

![[Pasted image 20230612101936.png]]

cuando abri el proyecto me aparecio esto:

![[Pasted image 20230612103022.png]]

le di OK y seguidamente en la esquina inferior derecha me aparecio que el password no estaba configurado en el archivo properties, le di click al licnk y me aparecio esto:

![[Pasted image 20230612103335.png]]

este archivo forma parte del Gradle Script:

![[Pasted image 20230612103406.png]]

Pero en realidad no pertenece al proyecto, este se encuentra en HOME/.gradle:

![[Pasted image 20230612103809.png]]

ADICIONAL: configuracion de gradle con proxy
...

![[Pasted image 20230612103921.png]]

primero que nada este archivo lo metí al .gitignore, aunque no pertenece al proyecto pero no esta demas en un futuro que tenga un gradle.properties relacionado y por algun motivo tenga que poner KEYS y credenciales; despues de eso agregue el password en este gradle.properties file, abri - cerre el proyecto y verifique si ya los paquetes pueden descargarse:

![[Pasted image 20230612104358.png]]

a pesar de todas esas config no pude descargar los paquetes deseados, ni idea que más necesita, asi mismo en el gradle.properties puse los 2 passowrd (uno por cada proxy) pero nada. Quito el proxy del Android Studio y dejo comentareadas las lineas pertinentes en el archivo gradle.properties



03 -  npm

![[Pasted image 20230606105347.png]]

Para usar nvm con proxy:

![[Pasted image 20230606110251.png]]


04 - yarn

yarn es el unico que no tiene archivo de configuracion, sin embargo se me ocurrió usar el comando get de yarn para ver que salia y mi sorpresa es esta:

![[Pasted image 20230606113909.png]]

probando si la configuracion funciona:

![[Pasted image 20230606115601.png]]

05 - ejecutar una aplicacion node:

por muy pelado que se escucha aun y cuando puedo replicar hacia repos npm al ejecutar la linea : npm run devdays no podia conectarme a mongodb; despues de intentar de tantas maneras logre siguiendo este camino:

- establecer proxychains a un equipo que este en una DMZ:

![[Pasted image 20230612132720.png]]

- tener configurado proychains previamente (ver inicio de este HTML) y pasarme al directorio de trabajo de la aplicacion:

![[Pasted image 20230612132848.png]]


06 - apt:

![[Pasted image 20230609151926.png]]

![[Pasted image 20230609152007.png]]

07 - proxychains con navegador:

- el archivo que debo configurar es /etc/proxychains4.conf
-  modificaciones necesarias:
	- socks5 127.0.0.1 8080 (comentar la linea con el 9050 que esta al final y a continuacion agregar esta)
	- comentar la linea strict_chain
	- activar la linea dynamic_chain
- no es necesario activar el servicio tor, aunque si quiero navegar anonimo:
	- sudo systemctl start | status tor
- abrir el puente con el servidor 10.0.0.65:
	- ssh -D 8080 root@10.0.0.65
- desde mi consola ejecuto firefox:
	- proxychains4 firefox
	- para probar previamente el puente puedo usar curl:
	- proxychains4 curl ifconfig.co/country

![[Pasted image 20230815150431.png]]

cuando aparece que la conexion esta OK es que efectivamente hay navegacion.

08 - forzar a que proxychains use un archivo de configuracion en especial:

![[Pasted image 20230815120731.png]]

09 - proxychains (tunneling), navegador y Burp Suite

La siguiente es una transcripcion del Discord de Burp sobre esta duda:

![[Pasted image 20230815111622.png]]

![[Pasted image 20230815111634.png]]

![[Pasted image 20230815111705.png]]

![[Pasted image 20230815111733.png]]

![[Pasted image 20230815111758.png]]

Inicialmente hice estos cambios y el resultado es este:

![[Pasted image 20230816133828.png]]

El proxy listener es de rigor pues es necesario para capturar las trazas con el Intercept.

Respecto al tunel:

![[Pasted image 20230816133942.png]]

Respecto a mi navegador:

![[Pasted image 20230816134044.png]]

![[Pasted image 20230816134138.png]]

![[Pasted image 20230816134244.png]]


antes de depurar este error voy a instalar el certificado de BurpSuite pensando que era eso, pero en realidad no, pues pude tener conectarme con Burp desde chromium, pero desde Firefox no pues no se si el proxychains esta bloqueando esa comunicacion.


10. go get: se puede configurar un proxy con go env pero actualmente solo pueden pasarse credenciales si el proxy es HTTPS, en el caso del OJ por tanto no podré conectarme por este medio por aspectos de seguridad del mismo proyecto.


11.  conectarse a un servicio en linea por medio del proxychains

![[Pasted image 20230907082520.png]]

![[Pasted image 20230907082559.png]]

![[Pasted image 20230907082705.png]]







12. 