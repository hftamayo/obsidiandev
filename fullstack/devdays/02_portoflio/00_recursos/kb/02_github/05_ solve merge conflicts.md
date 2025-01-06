
El siguiente escenario es n merge conflict entre las ramas typescript y experimental, fue descubierto al momento de hacer el PR entre éstas, ==la estrategia es resolver el conflicto local, hacer el respectivo push y automaticamente el PR se resuelve:


==haciendo un git status

![[Pasted image 20240611095008.png]]

![[Pasted image 20240611095251.png]]

puedo abrir con un editor de texto los archivos en conflicto :

![[Pasted image 20240611100223.png]]

![[Pasted image 20240611100636.png]]

lo dité a mano y me quedo así:

![[Pasted image 20240611101026.png]]

al volver a hacer un git status:

![[Pasted image 20240611101306.png]]

![[Pasted image 20240611101924.png]]

consultando con gemini:

![[Pasted image 20240611102235.png]]


el detalle de estos 3 archivos es que no existen en experimental pero si en typescript

![[Pasted image 20240611104418.png]]

entonces solo voy a borrarlos en experimental pues ni siquiera estan en el cache de git:

![[Pasted image 20240611104520.png]]

![[Pasted image 20240611104559.png]]

![[Pasted image 20240611104746.png]]

![[Pasted image 20240611105051.png]]

al hacer el push automaticamente el PR se ejecutó

![[Pasted image 20240611105138.png]]

![[Pasted image 20240611105157.png]]

![[Pasted image 20240611105301.png]]