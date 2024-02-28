siguiendo la guia de Rossana Suarez, el manifesto se llama values.yaml:

![[Pasted image 20240227131206.png]]

en el yaml se puede er que yo utilizo mi repo:

![[Pasted image 20240227131136.png]]

actualizando la imagen del pod:

microk8s.helm3 upgrade jenkins jenkins/jenkins -n jenkins -f values.yaml

![[Pasted image 20240227140230.png]]

chequeando el estado de la actualizacion:

![[Pasted image 20240227140437.png]]

![[Pasted image 20240227140521.png]]

![[Pasted image 20240227140629.png]]

en esta parte del describe sale la imagen que estoy usando:

![[Pasted image 20240227140824.png]]

entrando al cluster para verificar el estado de kubectl:

![[Pasted image 20240227141049.png]]


