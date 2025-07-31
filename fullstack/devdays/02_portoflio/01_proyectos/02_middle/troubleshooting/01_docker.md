
## 1. despues de un update:

==despues de un update a mi OS cuando quiero arrancar un container obtenia este error de una red que no existia previamente:==

```
hftamayo@csj-Latitude-5510:~$ docker start redisdev                                                  
Error response from daemon: could not find a network matching network mode mysqldev_network: network 
mysqldev_network not found 

```


Trate de seguir esta ruta de comandos:

```
hftamayo@csj-Latitude-5510:~$ sudo rm -rf /var/lib/docker/network/files/local-kv.db
hftamayo@csj-Latitude-5510:~$ clear
hftamayo@csj-Latitude-5510:~$ sudo systemctl start docker
hftamayo@csj-Latitude-5510:~$ docker ps -a

hftamayo@csj-Latitude-5510:~$ docker start redisdev
Error response from daemon: network 1288e8314db0e9034a21c59dd4ba00cacb2678d2981c20413548c435895d7ca5 
not found
Error: failed to start containers: redisdev
hftamayo@csj-Latitude-5510:~$ docker start pgdev
Error response from daemon: network 1288e8314db0e9034a21c59dd4ba00cacb2678d2981c20413548c435895d7ca5 
not found
Error: failed to start containers: pgdev
hftamayo@csj-Latitude-5510:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
fbb0f621b204   bridge    bridge    local
b535d21fb975   host      host      local
4b2f3db5d1ee   none      null      local
hftamayo@csj-Latitude-5510:~$ docker inspect redisdev --format='{{.HostConfig.NetworkMode}}'
mysqldev_network
hftamayo@csj-Latitude-5510:~$ docker inspect pgdev --format='{{.HostConfig.NetworkMode}}'
developer_network
hftamayo@csj-Latitude-5510:~$ docker inspect goresttodo --format='{{.HostConfig.NetworkMode}}'
developer_network
hftamayo@csj-Latitude-5510:~$ docker inspect reacttodo --format='{{.HostConfig.NetworkMode}}'
developer_network
hftamayo@csj-Latitude-5510:~$ docker inspect mysqldev --format='{{.HostConfig.NetworkMode}}'
developer_network

```

==regadas:
1. aun tenia un container atado a una red que ya existia, posiblemente he estado teniendo falsos positivos con la itneracción de este container
2. borre todas las redes existentes

pues bien aca una ruta de solucion
```

1. docker network ls -> apunto el nuevo id de la red
2. docker start mysqldev -> aca voy a obtener el id de la red anterior:

Error response from daemon: network 1288e8314db0e9034a21c59dd4ba00cacb2678d2981c20413548c435895d7ca5 not found
Error: failed to start containers: redisdev


3. Ahora tengo que encontrar el ID del container:
docker inspect mysqldev --format='{{.Id}}'

con ese ID debo editar el correspondiente archivo config.v2.json de esta ruta:
sudo nano /var/lib/docker/containers/$CONTAINER_ID/config.v2.json

en el archivo hostconfig.json ubicado en la misma ruta tambien se puede comprobar el nombre de la red a la que se desea attachar

4. sudo systemctl stop docker
5. sudo systemctl stop docker.stop
6. sudo systemctl start docker
7. docker start mysqldev
8. docker ps -a

```



