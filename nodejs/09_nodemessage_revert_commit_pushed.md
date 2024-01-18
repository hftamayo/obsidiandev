proceso para revertir un commit que ya esta pusheado al github.

En este hay un agujero de seguridad:

![[Pasted image 20221117130104.png]]

este agujero lo tengo que corregir en tres commits

![[Pasted image 20221117133305.png]]


y desde el commit "connecting to..." en los siguientes el mismo archivo sufrio cambios, asi mismo, si hago un rollback de todo el commit hay otros archivos que sufrieron modificaciones  y que no tengo motivos para eliminarlos.

Por tanto, tomé la ultima modificación del app.js (adjusting save...), me voy a ir hasta el "connecting" y desde ahi haré rollback:

![[Pasted image 20221117133623.png]]


sin embargo, hice este cambio pero se creo otro commit, lo peor de todo es que el agujero sigue ahi:

![[Pasted image 20221117134748.png]]

viendo los commits que afectan al archivo app.js

![[Pasted image 20221117135207.png]]

Escribiendo una nueva historia:

![[Pasted image 20221118121911.png]]

![[Pasted image 20221118122154.png]]

aca tuve que volver a hacer una modificacion pajarito a app.js, la idea es que salga modificada

![[Pasted image 20221118122711.png]]

![[Pasted image 20221118122738.png]]

![[Pasted image 20221118123036.png]]

![[Pasted image 20221118123229.png]]

![[Pasted image 20221118123255.png]]

![[Pasted image 20221118124547.png]]

Pues efectivamente despues de tanta vuelta los commits originales no se borran, de hecho, otros comentarios decian lo mismo, asi que este es el resumen:

1. post en redit: https://www.reddit.com/r/git/comments/yy0xr0/is_it_possible_to_edit_just_one_file_from_a_commit/
2. orientacion sobre como eliminar data sensible en un git: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository
3. libro oficial de git: https://git-scm.com/book/en/v2
4. efectivamente si ya meti algo a un commit y si lo pushee la unica forma de eliminar eso es borrar el repo, obviamente no es viable por los PR en ejecucion
5. no subir credenciales al codigo, desde las primeras versiones usar caminos seguros