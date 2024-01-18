
![[Pasted image 20230506120704.png]]

![[Pasted image 20230506125201.png]]

![[Pasted image 20230506125228.png]]

![[Pasted image 20230506125259.png]]

mando al repeater y pruebo acceder a admin:

![[Pasted image 20230506125344.png]]

hago una nueva llave RSA, todas las anteriores puedo borrarlas:

![[Pasted image 20230506125426.png]]

despues de dar OK, hago click derecho sobre la nueva llave y seleccionó Copy Public Key as JWK.


![[Pasted image 20230506125712.png]]

luego me voy al Exploit Server y hago la siguiente cadena:

![[Pasted image 20230506125827.png]]

para segurirad presiono Store. Luego aca pego la Publick Key en formato JWK:

![[Pasted image 20230506125942.png]]

me vuelvo al repeater, especificamente a la pestaña JWT y en el "kid" del payload lo cambio por el "kid" de la llave que acabo de crear, tambien modifico wiener por adminitrator:

![[Pasted image 20230506130050.png]]

![[Pasted image 20230506130130.png]]

adicionalmente, en el JWS Header agrego el valor jku y pongo la url de mi exploit server:

![[Pasted image 20230506130433.png]]


![[Pasted image 20230506130416.png]]


finalmente firmo los cambios con la llave RSA que recien fue creada:

![[Pasted image 20230506130558.png]]

finalmente en el Exploit Server presiono "store" para que la llave sea confirmada, me paso al repeater e intento tener acceso a admin:

![[Pasted image 20230507064406.png]]


![[Pasted image 20230507064334.png]]