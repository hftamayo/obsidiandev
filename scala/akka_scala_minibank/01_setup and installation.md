El video tutorial es este: https://www.youtube.com/watch?v=M1PeE90DC38

siguiendo el video tutorial, las dependencias las digite a mano en el archivo build.sbt sin embargo, cuando quice descargarlas seguí esta secuencia:

View->Tool Windows-> SBT

![[Pasted image 20230418124229.png]]

al darle click en el boton de refresh (el primero de la izquierda), las dependencias se empiezan a descargar sin embargo me tiraba un error y que viera el log de todo el IDE.

Para resolverlo de la manera más efectiva que encontré le di click en la parte inferior al icono sbt shell y utilice el comando "reload"; despues de unos minutos aparece que el archivo build.sbt ha cambiado y encima de él me aparece un icono de actualizar, le di click y todas las dependencias se actualizaron:

![[Pasted image 20230418124436.png]]

para confirmar que puedo agregar una nueva dependencia y repetir el proceso agregaré esta al build.sbt: 

  "ch.qos.logback"    %  "logback-classic"            % "1.2.10",

efectivamente el proceso funcionó.

Respecto al yaml de Docker, despues de escribirlo abrí una terminal en el proyecto y utilicé el comando: docker-compose up para descargar la imagen de cassandra:

![[Pasted image 20230418130041.png]]

![[Pasted image 20230418130725.png]]

si quiero detener la imagen uso el comando docker stop + container id

si quiero ejecutarla:
docker ps
docker ps -all

![[Pasted image 20230419120634.png]]

docker restart + nombre del container
docker ps -a  para verificar que esta en escucha:

![[Pasted image 20230419120947.png]]

si todo sale bien podré usar el siguiente comando para conectarme al csql shell de cassandra:

![[Pasted image 20230419121340.png]]

