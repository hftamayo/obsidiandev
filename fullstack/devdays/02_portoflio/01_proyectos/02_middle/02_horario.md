lear## Projectos middle

### ToDo:
- UI reutilizable (theme)
- cada MFE debe tener su propio SDK para host integration
- healthcheck puede tener un cliente SDK para integracion con los hosts/remotes
- Contracts first: treat each shared MFE + SDK as a versioned product with clear public API (routes, events, methods, styles, auth handshakes).
  - Keep MFEs stable and backward compatible; evolve via semver and deprecation policies.
Otros patrones a considerar:
Repository Pattern para el data access, Observer pattern para real time updates, Stategy pattern for diff auth methods

### DevOps:
- incluir en el pipeline CI(DevSecOps): sonarqube (code quality), triby (image vuln scanner)
- migrar el CD hacia gitOps: ArgoCD (automatizar deploys con k8s), Helm para versiones manifests & packaging, monitores (grafana: metrics y dashboard)

### Status:

| #   | Proyecto         | Done | Next                                                                                   |
| --- | ---------------- | ---- | -------------------------------------------------------------------------------------- |
| 1   | boabsenses jsb   |      | roles entities, set de la relacion: ous, roles, employees,<br>luego excepciones y dtos |
| 2   | boabsenses react |      | probar si los UI controls <br>y tailwind funcionan                                     |
| 3   | reacttodo        |      | preparado para iniciar unit testing                                                    |
| 4   | vanillatstodo    |      | trabajar en el dashboard grafana                                                       |


___

### Roster de Core Business:
1. sistema de incapacidades focusITO:
	1. Back Office: Java Spring Boot
	2. Front Office: Node JS
	3. Data Layer: PostgreSQL
	4. API Protocol: Restful


2. el challenge de focusITO de frontend (consumir una API)

3. El challenge de applaudo sobre la personal agenda: golang+kotlin+restful

4. pulir el proyecto de Laserants junto e incorporar reqs del challenge de Kodigo -> CQRS+grapqhl (Cliente: NodeJS, BackOffice: Golang + ReactJS), Event Sourcing+web socket (logs y health check), saga pattern, circuit braker, event driven arch con solace y boomi, https, webhooks
5. usar volumenes en los docker datalayer


6. el challenge de applaudo del inventario: inicialmente solo la API en jsb+hibernate+restful

7. generar QRs en los proyectos que sea competente
8. plataforma de prestamos, cupones y descuentos
9. one to many form, dataTable
10. shorturl


## Backend:

1. integration tests
 
### Status:

| #   | Proyecto             | Done                                                  | Next                                                |
| --- | -------------------- | ----------------------------------------------------- | --------------------------------------------------- |
| 1   | nodetodo             | version 0.2.0-stable ready                            |                                                     |
| 2   | jsbtodo              | version 0.2.0-stable ready                            |                                                     |
| 3   | gotodo               | version 0.2.0 for testing                             |                                                     |
| 4   | reacttodo            | version 0.1.4 for testing                             |                                                     |
| 5   | reactsotiria         | version  0.0.1-main, <br>project halted               | please refer to <br>obsidiandev for <br>roadmap     |
| 6   | gologger<br>mservice | - comprender la version actual<br>- crear la metadata |                                                     |
| 7   | vanillatstodo        | GH pipelines no hardcoded <br>values                  | correr los actions <br>para saber que todo funciona |

## Features:

| Feature                                                     | Jsbtodo         | Nodetodo    | Gotodo |
| ----------------------------------------------------------- | --------------- | ----------- | ------ |
| input validation                                            |                 |             |        |
| rate-limiting                                               | X               | X           | X      |
| caching                                                     |                 |             | X      |
| Optimistic updates                                          |                 |             |        |
| versioning                                                  | X               | X           | X      |
| centralized error handling                                  | X               | 75%         | 80%    |
| grace failure                                               |                 |             |        |
| RBAC                                                        | X (hierachical) | X (bitwise) |        |
| pagination                                                  | X               | X           | X      |
| payload compression                                         |                 |             |        |
| encryption                                                  |                 |             |        |
| Single Entry point <br>(REST API Gateway)                   |                 |             |        |
| SSRF                                                        |                 |             |        |
| Unit Testing                                                | X               | X           | X      |
| Integration Testing                                         |                 |             |        |
| Full spec coverage                                          |                 |             |        |
| Injection dependency                                        | X               | X           | X      |
| HTTPS                                                       |                 |             |        |
| HealthCheck socket                                          |                 |             |        |
| logging                                                     |                 |             |        |
| Semantic paths                                              |                 |             |        |
| query language on pagination, <br>sorting and filtering     |                 |             |        |
| Enum de mensajes de error, <br>warning of success esperados |                 |             |        |
| Goroutines y channels, <br>goscheduler                      |                 |             |        |
| low latency, high availability                              |                 |             |        |
| Avoid record duplication by title/name                      |                 |             |        |
| 3rd party API integration                                   |                 |             |        |
| API testing against BOLA, <br>missconfigured JWT            |                 |             |        |
| Multifactor Auth, account lockout                           |                 |             |        |
| asegurarse de usar prepared statement                       |                 |             |        |
| payment processing                                          |                 |             |        |
| Design patterns: factory, singleton, <br>observer           |                 |             |        |
| Race conditions con giftcads y <br>transferencias de dinero |                 |             |        |
| backup y restore de la base de datos                        |                 |             |        |
| mongodb ip access list entry                                |                 |             |        |

