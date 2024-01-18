
![[Pasted image 20230210123003.png]]



![[Pasted image 20230210131741.png]]

![[Pasted image 20230210131814.png]]

click en attach a social profile:

![[Pasted image 20230210131906.png]]

![[Pasted image 20230314121848.png]]

estas son las trazas registradas:

![[Pasted image 20230314122024.png]]

llama la atencion que no se registro la fase del link de la social media. cerrando sesiones y repitiendo el proceso; aca estoy recibiendo este error cuando activo a burp como proxy:

![[Pasted image 20230314122237.png]]

es como si tuviera un error en la gestion del protocolo http/https . Recordando que estoy en un Access Point de libre navegacion. 

Update sobre este issue: es un rollo con el crack del burp pro, temporalmente me pase a la version comminuty 2023. Bien raro que si no me conecto a internet, cómo fue que el crack se arruino?

Siguiendo el video de Michael Summer el obutvo el siguiente endpoint del lab, pero aca podemos ver que lo obtuve de la autoridad que esta generando el token:

![[Pasted image 20230316115203.png]]

aca este link debería tener protección vía CSRF.

ahora, voy a presionar el boton attach a social page para copiar el url del auth.
antes de presionar sign-in pongo el intercept en modo on:

este endpoint lo dejo pasar:

![[Pasted image 20230316115431.png]]

este es el que debo copiar:

![[Pasted image 20230316115527.png]]


hago click derecho y copio el URL:

https://0a7c00100379aba8c0811dbf00800093.web-security-academy.net/oauth-linking?code=JcdEkRs9QUCCBLLsbdmKywmF5pvlbyKKdYVDow0nOpB

inmediatamente hago un drop de la conexion y pongo el intercept en modo off:

![[Pasted image 20230316115701.png]]

hago click en el back del navegador y voy al exploit server:

![[Pasted image 20230316115816.png]]

en el exploit server pongo este payload:

![[Pasted image 20230316120039.png]]

en esencia es el link que copiamos con el token de autorizacion

Store y luego Deliver exploit to Victim

me voy a la pagina del lab y le doy click en My Account, sigo siendo wiener, luego otra vez al exploit server presiono deliver exploit to victim y otra vez en My Account, sigo siendo Wiener

![[Pasted image 20230316120348.png]]


luego de Esto Logout, luego me voy a My Account y esta vez uso "login with social media", debería ver algo así:

![[Pasted image 20230316120454.png]]

![[Pasted image 20230316120513.png]]

![[Pasted image 20230316120602.png]]