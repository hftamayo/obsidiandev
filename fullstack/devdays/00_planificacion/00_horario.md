
## Ordernar

nodetodo: MVC arch, ready for testing
nodesotiria: decorators+CQRS pattern, early development stage

reactodo: features arch, ready for testing
reactsotiria: crud para soportar de manera generica cualquier domain, necesita cliean architecture

javaspringtodo: multi layer arch, ready for testing


Objetivo de los proyectos backend:

- good security
- it’s maintainable 
- it’s scalable 
- it’s deployable 

Race conditions: restablecer password y cuentas, OTP y login workflow

hacer un endpoint para activarUsuario y para bloquearlo

- agregar un captcha durante login
- activacion de cuentas usando correo electronico y loggeo enviando codigo al correo
- aplicar el uso de strype

Recommendations:

Error Handling:
Implement exception handling to gracefully handle potential errors during authentication or registration.
Return appropriate error responses with informative messages (e.g., 400 for invalid input, 401 for authentication failure, 500 for server-side errors).
Input Validation:
Validate user input data in both login and register methods to prevent invalid data from entering your system.
Use Spring's validation framework or manual validation techniques.
Security Considerations:
Ensure proper password hashing and storage mechanisms in your AuthService.
Consider input sanitization and additional security measures to mitigate vulnerabilities.
CSRF Protection:
If applicable, implement CSRF protection for the /register endpoint, especially if accepting form-based submissions.
Testing:
Write comprehensive unit tests to verify the controller's behavior under various scenarios, including both successful and unsuccessful login and registration attempts.
Additional Considerations:

Logging: Consider logging essential events (successful logins, registration attempts, errors) for auditing and troubleshooting purposes.
Documentation: Clearly document the expected request and response formats for each endpoint to guide API consumers.


## Mayo 2025:
### ==Backend:
- jsbtodo y nodetodo: unit testing, puesta a punto, graphql version, aun monolitos

- goresttodo:
1. **Unit Testing**
    
    - Start with core task functionality
    - Test middleware (especially CORS)
    - Test error handling
    - Mock dependencies
2. **Decoupling Services**
    
    - Move health check to separate package
    - Create dedicated error logging service
    - Isolate seeding functionality
    - Define clear interfaces

### Recommended Additional Steps

3. **Infrastructure Improvements**
    
    - Implement configuration management
    - Enhance logging system
	- Add metrics collection
    - Improve error handling patterns
    - Document API endpoints
4. **Quality Assurance**
    
    - Add integration tests
    - Implement CI/CD pipeline
    - Add code coverage reporting
    - Set up linting rules
    - Add API documentation (Swagger/OpenAPI)
5. **Production Readiness**
    
    - Add graceful shutdown
    - Implement proper connection pooling
    - Add rate limiting
    - Improve caching strategy
    - Set up monitoring

### Priority Order

1. Unit tests first - ensures reliability
2. Decoupling - improves maintainability
3. Infrastructure - strengthens foundation
4. QA - ensures quality
5. Production readiness - prepares for deployment

### nodesotiria:

1. **Current Stage - Infrastructure and Basic Setup**:
   - ✅ Environment configuration
   - ✅ Database setup with Prisma
   - ✅ CQRS pattern implementation
   - ✅ Basic health check endpoints
   - ✅ Decorator pattern for logging

2. **Next Steps - API Layer Improvements**:
   - DTOs for standardizing API responses
   - Request validation
   - Error handling middleware
   - Rate limiting
   - API documentation (Swagger/OpenAPI)

3. **Authentication & Authorization**:
   - JWT implementation
   - Role-based access control
   - Password hashing
   - Session management

4. **Caching & Performance**:
   - Redis integration
   - Response caching
   - Rate limiting implementation
   - Query optimization

5. **Business Logic**:
   - User management
   - Role management
   - Task management
   - Business rules implementation

6. **Testing**:
   - Unit tests
   - Integration tests
   - E2E tests
   - Test coverage reporting

7. **Advanced Features**:
   - Logging system
   - Monitoring
   - CI/CD setup
   - Docker containerization

Would you like to proceed with any specific area from this roadmap?


### ==Frontend:

- terminar el proyecto de reactsotiria
- reacttodo: microfrontends: dashboard, settings, tasks, roles, users, rpt

Suggested Order
Complete core functionality
Implement auth workflow
Stabilize existing features
Plan micro frontend architecture
Begin gradual migration

Why Wait?
Better understanding of system boundaries
Clearer separation of concerns
More stable core functionality
Reduced risk of over-engineering
More informed architectural decisions

Plan:

Short tem:
1. Complete TaskBoard Functionality
- Finish task update workflow
- Implement task filtering/sorting
- Add pagination support
- Improve error handling
- Complete task state management
- Add loading states and optimistic updates

2. Implement Testing Strategy
Write unit tests for:
- Core components (TaskBoard, TaskRow, AddTaskForm)
- Custom hooks (useTaskBoard, useTaskMutations)
- Services (taskService, apiClient)
- State management: Add integration tests for task workflows
- Set up CI pipeline for tests

3. Code Decoupling
- Create separate feature modules for: Dashboard, Settings, Health Check
- Implement proper routing
- Extract shared components
- Create independent state management
- Separate API services

Benefits of This Approach:
Solid foundation before scaling
Better testability
Cleaner architecture
Easier to maintain
Ready for future microservices


## 2025:

