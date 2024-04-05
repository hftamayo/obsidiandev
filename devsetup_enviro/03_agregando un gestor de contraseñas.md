PopOS trae por default un gestor de contraseñas el cual se activa cuando detecta una nueva clave, en el caso de Zorin no sucede eso:

![[Pasted image 20240403162439.png]]

==en las distros basadas en gnome existe keyring, el cual se actualiza cada vez que hay una nueva passphase; tambien puedo usar la gestion con el ssh-agent:

![[Pasted image 20240404084955.png]]

==sin embargo usando el agente ssh la gestion de contraseñas unicamente dura para la sesion activa de manera que en el siguiente reboot o inclusive si cierro la terminal las claves serán solicitadas nuevamente, asi que agregando esta modificacion al .bashrc:

# Start ssh-agent if not already running
if ! pgrep -q -u "$USER" ssh-agent; then
    ssh-agent -s > "${TMPDIR:-/tmp}/ssh-agent"
fi
source "${TMPDIR:-/tmp}/ssh-agent"

# Add the private key to the agent
ssh-add <private_key>

![[Pasted image 20240405082130.png]]

al hacer un source .bashrc obtuve 1 warning: que la opcion -q no era valida y tambien tuve que digitar el passphrase, entonces esto me tocaría hacerlo en cada boottime, buscando estas alternativas:

1. modificando el script:

![[Pasted image 20240405082847.png]]

![[Pasted image 20240405083504.png]]

el rollo es que siempre que usa la terminal, las passphrase tengo que volverlas a digitar

2. usando gnome-keyging para gestion de paquetes en entorno graficos

==sudo apt install gnome-keyring

asegurandome que el keyring se inicie una vez que el entorno grafico esta activado - estas lineas se ponen en el bashrc-:

if [ -n "$DESKTOP_SESSION" ];then
    eval $(gnome-keyring-daemon --start)
    export SSH_AUTH_SOCK
fi

![[Pasted image 20240405085946.png]]

tambien desactivando el openssh agent para que no choque con el gnome-keyring:

![[Pasted image 20240405092710.png]]

al reiniciar probe agregar una llave pero el gnome-keyring no salta, al menos en Zorin parece que no habrá forma pero el PopOS esto funciona bien transparente.


3. en entorno consola puedo instalar estos paquetes:

==sudo apt install -y keychain

