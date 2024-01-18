![[Pasted image 20220807184951.png]]

verificando el codigo fuente de la pagina HOME del lab encontramos que esta el siguiente script:

![[Pasted image 20220807185532.png]]

addEventListener tiene la variable message  y dentro de esta una funcion hard codeada que espera obtener una string http o bien https.

luego tiene un location.href.

el objetivo es explotar la variable url. Vamos al exploit server y ponemos la siguiente cadena:

![[Pasted image 20220807185842.png]]

Store y luego Deliver to the Victim.

Notese que en la imagen anterior tengo un typo, al corregirlo y deliver to the victim:

![[Pasted image 20220807190111.png]]

en esencia el http y * hace mención que cualquier url no seguro puede ser desplegado la funcion postMessage