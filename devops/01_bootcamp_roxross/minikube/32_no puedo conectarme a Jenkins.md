
durante la implementacion del frontend del ejercicio 3 de Rossana Suarez, el pipeline nunca terminó de ejecutarse, pensé que se había trabado y cerré la UI de Jenkins, sin embargo ya no pude conectarme.

Al siguiente día (lunes), encontré esto:

![[Pasted image 20240520135915.png]]

aca puedo ver que:
- los pods estan en ejecucion
- Jenkins y uno de los backends -e primero que deployé- se reiniciaron el domingo y ellos sólos se levantaron

==si intento conectarme a Jenkins me aparece que la conexion es rechazada.