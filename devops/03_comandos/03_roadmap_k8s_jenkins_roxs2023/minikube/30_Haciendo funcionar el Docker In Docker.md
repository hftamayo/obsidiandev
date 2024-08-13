pues me tomó cerca de 10 releases para llegar a una solucion, en esencia el flujo era:
1. asegurarme de los gid del grupo docker tanto en el hostOS como en el pod
2. poder hacer rw en el docker.sock y finalmente asi lo logré:


![[Pasted image 20240508152311.png]]

aun y cuando instalé el paquete sudo y tambien jenkins esta en el grupo sudoers, hacerlo parte del grupo root hizo la magia pues ahora dentro del pod docker.sock se monta con el grupo docker:

![[Pasted image 20240508152604.png]]

==indudablemente que esto no es lo ideal pues si esto en produccion se escala el usuario jenkins autometicamente controlo el pod y alguna acitividad podría lograr en el host OS pero al final pude salir de este rabit hole

Despues de esto puedo ir probando el pipeline con toda normalidad y las etapas se van ejecutando si no hay observaciones gramaticales en el codigo.