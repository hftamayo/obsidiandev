
durante la implementacion del frontend del ejercicio 3 de Rossana Suarez, el pipeline nunca terminó de ejecutarse, pensé que se había trabado y cerré la UI de Jenkins, sin embargo ya no pude conectarme.

Al siguiente día (lunes), encontré esto:

![[Pasted image 20240520135915.png]]

aca puedo ver que:
- los pods estan en ejecucion
- Jenkins y uno de los backends -e primero que deployé- se reiniciaron el domingo y ellos sólos se levantaron

==si intento conectarme a Jenkins me aparece que la conexion es rechazada.

* kubectl logs -n jenkinsvit "pod name"
*  kubectl describe pod "pod-name" -n jenkinsvit
* kubectl get services -n jenkinsvit

![[Pasted image 20240520161031.png]]

aca lo unico raro es que hay varios nodeports. ==Tratando de conectarse desde el hostOS a los servicios usando localhost o la ip asignada a minikube:

![[Pasted image 20240521123008.png]]


==conectando al frontend:

![[Pasted image 20240521123211.png]]


==conectando al backend:

![[Pasted image 20240521123346.png]]


==conectando a jenkins:

![[Pasted image 20240521123527.png]]

segun el anterior resultado, 2 servicios estan arriba y uno abajo. ==verificando el estado de minikube:

![[Pasted image 20240521124511.png]]

==scripts para conectarse a los nodeports que si respondieron:

![[Pasted image 20240521130705.png]]

![[Pasted image 20240521130720.png]]

![[Pasted image 20240521130747.png]]