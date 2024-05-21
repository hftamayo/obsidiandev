


PASOS:
=

1. crear la imagen de Docker eliminando el cache de intentos anteriones

![[Pasted image 20240521111641.png]]

==Los siguientes comandos se ejecutan en el ROOT del proyecto:

![[Pasted image 20240521111733.png]]

docker buildx build --no-cache --platform linux/amd64 -t hftamayo/jsbtodo:experimental-0.0.1 .

![[Pasted image 20240521111804.png]]

==estando fuera de un k8s cluster y si me deseo conectar a un docker container en el mismo host, en mi connectionString puedo incluir la IP del host, otra solucion más portable es hacer un docker compose que haga una red y vincule los containers necesarios, en esta etapa bastará con actualizar la IP del container

docker run --name jsbtodo -p 8002:8002 -v $(pwd)/src/main/resources:/resources hftamayo/jsbtodo:experimental-0.0.1

==para que el comando anterior funcione ya debe existir el archivo properties, en el container unicamente se mapea el volumen, en el Dockerfile ya se especifico que archivo es el que se debe levantar.

![[Pasted image 20240521113635.png]]






2. 