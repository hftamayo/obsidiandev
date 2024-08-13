==tratando de borrar todo:

![[Pasted image 20240411145422.png]]

![[Pasted image 20240411145520.png]]

![[Pasted image 20240411145646.png]]

![[Pasted image 20240411145942.png]]

![[Pasted image 20240418144148.png]]

![[Pasted image 20240418142815.png]]


==reinstalando todo:

![[Pasted image 20240418143359.png]]

aca me llama la atencion que dice que el deployment esta sin cambios

![[Pasted image 20240418143435.png]]

el servicio aparece arriba pero no hay pods:

![[Pasted image 20240418143504.png]]

==imagino que debi haber borrado el deployment:

![[Pasted image 20240418143906.png]]

![[Pasted image 20240418144132.png]]

la imagen anterior la pondré al inicio de esta pagina, corriendo otra vez el manifesto:

![[Pasted image 20240418144521.png]]

==probando si me puedo conectar:

![[Pasted image 20240418145925.png]]

==tratando de obtener mas info del pod:

![[Pasted image 20240418150002.png]]

==en este pod si pude obtener el log:

![[Pasted image 20240418150507.png]]

==en efecto en el log no aparee nada raro. la AI me recomendo limitar el uso de la memoria pues el error OOMKilled esta basado en esto, en ese sentido hice este cambio en el manifesto de deployment:

![[Pasted image 20240418150959.png]]

==tratando de aplicar los cambios:

![[Pasted image 20240418151837.png]]

![[Pasted image 20240418152024.png]]

![[Pasted image 20240418152152.png]]

==verificando el log del pod:

![[Pasted image 20240418153854.png]]

....

![[Pasted image 20240418153832.png]]


==probando conectarme:

![[Pasted image 20240418160326.png]]


==todos los servicios de k8s estan arriba, favor ver el ultimo comando:

![[Pasted image 20240418160451.png]]

minikube funciona deployando el pod en una especie de VM que es accesible por medio de esa IP, de hecho si pruebo conectarme con curl tengo salida:

![[Pasted image 20240418160633.png]]

nota que tengo un 403, el cual me pide autenticacion, lo cual es lo esperado
