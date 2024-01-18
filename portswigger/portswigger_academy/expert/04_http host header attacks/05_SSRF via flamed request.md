
![[Pasted image 20230208122106.png]]

mando el root al repeater:

![[Pasted image 20230208123641.png]]

copio el url del lab y lo pego en el GET:

![[Pasted image 20230208123802.png]]

en el host voy a poner la direccion del burp collab:

![[Pasted image 20230208123846.png]]

![[Pasted image 20230208124147.png]]

hago el poll y luego vuelvo a realizar el send request

![[Pasted image 20230208124223.png]]

![[Pasted image 20230208124308.png]]

aca mismo envio esta traza al intruder:

![[Pasted image 20230208130115.png]]

(notese que aca en la imagen hay 2 payloads y es porque olvide presionar el boton clear, me di cuenta cuando termino el ataque y no obutve resultados, el unico payload que necesita este atque es del Host 192.168.0.0)

cambio al rango de ip que da el lab y hago un atque de numeros



![[Pasted image 20230208124509.png]]

![[Pasted image 20230208130406.png]]

aca mismo envio el resultado positivo al repeater:

![[Pasted image 20230208130457.png]]

![[Pasted image 20230208130530.png]]

![[Pasted image 20230208130617.png]]

agregando el csrf puesto que lo olvidé:

![[Pasted image 20230208130807.png]]

![[Pasted image 20230208130828.png]]