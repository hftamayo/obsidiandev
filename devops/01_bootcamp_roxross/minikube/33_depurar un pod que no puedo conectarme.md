Let's recap: currently the state of the jenkins pod is:

![[Pasted image 20240522114428.png]]

![[Pasted image 20240522114908.png]]

==verificando los logs:

![[Pasted image 20240522115105.png]]
....

![[Pasted image 20240522115134.png]]

==verificando el servicio:

![[Pasted image 20240522120033.png]]

pues en sintesis el pod se ve en buen estado y funcionando

DEBUGGING:
=

==1. entrar al pod y verificar conexion:

![[Pasted image 20240522120356.png]]

bien curioso porque en todos los anteriores chequeos el estado del pod aparece en running pero no puedo iniciar una sesion porque esta parado, ==borrando el pod existente para que el cluster lo reinicie:

==el proceso de borrado se tardo un poco y el request de obtener la nueva lista como 2 minutos:

![[Pasted image 20240522121124.png]]

==probablemente obtuve este error porque el comando lo ejecuté desde el tunnel con jenkins, cerré esa sesion  y abri una normal de SSH:

![[Pasted image 20240522121240.png]]

puesto que esto ya es un tema estrictamente del cluster ==se procede a monitorear y debuggear el cluster:

![[Pasted image 20240522121538.png]]

que ==apiserver este en modo stopped es bien emblematico pues este servicio es el encargado de gestionar todos los requests, seria chivisimo saber los motivos, haciendo un minikube logs:

![[Pasted image 20240522121804.png]]

...

![[Pasted image 20240522122006.png]]

==la verdad que es in log bien canijo de depurar, asi que vamos a reiniciar el cluster:

![[Pasted image 20240522122451.png]]

![[Pasted image 20240522122505.png]]

![[Pasted image 20240522122743.png]]

==obteniendo info de los pods, luego abriendo un ssh tunnel para verificar si puedo conectarme a JEnkins:

![[Pasted image 20240522123154.png]]

![[Pasted image 20240522123212.png]]



==2. haciendo un poco de forense:

me sabe mal haber tenido que reiniciar todo el nodo sin saber los motivos, cuando logré entrar a Jenkins descubrí un par de cosas:

==la ultima fase del deploy tomó casi 11 horas (parece que cerrar la sesion no funciona para detener todo :-)  ):

![[Pasted image 20240522123655.png]]

==el detalle del log:

![[Pasted image 20240522123811.png]]

pues simplemente el nodo se frickeo, no dio bola, se reinicio y luego intento seguir, el final del log no es muy diferente:

![[Pasted image 20240522123917.png]]


COMMENTS:
=

==3. probablemente para un equipo de devs minikube no sea una buena opcion

==4. es clave que los devs tengan un entorno de deploy de la CI/CD que sea para pruebas

==5. seguramente los entornos Prod deberán de tener un nodo espejo para cualquier contenigencia

==6. seguramente los anteriores comments son de un enfoque SRE


![[Pasted image 20240522125710.png]]



