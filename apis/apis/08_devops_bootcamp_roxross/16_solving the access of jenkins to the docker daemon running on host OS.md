
![[Pasted image 20240318123504.png]]

![[Pasted image 20240318124631.png]]

![[Pasted image 20240318124957.png]]

![[Pasted image 20240318125217.png]]

![[Pasted image 20240318125307.png]]

==chequeando el estado del volumen:

![[Pasted image 20240318125712.png]]

![[Pasted image 20240318130045.png]]

![[Pasted image 20240318130957.png]]

![[Pasted image 20240318131104.png]]

==con el siguiente comando le doy permiso al container de Jenkins para usar el docker daemon del hostOS:==

docker run -d -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts

![[Pasted image 20240318142044.png]]

==obviamente este comando no me da chance en mi proceso de automatizacion pues genera un docker de jenkins fuera del cluster, entonces lo que hice es:
1. limpiar mi Dockerfile de la instalacion de comandos docker puesto uqe no tiene sentido ya que utilizaré el docker daemon del hostOS y los containers se van a deployar en el host.
2. simplificar el jenkins deployment pues ya no necesito montar el docker binario en el cluster
3. la idea es que ambos archivos esten aptos para usar el docker del hostOS
4. una vez deployado todo usaré un script para verificar si efectivamente jenkins tiene acceso al daemon:

![[Pasted image 20240319091436.png]]

![[Pasted image 20240319091532.png]]

![[Pasted image 20240319092206.png]]

![[Pasted image 20240319092314.png]]

probando con una pipeline:

![[Pasted image 20240319092356.png]]

![[Pasted image 20240319093242.png]]

![[Pasted image 20240319093425.png]]

al ejecutarlo:

![[Pasted image 20240319094203.png]]






5. 



