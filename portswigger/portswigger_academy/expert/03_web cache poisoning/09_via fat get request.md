![[Pasted image 20230110121856.png]]

despues de visitar un post tengo una mejor vision de los endpoints:

![[Pasted image 20230110122148.png]]

![[Pasted image 20230110122212.png]]

Mando el geolocate.js al Repeater y vemos el siguiente resultado:

![[Pasted image 20230110122306.png]]

![[Pasted image 20230110122325.png]]

verificando si hay algun cambio en el response si uso un parametro cualquiera:

![[Pasted image 20230110122454.png]]

![[Pasted image 20230110122514.png]]

puesto que funciono podemos tratar de hcer una inyección de codigo:

![[Pasted image 20230110122627.png]]

![[Pasted image 20230110122645.png]]

envenenado el cache con este payload (enviar request hasta que salga Hit):

![[Pasted image 20230110122806.png]]

tengo que enviar los request hasta que Age pase de 35 y luego a 0; a pesar que en el response efectivamente me da tanto el alert como la renovación del cache (35 a 0), no lograba resolver el lab.

Viendo el video de Michael Summer el utilizó el payload así:

![[Pasted image 20230110124035.png]]

![[Pasted image 20230110124058.png]]

reiniciaré toda la sesión y volveré a probar pues a Michael Sommer si le funcionó.

1. reiniciar el lab me sirvio
2. cuando haga el envenenamiento del cache debo dar un espacio 
3. hasta que ya el cache se haya reiniciado me deberá aparecer el alert, la ronda de 0 a 35 siempre aparece setCountry:

![[Pasted image 20230113143841.png]]

![[Pasted image 20230113143901.png]]

![[Pasted image 20230113143925.png]]