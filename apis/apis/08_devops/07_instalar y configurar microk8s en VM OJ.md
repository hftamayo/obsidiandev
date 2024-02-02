
1. Guia de instalacion: https://techviewleo.com/microk8s-and-k0s-kubernetes-installation-on-debian/  y esta tambien: https://microk8s.io/docs/getting-started
2. comandos:

	sudo apt install -y snapd
	sudo snap install microk8s --classic
	snap info microk8s
	snap list microk8s
	verificar si snap esta en el PATH, si no agregarlo:

![[Pasted image 20240201131007.png]]


![[Pasted image 20240201130931.png]]


agregar mi usuario al grupo:

![[Pasted image 20240201131308.png]]

cerrar sesion y hacer un id para ver si estoy en el grupo:

![[Pasted image 20240201131755.png]]

![[Pasted image 20240201132218.png]]



![[Pasted image 20240202115034.png]]

![[Pasted image 20240202115137.png]]

==probando crear un deployment de nginx:

![[Pasted image 20240202115406.png]]

![[Pasted image 20240202115512.png]]

==exponer el servicio y luego borrar el deployment y el pod:

microk8s.kubectl expose deployment nginx --port 80 --target-port 80 --type ClusterIP --selector=run=nginx --name nginx

![[Pasted image 20240202120022.png]]

==otros comandos utiles:

- microk8s.stop
- microk8s.start
- microk8s.reset
- snap remove microk8s

3.  Instalar Jenkins en el cluster

![[Pasted image 20240202121200.png]]

![[Pasted image 20240202121315.png]]

el ultimo comando es por si deseo instalar jenkins en bare metal en el cluster, sin embargo, los siguientes es para crear una imagen personalizada:

primero desinstalar la version existente:

![[Pasted image 20240202121912.png]]

![[Pasted image 20240202122503.png]]

![[Pasted image 20240202122701.png]]

copiando el Docker proporcionado por Rossana Suarez:

![[Pasted image 20240202124638.png]]

![[Pasted image 20240202124827.png]]

![[Pasted image 20240202125051.png]]

==creando un secreto para docker

kubectl create secret docker-registry registry --docker-server='Docker registry URL here' --docker-username='docker registry username' --docker-password='password here' --docker-email='docker registry email'

![[Pasted image 20240202125642.png]]

actualizando el values.yaml con los datos de mi imagen: (ojo que en este paso se va a usar el secret que fue creado arriba)

![[Pasted image 20240202130352.png]]

![[Pasted image 20240202130622.png]]

![[Pasted image 20240202130756.png]]

el yaml de arriba me genero un error, la version correcta es:

![[Pasted image 20240202132032.png]]

despues de esto la instalacion pudo completarse:

![[Pasted image 20240202132257.png]]
...

==4. verificando el despliegue de jenkins:

![[Pasted image 20240202132558.png]]

![[Pasted image 20240202133012.png]]

Verificando mis nodos activos para identificar el nombre de jenkins:

==microk8s.kubectl describe nodes

![[Pasted image 20240202133520.png]]

verificando que tiene el nodo:

![[Pasted image 20240202133828.png]]

...

![[Pasted image 20240202134015.png]]

![[Pasted image 20240202134111.png]]

el error más orientador es este:

==Unable to retrieve some image pull secrets (registry); attempting to pull the image may not succeed.


Y aca esta relacionado por la forma en como hice el secret, los valores que debo usar son:

docker registry: https://index.docker.io/v1/
-n jenkins: al final poner esto para especificar el namespace al que pertenece el registro

![[Pasted image 20240202134833.png]]

borrando el pod actual para que k8s haga uno nuevo usando el nuevo valor de secret:

![[Pasted image 20240202135100.png]]

verificando:  ==microk8s.kubectl describe pod jenkins-0 -n jenkins

![[Pasted image 20240202135342.png]]