### Enero:
- ~~examen github foundations
- curso de open tofu: https://trainingportal.linuxfoundation.org/learn/course/getting-started-with-opentofu-lfel1009/course-introduction/course-information
- curso free de go con goland: https://www.bytesizego.com/mastering-go-with-goland?subscriber=4dd04721d38a3f1e88672bee7f606f0d967e6af54efe6280642eefd78afb7228
- 

==Pendiente de leer Enero:
- https://geovannycode.com/en/hexagonal-architecture-flexible-software/
- https://geovannycode.com/integracion-de-stripe-con-spring-boot-y-vaadin/
- https://dev.to/1shubham7/step-by-step-guide-to-implementing-jwt-authentication-in-go-golang-2e24
- https://vladmihalcea.com/the-best-way-to-lazy-load-entity-attributes-using-jpa-and-hibernate/
- https://vladmihalcea.com/spring-jpa-dto-projection/
- https://konradreiche.com/blog/two-common-go-interface-misuses/
- https://curiosum.com/blog/bringing-solid-to-elixir
- https://dev.to/indrayyana/building-a-restful-api-with-go-fiber-an-express-inspired-boilerplate-3713
- 



### entrenar al algoritmo de linkedin para mostrar preferencias en las siguientes tecnologías:

- scala
- jsb
- golang
- kotlin
- info o tutoriales sobre devops


## Internships y empleos:

* Node.JS + React.JS
* Java

### Aprendizajes:

- Golang
- Scala
- Kotlin
- RoR


### calendario de ejecucion:


| #   | Proyecto                         | 2024                                           | 2024                                                                                    | 1Q 2025                                                                                                                        | 1Q 2025   | 2Q 2025 | 2Q2025    |
| --- | -------------------------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | --------- | ------- | --------- |
|     |                                  | Meta                                           | Resultado                                                                               | Meta                                                                                                                           | Resultado | Meta    | Resultado |
| 1   | Nodetodo                         | test unitarios y de integracion finalizados    | Test unitarios finalizados, algunos con bugs                                            | **unstable**: unit tests sin bugs / integration test / performance test<br><br>**experimental**: agregar RBAC, race conditions |           |         |           |
| 2   | buddyman_<br>node_pg<br>-graphql | pg y micro mongo en experimental               | pg finalizado                                                                           |                                                                                                                                |           |         |           |
| 3   | jsbtodo                          | que corra y conecte sin problemas              | depurando login                                                                         | **experimental**: API finalizada<br><br>**unstable**: unit / integration testing                                               |           |         |           |
| 4   | reactsotiria                     | crud finalizado e integrado                    | working on navButtons which are parts of the crud footer                                | **experimental**: <br>- crud funcional<br>-rutinas de salidas de datos                                                         |           |         |           |
| 5   | reacttodo                        | funcional y con tests de integracion y con e2e | 0.1.0 en pre-release lista para expandirse en experimental y unstable                   |                                                                                                                                |           |         |           |
| 6   | android                          | tutorial finalizado y app depurada             | proyecto restrasado                                                                     | aplicacion finalizada, funcionando y comprendida                                                                               |           |         |           |
| 7   | laserants-ecommerce              | deployar en el EKS                             | proceso delimitado y clarificado en bare metal, hay 2 pasos que se ejecutan manualmente |                                                                                                                                |           |         |           |
| 8   | Bug Bounty                       | Seleccionar 3 vulnerabilidades backend         | command injection, race conditions, business logic, sqli, nosqli                        | Desarrollar habilidades teoricas y practicas en command injection, race conditions, business logic, sqli, nosqli               |           |         |           |
| 9   | goresttodo                       |                                                |                                                                                         | version graphql                                                                                                                |           |         |           |
| 10  | nodegraphqltodo                  |                                                |                                                                                         | basado en la funcionalidad de nodetodo                                                                                         |           |         |           |

* Todos los proyectos de desarrollo deben contar con: CI/CD pipeline para implementacion on premise y en cloud

==periodo del 21/12 al 03/01/2025:

- micro datalayer: finalizar el template para micro con mongo
- jsbtodo: debuggear el proceso de signup y login
- kotlintodo: finalizar el tutorial
- laserants-ecommerce: deployar en EKS+github actions

- Influencers infosec: farah hawa, john hammond, james kettle, jason haddix, lupin y su grupo, infosecau (shubs)

### Lunes a Viernes: 

1. Todo (monolito+Rest):
	1. FrontEnd: 7:45-8:30
	2. BackEnd: 8:40-9:25
2. JsbTodo (monolito+Rest):
	1. BackEnd: 9:35 - 10:20
3. ReactSotiria (monolito+Rest):
	1. FrontEnd: 10:40-11:25
	2. BackEnd:11:35-12:20
4. DevOps:
	1. AZ-400: 12:30-13:15
	2. almuerzo
	3. practica: 13:45-14:30
5. DFIR: tryhackme path: 14:35-15:35
6. Kotlin:
	1. 15:45-16:00

7. Infosec:
	1. Lunes a Jueves: 
		1. 18:15 - 18:35 -> APISec UNi
		2. 18:45-19:30 -> Offensive / defensive
	2. 
	3. Viernes: maldev: tutoriales de ricardo narvaja ode cerberus

### Fines de Semana:

Sabado:
- Golang: 4:15-5:00
- Serverless y Lambda:  5 - 6:00

- Cracking the interview: 17 - 18:45
- DFIR tryhackme path: 18:45-19:45

Domingo:
- Golang: 4:15 - 5:00
- Lectura libre: despues del almuerzo
- Serverless: y Lambda: 17:45 - 18:30
- APISec Uni: 18:45 - 19:30

Pendientes:
* scala, RoR, 


