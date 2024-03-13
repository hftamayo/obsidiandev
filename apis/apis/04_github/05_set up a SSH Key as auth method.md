
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



