

verificando el status de mi pod nginx:

![[Pasted image 20240227145718.png]]

Instalar nginx en un pod del cluster:

![[Pasted image 20240227151752.png]]

recolectando info del anterior deploy:

![[Pasted image 20240227152657.png]]

como puedo ver el pod es un clusterIP por tanto no es accesible fuera del cluster, haciendo el cambio a NodePort:

microk8s.kubectl patch svc nginx --type merge -patch "$(jq '.spec.type="NodePort" | .spec.ports[0].nodePort=31080' -)"

![[Pasted image 20240227153453.png]]

sin embargo este comando nunca termino, voy a borrar el servicio y regenerarlo:

![[Pasted image 20240227153715.png]]

![[Pasted image 20240227154138.png]]

verificando el puerto de escucha del NodePort:

![[Pasted image 20240227154845.png]]

no puedo alcanzarlo, voy a borrar el servicio y hacer un manifiesto para el deploy:

![[Pasted image 20240228134101.png]]

![[Pasted image 20240228134156.png]]

exponiendo el pod fuera del cluster:

![[Pasted image 20240228134415.png]]

![[Pasted image 20240228134834.png]]

![[Pasted image 20240228135114.png]]


aplicando las reglas de reverse proxy:

![[Pasted image 20240228151738.png]]


![[Pasted image 20240228151900.png]]

![[Pasted image 20240228151933.png]]

chequeando los logs:

![[Pasted image 20240228152354.png]]

==TROUBLESHOOTING:==
=
este enfqoue es totalmente erroneo debido a que:
1. el nginx nunca va a poder rutear algo que esta fuera del cluster, simplemente no tiene sentido
2. el frontend esta fuera del alcance del proxy puesto que nunca va a poder reotientar comunicacion del root hacia el frontend
3. este enfoque se deja deprecado hasta que las aplcciones esten en el cluster