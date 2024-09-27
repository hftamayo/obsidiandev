en esencia en este metodo unicamente necesito el service principal, en esta imagen se lista que tengo ademas del SP el group y el registry:
![[Pasted image 20240916113636.png]]

verificar que la subscripcion este registrado con ACS:
```
az provider register --namespace Microsoft.ContainerInstance

az provider show --namespace Microsoft.ContainerInstance --query "registrationState"

Crear una subred:

az network vnet create --resource-group laserantsgroup --name laserantsvnet --address-prefix 10.0.0.0/16 --subnet-name lasersubnet --subnet-prefix 10.0.0.0/24

```

![[Pasted image 20240916131918.png]]


==y del lado de Azure eso es todo, ahorita tengo que crear el secret del AZURE_CREDENTIAL de este SP:

![[Pasted image 20240916120855.png]]

PR para deployar el action a Unstable branch:

![[Pasted image 20240916120828.png]]

after de un par de ajustes:

![[Pasted image 20240916124642.png]]

despues de algunos ajustes:
![[Pasted image 20240916130654.png]]


## INTERACCION ENTRE LOS CONTENEDORES:

### Data Layer: pgecommerce

- la imagen de la DB contiene además del runtime de pg, un script db.sql que crea la base de datos y el usuario
- es necesario que el contenedor de la aplicacion esten en la misma red para que puedan resolverse
- el container ==no necesita leer el env file del backend puesto que el script db.sql es ejecutado en el init process por la imagen, per se, esto puede significar un agujero de seguridad ==
- por tanto el container puede crearse asi:

```
docker run --name pgecommerce -p 5432:5432 hftamayo/pgecommerce:0.0.1

```

### Monorepo: 