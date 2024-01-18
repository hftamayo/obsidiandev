![[Pasted image 20230520054943.png]]

walkthrough by:  https://www.youtube.com/watch?v=HhqzXU5NB8g

apago el intercept y le doy view details en cualquier producto:

![[Pasted image 20230520055600.png]]

reviso los inventarios disponibles:

![[Pasted image 20230520055644.png]]

me paso al intercept y veo los endpoints disponibles:

![[Pasted image 20230520055733.png]]

productStock es el que me genera la salida asi que le doy click derecho y luego la opcion Scan

![[Pasted image 20230520055822.png]]

luego me voy a Target-> Scan

![[Pasted image 20230520055924.png]]

en esencia el Scan me revisa e identifica posibles vulns a explotar

en productStock le doy "Add to Scope" pero aca no entendí para que nos sirve esta opcion, es como si apareciera otra vez en el scan:

![[Pasted image 20230520060237.png]]

en el panel de la derecha del scan aparecen las vulns sugeridas que puedo explotar, en este caso el lab es vulnerable a un ataque de XXE, especificamente XLI que esta en un lab:

![[Pasted image 20230520060551.png]]

el payload es este: 
"foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo"

entonces mando el endpoint productStock al repeater y en lugar del id del nodo pego el payload:

![[Pasted image 20230520060730.png]]


![[Pasted image 20230520060702.png]]

si todo sale bien:

![[Pasted image 20230520060804.png]]

![[Pasted image 20230520060833.png]]