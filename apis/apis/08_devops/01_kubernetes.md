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

GitOps:
=
