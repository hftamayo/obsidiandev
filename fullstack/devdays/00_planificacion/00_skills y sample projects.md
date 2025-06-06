
# ==soft skills:== 

- problem solving
- attention to details
- work independently, few supervision
- manage multiple priorities
- saber manejarse en un fast paced environment
- terminar los proyectos (no dejar nada a medias en todo el ciclo)
- siempre poner atencion a los reqs y features del cliente
- no bugs

# ==tech skills:

## ==arquitectura y otros skills que aplican a cualquier tecnologia:==

- TDD y BDD
- microservices, graphql, federated graphql, restful
- code best practices: SOLID, code smells, refactoring
- aplicar el OWASP Top 10
- Scaling applications
- frontend: microfrontend, dashboards

## ==areas de aplicacion de los backend==:

- logistica?, dashboard?, ERP?, CRM?, PoS super, retail, pyme?, payment?

- Java: banking, microcredits, fintech, aseguradoras
- Node: retail on line, ecommerce, online entretainment
- Scala: data engineering
- RoR: back office
- Go: back office, facturacion electronica, cualquier cosa escalable (core de microservicios)
- ejercicios en general para backend: https://projects.masteringbackend.com/projects


## ==Perfiles de lenguajes de programacion targets segun Jetbrains Ecosystem

Rust:

Rust for web backends, que es WebAssembly
Plugins: Rust analyzer, Intellij Rust
VsCode
Build system: cargo
Uso de Rust: CLI Tools, Systems Programming, web development

Ruby:

version mas usada: 2.7 y 3.1 -> un resto de proyectos legacy
la idea es migrarse a la 3.2
gestor de versiones: rbenv
gem management tool: bundler
version mas usada de RoR: 6.0
App server en produccion: Puma
IDE: RubyMine
Unit testing framework: RSpec
Code quality: Rubocop
type specification tool: Yard

Scala:
version mas usada: 2.13
compilation target: JVM
unit test framework: ScalaTest
framework para web development: http4s, Akka
framework de uso general: Cats, Akka
IDE: IntelliJ
build system: sbt
scala environment: sbt console
tools: scalafmt
la mayoria de los proyectos estan tratando de migrar a Scala 3
features de Scala mas usado: Enums, extension methods, given instances, union


## ==bases de datos target:
* SQL: PG, MySQL
* NoSQL: MongoDB, Cassandra
* Caching: Redis

## ==FrontEnd:
- React JS: Redux + Express, Shadcn, Zod, React Query, Zustand (para proyectos pequeños)
- Kotlin:
- React Native:
- CSS Frameworks: Tailwind, Material UI
- Build pipeline and tools: Vite, Babel
- Testing: Jest, React Testing library, vite-test
- Next.JS?
- Librerias recomendadas por Midu:
	- Slider -> flicking
	- Router -> wouter
	- graficas -> recharts
	- Drag n Drop -> DnDkit
	- Estilos -> tailwind
	- autenticacion -> auth.js
	- tablas -> tanstack table
	- notificaciones -> sonner
	- estado global -> zustand
	- lista  virtual infinita -> virtua
	- modales -> react aria modal
	- componentes UI -> shadcn UI
	- animaciones -> framer motion
	- formularios -> react hook form o tanstack form
	- data fetching -> tanstack query
	- internacionalizacion -> react-i18next
	- 

## ==Backend

### Node.JS:
- typeORM o PrismaORM
- Express
- - Experience with Node.js development: our usual stack is based on Node.js, Express/Nest.js, MongoDB/Mongoose or TypeORM/PostgreSQL.


## ==Devops

### DevOps Engineer:
* Terraform: para beginners
* Pulumi: el siguiente nivel de terraform pues soporta python, go, etc

## SAMPLE PROJECTS:

- bagels NYC: https://www.utopiabagelsny.com
- the farmers dog: https://www.thefarmersdog.com
- demo de un carrito de compra: https://www.linkedin.com/feed/update/urn:li:activity:7238197683927085056/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7238197683927085056%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29
- App de recetas: https://www.linkedin.com/posts/moises-prado_openia-typescript-nestjs-activity-7236491513344512000-8wOV?utm_source=share&utm_medium=member_desktop
- Gusto Payroll App: https://gusto.com/

## Proyectos sugeridos para codear:

==Estos productos aportan al usuario: nivel de satisfaccion, usabilidad, utilidad,

