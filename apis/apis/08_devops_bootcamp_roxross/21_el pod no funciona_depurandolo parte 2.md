![[Pasted image 20240410140934.png]]

...

![[Pasted image 20240410141127.png]]

![[Pasted image 20240410141613.png]]

notese que el pod esta tratando de levantarse sin exito, asi mismo no se puede logear puesto que -quizas- el pod esta en crash, ==aca las recomendaciones de la AI:

![[Pasted image 20240410141851.png]]

![[Pasted image 20240410142222.png]]

![[Pasted image 20240410142500.png]]


==creando el container y que ver al mismo tiempo el output:

![[Pasted image 20240410143114.png]]

...

![[Pasted image 20240410143155.png]]

==aca se puede ver que el container esta bien, entonces el rollo es con algun manifesto del deployment

![[Pasted image 20240410143842.png]]

==decidi quitarle 2 directivas: runAsUser y el service-account al manifesto:

![[Pasted image 20240411134408.png]]

![[Pasted image 20240411141610.png]]

==lo que haré es dejar todos los manifestos a su version original:

![[Pasted image 20240411141705.png]]

==generando  y subiendo la nueva version de la imagen:

![[Pasted image 20240411142956.png]]

![[Pasted image 20240411143056.png]]

==actualizando el deployment:

==los persistentVolume y persistentVolumeClaimed tienen que borrarse antes de recrearse:

![[Pasted image 20240411143851.png]]

![[Pasted image 20240411144352.png]]

==pero siempre obtengo el pod en modo crash:

![[Pasted image 20240411144743.png]]

