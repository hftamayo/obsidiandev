![[Pasted image 20230604070038.png]]

payload:
Connection : keep-alive
Content-Type: application/x-www-urlencoded
Content-Length: 8
Transfer-Encoding: chunked

0

G

accesando al laboratorio y activo inmeditamente el proxy, hago F5 para refrescar:


![[Pasted image 20230604071151.png]]

verifico el history y envio el root al repeater:

![[Pasted image 20230604071229.png]]

![[Pasted image 20230604071259.png]]

unicamente dejo hasta el user-agent y cmabio el request de GET a POST:

![[Pasted image 20230604071429.png]]

copio el payload:

![[Pasted image 20230604071531.png]]

![[Pasted image 20230604071545.png]]

no funciono por estos motivos:
-en la version 2022 hay que cambiar el parametro del Request Header:

![[Pasted image 20230605202906.png]]

si no hacia esto siempre se cambiaba al HTTP 2 y por eso no funcionaba.

-el payload es mucho mas sencillo:

![[Pasted image 20230605202946.png]]

cuando lo envie por segunda vez:

![[Pasted image 20230605203035.png]]

![[Pasted image 20230605203049.png]]