
Pre-requisitos para la generacion de la imagen:

1. tener el container de la base de datos deployado, el archivo yml y sus auxiliares deben estar bien limpios y claros sobre lo que se requiere, favor ver el repositorio personal de dockerservices en github
2. el pom.xml y los archivos properties deben estar bien limpios, inicialmente las credenciales de conexion estoy usandolas bare metal para asegurarse de comprender el proceso de conexion
3. la unica forma de alcanzar el db container desde el host (a la hora de ejecutar el mvn clean package) es tenerlo mapeado en el /etc/hosts:

![[Pasted image 20230829144853.png]]

4.  los archivos properties lucen asi:
![[Pasted image 20230829145649.png]]

![[Pasted image 20230829145732.png]]

notese todos los comments que he hecho pero ha sido debido a un proceso de depuración a base de prueba y error. Desde aca se levanta el properties que es para docker:

![[Pasted image 20230829145828.png]]


Al momento definimos que:
* en fase inicial de desarrollo pueden hacerse pruebas con una instalacion mysql local y en el puerto 8080
* si quiero trabajar unicamente con docker puede usar la conexion en application-docker.properties


3. Al ejecutar maven clean package estoy obteniendo este error:

![[Pasted image 20230829152230.png]]

![[Pasted image 20230829152306.png]]

* no entiendo porque motivos los test tratan de conectarse al 172.22.0.1
* vale la pena mantener los scripts en fase de produccion? 

4. Sobre este error este link fue de mucha utilidad: https://www.baeldung.com/spring-junit-failed-to-load-applicationcontext
Puesto que estoy usando más de un properties entiendo que hay que informarlo en la aplicacion, me parece que tiene mucho que ver con la gestion de profiles

5.  Posible solucion #1
* Intellij -> File -> invalidate cache -> restart (no fue necesario cambiar ninguna opcion)
* luego de eso me di cuenta que habia un script de Testing pero el default: 

![[Pasted image 20230830093115.png]]

unicamente lo borre y despues de eso corrí el proyecto desde el IDE y funciono:

![[Pasted image 20230830093226.png]]

* probando generar el jar desde maven: 

![[Pasted image 20230830093353.png]]
....

![[Pasted image 20230830093423.png]]

![[Pasted image 20230830093522.png]]

activando el perfil de docker y tratando de generar el jar:

![[Pasted image 20230830093624.png]]

application-docker.properties:

![[Pasted image 20230830093657.png]]

![[Pasted image 20230830093835.png]]

....

![[Pasted image 20230830093924.png]]

* Verificando el timestamp de los archivos hay una diferencia por tanto puedo inferir que el properties del jar esta apuntando al mysqldocker.
* Verificando el estado del container:

![[Pasted image 20230830094113.png]]

6. Aparentemente todo ha funcionado, posiblemente el origen del error era el script de Test que estaba en blanco. Verificando si al fin puedo generar la imagen del app:

* apagando el container y haciendo un prune:

![[Pasted image 20230830094542.png]]

* ejecutando docker compose:

![[Pasted image 20230830102415.png]]

![[Pasted image 20230830102444.png]]

![[Pasted image 20230830102545.png]]

Siguiendo la indicacion de hacer un buid:

![[Pasted image 20230830102750.png]]

...

![[Pasted image 20230830102825.png]]

7. la IP Address de mysqldocker es:

![[Pasted image 20230830103718.png]]

la de jsbtodorest no obtiene IP pero pertenece a la misma red:

![[Pasted image 20230830103856.png]]

el parametro de conexion es:

![[Pasted image 20230830104235.png]]

verificando el docker-compose:

![[Pasted image 20230830104306.png]]

lo unico distinto que veo es que el nombre del servicio es dbmysqldocker y el nombre del container es mysqldocker, homologando ambos nombres:

![[Pasted image 20230830104403.png]]

haciendo prune, eliminando imagenes y luego reintentando:

![[Pasted image 20230830104721.png]]

![[Pasted image 20230830105200.png]]

![[Pasted image 20230830105225.png]]

Ahora el jsbtodorest ni siquiera ve al container mysql, al menos antes trataba de conectarse a alguna ip de la red mysqlnetwortk

8. Verificando en el container de mysql si el usuario y la tabla existen:

![[Pasted image 20230913142536.png]]

![[Pasted image 20230913142603.png]]

![[Pasted image 20230913142631.png]]

![[Pasted image 20230913142738.png]]

![[Pasted image 20230913142758.png]]

el estado de los contenedores es este:


![[Pasted image 20230913142848.png]]

el error en el log del contenedor java es:

![[Pasted image 20230913143109.png]]

levante el contenedor de java y la aplicacion se mantiene estable. Al parecer es que al construir ambos contenedores mysql necesita tiempo para ponerse en status ONLINE.

la ruta que segui es:
- limpiar mi docker compose de lineas innecesarias
- agregar un healthy check en el mysql para hasta cuando ya este lista la db se levante el container java
- hacer uso de chatgpt para escritura del Dockerfile y el composer


9. Al final las versiones que si funcionaron son: , favor ver la rutina de healthy check




10. LECCIONES APRENDIDAS:
* cuando entras en un loop de prueba y error es mejor parar pues me vuelvo inproductivo
* todo cambio en algun archivo de la aplicacion significa que hay que volver a generar el jar
* En el mismo docker-compose tuve que poner las credenciales y el parametro de conexion, obviamente deben ser las mismas del application.properties
* todo cambio en el container de la app es util reiniciar el  container de la bd, primero mysql y unos minutos despues la app
* el env debe llamarse asi y se espera que este en el root directory de la aplicacion

11. 