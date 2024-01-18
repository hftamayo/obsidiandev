![[Pasted image 20230103145045.png]]

![[Pasted image 20230103145707.png]]

despues del login vamos al desktop y vemos un detalle de producto

![[Pasted image 20230103145911.png]]

![[Pasted image 20230103145943.png]]

![[Pasted image 20230103150038.png]]

limpiamos el template para ver las vars del server side que estamos usando:

![[Pasted image 20230103150759.png]]

probando si el template soporta java:

![[Pasted image 20230103151012.png]]

![[Pasted image 20230103151043.png]]

puesto que la salida muestra llamadas a script python podemos probar template ijection para django o flask. Cobalt tiene varios ejemplos en este link:
https://www.cobalt.io/blog/a-pentesters-guide-to-server-side-template-injection-ssti

![[Pasted image 20230103151510.png]]

notese estas lineas: {'False': False, 'None': None, 'True': True}
corresponden a las variables que espera el template. Verificando si la variable Setting genera output:

![[Pasted image 20230103151806.png]]

en django se puede obtener info de las cuentas admin:

![[Pasted image 20230103151940.png]]

De igual forma se obtiene salida de las SECRET KEYS:

![[Pasted image 20230103152109.png]]

hacemos el submit de la solucion:

![[Pasted image 20230103152211.png]]

