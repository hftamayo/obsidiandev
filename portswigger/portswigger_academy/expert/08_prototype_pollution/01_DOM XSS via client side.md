![[Pasted image 20230523204359.png]]

el walkthrough es de Emanual Picariello y el ingles no es muy claro, pero en este lab en esencia es inyectar con un nuevo objeto el DOM:

si en la consola del navegador uso el comando Object.prototype puedo ver el contenido del prototype asociado al website:

![[Pasted image 20230523205424.png]]

si en el url agrego una variable que logre inyectar el contenido:

![[Pasted image 20230523205712.png]]

al volver a verificar en la consola:


![[Pasted image 20230523205731.png]]

entonces como el sitio es susceptible, esencialmente puedo inyectar lo que quiera:

revisando los recursos del sitio encontramos un script donde hace mencion a transport_url:

![[Pasted image 20230523210025.png]]

esencialmente de esa variable toma el config, entonces si logro ionyectar algo aca puedo envenenar el DOM:

![[Pasted image 20230523210154.png]]

![[Pasted image 20230523210211.png]]



