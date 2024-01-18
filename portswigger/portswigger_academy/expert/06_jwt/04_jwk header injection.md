	![[Pasted image 20230506112902.png]]


![[Pasted image 20230506115035.png]]


![[Pasted image 20230506115124.png]]

![[Pasted image 20230506115213.png]]

![[Pasted image 20230506115257.png]]

mando my-account al repeater y tratando de acceder al recurso admin:

![[Pasted image 20230506115355.png]]

en el JWT Editor Key hago una llave RSA de 2048 bits en formato JWK:

![[Pasted image 20230506115550.png]]

![[Pasted image 20230506115615.png]]

Me regreso al repeater y voy a la pestaña de JWT Token, en el payload cambio de wiener a administrator:

![[Pasted image 20230506115846.png]]

![[Pasted image 20230506115946.png]]

presiono el boton Attack y en el menu contextual que aparecerá selecciono Embedded JWK, selecciono la llave RSA y OK:

![[Pasted image 20230506120108.png]]

aparentemente no hay cabios:

![[Pasted image 20230506120233.png]]

pero si ahora pruebo acceder al recurso admin desde Raw:

![[Pasted image 20230506120300.png]]

accediendo al endpoint y borrando la cuenta carlos:

![[Pasted image 20230506120336.png]]

![[Pasted image 20230506120407.png]]

![[Pasted image 20230506120430.png]]




