![[Pasted image 20240424132527.png]]

![[Pasted image 20240424132551.png]]

![[Pasted image 20240424132649.png]]

![[Pasted image 20240424132756.png]]

![[Pasted image 20240424132929.png]]

![[Pasted image 20240424133143.png]]


==aca me generó un error de permisos, favor remitirse a jenkins-role y jenkins-rolebinding yamls para el codigo, aplicandolos en el deployment:

![[Pasted image 20240426144520.png]]

==verificando si el pod esta arriba y si los cambios fueron efectivos:

![[Pasted image 20240426144810.png]]

![[Pasted image 20240426145139.png]]

==despues de eso, tuve que volver a realizar un cambio en el jenkins-role extendiendo los permisos a servicios:

![[Pasted image 20240426150559.png]]


![[Pasted image 20240426150637.png]]

==probando la fase del deploy del docker: