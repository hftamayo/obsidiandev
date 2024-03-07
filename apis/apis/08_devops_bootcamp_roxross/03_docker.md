==1. Habilitar experimental en docker

- idealmente debería ser el docker del repositorio: sudo apt-get upgrade docker-ce

==2. modificar el daemon.json agregando esta linea.

![[Pasted image 20240119120955.png]]

==3. reiniciar el servicio: 

![[Pasted image 20240119121121.png]]

==4. crear la carpeta plugins en el HOME de docker:

![[Pasted image 20240119121420.png]]

==5. descar en esta carpeta el plugin de buildx:

wget https://github.com/docker/buildx/releases/download/v0.7.1/buildx-v0.7.1.linux-amd64

==6.  dar permisos de ejecucion y simplificar el nombre

![[Pasted image 20240119121753.png]]

==7. verificar si esta funcionando:

![[Pasted image 20240119121921.png]]

8. generar una imagen multiplataforma multiarquitectura de un backend escrito en CSharp:

==docker buildx build --platform linux/amd64 -t ch03be_worker:stable-1.0.0 .

el Dockerfile debe verse así:

![[Pasted image 20240119125334.png]]

el comando a utilizar debe ser así:

docker buildx build --platform linux/amd64 -t ch03be_worker:stable-1.0.0 .

![[Pasted image 20240119125600.png]]




9. 