- Design a Parking Lot
- Design an elevator system
- Design API rate limiter
- Design a logging system
- Design a hotel management system
- Design a movie ticket booking system
- Desarrollo, implementación y venta de ERP ANYSOFTWARE, en Industrias, Droguerías, Farmacias, Minisúper, Ferreterías y empresas de servicio  -> erp del maestro luis mendoza
- Grubhub clon
- Autotrader clon
- App para encargar pupusas por adelantado
- app para reservar citas en peluqueria, fisioterapia, medica, verificar doctoralia mx

___
	
	1. Working with RESTful APIs
	
		Create APIs: Build RESTful APIs with Express.
		
		Handle Query Parameters: Parse URL parameters and query strings.
		
		Send JSON Responses: Format and send JSON data to clients.
	
	2. Middleware and Error Handling
	
		Middleware Basics: Use next() for request flow.
		
		Error Handling: Implement custom error-handling middleware.
		
		Logging: Use libraries like morgan for logging requests.
	
	3. Database Integration
	
		Connect to Databases: Integrate MongoDB (Mongoose), MySQL, or PostgreSQL.
		
		Perform CRUD Operations: Build database-backed routes for Create, Read, Update, Delete operations.
	
	4. Authentication and Authorization
	
		Authentication: Implement user authentication using sessions, cookies, or JSON Web Tokens (JWT).
		
		Authorization: Restrict routes to specific user roles.
	
	5. File Uploads and Static Files
	
		File Uploads: Use multer for handling file uploads.
		
		Serve Static Files: Use express.static() to serve images, CSS, and JavaScript files.
	
	6. Advanced Features
	
		CORS: Enable Cross-Origin Resource Sharing for APIs.
		
		Rate Limiting: Protect APIs from abuse using rate-limiting middleware.
		
		Real-Time Features: Integrate with WebSockets for live data.
	
	7. Testing and Debugging
	
		Unit Testing: Test routes using supertest and Jest or Mocha.
		
		Debugging: Use tools like node-inspect or debug library.
	
	8. Deployment
	
		Prepare for Deployment: Use environment variables and production-ready configurations.
		
		Deployment Platforms: Deploy on Heroku, Vercel, or AWS Elastic Beanstalk.
		
		Scaling: Optimize your app for performance and scalability.


### ==Perfil competitivo para Restful:
	### 1. **Advanced Database Management**:
	
	- **SQL and NoSQL Databases**: Deepen your knowledge of both SQL (e.g., PostgreSQL, MySQL) and NoSQL (e.g., MongoDB, Redis) databases.
	- **ORMs**: Gain proficiency in Object-Relational Mappers (ORMs) like Prisma, TypeORM, and Sequelize.
	- **Database Optimization**: Learn about indexing, query optimization, and database scaling techniques.
	
	### 2. **API Design and Development**:
	
	- **RESTful APIs**: Master the principles of REST and best practices for designing RESTful APIs.
	- **GraphQL**: Learn GraphQL for more flexible and efficient data querying.
	- **API Documentation**: Use tools like Swagger or Postman to document your APIs.
	
	### 3. **Security**:
	
	- **Authentication and Authorization**: Implement secure authentication (e.g., JWT, OAuth) and authorization mechanisms.
	- **Data Encryption**: Understand how to encrypt data in transit and at rest.
	- **Vulnerability Mitigation**: Learn about common security vulnerabilities (e.g., SQL injection, XSS) and how to mitigate them.
	
	### 4. **Testing and Quality Assurance**:
	
	- **Unit Testing**: Write unit tests for your code using frameworks like Jest or Mocha.
	- **Integration Testing**: Ensure different parts of your application work together correctly.
	- **End-to-End Testing**: Use tools like Cypress or Selenium to test the entire application flow.
	- **Test-Driven Development (TDD)**: Practice TDD to write tests before implementing functionality.
	
	### 5. **Performance Optimization**:
	
	- **Profiling and Monitoring**: Use tools to profile and monitor your application’s performance.
	- **Caching**: Implement caching strategies (e.g., Redis, Memcached) to improve performance.
	- **Load Balancing**: Learn about load balancing techniques to distribute traffic across multiple servers.
	
	### 6. **DevOps and CI/CD**:
	
	- **Continuous Integration/Continuous Deployment (CI/CD)**: Set up CI/CD pipelines using tools like Jenkins, GitHub Actions, or GitLab CI.
	- **Containerization**: Use Docker to containerize your applications for consistent environments.
	- **Orchestration**: Learn Kubernetes for managing containerized applications at scale.
	
	### 7. **Cloud Services**:
	
	- **Cloud Platforms**: Gain experience with cloud platforms like AWS, Azure, or Google Cloud.
	- **Serverless Architecture**: Explore serverless computing with AWS Lambda, Azure Functions, or Google Cloud Functions.
	- **Infrastructure as Code (IaC)**: Use tools like Terraform or AWS CloudFormation to manage infrastructure.
	
	### 8. **Microservices Architecture**:
	
	- **Service Communication**: Understand how to manage communication between microservices (e.g., REST, gRPC, message queues).
	- **Service Discovery**: Implement service discovery mechanisms.
	- **Resilience and Fault Tolerance**: Use patterns like Circuit Breaker and Bulkhead to build resilient microservices.
	
	### 9. **Logging and Monitoring**:
	
	- **Centralized Logging**: Implement centralized logging using tools like ELK Stack (Elasticsearch, Logstash, Kibana) or Prometheus.
	- **Application Monitoring**: Use monitoring tools like Grafana, New Relic, or Datadog to track application performance and health.
	
	### 10. **Soft Skills**:
	
	- **Problem-Solving**: Enhance your problem-solving skills to tackle complex technical challenges.
	- **Communication**: Improve your ability to communicate technical concepts to non-technical stakeholders.
	- **Collaboration**: Work effectively in team environments, using tools like Git for version control and collaboration.


