
referencias:
https://www.docker.com/blog/9-tips-for-containerizing-your-node-js-application/ -> tips and best practices
https://www.youtube.com/watch?v=-pTel5FojAQ
repo de ejemplo: https://github.com/dockersamples/awesome-compose/blob/master/react-express-mongodb/docker-compose.yaml

https://snyk.io/blog/10-best-practices-to-containerize-nodejs-web-applications-with-docker/

proceso de codificacion:
1. .dockerignore
2. Dockerfile
3. docker-compose.yaml
4. docker-compose -f docker-compose.yaml up -d

![[Pasted image 20221103151414.png]]

![[Pasted image 20221103151504.png]]

![[Pasted image 20221103151606.png]]

![[Pasted image 20221103151642.png]]

![[Pasted image 20221103151724.png]]

notese que aca en la imagen no aparece puertos expuestos, por tanto no voy a poder comunicarme con el contenedor; hice los cambios al yaml y los volvi a confirmar con el comando  docker-compose up -d

![[Pasted image 20221104114547.png]]

si quiero reiniciar el docker:

docker-compose ps

si quiero detenerlo:
docker stop  container - id 

si quiero iniciarlo:
docker start nombre del container

![[Pasted image 20221104115525.png]]

notese que en la imagen, cuando hice docker ps ya no aparece el puerto expuesto, volvi a modificar el yaml quitandole el comando expose y dejando nada más el comando port, en el dockerfile ya esta esa comando asi que ejecute los cambios pero esta vez docker-compose ps los confirmo, favor verificar la siguiente secuencia:

![[Pasted image 20221104120840.png]]

haciend nuevamente un docker ps ya aparece cerrado el puerto de conexion:

![[Pasted image 20221104121037.png]]

puse la pregunta en stackoverflow y me dijeron que si ejecuto el comando docker-compose up sin el -d me aparecen los logs:

![[Pasted image 20221104130153.png]]


notese que es un error que tengo en el Dockerfile y que a pesar que lo corregí dice que el container esta up-to-date. Entonces en el grupo de reddit busque y este comando reinicia todo el proceso de creacion del container:

docker-compose up --build

![[Pasted image 20221104130603.png]]
...
![[Pasted image 20221104130634.png]]


notese que se queda parado en la ejecucion del node, voy a verificar si el container esta activo, seguir por favor la secuencia de comandos:

![[Pasted image 20221104130908.png]]
voy a apagar el equipo por motivos de fin de la jornada y luego vere si el contenedor se ejecuta sin problemas.
