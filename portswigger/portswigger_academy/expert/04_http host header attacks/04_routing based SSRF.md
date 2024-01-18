
![[Pasted image 20230207122608.png]]

mando este request al repeater:

![[Pasted image 20230207125300.png]]

![[Pasted image 20230207125325.png]]

sin enviar el request inicio el burp collaborator client, copio la ip del server y regreso al repeater, aca en el Host sustituyo el del lab por el del collab client:

![[Pasted image 20230207125429.png]]

![[Pasted image 20230207125525.png]]

ahora si presiono Send, si recibo un 200 me regreso al collab y hago un poll now

![[Pasted image 20230207125637.png]]

![[Pasted image 20230207125705.png]]

Cieerro el collab client. ahora que ya genere trafico me regreso al Http history y el get original lo envío al intruder


![[Pasted image 20230207130022.png]]

en el host voy a poner el rango de ip que dice en la indicacion del lab, el ultimo octeto será el payload. El ataque será Sniper:

![[Pasted image 20230207130146.png]]

Personalizando el payload:

![[Pasted image 20230207130309.png]]

al enviar el ataque sale este warning, solo hay que darle ignore:

![[Pasted image 20230207130344.png]]

cuando termina ordeno los resultados por el tipo status devuelto y el 302 nos da la ip del backend:

![[Pasted image 20230207130548.png]]

desde esta misma ventana, en el raw, hago click derecho y mando este resultado al repeater:

![[Pasted image 20230207130631.png]]

envio el request y obtengo un 302:

![[Pasted image 20230207130730.png]]

si en el GET hago esta modificacion -que se supone es el target del lab-:

![[Pasted image 20230207130810.png]]

ahora que ya estamos en el admin panel nos vamos al final y vemos que hay un endpoint que ejecuta un delete y tiene un token csrf:

![[Pasted image 20230207130916.png]]

ambos valores los reutilizo en el GET request

![[Pasted image 20230207131140.png]]

antes de enviarlo debo agregar en la cookie el valor de la cookie de sesion pero en mi caso ya me aparecía en el request:

![[Pasted image 20230207131430.png]]

michael sommer la tuvo que copiar del Response y pegarla en el Cookie del request pero a mi no me paso así:

![[Pasted image 20230207131508.png]]





