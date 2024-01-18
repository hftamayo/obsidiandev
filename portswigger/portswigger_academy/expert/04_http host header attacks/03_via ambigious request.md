![[Pasted image 20230123114620.png]]

![[Pasted image 20230123120712.png]]

![[Pasted image 20230123120735.png]]

modificando la direccion del host en el request:
![[Pasted image 20230123120817.png]]

![[Pasted image 20230123120844.png]]

![[Pasted image 20230123120902.png]]

verificando si puedo usar un parametro dummy y cambiando la direccion del host a la real:

![[Pasted image 20230123120952.png]]

![[Pasted image 20230123121009.png]]

verificando si puedo usar 2 hosts para envenenar el cache:

![[Pasted image 20230123121059.png]]

![[Pasted image 20230123121125.png]]

notese en la imagen de arriba que tracking.js apunta al segundo host. Verificando si el cache persiste (miss a hit):


![[Pasted image 20230123121218.png]]

![[Pasted image 20230123121247.png]]

cargando el exploit server y apuntando hacia tracking.js:

![[Pasted image 20230123121535.png]]

guardo este payload:

![[Pasted image 20230123121600.png]]


ahora que guarde el payload copio la direccion del exploit server y la pongo en el segundo host:

![[Pasted image 20230123121820.png]]

![[Pasted image 20230123121842.png]]

notese que en la imagen de arriba el script ya apunta al payload del exploit server y por tanto va a revelar la cookie:

![[Pasted image 20230123121944.png]]


