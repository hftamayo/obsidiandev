
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

