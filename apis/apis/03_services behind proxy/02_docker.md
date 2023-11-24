
Nota: yo creo que instale Docker usando snap solamente por probar si habia alguna diferencia entre usar apt, snap u otro gestor de paquetes emergentes, pero para ambiente dev y produccion la recomendacion es:

1. usar el mecanismo de instalacion oficial (usando apt y habilitando repos especificos)
2. la instalacion de docker mediante snap no utiliza algunos json o algunos otros archivos no estan en las ubicaciones esperadas asi que no se recomienda esta opcion para un dev o mucho menos produccion

3. Configurar docker y proxy si lo instale usando apt: https://docs.docker.com/network/proxy/ y este es mucho mejor: https://docs.docker.com/config/daemon/systemd/#httphttps-proxy

instalar docker: https://www.dir-tech.com/en/how-to-install-docker-on-ubuntu-23-04/

2. configurar docker y proxy si lo instale usando snap: https://forum.snapcraft.io/t/configure-proxy-to-be-used-by-dockerd/9647

![[Pasted image 20230809125524.png]]

![[Pasted image 20230809130532.png]]

![[Pasted image 20230809130618.png]]

un post alternativo que encontre para configurar es este: https://askubuntu.com/questions/1296892/snap-edit-docker-configuration

probando si docker jala sin estos cambios al proxy:

![[Pasted image 20230809144311.png]]

haciendo los cambios y verificando si puedo conectarme

![[Pasted image 20230809145010.png]]

![[Pasted image 20230809145320.png]]

![[Pasted image 20230809145803.png]]

![[Pasted image 20230809145728.png]]

![[Pasted image 20230809145917.png]]

![[Pasted image 20230809150002.png]]

![[Pasted image 20230809150136.png]]

Pero aca hay una pequeña regada, al inicio del archivo dice esto:

![[Pasted image 20230816082944.png]]

al siguiente dia que quice conectarme a descargar una imagen no pude, para hacer estos cambios permanentes:

1. en una instalacion baremetal la carpeta de archivos de configuracion para eo docker daemon es:

![[Pasted image 20230816124524.png]]

2. en una instalación por snap la sustituta es

![[Pasted image 20230816124600.png]]

examinando el archivo snap service veo que apunta al /etc/environment para configuraciones:

![[Pasted image 20230816125928.png]]

3. Intentando con daemon.json 

![[Pasted image 20230816131055.png]]


![[Pasted image 20230816131307.png]]

![[Pasted image 20230816131342.png]]

pero al reiniciar mi equipo al hacer docker info ya no veo la data del proxy, hay 2 carpetas dentro del entorno docker:

![[Pasted image 20230817140712.png]]

![[Pasted image 20230817140830.png]]

pero tampoco funciono. En esta pagina (https://docs.docker.com/config/daemon/) dice que un http proxy es lo unico que no puede configurarse en daemon.json:

![[Pasted image 20230817141338.png]]

en estos recursos : https://docs.docker.com/desktop/settings/linux/#proxies y https://docs.docker.com/network/proxy/ pero para una instalación snap parece que es distinto.

entonces usando el archivo environment para ver si lo lee:

![[Pasted image 20230817141942.png]]

Esta vez use estos comandos para reiniciar docker:

![[Pasted image 20230817142047.png]]

docker info

![[Pasted image 20230817142106.png]]

efectivamente al reiniciar el equipo y hacer un docker info se mantiene la info del proxy, entonces la solucion fue modificar el /etc/environment

4.  Eliminar contenedores que estan detenidos por algun error:

![[Pasted image 20230822100459.png]]

![[Pasted image 20230822101102.png]]

El contenedor java esta detenido (iddle)

![[Pasted image 20230822101230.png]]

eliminar imagenes:

docker rmi "image id"

detener todos los contenedores:

docker stop $(docker ps -a -q)