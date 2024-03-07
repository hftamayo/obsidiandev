==instalando el plugin de kubernetes

![[Pasted image 20240206131346.png]]

en los Available plugins busco este:

![[Pasted image 20240206132508.png]]



![[Pasted image 20240206132340.png]]


despues de instalar reinicie jenkins de manera manual (manage jenkins)

![[Pasted image 20240206133127.png]]

==Creacion del pipeline

Dashboard -> New Item

![[Pasted image 20240207121252.png]]

![[Pasted image 20240207124541.png]]


![[Pasted image 20240207124515.png]]

==Opcion A: agregando usuario y password de mi github a jenkins

![[Pasted image 20240207124344.png]]

![[Pasted image 20240207124431.png]]


![[Pasted image 20240207124639.png]]



==Opcion B: usando una llave privada para coneccion SSH de Jenkins a mi repo:

Generar una llave publica y privada para mi server donde tengo corriendo al jenkins:

==PENDIENTE



Generando una llave SSH para que Jenkins se conecte a mis repos:

![[Pasted image 20240207123557.png]]

![[Pasted image 20240207123820.png]]

==verificando el overview de la pipeline recien creada

==ejecutando la pipeline


le di correr y a los 4 segundos me generó error:

![[Pasted image 20240208123032.png]]

![[Pasted image 20240208122957.png]]

==pasos para depurar:

1. ver el video de la clase de Rossana pues aca si estoy perdido
2. validar con un linter si mi Jenkinsfile no tiene observaciones
3. ir probando stage por stage


INTENTO 2:
=

