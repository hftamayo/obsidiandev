
1. Instalar Docker CE en RHEL 7.9
	1. links para instalacion: https://computingforgeeks.com/install-docker-ce-on-rhel-7-linux/?expand_article=1
	2. ayuda para agregar el repo de docker a Red Hat:  https://www.scmgalaxy.com/tutorials/centos-issues-could-not-fetch-save-url-timeout-yum-remo/
	3. Por algun motivo la version de RH de la corte no permite conectarse al repo de docker-ce y al parecer es la configuracion del proxy:
	4. Configurar el proxy en RHEL: https://www.thegeekdiary.com/how-to-configure-proxy-server-in-centos-rhel-fedora/
	5. Asi que es un gran desmadre relacionado al curl que sale por medio del proxy luego certificados SSL que hasta deben publicarse en el firewall, asi que por el momento dejo en pausa la instalacion de Docker-CE
	
2. Container con Podman y Buildah
	1. podman for docker users: https://developers.redhat.com/blog/2019/02/21/podman-and-buildah-for-docker-users?extIdCarryOver=true&sc_cid=701f2000001OH7EAAW
	2. podman in rootless mode: https://fossies.org/linux/podman/docs/tutorials/rootless_tutorial.md
	3. deploying application on kubernetes: https://developers.redhat.com/topics/kubernetes
	4. podman compose or docker compose: https://www.redhat.com/sysadmin/podman-compose-docker-compose
	5. resolver el rollo del podman.socket en RHEL 7: https://access.redhat.com/discussions/6534351
	6. en la version 7.9 se llaman io.podman...
	![[Pasted image 20230921111623.png]]
	7. instalar python3 en RHEL: https://developers.redhat.com/blog/2018/08/13/install-python3-rhel  <- en este link tambien esta como habilitar un venv
			este comando me habilita python3:
			scl enable rh-python36 bash
 
	8. puesto que RH 7 instala la version 3.6 debo descargar pip desde aca: wget https://bootstrap.pypa.io/pip/3.6/get-pip.py
	9. python3 ./get-pip.py y luego python3 -m install --upgrade pip
	10. instalar pip: https://www.redhat.com/sysadmin/install-python-pip-linux
	11. desplegar la version de pip: pip3 -V
	12. antes de correr el podman compose ya debería estar activo el rootless, pero ya sea corriendolo o no me da un error del comando "podman network exists" y entiendo que eso es porque yo tengo un RH 7 y la version de podman-compose es muy vieja
	13. probando hace un downgrade segun sugerencia de aca: https://github.com/containers/podman-compose/issues/373
	14. al final el yaml que si me funciona obtuve esto:

 ![[Pasted image 20230921124622.png]]
 
 15. conclusiones iniciales:
	 1. el repo de imagenes del docker market no es el mismo y parece que no es accesible
	 2. aun usando podman la configuracion no furula (este yaml si funciona en docker)
	 3. la mayoría de configs debería hacerse como root y eso es inseguro
	 4. con razon no hay mucha documentacion sobre podman

4. 