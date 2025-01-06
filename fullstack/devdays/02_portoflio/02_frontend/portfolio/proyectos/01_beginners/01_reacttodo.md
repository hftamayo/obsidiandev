
![[Pasted image 20241216154239.png]]

## Ficha Técnica:
* Version: 0.1.0
* fecha: 12-16-2024
* Stack técnico:

## Roadmap del FrontEnd hacia MVP (Minimum viable Product) 0.2.0:

### Unstable:
* Unit Testing de todo el frontend
* Integration Testing
* End 2 End Testing usando Playwright
* Performance Testing

### Experimental:
* Funcion de drag and drop en Tasks
* Resaltar en el menu la route que se esta desplegando
* signup/login/logout
* Dashboard con indicadores
* Reportes predefinidos exportables a json, xls, pdf
* Dark mode
* microfrontends
* componente de health check
* rutina de errores conectada con el backend
* internacionalizacion
* mejorar el menu contextual del usuario e incluir un avatar
* aplicacion de Patrones de diseño
* skeleton loading component para mientras diseño el dashboard
* verificar si necesito tener algun patron previo a implementar dark mode


### DevOps:
* CI/CD Pipeline para despliegue on premise: 
	* gitlab+jenkins+minikube+docker+monitoring on Debian
	* gitlab+jenkins+openshift+containerd+monitoring on RHEL
* CI/CD Pipeline para despliegue cloud:
	* Github+CircleCI+EKS+Terraform+Monitoring on AWS
	* Github+Github Actions+AKS+Terraform+Monitoring on Azure

### BackEnd:

* Event driven architecture
* MongoDB -> Mongoose
* RDBMS -> Prisma / typeORM