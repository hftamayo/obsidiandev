
![[Pasted image 20240226144009.png]]

durante el proceso de deployar el pipeline del ejercicio 3 de Rossana me encontré que no podía ejecutar kubectl, estas son las premisas:

1. recordar que jenkins lo deployé en un container dentro del cluster, por tanto el kubectl que intenta acceder no es el de mi host (microk8s.kubectl) es el del contenedor
2. cualquier depuracion debo hacerla desde dentro del cluster
3. el Dockerfile ya instaló kubectl asi que debo chequear que paso:


![[Pasted image 20240226144245.png]]

4. proceso para entrar en el pod:

![[Pasted image 20240226144632.png]]

5.  verificar si las tools que necesito estan:

![[Pasted image 20240226145414.png]]


6. describir el pod desde el hostOS:

![[Pasted image 20240226145318.png]]


7. descargando kubectl desde dentro del pod (esta es una manera sucia y no recomendable, si el pod se reinicia se pierde el comando, es mejor modificar el Dockerfile y reiniciar el proceso):

![[Pasted image 20240226150224.png]]

pude instalarlo en /tmp:

![[Pasted image 20240226150416.png]]

moviendolo a $HOME para probar si puedo escribir:

![[Pasted image 20240226150548.png]]

probando si lo puedo poner en el PATH:

![[Pasted image 20240226150731.png]]


intenté correr el pipeline pero igual me dio error, entonces me va a tocar volver a hacer el pod.

8. regenerando la imagen:


![[Pasted image 20240226154901.png]]

![[Pasted image 20240226154920.png]]

agregué un par de confirmaciones para cada stage, generando la imagen junto con un log:

docker build -t hftamayo/jenkins . 2>&1 | tee jenkins.log

![[Pasted image 20240226155150.png]]

![[Pasted image 20240226155225.png]]

![[Pasted image 20240226155312.png]]

de hecho la diferencia en el tamaño de la imagen es clave, probablmente hubo algun prlblema en el primer proceso voy a borrar la anterior y subiré la nueva el hub:

![[Pasted image 20240226155506.png]]

![[Pasted image 20240226155755.png]]

![[Pasted image 20240226155933.png]]


![[Pasted image 20240226160146.png]]
