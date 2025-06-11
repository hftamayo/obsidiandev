## Junio 2025:
### ==Backend==:

### JsbTodo:
- correr los test para publicar el release
- integrar todas las funcionalidades de goresttodo 0.3.6

### NodeTodo:
- idem

### Nodesotiria:


### GoRestTodo:
- Clean code
- Create dedicated error logging service
- integration testing
- Add graceful shutdown
- Implement proper connection pooling
- Set up monitoring


## ==Frontend==:

### Todo:
- cleaning code
- Testing de la release 0.1.1

### Sotiria:
- release de la version 0.1.0

_______
## Mayo 2025:
### ==Backend:
- jsbtodo y nodetodo: unit testing, puesta a punto, graphql version, aun monolitos

- goresttodo:

1. **Decoupling Services**
    
    - Move health check to separate package
    - Isolate seeding functionality
    - Define clear interfaces

### Recommended Additional Steps

3. **Infrastructure Improvements**
    
    - Implement configuration management
    - Add metrics collection
    
4. **Quality Assurance**
    
    - Add integration tests
    - Implement CI/CD pipeline

5. **Production Readiness**
    
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

## 2025:

### Enero:

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

### calendario de ejecucion:


| #   | Proyecto            | 2024                                   | 2024                                                                                    | 1Q 2025                                                                                                          | 1Q 2025   | 2Q 2025 | 2Q2025    |
| --- | ------------------- | -------------------------------------- | --------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------- | ------- | --------- |
|     |                     | Meta                                   | Resultado                                                                               | Meta                                                                                                             | Resultado | Meta    | Resultado |
| 7   | laserants-ecommerce | deployar en el EKS                     | proceso delimitado y clarificado en bare metal, hay 2 pasos que se ejecutan manualmente |                                                                                                                  |           |         |           |
| 8   | Bug Bounty          | Seleccionar 3 vulnerabilidades backend | command injection, race conditions, business logic, sqli, nosqli                        | Desarrollar habilidades teoricas y practicas en command injection, race conditions, business logic, sqli, nosqli |           |         |           |