### Frontend:

### Up next:

1. Aplicar theme en signup and login UI components
2. crear un mecanismo central de interaction handling (error, confirm, result)
3. Tanstack tables, los crud requieren del adapter pattern para el parseo entre las entidades y el modelo de tablas
4. trpc + react query -> data
5. date-fns -< date utils
6. nuqs -> search params
7. one to many form
8. probar otro design pattern para el frontend

| Feature                                                                           | ReactTodo | MobileTodo |
| --------------------------------------------------------------------------------- | --------- | ---------- |
| CORS policies                                                                     | X         |            |
| Auth+author(JWT handling, <br>token refresh, role based UI)                       |           |            |
| Error handling                                                                    |           |            |
| Security headers                                                                  |           |            |
| Performance optimization                                                          |           |            |
| State management                                                                  | X         |            |
| form validation                                                                   | X         |            |
| loading states (skeleton, spinners)                                               |           |            |
| Optimistic updates                                                                |           |            |
| Offline support <br>(service workers, cached data, <br>sync when online)          |           |            |
| Responsive (mobile first, <br>breakpoints, touch interaction)                     |           |            |
| accessibility (ARIA labels, <br>keyboard navigation, screen readers)              |           |            |
| code splitting (lazy loading, <br>dynamic imports, bundle opt)                    |           |            |
| API integration (request/response<br>interceptors, retry logic, caching)          |           |            |
| Realtime features (websockets, <br>polling, live updates)                         |           |            |
| File upload/download (drag/drop, <br>progress indicators, file validation)        |           |            |
| search & filtering (debounced search, <br>advanced filters, sorting)              |           |            |
| pagination & virtualization (<br>infinite scroll, virtual lists, performance<br>) |           |            |
| Theme & dark mode                                                                 |           |            |
| SEO optimization (meta tags, <br>structured data, SSR/SSG)                        |           |            |
| Security (XSS, CSRF, input sanitization)                                          |           |            |
| Unit Testing                                                                      |           |            |
| E2E testing                                                                       |           |            |
| Data visualization (<br>charts, graphs, interactive dashboards )                  |           |            |
| PWA                                                                               |           |            |
| Enforce boundaries <br>(ESLint rules to prevent cross-features)                   |           |            |
| Storybook for UI components                                                       |           |            |
| Feature flags for gradual rollouts                                                |           |            |
| Apollo client y URQL <br>(https://nearform.com/open-source/urql/docs/comparison/) |           |            |
|                                                                                   |           |            |
|                                                                                   |           |            |
|                                                                                   |           |            |
|                                                                                   |           |            |
|                                                                                   |           |            |
|                                                                                   |           |            |
|                                                                                   |           |            |
|                                                                                   |           |            |

### DevOps


| Feature                                                | Project |
| ------------------------------------------------------ | ------- |
| Blue-green                                             |         |
| canary                                                 |         |
| analisis estatico y dinamico <br>de codigo en pipeline |         |



## Lunes, Miercoles, Viernes

1. 7:45 - 8:30 am: ReactSotiria
2. 8:45-9:30 am: ReactSotiria
3. 9:45-10:30 am: APISecUniversity
4. 10:45-11:30: GoSotiria
5. 11:45-12:30: GoSotiria
6. 13:00-13:45: Github Actions
7. 14:00-14:45: JsbTodo
8. 15:00-15:45: JSbTodo
9. 18:00-19:30: PortSwigger


## Martes y Jueves:

1. 7:45 - 8:30 am: DevOps
2. 8:45-9:30 am: DevOps
3. 9:45-10:30 am: APISecUniversity
4. 10:45-11:30: GoSotiria
5. 11:45-12:30: GoSotiria
6. 13:00-13:45: Github Actions
7. 14:00-14:45:ReactSotiria
8. 15:00-15:45: ReactSotiria
9. 18:00-19:30: PEntesting


## Sabados:

1. 5:00 - 5:45 : nodesotiria
2. 6:00-6:45 : nodesotiria
3. 17:00-17:45:  pentesting
4. 18:00-18:45:  pentesting

## Domingos:
1. 5:00 - 5:45 : bounties
2. 6:00-6:45 : bounties

