==1. Habilitar experimental en docker

- idealmente debería ser el docker del repositorio: sudo apt-get upgrade docker-ce

==2. modificar el daemon.json agregando esta linea.

![[Pasted image 20240119120955.png]]

==3. reiniciar el servicio: 

![[Pasted image 20240119121121.png]]

==4. crear la carpeta plugins en el HOME de docker:

![[Pasted image 20240119121420.png]]

==5. descar en esta carpeta el plugin de buildx:

wget https://github.com/docker/buildx/releases/download/v0.7.1/buildx-v0.7.1.linux-amd64

==6.  dar permisos de ejecucion y simplificar el nombre

![[Pasted image 20240119121753.png]]

==7. verificar si esta funcionando:

![[Pasted image 20240119121921.png]]

8. generar una imagen multiplataforma multiarquitectura de un backend escrito en CSharp:

==docker buildx build --platform linux/amd64 -t ch03be_worker:stable-1.0.0 .

el Dockerfile debe verse así:

![[Pasted image 20240119125334.png]]

el comando a utilizar debe ser así:

docker buildx build --platform linux/amd64 -t ch03be_worker:stable-1.0.0 .

![[Pasted image 20240119125600.png]]

==logearse en docker hub y subir una imagen==

un approach es crear en el .bashrc dos variables (user y pass) para gestionar las credenciales del docker hub, sin embargo, éstas son plain text y eventualmente se pueden escalar, asi que utilizaré un docker credential helper:

==setting up de un docker credential helper==:


![[Pasted image 20240313114045.png]]

![[Pasted image 20240313120022.png]]

![[Pasted image 20240313121017.png]]

{
        "auths": {
                "https://index.docker.io/v1/": {
                        "auth": "aGZ0YW1heW9AZ21haWwuY29tOmIxMjZrNGRta2pjeTNVVA=="
                }
        }
}


![[Pasted image 20240313121642.png]]

![[Pasted image 20240313123808.png]]

==moviendo el executable docker-credential-pass a /usr/local/bin:

![[Pasted image 20240313124713.png]]


generando un perfil para mi genkey:

![[Pasted image 20240313125146.png]]

el passphrase lo tengo en mi repo:

![[Pasted image 20240313124902.png]]

![[Pasted image 20240313125101.png]]

se nota que gpg siempre usa la estructura de llaves privadas y publicas. Este es el key ID: 1FE13B549A0685C080163F2551DC46AC5F91ED3C

Ahora, para listar las ==gpg existentes:==

==gpg --list-secret-keys --keyid-format LONG

![[Pasted image 20240313125437.png]]

==generando un password para este ID:

![[Pasted image 20240313125741.png]]

actualizando el nombre del comando helper que utilizaré para gestionar las credenciales:

![[Pasted image 20240313131223.png]]

ejecutando el comando ==docker login== para capturar las credenciales y que de este momento en adelante sean gestionadas por docker-credential-pass:

![[Pasted image 20240313131629.png]]


bueno, aca hay un typo en mi config.json:

![[Pasted image 20240313142728.png]]

notese que en credStore inicialmente no hay la comilla de cierre, pues una vez que ya lo solucioné veamos que pasa:

![[Pasted image 20240313142847.png]]


exportando la llave publica:

![[Pasted image 20240313133433.png]]

==POR ALGUN MOTIVO NO FUNCIONA Y ANTES DE DEPURARLO, EN LUGAR DE SEGUIR USANDO UNA CLAVE VOY A DOCKER HUB A GENERAR UNA PERSONAL ACCESS TOKEN EL CUAL LA VOY A REGISTRAR CON GPG:



![[Pasted image 20240313133844.png]]

![[Pasted image 20240313134011.png]]


![[Pasted image 20240313134159.png]]

==MODIFICO EL CONFIG.JSON PARA AGREGAR EL TOKEN==:

![[Pasted image 20240313143443.png]]

![[Pasted image 20240313143320.png]]

==MODIFICAR A PATA EL CONFIG.JSON NO DA BOLA, siguiendo este metodo:


entonces ahora que ya tengo mi token debo hacer un pass init para mi perfil:


![[Pasted image 20240314125010.png]]

en la primera flecha yo debo copiar toda la cadena porque en la parte de arriba use nada más los primeros 7 y por eso es que no me guardaba las credenciales:


![[Pasted image 20240314125131.png]]

==recordatorio: en la segunda flecha usar el PAT


==probando si puedo subir una imagen:


![[Pasted image 20240314125629.png]]

==al final resulta que esto no debe hacerse asi, que lo ideal es gestionar credenciales de docker usando Hashicorp Vault o AWS Secrets Manager"== 


==ELIMINANDO EL DOCKER HELPER DEL SERVER EN PRODUCCION:
=


![[Pasted image 20240314142316.png]]

![[Pasted image 20240314142401.png]]

![[Pasted image 20240314142636.png]]

![[Pasted image 20240314143202.png]]



---------------------------------------------------------------------
OPERACIONES DEPRECADAS: rabbit holes
=



==reiniciamos el servicio docker ==

![[Pasted image 20240313144027.png]]

==obteniendo un error y tratando de subsanarlo:


![[Pasted image 20240314115103.png]]

probando:

![[Pasted image 20240314115227.png]]

==editando el archivo:

![[Pasted image 20240314115507.png]]


verificando permisos:


![[Pasted image 20240314115713.png]]

intentando de otra forma:

![[Pasted image 20240314120517.png]]

