1. Para instalar docker en el RHEL seguí como referencia estos links:
https://www.linuxhelp.com/how-to-install-docker-ce-on-rhel-7-6
https://computingforgeeks.com/install-docker-ce-on-rhel-7-linux/


2. Docker para la version 1 de la restAPI (spring boot + mysql) 
- link de referencia: https://apisero.com/dockerizing-a-spring-boot-application-with-mysql-database/


![[Pasted image 20230809144030.png]]

46d2da796325d2bf68f8bb83f12aca52a73747679382ec34394c4053589f3920

![[Pasted image 20230809150205.png]]

![[Pasted image 20230809150244.png]]

3. eliminar una imagen de docker

![[Pasted image 20230810142455.png]]

![[Pasted image 20230810142649.png]]

![[Pasted image 20230810142717.png]]

verificando si hay un container relacionado a mysql:

![[Pasted image 20230810143046.png]]

borando imagen y contenedor de rest01:

puesto que tiene un container asociado a la imagen primero hay que eliminar el container:

![[Pasted image 20230810143801.png]]

![[Pasted image 20230810143834.png]]

![[Pasted image 20230810143926.png]]


4.  Crear el Dockerfile para la imagen y usuario de mysql

Este sitio ayuda a crear el yaml con el comando que se pegue: https://www.composerize.com/

![[Pasted image 20230810145805.png]]

descargar la imagen docker:


arrancar un container con la imagen de mysql previamente descargada:

![[Pasted image 20230810145956.png]]

docker-compose up para ejecutar el yaml:

bajar el container:
docker-compose down

eliminar containers y volumenes innecesarios:
docker system prune -a
docker volume prune

Estos ultimos 2 comandos se recomiendan para mtto del sistema

5. Un comentario general es que:
	1. Dockerfile: sirve para generar la imagen de una aplicación
	2. myfile.yml: es una automatizacion de procesos de setup del entorno de trabajo: claves de usuario, volumenes, ubicaciones, nombre de contenedores, imagenes en uso
	3. Para el caso de aplicaciones spring boot debido a su arquitectura busca que todo su runtime este en un sólo layer, pero la filosofía de Docker es decoupling por tanto el proceso de generar los contenedores puede hacerse utilizando build packs

links utiles para generar la imagen de la aplicacion:
first approach: https://www.codejava.net/docker/create-docker-image-for-spring-boot-app
usando multi layer con build pack: https://www.baeldung.com/spring-boot-docker-images 

6. Ejecutar el yml file:

una vez que tuve el yml y el Dockerfile completo en su v1, lo parti con la intención de ejecutar y crear el container de mysql y probar que sucede cuando se hacen actualizaciones al yml

- Correr el yml:

![[Pasted image 20230815145114.png]]
imagino que este error es por el proxy de la CSJ. Efectivamente me di cuenta que en el archivo de servicio de docker se habia borrado -pues se autoregenera- los detalles del proxy, lo volvi a establecer (ver la sección services behind proxy de este obsidian para mayores detalles) y funciono:

![[Pasted image 20230816083621.png]]

notese que no requiere de proxychains para correr el yml.

Verificando imagen y contenedor:

![[Pasted image 20230816083728.png]]

Verificando volumenes:

![[Pasted image 20230816090735.png]]

redes locales:

![[Pasted image 20230816091246.png]]


7. Conectarse al docker MySQL:

![[Pasted image 20230816093407.png]]

![[Pasted image 20230816093547.png]]

8. comandos de administracion del contenedor:
- resetear: docker restart {container name / id}
- detener: docker stop {container name / id}

![[Pasted image 20230816123258.png]]

el contenedor se detiene pero siempre esta registrado

- remover: docker rn {container name / id}

9. 