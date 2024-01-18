![[Pasted image 20230528190724.png]]

![[Pasted image 20230528190827.png]]

verificando el c ontenido de prototype y verificando si puedo inyectar un cambio (en este caso verificar la pestana sources):

/?__proto__car=car

![[Pasted image 20230528191625.png]]

aca podemos tener acceso a los scripts del sitio, estan en la carpeta resources:

![[Pasted image 20230528192144.png]]

en el script searchLoggerAlternative identificamos la variable manager.sequence la cual puede usarse para ejecutar codigo:

![[Pasted image 20230528192316.png]]

el payload a usar es este: /?__proto__.sequence=alert(1)-

es importante el guion al final porque es como una inyeccion:

![[Pasted image 20230528193015.png]]

