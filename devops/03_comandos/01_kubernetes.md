comandos utiles:
=

kubectl describe pod/nombre_pod

kubectl exec -it nombre_pod -- bash

kubectl logs nombre_pod --tail=30

kubectl port-forward pod/nombre_pod 9999:80

Pod YAML -> equivalentes a los docker-compose yaml

![[Pasted image 20231214080419.png]]

Instalar kubectl y  en RHEL (esto debería instalarse en servers que salen a produccion):
=
![[Pasted image 20231214083214.png]]

sudo yum update
sudo yum install kubectl

![[Pasted image 20231214083521.png]]


instalando docker como gestor de contenedores:
sudo yum remove -y podman
sudo yum install -y docker 
sudo systemctl enable docker 
sudo systemctl start docker

Instalando kubelet y kueadm:

sudo yum install -y kubelet kubeadm
sudo systemctl enable kubelet

deshabilitando SELinux para evitar conflictos con k8s:

sudo setenforce 0 
sudo systemctl stop firewalld 
sudo systemctl disable firewalld

de manera opcional se puede instalar k9s que es una herramienta para gestion de k8s:

configurar el server con k8s para production ready:
=

dd




Install minikube en mi equipo de desarrollador:
=
https://kubernetes.io/docs/tasks/tools/

anuncio de cambio de repositorios para minikube: https://kubernetes.io/blog/2023/08/31/legacy-package-repository-deprecation/

como migrar de los legacy repos al nuevo: https://kubernetes.io/blog/2023/08/15/pkgs-k8s-io-introduction/

guia de instalacion de minikube: minikube.sigs.k8s.io/docs/start -> tomar en cuenta que estea herramienta requiere 2 nucleos, 2 GB de RAM y 20 GB de disco, instalar esto en una laptop no da mucha bola

kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster

Servicios K8s:
=

por favor referirse a DevOpsCube

templates:
=

1. deployment:
2. replica set:
3. volumenes emptydir:
4. volumenes hostpath:

Helm, Kustomize:
=


Tools:
=

1. kubelinter: github.com/stackrox/kube-linter
2. validkube.com: herramienta en linea para validar yaml de k8s
3. como depurar si un deploy de k8s no funciona: https://www.youtube.com/watch?v=eZF-j6Fyi3w&t=1996s a partir del 2:15 hace una demo y muestra como la depuró

kubernetes:
=



gestor de multi clusters: https://k8slens.dev


kubectl get nodes

kubectl get nodes -o wide

kubectl get pod -A : ver los pods del core del cluster

kubectl describe pod podname

kubectl apply -f name.yaml

kubectl exec -it podname -- bash

kubectl logs podname

kubectl delete pod podname -> no se vuelve a regenerar

kubectl get pod --show-labels

kubectl get pod -l owner=hftamayo

kubectl get pod -Lowner -> para crear una columnar con el valor la var owner

![[Pasted image 20240606134210.png]]

![[Pasted image 20240606134230.png]]

![[Pasted image 20240606134022.png]]

![[Pasted image 20240606134329.png]]

==COMO AGREGAR RELEASES A LOS YAML FILES:
=

![[Pasted image 20240610141123.png]]

VERIFICANDO EL DEPLOY Y LOS OBJETOS CREADOS ALREDEDOR:

![[Pasted image 20240610141226.png]]

==PUESTO QUE LA ETIQUETA ANNOTATIONS SE LA PUSE AL YAML DEL DEPLOY ENTONCES PARA VERLO:

![[Pasted image 20240610141413.png]]

==AHORA, SI MANUALMENTE YO HAGO EL CAMBIO A version 1.0.1 y nuevamente hago el deploy y despliego los objetos para saber si todo funciono entonces ahi me aparece el update:

![[Pasted image 20240610141531.png]]

==ahora bien, si necesto hacer un rollback del ultimo deploy y nuevamente verifico:

![[Pasted image 20240610141649.png]]

![[Pasted image 20240610141710.png]]


![[Pasted image 20240610143032.png]]

![[Pasted image 20240610144345.png]]

![[Pasted image 20240610144453.png]]

![[Pasted image 20240610144510.png]]

==Hostpath no se recomienda para produccion, puede ir bien con minikube o kind para los devs


==esquema de la arquitectura del juego pacman desplegado en K8S:

![[Pasted image 20240611140454.png]]


==ejemplo de K8s gitpos:

![[Pasted image 20240611140803.png]]

==en este flow, es el operador -argo o flux- quien es el encargado de desplegar la aplicacion en la infraestructura, es asi que al CI/CD se le quita carga.

