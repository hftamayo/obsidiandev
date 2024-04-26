1. ==Escenario #1: backend y frontend en el mismo host sin HTTPS:

1. userController:

![[Pasted image 20240227095032.png]]

2. CORS:

![[Pasted image 20240227095134.png]]


3. componente en el frontend que envuelve las peticiones HTTP:

![[Pasted image 20240227100314.png]]


4. resultado: la cookie si se guarda en el navegador por la politica ==SameSite== pues los requests entre los mismos hosts si funcionan, por supuesto que este escenario es el menos probable pues conservamos el enfoque monolito

2. Escenario #2: backend en un container en hostB y frontend en baremetal en hostA
	1. mismo setup del escenario #1
	2. resultado: la cookie no se guarda en el frontend por la politica de no compartir cookies entre cross domains, el atributo Lax ==es muy estricto para cross site cookies== , este escenario es util cuando estoy en fase de desarrollo, inclusive es aun más util si tengo equipos de front y back devs.

		las siguientes son respuestas de escenario similares:
	![[Pasted image 20240227104814.png]]

tratar de usar cookies entre distintos sitios es un error de arquitectura pues como dice el enunciado de arriba esto simplemente no tiene sentido.

==explicado con mayor detalle por Gemini==:

![[Pasted image 20240227113423.png]]

![[Pasted image 20240227113504.png]]


Aca algunas alternativas:

![[Pasted image 20240227110536.png]]


![[Pasted image 20240227115042.png]]

![[Pasted image 20240227115107.png]]

![[Pasted image 20240227115139.png]]

![[Pasted image 20240227115229.png]]


==uso de subdomains o reverse proxy==

![[Pasted image 20240227115746.png]]

![[Pasted image 20240227115822.png]]

![[Pasted image 20240227115843.png]]

==en ambos casos yo debo tener mis propios controles de CSRF, XSS, JWT tampering o incluso implementar Oauth2.0==

==pensando en la implementacion en produccion== 

![[Pasted image 20240227120743.png]]

![[Pasted image 20240227120828.png]]

![[Pasted image 20240227120855.png]]

![[Pasted image 20240227120921.png]]

![[Pasted image 20240227120945.png]]


tomado de:
https://stackoverflow.com/questions/66503751/cross-domain-session-cookie-express-api-on-heroku-react-app-on-netlify

https://stackoverflow.com/questions/71071547/cookies-not-sent-with-cross-origin-request/72663708#72663708


Solucion #1: implementar un Reverse Proxy en el backend



4. Escenario #3: backend y frontend en contenedores en el mismo host 


5. Escenario #4: backend y frontend en contenedores pero en hosts distintos

6. Escenario #5: backend y frontend en clusters en la nube