![[Pasted image 20230608015645.png]]


![[Pasted image 20230608020603.png]]

![[Pasted image 20230608020630.png]]

![[Pasted image 20230608020700.png]]

![[Pasted image 20230608020717.png]]

![[Pasted image 20230608020744.png]]

paso est endpoint al repeater

![[Pasted image 20230608020827.png]]

![[Pasted image 20230608020842.png]]

click derecho sobre el endpoint -> engadedment tools -> Generate CSRF PoC

![[Pasted image 20230608021025.png]]

click en el boton Options y selecciono 'include auto submit script' y presiono el boton Regenerate (de la esquina inferior izquierda):

![[Pasted image 20230608021221.png]]

luego copy HTML y me voy al Exploit Server:

![[Pasted image 20230608021414.png]]

hago el cambio al metodo GET y cambio el valor del email para notar el cambio:

![[Pasted image 20230608021708.png]]

tambien agrego esta linea: INPUT HIDDEN VALUE POST:

![[Pasted image 20230608021927.png]]

luego Store y View Exploit:

![[Pasted image 20230608022000.png]]

automaticamente recibo el redirect y veo el cambio, regreando al exploit server, cambio otra vez el correo y esta vez deliver to the victim:


![[Pasted image 20230608022138.png]]

![[Pasted image 20230608022217.png]]


