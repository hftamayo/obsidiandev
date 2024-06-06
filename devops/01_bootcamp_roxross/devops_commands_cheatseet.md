
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

