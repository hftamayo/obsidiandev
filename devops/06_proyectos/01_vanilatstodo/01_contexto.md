- el repo lo forkie hacia mi cuenta htues de github
- aca setie los secrets relativos a AWS y al Docker Hub
- El workflow inicial será: AWS+Github Actions + Terraform desde un contenedor local que servirá de cliente

![[Pasted image 20250306145326.png]]


- ## Estructura de los yaml files:
	- ==deploy_infrastructure.yml== -> deploya IaC hacia el cloud provider
	- ==destroy_infrastructure.yaml== -> destruya la IaC provisionada
	- ==main.tf== -> contiene toda la provision de los artifacts, el trigger es deploy y el destructor es destroy_infrastructure
	- ==deploy-to-eks:== -> este es el archivo que será triggereado por github actions y que: descarga el docker container, lo deploya en el kubernetes y lo publica
- ## Esquema de usuarios cloud y pattern de claves
	- F