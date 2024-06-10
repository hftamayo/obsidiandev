
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


