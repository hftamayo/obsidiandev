
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

=============
IMPORTANTE: DE ACA EN ADELANTE USO EL METODO DE ROSSANA SOBRE LA INSTALACION DE JENKINS EN EL CLUSTER DE K8S USANDO HELM
=========



![[Pasted image 20240202122701.png]]

copiando el Docker proporcionado por Rossana Suarez: (https://github.com/roxsross/bootcamp-devops-2023/blob/ejercicio3-despliega/jenkins/helm/Dockerfile)

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

==aca no es necesario volver a ejecutar ningun comando pues el pod se vuelve a generar.

verificando:  ==microk8s.kubectl describe pod jenkins-0 -n jenkins

![[Pasted image 20240202135342.png]]
...

![[Pasted image 20240205115634.png]]

nuevamente me generó el error que no puede descargar la imagen desde el origen que especifiqué en el values.yaml , verificando nuevamente mi values.yaml:

![[Pasted image 20240205122544.png]]

y mi docker hub:

![[Pasted image 20240205122609.png]]

ajustando cambios y probando:

![[Pasted image 20240205122647.png]]

en este caso, tambien movi de lugar el values.yaml entonces ahora si necesito hacer un cambio al secret:

borrando el pod:
![[Pasted image 20240205123142.png]]

actualizando el despliegue de jenkins con el nuevo valor de values.yaml y junto con su nueva ubicacion:

![[Pasted image 20240205124230.png]]

Verificando status:

![[Pasted image 20240205125133.png]]

![[Pasted image 20240205125308.png]]

...

![[Pasted image 20240205125335.png]]

verificando si puedo haer un pull de la imagen:

![[Pasted image 20240205125545.png]]

efectivamente la imagen no existe pues sigue buscando la version 1.0.0 cuando lo correcto es latest. Ahora bien puesto que movi el values.yaml es probable que aun no se haya actualizado el deploy, borrando la version actual:

![[Pasted image 20240205125856.png]]

volviendo a ejecutar la instalacion con la nueva ubicacion y nuevos valores en el values.yaml:

![[Pasted image 20240205130351.png]]

![[Pasted image 20240205130212.png]]

![[Pasted image 20240205130307.png]]

esperando algunos momentos estoy teniendo esta salida:

![[Pasted image 20240205131632.png]]

verificando los servicios y el log del pod:

![[Pasted image 20240205131707.png]]

![[Pasted image 20240205131735.png]]

![[Pasted image 20240205131805.png]]

sin embargo, si trato de hacer un port forward:

![[Pasted image 20240205131851.png]]

pues al dia siguiente y despues de haber buscado ayuda en el discord sin ningun exito, me le quedé viendo a la pantalla e hice un history, ahi me di cuenta que en algun momento probé con el namespace y decidí volver a probar:

![[Pasted image 20240206120816.png]]

![[Pasted image 20240206122509.png]]

puesto que la consola quedó ocupada, haré un Ctrl+C y tengo que hacer los siguientes cambios:

1. puesto que el servicio es de tipo ClusterIP unicamente es accesible entre los equipos que pertenecen al cluster, si lo quiero acceder desde mi navegador o desde otro equipo debería ser NodePort, favor ver esta tabla;

![[Pasted image 20240206122731.png]]

2.  ejecutar el port forwarding con un parametro para poder seguir usando la consola
3. verificar si efectivamente el puerto esta abierto
4. el port forward es accesible unicamente desde dentro del hostOS

Aca algunas capturas:

![[Pasted image 20240206123245.png]]

Deteniendo el proceso y forwardeando el puerto 8080 con la interface 0.0.0.0:

![[Pasted image 20240206124857.png]]

![[Pasted image 20240206125236.png]]

probando otra vez el port forwarding:

![[Pasted image 20240206125352.png]]

![[Pasted image 20240206125452.png]]


obteniendo la clave:

![[Pasted image 20240206130043.png]]

admin at 

![[Pasted image 20240206130538.png]]


AMPLIANDO EL METODO ANTERIOR
=

el problema de usar port forwarding es que yo necesito tener siempre una sesion remota donde fue ejecutada el port forwarding, si me desconecto jenkins se cierra, entonces yo debería tener el deploy de este servicio en modo NodePort, aca el proceso que espero de resultado:


archivo values antes de ser modificado:

![[Pasted image 20240219154152.png]]

archivo modificado:

![[Pasted image 20240219154400.png]]

verificando el estado de los pods antes de ejecutar la actualizacion:

![[Pasted image 20240219154502.png]]

corriendo las actualizaciones:

![[Pasted image 20240219155310.png]]

probando con Helm:

![[Pasted image 20240219155520.png]]

![[Pasted image 20240219155715.png]]

![[Pasted image 20240219155927.png]]


como podemos ver, el servicio no es accesible aun hacia afuera del host, asi que esta es la explicación de gemini:

![[Pasted image 20240220150744.png]]

![[Pasted image 20240220151026.png]]

recordemos que microk8s tiene 1 nodo y justo es la salida del comando get nodes y esta en modo Ready.

para verificar la ip del nodo:

![[Pasted image 20240220151521.png]]

conectandome por el puerto 30568:

![[Pasted image 20240220151544.png]]

por qué el puerto 30568 no aparece como LISTEN en el comando netstat? 

![[Pasted image 20240220151728.png]]



