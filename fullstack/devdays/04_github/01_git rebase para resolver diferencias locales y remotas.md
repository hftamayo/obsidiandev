	Dealing with differences:

1. En el proyecto de Spring los cambios al codigo los gestiono en la rama experimental, los testing en unstable
2. Hice un cambio en experimental, ejecute el PR y el Merge a Unstable
3. en mi equipo en la rama Unstable tenia 2 commits sin replicar a la nube
4. cuando hice git pull obtuve el mensaje que tenía diferencias y esto es muy claro ya que:
	1. se vienen los cambios que se arrastran de experimental
	2. hay cambios sin confirmar en la nube de los archivos locales de manera que cuando trato de hacer pull hay versiones distintas y ahi es donde genera la alerta

Para superar este impasse
1. antes de hacer un pull y un PR merge hacer push de mis cambios 
2. en caso que hice primero un PR merge y tengo cambios no confirmados puedo hacer un git pull --rebase
3. pull rebase hará:
	1. push de mis cambios locales
	2. pull de las actualizaciones que vienen el PR

==caso de uso: 
1. en la rama local unstable de mi API java hice cambios
2. cuando la quice replicar (git push) resultó que tengo 22 cambios que subir y 13 que bajar
3. hice un git pull pero no funciono por las divergencias ya señaladas
4. hice un git pull --rebase y me señalo todas estos cambios:

![[Pasted image 20240905144152.png]]

haciendo un git status:

![[Pasted image 20240905145325.png]]

la IA (gemini) me sugirió esto:
```
git rm src/test/java/com/hftamayo/java/todo/Controller/TodoControllerPriorTest.java
git rm src/test/java/com/hftamayo/java/todo/Repository/TodoRepositoryPriorTest.java
git rm src/test/java/com/hftamayo/java/todo/Services/TodoServicePriorTest.java
```

despues hice otro git status:

![[Pasted image 20240905145747.png]]

con estos archivos lo que hice fue hace un git add .

![[Pasted image 20240905151304.png]]

luego:
==git commit -m "xxx"
git rebase --continue

y empeze a obtener este mensaje: 

![[Pasted image 20240905152122.png]]

entonces hice un rebase --skip:

![[Pasted image 20240905152221.png]]

en efecto esos son mis ultimos commits.

![[Pasted image 20240905152308.png]]

![[Pasted image 20240905152342.png]]



5. 