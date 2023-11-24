Dealing with differences:

1. En el proyecto de Spring los cambios al codigo los gestiono en la rama experimental, los testing en unstable
2. Hice un cambio en experimental, ejecute el PR y el Merge a Unstable
3. en mi equipo en la rama Unstable tenia 2 commits sin replicar a la nube
4. cuando hice git pull obtuve el mensaje que tenía diferencias y esto es muy claro ya que:
	1. se vienen los cambios que se arrastran de experimental
	2. hay cambios sin confirmar en la nube de los archivos locales de manera que cuando trato de hacer pull hay versiones distintas y ahi es donde genera la alerta

Para superar este impasse
1. antes de hacer un pull y un PR merge hacer push de mis cambios 
2. en caso que hice primero un PR merge y tengo cambios no confirmados puedo hacer un git pull --rebase
3. pull rebase hará:
	1. push de mis cambios locales
	2. pull de las actualizaciones que vienen el PR