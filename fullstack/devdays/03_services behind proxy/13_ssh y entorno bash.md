
proceso de configuracion de mi bash para poder conectarse via SSH a un server remoto

1. ==setup de bash


![[Pasted image 20240819122046.png]]

==2. setup de apt:

![[Pasted image 20240819122803.png]]

==contenido de 8080proxy

![[Pasted image 20240819123111.png]]

# OJO: aca tengo un typo bien serio, es con doble dos puntos en el http:

![[Pasted image 20240819125043.png]]

==en mi caso reinicié el equipo. luego de eso:
```
sudo apt update
sudo apt install netcat
```


==3. setup de ssh:

modificar el archivo ssh_config:

![[Pasted image 20240819125527.png]]

la siguiente linea tengo que actualizarla:
![[Pasted image 20240819125712.png]]

![[Pasted image 20240819125918.png]]

cerre mi bash y cuando volvi a abrir obtuve este error:

![[Pasted image 20240819130353.png]]

==se supone, segun Copilot, que netcat no pudo gestionar las creds planas

4. ==intentando con otro paquete:

```
1. vuelvo a poner como un comentario el ProxyCommand que setee en el paso anterior
2. sudo apt install corkscrew
```

![[Pasted image 20240819130833.png]]

![[Pasted image 20240819131120.png]]

![[Pasted image 20240819131346.png]]

![[Pasted image 20240819131428.png]]

aunque me dio error pero pude ejecutar el comando


==5.  probando replicar a un server github via SSH

