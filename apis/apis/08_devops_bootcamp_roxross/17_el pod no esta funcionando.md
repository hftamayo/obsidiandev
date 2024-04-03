
==durante el proceso de tratar de arreglar los permisos del jenkinsvitamines para acceder al bare metal docker me encontré con esto:

![[Pasted image 20240320145545.png]]

==verificando el estado del pod:

![[Pasted image 20240320150359.png]]

![[Pasted image 20240320150523.png]]

![[Pasted image 20240320151531.png]]

![[Pasted image 20240320151858.png]]

==Yes, if you delete a pod in **microk8s**, Kubernetes will automatically create a new pod to replace it. This behavior ensures that the desired number of replicas (specified in your deployment or statefulset) is maintained. Pods are considered disposable, and Kubernetes manages their lifecycle by creating new ones when needed. 🚀🌟

![[Pasted image 20240320152017.png]]

==en esta etapa ejecute el numeral 18 donde aplique una service-account con los manifestos service-account.yml y clusterRoleBinding.yml

![[Pasted image 20240402152753.png]]


==tengo que actualizar el deployment del jenkins para que use esta cuenta:

![[Pasted image 20240402154652.png]]


actualizando el deployment:

![[Pasted image 20240402155614.png]]

