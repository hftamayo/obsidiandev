![[Pasted image 20240315153222.png]]


![[Pasted image 20240315153522.png]]

respecto a actualizar el cluster de k8s podría incluir en el manifesto del deployment un ==imagePullPolicy : always== pero indudablemten que eso se harta muchos recursos, en su lugar usaré esta opcion:


![[Pasted image 20240315154514.png]]

==monitoreando el resultado ==

![[Pasted image 20240315154655.png]]

![[Pasted image 20240315154747.png]]

![[Pasted image 20240315155036.png]]

![[Pasted image 20240315155054.png]]

pues efectivamente se reinició el servicio, quizas como buena practica debería utilizar tags para la imagen de docker.

