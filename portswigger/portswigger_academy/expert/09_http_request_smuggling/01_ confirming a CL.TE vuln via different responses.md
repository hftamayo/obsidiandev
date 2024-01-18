![[Pasted image 20230705195505.png]]

![[Pasted image 20230705200513.png]]

Para estos laboratorios se recomienda utilizar la extension HTTP Request Smuggler:

![[Pasted image 20230705201152.png]]


seguire esta solucion pues parece ser muy viable: https://www.youtube.com/watch?v=MmcRRe-q8bE

![[Pasted image 20230706074123.png]]

![[Pasted image 20230706074144.png]]

change request method:

![[Pasted image 20230706074308.png]]

borrando una serie de lineas:

![[Pasted image 20230706074408.png]]

hago estos cambios al request:

![[Pasted image 20230706074746.png]]

activo el boton de la "n" en la esquina superior derecha para ver los saltos:

![[Pasted image 20230706074848.png]]

ya que estoy sin caracteres innecesarios le doy enviar:

![[Pasted image 20230706075206.png]]

obtuve un 200 y el chero obtuvo un 500, incluso le cambie el request a 1.1 y siempre fue 200:

![[Pasted image 20230706075456.png]]

hago estos cambios:

![[Pasted image 20230706075611.png]]

luego le doy click derecho -> extensions -> HTTP Request Smuggler -> convert to chunked

![[Pasted image 20230706075742.png]]

voy a eliminar el Transfer Encoding, me salio doble porque tengo un typo:

![[Pasted image 20230706075825.png]]

luego cambio el content length a 4:

![[Pasted image 20230706075920.png]]

y luego en esta ruedita:

![[Pasted image 20230706075942.png]]

desmarco la opcion: Update Content-Length. Activo la "n" para ver los saltos:

![[Pasted image 20230706080120.png]]

notese que le di ENTER despues del 34 para que me quede igual al del tutorial, luego deberia hacer Send y recibir un 200 y luego un 404, pero obtuve un 500:


![[Pasted image 20230706080306.png]]

cambiando de 34 a 36 asi como esta en el tutorial pero nada, volvi a hacer el tutorial y lo mismo, al final vi la solucion de portswigger y esa funciono:

![[Pasted image 20230706180839.png]]

![[Pasted image 20230706180852.png]]

![[Pasted image 20230706180906.png]]




