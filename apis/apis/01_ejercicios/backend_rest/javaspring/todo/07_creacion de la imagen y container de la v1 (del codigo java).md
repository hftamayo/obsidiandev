1. Creando el JAR del aplicativo:

![[Pasted image 20230817150322.png]]

![[Pasted image 20230817150432.png]]

Luego me voy al menu Build -> Build Artifacts y selecciono Build, si todo salio bien debería ver un nuevo archivo junto con directorios:

![[Pasted image 20230817150706.png]]

![[Pasted image 20230817150903.png]]


2.  Dockerfile
![[Pasted image 20230817151103.png]]

3.  docker compose (yml):

![[Pasted image 20230817151150.png]]

4. Ejecutando el yml

![[Pasted image 20230817153533.png]]

![[Pasted image 20230817153617.png]]

![[Pasted image 20230817153717.png]]

5. Depurando al Exited (1) :

![[Pasted image 20230818091957.png]]

1. sudo apt install maven
2. desde el directorio principal de la aplicacion: mvn clean package

![[Pasted image 20230818093821.png]]

en las lineas 43, 49, 59 y 62 de mi pom.xml son las que se refieren a RELEASE, entonces imagino que tengo que poner las versiones que necesito.

Asi mismo. estoy obteniendo este error:

![[Pasted image 20230818110236.png]]

en el proyecto tengo configurado la version 17 de java:

![[Pasted image 20230818110313.png]]

pero en el sistema tengo la version 11:

![[Pasted image 20230818110345.png]]

efectivamente estoy usando versiones distintas, trataré de actualizar la version del sistema a la 17:

![[Pasted image 20230818110607.png]]


esta es la del sistema:
![[Pasted image 20230818110904.png]]


editando el .bashrc para cambiar de version:


![[Pasted image 20230818110818.png]]

![[Pasted image 20230818111037.png]]

revirtiendo cambios y tratando de instalar la version open 17 en todo el sistema:

![[Pasted image 20230818111759.png]]

![[Pasted image 20230818111850.png]]

![[Pasted image 20230818111936.png]]

en caso que en algun momento quiera cambiar de version de java:

![[Pasted image 20230818112237.png]]

actualizando en el intellij este cambio y luego intentando con maven me sigue dando error, veamos la configuracion de maven:

![[Pasted image 20230818152426.png]]

puesto que maven fue instalado cuando aun no había la version 17 sigue usando la anterior,  actualizando maven con el cambio al jdk 17:

![[Pasted image 20230818153326.png]]

![[Pasted image 20230818153403.png]]

mvn clean package desde el proyecto:

![[Pasted image 20230818153752.png]]

....

![[Pasted image 20230818153813.png]]

3. puesto que el proceso termino bien, en este caso la ubicación del jar es otra:

![[Pasted image 20230818154051.png]]

haciendo los cambios en el dockerfile:

![[Pasted image 20230818154402.png]]

verificando contenedores y luego generando el compose nuevamente:

![[Pasted image 20230818154444.png]]

![[Pasted image 20230818154600.png]]

notese que los cambios aun no se han replicado pues el container sigue usando todo.jar

usando el comando docker-compose up --build

![[Pasted image 20230818154826.png]]

...

![[Pasted image 20230818154903.png]]

justo aca menciona que enviro.properties no existe en la imagen. Haciendo los cambios al Dockerfile:

![[Pasted image 20230821094309.png]]

regenerando la imagen me dio un error relativo al modificador java.security, particularmente en ./random, asi que quite esa linea, despues que hice esto empece a obtener una serie de errores respecto al url del aplicativo:

![[Pasted image 20230821105758.png]]

modifique el pom.xml y el application.properties, asi que volvi a generar el jar y luego ha realizar un rebuild de la imagen:

![[Pasted image 20230821110027.png]]

mvn clean package

![[Pasted image 20230822103334.png]]

docker-compose up --build pero sigo recibiendo el error de que no encuentra un Driver.

![[Pasted image 20230822144444.png]]

lo que hice fue quitar el uso de enviro.properties y poner las credenciales planas en application.properties, cuando ejecute la aplicacion ahi me di cuenta que los contenedores no pueden comunicarse, si bien la aplicacion me funciona cuando la corro desde el IDE y el mysql container esta arriba, la comunicacion no funciona entre el jar en el contenedor y el mysql en contenedor.

Evidentemente hay un problema de comunicación entre el jar y el docker. Si en application properties dejo localhost funciona desde el IDE ygenerando el jar pero no asi con el docker, en cambio si desde el el properties ya incluyo el nombre del container obtengo este error:

![[Pasted image 20230823114203.png]]

este error lo obntengo desde mvn clean package.

En el tema de los containers un comando bien util es:
docker logs -f <nombre del container>

Para seguir el proceso de depuracion ver el item 08 de este archivo