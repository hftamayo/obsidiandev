
![[Pasted image 20230418140409.png]]

![[Pasted image 20230420132323.png]]

![[Pasted image 20230420132357.png]]

envío el endpoint my-account al repeater:

![[Pasted image 20230420132433.png]]

si intento acceder a admin:

![[Pasted image 20230420132510.png]]

![[Pasted image 20230420132538.png]]

verifico el hash del JWT haciendo click sobre la segunda cadena separada por puntos:

![[Pasted image 20230420132640.png]]

![[Pasted image 20230420132658.png]]

cambio de wiener a administrator y guardo cambios:

![[Pasted image 20230420132736.png]]

envío la traza nuevamente:

![[Pasted image 20230420132803.png]]

ya con esto tengo elevacion de privilegios, busco en el codigo de la pagna y encuentro un endpoint para borrar la cuenta de carlos:

![[Pasted image 20230420132849.png]]

copio el link y siempre con un metodo GET ejecuto el endpoint:

![[Pasted image 20230420132949.png]]

![[Pasted image 20230420133007.png]]

