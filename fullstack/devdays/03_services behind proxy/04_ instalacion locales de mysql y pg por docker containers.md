
![[Pasted image 20230816095351.png]]

mysql es utilizado por el proyecto javaspringtodo y postgres es utilizado por gobank:

desinstalando mysql-server:

![[Pasted image 20230816095445.png]]

sudo systemctl stop mysql
sudo apt purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*

![[Pasted image 20230816100709.png]]

....

![[Pasted image 20230816100733.png]]

desinstalacion de postgres:

![[Pasted image 20230816100849.png]]

sudo apt-get purge --remove postgresql postgresql-14 postgresql-client-common postgresql-common postgresql-contrib

![[Pasted image 20230816102226.png]]


![[Pasted image 20230816102019.png]]

![[Pasted image 20230816102059.png]]

YAML para la imagen de MySQL sin mostrar credenciales:



====================
POSTGRES
====================

YAML para la imagen y container de Postgres sin exponer credenciales:

![[Pasted image 20230823115614.png]]
aca aunque yo haya expuesto el puerto 5435 dentro del container yo tengo que hacer el cambio que el peruto 5435 este escuchando, por tanto haré el cambio de 5435:5432


ejecutar el yml de postgres:

![[Pasted image 20230823120622.png]]

...

![[Pasted image 20230823120712.png]]

llama la atencion que el puerto 5432 esta abierto y en el yml especifique que es el puerto 5435

![[Pasted image 20230823120916.png]]

![[Pasted image 20230823120951.png]]

![[Pasted image 20230823121706.png]]

Accediendo al pg dockerizado:

![[Pasted image 20230823122333.png]]


Si por algun motivo hago cambios a los yml (de hecho, con mysql incluí el uso de secrets) basta con guardarlos y volver a ejecutar docker-compose up -d, unicamente los contenedores con cambios se van a actualizar

![[Pasted image 20230817153421.png]]

para reconstruir imagenes que ya existen aca en este WARNING sugiere el uso del comando:

docker-compose build o docker-compose up --build

eliminar containers y volumenes innecesarios:  este comando elimina imagenes y containers que esten apagados:

===================
docker system prune -a
===================

idem para volumenes:

===================
docker volume prune
=

comando para desplegar toda la configuracion del container que eta encendido:

docker container inspect "container name"
=

desplegar la ip del container:
docker container inspect "container name" | grep -i IPAddress
=
![[Pasted image 20230823150248.png]]








