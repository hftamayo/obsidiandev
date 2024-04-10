
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

verificando el resultado:

![[Pasted image 20240405093747.png]]

notese que ahora tengo un pod en ImagePullBackOff y esto tiene logica puesto que en el manifesto de deployment yo cambié la version de la imagen a 0.0.6 y esto no es asi pues no he actualizado la imagen, corrigiendo y hacer el deploy nuevamente:

![[Pasted image 20240405094048.png]]

vovlendo a generar los pods pues como podemos darnos cuenta aun queda el pod del ImagePullBackOff

![[Pasted image 20240405095553.png]]

en la imagen muestra que aun tengo un pod en ImagePullBackOff, verificando que esta pasando con el deployment:

![[Pasted image 20240405100442.png]]

el resultado fue que se excedio el tiempo de espera, forzando la actualizacion de los pods:

![[Pasted image 20240405100911.png]]

notese que tengo aun 2 pods, verificando cuantas replicas tengo:

![[Pasted image 20240405101217.png]]

esto es más congruente y lo que haré es reducir la replica a 1 pod:

![[Pasted image 20240405102917.png]]

aun asi siempre salen 2 pods, digamos que es aceptable en el momento, pero al habe depurado y levnatado la aplicacion ambos pods deberían estar en running.

COMO ENCONTRAR QUE PROBLEMA TIENE EL DEPLOYMENT
=

![[Pasted image 20240405103625.png]]

...

![[Pasted image 20240405103740.png]]

...

![[Pasted image 20240405103825.png]]

...

![[Pasted image 20240405103855.png]]

un dato bien importante es el nombre del nodo donde esta corriendo el pod:

![[Pasted image 20240405104530.png]]

verificando los datos del comtenedor asociado a este Jenkins:

![[Pasted image 20240405110428.png]]

aca el hallazgo más importante es que microk8s esta usando contained en lugar de docker para la gestion de contenedores, por eso es que yo no veía los containers listados:

![[Pasted image 20240405110524.png]]

SWITCHANDO DE CONTAINERD A DOCKER
=

![[Pasted image 20240405112614.png]]

![[Pasted image 20240405112636.png]]
