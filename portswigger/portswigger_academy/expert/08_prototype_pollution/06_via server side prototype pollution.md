![[Pasted image 20230601001849.png]]

Al momento de hacer estos lab (jun 2023) no hay resolucion validada de la comunidad entonces el procedimiento que segui son de otras fuentes en youtuve y todo es mas corto:

Payloads a probar:

"__proto__": {
	"foo":"bar"
}

"__proto__": {
	"isAdmin":true
}

![[Pasted image 20230601191009.png]]

![[Pasted image 20230601191032.png]]

pongo el intercept en off

![[Pasted image 20230601191119.png]]

notese que en la esquina superior derecha no dice que tengo acceso al admin panel


haciendo una modificacion a la data:

![[Pasted image 20230601191218.png]]

![[Pasted image 20230601191254.png]]

revisando el history:

![[Pasted image 20230601191327.png]]

change address lo mando al repeater y ahi pruebo los payload. Payload 1:

mucho ojo, despues de sessioId debo agregar una coma:

![[Pasted image 20230601191745.png]]

![[Pasted image 20230601191758.png]]

probando el payload2:

![[Pasted image 20230601191853.png]]

![[Pasted image 20230601191906.png]]

click derecho en el response y seleccoino "show response in the browser y me sale una ventana para copiar el url", lo pego en otra tab y me aparece un json:

![[Pasted image 20230601192105.png]]

cierro esa ventana y refresco la anterior:

![[Pasted image 20230601192129.png]]

![[Pasted image 20230601192148.png]]

![[Pasted image 20230601192205.png]]