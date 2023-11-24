
si por algun caso se me cerro la terminal con una sesion activa de tmux y quiero volver a conectarme:

tmux ls
tmux attach-session -t 0 (o el id que desee)

mover de posicion un panel:

es un tanto confuso pero es mas o menos al calculo pues tiene que ver el orden en que cada panel fue creado pero la combinacion:
Ctrl+o seguido de la llave cerrada } mueve de posicion el panel
tambien: ctrl+b + ctrl+o los rota