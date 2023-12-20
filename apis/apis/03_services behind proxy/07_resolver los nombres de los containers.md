esta situacion me paso con el Todo de JavaSpring, que no podía conectarse al contenedor de mysql; tambien me paso en el proyecto de desafio02 del bootcamp de devops: el backend js no podía conectarse al mongodb contenedor

Debug del desafio02

1. Escenario:

![[Pasted image 20231127084532.png]]

...

![[Pasted image 20231127084712.png]]

![[Pasted image 20231127084814.png]]

2. tratando de acceder al endpoint: 

![[Pasted image 20231127084924.png]]


3. Como paso 1 levanté la aplicacion en bare metal para poder tenre un monitoreo total de la salida frente a posibles errores que la app pueda generar
4. MongoDB si ya lo tengo en un contenedor y me consta que esta escuchando y en condiciones de manejar peticiones
5. cambian la ip del contenedor en mi hosts -pues como la tengo ahorita siempre pinea al 127.0.0.1 y eso no tiene sentido

![[Pasted image 20231127085225.png]]

![[Pasted image 20231127085300.png]]

6. lo matado de la risa fue que no había abierto el puerto de escucha en el yaml, es decir, que el contenedor estaba arriba pero no había forma de conectarse con él

![[Pasted image 20231127092753.png]]

una vez que regeneré el contenedor ya el backend me jalo



7. 