### ==Perfil competitivo como GraphQL:
	### 1. **Advanced Database Management**:
	
	- **SQL and NoSQL Databases**: Deepen your knowledge of both SQL (e.g., PostgreSQL, MySQL) and NoSQL (e.g., MongoDB, Redis) databases.
	- **ORMs**: Gain proficiency in Object-Relational Mappers (ORMs) like Prisma, TypeORM, and Sequelize.
	- **Database Optimization**: Learn about indexing, query optimization, and database scaling techniques.
	
	### 2. **GraphQL API Design and Development**:
	
	- **GraphQL Fundamentals**: Master the basics of GraphQL, including queries, mutations, and subscriptions.
	- **Schema Design**: Learn how to design efficient and scalable GraphQL schemas.
	- **Resolvers**: Understand how to write resolvers to fetch data for your GraphQL queries and mutations.
	- **GraphQL Tools**: Familiarize yourself with tools like Apollo Server, GraphQL Yoga, and GraphQL.js.
	
	### 3. **Security**:
	
	- **Authentication and Authorization**: Implement secure authentication (e.g., JWT, OAuth) and authorization mechanisms in GraphQL.
	- **Data Encryption**: Understand how to encrypt data in transit and at rest.
	- **Vulnerability Mitigation**: Learn about common security vulnerabilities (e.g., SQL injection, XSS) and how to mitigate them.
	
	### 4. **Testing and Quality Assurance**:
	
	- **Unit Testing**: Write unit tests for your GraphQL resolvers and schema using frameworks like Jest.
	- **Integration Testing**: Ensure different parts of your GraphQL API work together correctly.
	- **End-to-End Testing**: Use tools like Cypress or Selenium to test the entire application flow.
	- **Test-Driven Development (TDD)**: Practice TDD to write tests before implementing functionality.
	
	### 5. **Performance Optimization**:
	
	- **Profiling and Monitoring**: Use tools to profile and monitor your GraphQL API’s performance.
	- **Caching**: Implement caching strategies (e.g., Redis, Apollo Client cache) to improve performance.
	- **Batching and Throttling**: Use techniques like DataLoader to batch and cache database requests.
	
	
	### 8. **Microservices Architecture**:
	
	- **Service Communication**: Understand how to manage communication between microservices (e.g., REST, gRPC, message queues).
	- **Service Discovery**: Implement service discovery mechanisms.
	- **Resilience and Fault Tolerance**: Use patterns like Circuit Breaker and Bulkhead to build resilient microservices.
	
	### 9. **Logging and Monitoring**:
	
	- **Centralized Logging**: Implement centralized logging using tools like ELK Stack (Elasticsearch, Logstash, Kibana) or Prometheus.
	- **Application Monitoring**: Use monitoring tools like Grafana, New Relic, or Datadog to track application performance and health.
	