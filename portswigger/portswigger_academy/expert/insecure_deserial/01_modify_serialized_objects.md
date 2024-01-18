![[Pasted image 20220924201607.png]]

despuyes de logearse con la credencial wiener:peter aparece una cookie session que parece estar encodeada bajo el Base 64

![[Pasted image 20220924201834.png]]

Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjowO30%3d

me voy al Decoder y en el Decode as selecciono Base64:

![[Pasted image 20220924202053.png]]

O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0O30%3d

efectivamente se puede ser que es un objeto serializado PHP, admin tiene un b:0 y eso indica que es un false.

entonces envio el GET my-account ultimo al repeater:

![[Pasted image 20220924202328.png]]

en el inspector verifico nuevamente la cookie:

![[Pasted image 20220924202649.png]]

Me paso al Decoder y cambio el b:1 y lo encodeo como Base64, copio el nuevo valor:

![[Pasted image 20220924202919.png]]

Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjoxTzMwJTNk

me paso al repeater, hago doble click y pego el nuevo valor de la cookie session:

![[Pasted image 20220924203153.png]]

vuelvo a mandar el request en el render encontre esto:

![[Pasted image 20220924203258.png]]

vuelvo a mandar el request al repeater y verifico el render:

![[Pasted image 20220924203421.png]]

ahora tengo una nueva vista en el cookie session:

![[Pasted image 20220924203518.png]]

cambio el valor a 1:

![[Pasted image 20220924203545.png]]

enviando el request y verificando el render:

![[Pasted image 20220924203629.png]]

con el b:1 la cookie es asi:

Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjoxO30=

me fui al navegador y con el plugin cookie editor hice este cambio y refresco:

![[Pasted image 20220924203855.png]]


![[Pasted image 20220924203913.png]]

![[Pasted image 20220924203937.png]]

