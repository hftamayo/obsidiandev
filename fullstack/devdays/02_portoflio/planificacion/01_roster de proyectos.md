
## Caja de herramientas:

==directivas generales:;
- todos los micros poseen testing
- cumplen con principios SOLID y Clean Code
- TDD
- Los repos son privados e inician con el prefijo: tb_   (tb = toolbox)
- Codigo Javascript: Typescript

- Microservicio de Autenticacion:
	- Arquitectura Rest

- Microfrontend de Landing:
	- Landing para clientes
	- Landing para backoffice

- Microservicio de  BackOffice:
	- Gestion de Roles, Usuarios, verificacion de Logs
	- night mode
	
- Microservicio de Error Catching y log events:

- Microservicio de Searching especializada de data: el objetivo es que los usuarios avanzados puedan acceder a la data cruda y exportarla al micro de reporting

- Microservicio de opciones de configuracion

- Microservicio de backup y restore

- Microservicio de Dashboard:
	- Panel parametizable con los datos del cliente
	- mover el menu a la izquierda o la parte de arriba
	- mostrar opciones de menu desde una base de datos
	- acceso a opciones basado en roles
	- night mode
	
- Microservicio de Exportar a Excel y PDF:
	- Poder leer un yaml que trae el esquema del reporte
	- ensamblar la data con el yaml
	- formato de salida esperado: PDF/ XLS
	- Backend: Golang
	- Middleware: golang


## proyecto 1: Todo+Contacts+Budget -> nivel principiante

==Objetivo: construir un monolito Restful donde todos los elementos de la caja de herramientas este representado

==nombre de los repos:==
- tb_buddyman_fe
- tb_buddyman_be

==stack:==
- FrontEnd: 
	- React.JS
	- Tailwind
	- React Router
	- Redux
- BackEnd:
	- Node.JS
	- Express
	- RestfulAPI



0. todo:
	1. Backend:
		1. Node.JS
			1. Architecture: TDD y BDD
			2. Http Only Token
			3. Data Layer: MongoDB
			4. Typescript
			5. GraphQL y federated GraphQL
			6. Unit e Integration Test(Jest)
			7. Mecanismo central de logs
			8. Microservicios: Auth, Dashboard, BackOffice, Todos, Reportes
			9. pegarse a una api del clima y devolver al frontend
		2. Java Spring Boot
			1. Architecture: layered architecture y TDD
			2. Security package bien tuneado
			3. Hibernate
			4. Bearer Token
			5. Uso de clases DAO y DTO
			6. Data Layer: MySQL y Redis
			7. Micro servicios: AWS Lambda y Rabbit MQ
			8. Mecanismo central de logs
			9. Unit e Integration tests (JUnit)
			10. pegarse a una api de noticias tech y devolverla al frontend
		3. Golang
			1. Architecture: MVVN, Hexagonal, DDD
			2. Unit e integration tests con el proyecto goresttodo
			3. Data Layer: Postgres y Redis (frameworks Fiber y Gorm), DynamoDB (framework Gorm) y Cassandra
			4. Frameworks: Gin y Gorm junto con GraphQL con el proyecto gographqltodo
			5. uso de apache kafka, escalable, low latency
	2. Frontend:
		1. React.JS
			1. PoC #1: monolito usando Tailwind+Express+Redux
			2. PoC #2: monolito usando Next.JS
			3. PoC #3: microfrontends
			4. Para todos los PoC: SPAs, unit testing, customer acceptance testing, typescript, integracion de APIs del clima, dark theme, switch to diff languages, uso de Partials para New y Edit forms, no bugs en la consola, Landing page al estilo del landing de Discord, Direct messages entre usuarios, dashboard basicos, one to many CRUD, lookAndFeel tipo glossy de Windows, 
	3. Mobile:
		1. Kotlin:
			1. finalizar el tutorial de MVVN
			2. hacer una version adaptada y que se conecte a una API propia
	4. Api de Reportes:
		1. Lenguaje: golang
		2. exporta a PDF y XLS
		3. containerized
	5. Cloud:
		1. AWS
			1. Servicios que un dev tiene que conocer: EC2, S3, RDS, API Gateway, Lambda, IAM, CloudWatch, 
		2. migrar de on premise a cloud
		3. Terraform
	6. Certificaciones:
		1. Cloud: AWS Certified Developer Associate, AWS Solution Architect Associate, Azure Developer Associate
		2. Dev:  JSNAD, JSNSD
		3. DevOps: Certified Kubernetes Developer CKAD



2. Sotiria
	1. BackEnd:
		1. Rest:
			1. Java: Spring Boot, Rabbit MQ
			2. Rate Limiting
		2. Testing: End to End approach
			1. Unit Testing:
			2. Integration Testing:
		3. GraphQL:
		4. gRPC:
		5. CockRoachDB
		6. Redis
	2. FrontEnd:
		1. React: babel, webpack, Tailwind, Redux (vA) y Zustand (vB), funciones async/await, TypeScript, Jest para Unit Testing
	3. Mobiles:
		1. Kotlin: MVVN, Amplify, JetPack Compose para la UI
	4. DevOps:
		1. Docker
		2. Kubernete?
		3. GCP:
		4. AWS: Lambda, AWS API Gateway, IAM, KMS
			1. Deploys: NodeJS
		5. Azure:
		6. CI/CD:
	5. Microservices
	6. Architecture:
		1. TDD
		2. SOLID
3. Netflix Clon
	1. Pagos: Stripe
4. Youtube Clon / RealState Clon
	1. Pagos: Stripe, Crypto
5. Weather App:
	1. Web: microclimas
	2. Mobile: weather
