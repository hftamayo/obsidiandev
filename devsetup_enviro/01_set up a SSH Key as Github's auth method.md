
# RESUMEN DEL PROCESO - agosto 2024-:

```
ssh-keygen -t ed25519 -C "herbert.fernandez@ues.edu.sv"
eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ssh-add -l
   
   git remote -v
   git remote set-url origin <git link>
   git remote -v
   
   ir al settings de github, el avatar, dar de alta una nueva key con la llave publica y depues de eso probar con un git push
```

****
==ssh-keygen -t ed25519 -C "hftamayo@gmail.com"==

establecí un nombre personalizado a la llave y tambien agregué un passphrase, ésta la utilizaré cuando el gestor de cuentas del sistema operativo la cache y activo el boton "recordar esta contraseña..."

==git config --global core.sshCommand "ssh -i ~/.ssh/hftamayo_dell_ed25519"
chmod 600 .ssh/hftamayo...

comandos para confirmación:
==ssh-add -l==

me paso a un repo y ejecuto este comando: 

![[Pasted image 20240313111314.png]]

notese que aca ya obtuvimos info del repo por medio de ssh, ahora probando un push:

![[Pasted image 20240313111404.png]]

Ahora, me paso a otro repo y verifico el origen:

![[Pasted image 20240313111541.png]]

por cada repo existente debo hacer los siguientes cambios:
==git remote set-url origin git@github.com:hftamayo/devopsbc2023-ch03.git

![[Pasted image 20240313112137.png]]


==REPITIENDO EL PROCESO EN UN EQUIPO NUEVO
=


![[Pasted image 20240403161138.png]]

![[Pasted image 20240403161202.png]]

![[Pasted image 20240516082549.png]]

==notese que aunque el ultimo comando me generó error aun asi pude conectarme al repo:

![[Pasted image 20240516082655.png]]


## ===AGREGANDO UNA PUBLICA A MI GITHUB

![[Pasted image 20250205150611.png]]

![[Pasted image 20250205150639.png]]


## ==SETUP DE 2 GITHUB IDENTITIES EN EL MISMO OS

==PASO 1: Crear un archivo config en el directorio .ssh

![[Pasted image 20250205151449.png]]

==PASO 2: Clonando un repo usando la segunda identidad:

![[Pasted image 20250205152110.png]]

==PASO 3: setup del repo con la identidad a utilizar

![[Pasted image 20250205152308.png]]

![[Pasted image 20250205152600.png]]

==PASO 3: dosificando el $HOME/.gitconfig para manejar 2 identidades:

cuando tengo una identidad:

![[Pasted image 20250205152954.png]]

teniendo dos identidades:

![[Pasted image 20250205153051.png]]

==ahora, en cada repo debe setear que identidad necesito gestionar, este es un ejemplo:

`cd /path/to/your/repository; git config user.name "Herbert Fernandez Tamayo"; git config user.email "herbert.fernandez@ues.edu.sv"`
   
 ==asi mismo, para clonar un repo ==:

Esto esta determinado por el archivo .ssh/config

`git clone git@github.com-htues:<repository-name>.git
cd <repository-name>`

