![[Pasted image 20230117122432.png]]


![[Pasted image 20230117125500.png]]

![[Pasted image 20230117125549.png]]

mando al repeater la traza raiz

![[Pasted image 20230117125626.png]]

probando con un endpoint que no existe:

![[Pasted image 20230117125657.png]]

![[Pasted image 20230117125727.png]]

probando el siguiente payload:

![[Pasted image 20230117132905.png]]

no necesito cambiarlo, nada más debo seguir el siguiente proceso:

![[Pasted image 20230117130824.png]]


![[Pasted image 20230117131632.png]]

![[Pasted image 20230117131724.png]]

esto ocurre cuando el cache se envenena, noté que es cuando aparece una nueva cookie en el response:

![[Pasted image 20230117132428.png]]

entonces, vuelvo a enviar el request en el repeater hasta que se vuelve a envenenar el cache, una vez que ya llego a la nueva cookie, nada más en el lab le pego el mismo link al lab y ya funciona:
![[Pasted image 20230117132728.png]]


![[Pasted image 20230117132339.png]]