6. Fitness / Recomendaciones de lugares y comidas, ofertas:
7. LetsOrder para emprendedores
8. ChatBot Con Rust
9. AkiPago para pagos e intercambio de dinero
10. Aplicacion para hacer encuesta y obtener datos (predictive)
11. applicacion movil para insitucion educativa:  horarios, grupos, inscripciones, comunidad, job posting, mensajes privados, alertas, cuadro academico



1. Sotiria: (React.JS + Node.JS)
Frontend: 
- dockerizar y hacer pruebas con el backend en el entorno dev

Backend: 
- verificar los outputs de los endpoints tanto en curl como en Postman por posible data expose
- homologar y unir las interfaces ubicadas en middleware/types con types/user.interface.ts
- METAS JUNIO - AGOSTO:
  * Unit Testing and Integration Testing con Jest
- METAS SEPTIEMBRE - NOVIEMBRE:
  * GraphQL Version
- PROYECTOS EN COLA:
  * Microservicio de Reportes
  * Microservicio de autenticacion
  * Microservicio de dashboard y acceso basado en roles
  * Microservicio de DataTable, Delete Record, Busqueda y Ordenamiento
  * Microservicio de Agregar/Actualizar data
    

1. Todo: (JavaSpringBoot)

Backend:
  - log de todas las operaciones, ejemplo: el usuario ADMIN123 activo a la cuenta de usuario 4, rafamurillo en datetime
  - function para verificar si hay conexion con el backend y decidir el mejor camino para finalizar la aplicacion e informar al usuario
 


  
2. YooP Encuestador:  
  Movil:
   - Operaciones del encuestador
   
  YooP BackOffice: (HTMX + Golang)
  Backend:
  
    
3. LetsOrder Cliente:
  Movil
  
  FrontEnd
  
  BackEnd
  
  
4. LetsOrder BackOffice:
  FrontEnd
  
  BackEnd
  
  
5. Business Mate: (micorservicios)
  FrontEnd
  
  BackEnd
  
6. PomodoroDev (golang o rust client con websocket)
   - medir productividad diaria del dev
   - registrar commits id por proyecto
   - verificar dias más y menos productivo para tomar medidas de apoyo al equipo
   - seguimiento a los sprints
   - registro de bugs o features asi como su estado
   
7. PomodoroDev BackEnd
   
8. Fintech (JSB + React+Next.JS)
    - pago de serivico, intercambio de dinero (estilo TigoMoney)

9. Self Service Ventas:
* aplicacion movil que permita escanear un codigo de un producto, consultar sus features y agregarlos a la canasta
* el usuario puede pagar con su medio de pago electronico deseado
* pasa a despacho y se lleva la compra
* puede hacer reeña de su experiencia
* puede pedir domicilio oretirar
* tamien recibirá cupones

10. e-inventario:
	1. escaneas un item y te genera codigo QR y ubicacion
11. K-Dev:
	1. llevan un listado de features segun la etapa del proyecto que esta trabajando
	2. agregar recordatorios y links por etapa del proyecto que esta en ejecucion
	3. version web y movil
	4. sustituye anotaciones desperdigados y recursos tecnicos utiles pero que son dificiles de tracear, tambien evito el uso de notas en whatsapp
	5. pomodoro incluido
	6. el PM puede monitorear productividad diaria y anotaciones

13.  DevOps
	1. On-Premise:
		1. deploy de una CI/CD usando un minikube y Jenkins en Debian
		2. deploy de una CI/CD usando openshift en RHEL
	2. Cloud:
		1. ... usando Terraform
		2. .... usando TravisCI
		3. .... usando CircleCI
		4. ... usando Github Actions
		5. ... usandi Azure DevOps
		6. ... manejo de creds usando Hashicorp Vault
		7. 
		8. ... usando Azure Boards for Project Management
			1. Servicios AWS demandados para un AWS DevOps: CloudFormation, OpsWorks, Elastic Beanstak, CloudWatch, Lambda, Fargate, ECS, EKS, IAM roles, , autoscaling de servicios, API Gateway, RDS, DynamoDB
    

# 02 : listado de proyectos para practicar:

## proyectos generales:
## junior :

- countdown timer -> virtual DOM
- Quiz App -> frontend
- recipe App, notes, Todo y contactos, movies -> fullstack 101
- password generator -> backend
- weather app -> fullstack, moviles 102
- Tic, tac, toe -> JS, kotlin

## middle:
- Chat App, link shortener -> ruby, go, elixir
- twitter bot, discord bot
- Web App microservice: https://semaphoreci.com/community/tutorials/building-go-web-applications-and-microservices-using-gin
- Scallable chat service -> rust
- sistema de facturacion electronica -> golang
- chatbot
- App de fidelidad de clientes tipo puntos leal o de las gasolineras puma -> Kotlin
- App del super selectos -> kotlin

## proyectos java:

- Sorting tool -> aplicar el uso de collections y operaciones con archivos
- budget manager
- cinema room manager: sell tickets, check available seats, view sales statistics

## proyectos kotlin:
- parking lot: 
- phone book: sort and search practice
- maze runner game
- simple chat bot
- currency converter

## MERN project:
- carnival gift shop
- carnival gift shop backoffice

## GO:
- cinema room manager
- obscene vocabulary checker
- in memory notepad
- Loan Calculator
- flashcards
- Simple chat bot




## 03: microservicios propios

### autenticacion

Registro de usuario.  
• Validación de registro de usuario con email.  
• Autenticación de usuario.  
• Restablecer contraseña con envío de email.  
• Cambiar contraseña desde link de email.  
• Obtención de perfil de usuario autenticado.

### dashboard