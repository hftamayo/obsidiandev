
1. por algun motivo postman tenia activada la opcion de usar el proxy del sistema, por tanto tratando de conectarme a los contenedores obtenia errores 403 ó 401

![[Pasted image 20231006145811.png]]

2. una vez desactivado, en el /etc/hosts le di de alta al contenedor de go:

![[Pasted image 20231006145915.png]]

Usando gorestbank para las primeras exploraciones:


3.  Desde postman hago un GET Request usanod el nombre del contenedor:

![[Pasted image 20231006150034.png]]


4. completando el resto de request para esta API:






5. 