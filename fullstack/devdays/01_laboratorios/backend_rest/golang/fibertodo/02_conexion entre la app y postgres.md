1. verificando conexion con postgres baremetal

![[Pasted image 20230626155323.png]]

2. verificando conexion con postgres docker:

docker exec -it "nombre del container" bash

![[Pasted image 20230824104757.png]]


creacion del usuario, password y la base de datos:

![[Pasted image 20230823125820.png]]

![[Pasted image 20230823130456.png]]

![[Pasted image 20230823130613.png]]

chequear donde esta el hba.conf ya que no esta pidiendo el password:

![[Pasted image 20230823130905.png]]

al hacerle un cat al archivo:

![[Pasted image 20230823131019.png]]

modificando:

![[Pasted image 20230823131332.png]]

restaurando el contenedor.

![[Pasted image 20230823131617.png]]

pero salio mas efectivo:

docker stop id
=

docker start id
=

verificando que pida password:

![[Pasted image 20230823132029.png]]


3.  Al tratar de ejecutar la aplicacion me daba error porque no lograba encontrar el docker, inicialmente pense que era un rollo del container pero no, es la resolucion de nombres puesto que si hago mencion a un hostname alguien debe resolverlo - por ejemplo un servicio dns- pero en mi caso no tengo, entonces opte por ver la ip del server y ponerla en la conexion de la aplicacion:

![[Pasted image 20230823151040.png]]

![[Pasted image 20230823151147.png]]

llama la atencion que aunque expuse el puerto 5435 pg siempre sigue escuchando por el 5432, imagino que en el servicio se require hacer la modificacion o bien expongo asi: 5435:5432. pero antes de hacer esto corro la apliacion y funciona:

![[Pasted image 20230823151346.png]]



==listar las bases de datos disponibles, ponerla en uso:

![[Pasted image 20240520143337.png]]

==listar tablas disponibles y ver su esquema:

![[Pasted image 20240520143508.png]]



4. 