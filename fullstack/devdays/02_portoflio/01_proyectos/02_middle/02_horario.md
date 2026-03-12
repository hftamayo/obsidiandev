## Projectos middle

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
- CloudWatch dboard update:

 ```
 Suggested expansions:

- EKS control plane

- API server 5xx, throttling, latency (via CloudWatch Container Insights if enabled)

- Cluster status widget (link to EKS console)

- Nodegroup/EC2

- ASG desired/current/healthy capacity

- Instance CPU/credit balance (if burstable)

- Load balancers

- ALB/NLB: request count, target 5xx/4xx, target response time (P95), unhealthy targets

- VPC/network

- NAT GW bytes, error count

- VPC Flow Logs Insights link and top talkers query shortcut

- Storage

- EBS volume burst balance, throughput, queue length (for Prometheus storage if using EBS)

- CloudWatch Logs

- Links to /aws/eks/<cluster>/cluster and any app log groups

- Logs Insights saved queries (errors in last 15m, 5xx by path)

- Alarms overview

- Alarm status panel listing critical alarms (EKS API, NLB unhealthy, ASG capacity, NAT errors)

If you want, I can:

- Add these widgets to the Terraform 03_monitoring module’s dashboard definition.

- Enable Container Insights (if not already) to surface EKS metrics in CloudWatch.

- Create Saved Queries for Logs Insights and link them from the dashboard.
 
 ```

### Status:

| #   | Proyecto      | Next                                                                                                                                                                                                                                                                                         |
| --- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | absences-be   | -mejorar el code coverage del core business<br>- code a new domain using TDD<br>- no confiar directamente en lo que manda el frontend para no exponerse a broken access control<br>- implementar rate limit<br>- como gestionar los SSRF<br>(por favor chequear el mapa abajo de esta tabla) |
| 2   | absences-fe   | - unit testing<br>- playwright<br>- como evitar CSRF                                                                                                                                                                                                                                         |
| 3   | loggerservice | crear el scaffold                                                                                                                                                                                                                                                                            |
| 4   | rptservice    |                                                                                                                                                                                                                                                                                              |
| 5   | auth          |                                                                                                                                                                                                                                                                                              |
| 6   | devops        | probar si me funciona el deploy iac, dejarlo 25 minutos y luego un destroy iac<br><br>#3: CI pipeline (unit + integration tests)<br>#4: CD to cloud/K8s + post-deploy smoke tests<br>                                                                                                        |

---
## Short answer

**Build the rest of the CRUD first, but do it with a “quality-by-default” checklist baked into each new endpoint.**

So the order I’d recommend is:

1. **Finish the missing CRUD**
2. **Apply basic input validation/sanitization as you build it**
3. **Add/complete coverage checks around the new CRUD**
4. **Introduce rate limiting**
5. **Review SSRF risk where the app performs outbound calls**
6. **Then tighten the rest with deeper hardening/refactoring**

That gives you delivery **and** avoids building features that you’ll have to partially rewrite later.

---

## Why I would not postpone CRUD until all quality tasks are done

Because not all “quality” items have the same dependency relationship with CRUD.

### Some quality tasks should be done **during** CRUD

These are cheap to add early and expensive to retrofit later:

- **Request validation**
- **DTO boundary discipline**
- **basic input normalization**
- **tests for expected/invalid payloads**
- **consistent error handling**

If you skip these now, every new endpoint becomes future cleanup work.

### Some quality tasks can safely come **right after** CRUD

These are usually cross-cutting and can be layered in once the endpoint surface is clearer:

- **coverage measurement/reporting**
- **rate limiting**
- **security review for SSRF**, if outbound HTTP/file fetch behavior exists
- **broader abuse-protection policies**

These benefit from knowing what routes and use cases actually exist.

---

## My recommended strategy

## Option A — best practical choice

### **Feature-first, with mandatory guardrails**

For each missing CRUD operation, implement it with this minimum bar:

- request DTO validation
- domain/service-level business rules
- controller tests
- service tests
- integration test for happy path + key failure paths
- basic malicious/invalid input cases

This is usually the sweet spot.

You avoid two bad extremes:

- **“Ship features now, fix quality later”** → technical debt party
- **“Perfect the foundation before adding features”** → analysis paralysis in a suit

---

## Suggested execution order

## Phase 1 — Finish the remaining CRUD

Do this first because it is your functional baseline.

While implementing each endpoint, include:

- **bean validation** on incoming requests
- strict field constraints:
    - required vs optional
    - max lengths
    - allowed characters where relevant
    - enum/whitelist validation where applicable
- reject unknown/invalid shapes if your API contract requires that
- keep request DTOs separate from persistence/domain entities
- test valid + invalid + edge-case payloads

### Why this matters

If CRUD is incomplete, every later task is being applied to a moving target.

---

## Phase 2 — Check coverage immediately after CRUD lands

Once the CRUD surface is complete, evaluate:

- what lines/branches are uncovered
- which business rules are untested
- whether error paths are missing tests
- whether controllers are only tested for happy path

### Why not do this before CRUD?

Because your coverage number before the missing CRUD exists is not very useful.  
It’s like weighing a cake before you finish baking it.

Coverage is most helpful **once the main feature set is present**.

---

## Phase 3 — Harden input handling

You mentioned “sanitize input data” with a zero-trust mindset. That is good, but I’d phrase it carefully:

### Prefer this order:

1. **Validate**
2. **Normalize**
3. **Sanitize only when needed for a specific sink**

That distinction matters.

### Examples

- For names, codes, emails, IDs:
    - validate format/length
    - normalize whitespace/case if appropriate
- For free text that may later be rendered in HTML:
    - output-encode or sanitize for that rendering context
- For SQL:
    - use parameterized queries/JPA, not string concatenation
- For logs:
    - avoid logging raw user input blindly
- For file paths/URLs:
    - strict allowlists and parsing rules

### Important note

“Sanitize everything” sounds safe, but can become fuzzy and inconsistent.  
A stronger model is:

- **accept only expected input**
- **reject unexpected input**
- **encode/sanitize at dangerous sinks**

That’s a more solid zero-trust approach.

---

## Phase 4 — Rate limiting

Rate limiting is valuable, but it doesn’t usually need to block CRUD development unless:

- you are already exposing the app publicly, and
- abuse risk is immediate, especially on auth/search/heavy endpoints

### Prioritize rate limiting for:

- login/auth
- password reset / verification endpoints
- expensive search/list endpoints
- bulk operations
- endpoints that can enumerate data
- endpoints callable anonymously

### Why after CRUD?

Because rate limiting rules are easier to define well when the endpoint set is mostly complete.

---

## Phase 5 — SSRF assessment

This one depends on whether your app makes **outbound requests**.

### If your app does **not** fetch external URLs / webhooks / files / remote resources

Then SSRF is probably **not a current priority**.

### If your app **does** accept or construct URLs for outbound access

Then SSRF jumps much higher in priority.

Typical SSRF-risk situations:

- user supplies a URL to fetch
- webhook callback testing
- avatar/document import from URL
- PDF/image generation from remote resource
- integrations with arbitrary endpoints

### If SSRF is relevant, defend with:

- allowlists of domains or hosts
- block private/internal IP ranges
- block localhost and link-local
- resolve and verify DNS carefully
- restrict schemes to `http/https` if needed
- disable redirects or re-validate after redirects
- set short timeouts
- restrict ports
- don’t let users control raw internal destination URLs

So: **check whether SSRF is even applicable before spending much effort there**.

---

## What I would do in your situation

## Recommended plan

### **Step 0**

Finish the missing CRUD.

### **But while doing it, require these non-negotiables**

- request validation
- clean DTO boundaries
- tests for invalid input
- consistent exception/error responses

### **Immediately after CRUD**

- measure coverage
- close meaningful test gaps
- add rate limiting
- assess SSRF based on actual outbound integrations
- then do a focused hardening pass

---

## A practical rule for prioritization

Use this matrix:

### Do **now during CRUD** if the task is:

- cheap to add incrementally
- hard to retrofit
- directly tied to request/response design

That means:

- validation
- normalization
- error handling
- tests

### Do **after CRUD** if the task is:

- cross-cutting
- policy-oriented
- easier to tune once endpoints exist

That means:

- coverage reporting
- rate limiting
- broader security reviews like SSRF

---

## My final recommendation

## **Do not pause feature delivery to complete every quality task first.**

Instead:

## **Finish the remaining CRUD on top of the current codebase, but implement each piece with baseline quality/security controls from day one.**

In one sentence:

> **Feature completion first, but never “feature now, quality later”; do “feature with guardrails,” then add the broader cross-cutting protections once the CRUD surface is complete.**

---

## If you want, I can help you turn this into a concrete next-step plan such as:

- a **priority backlog order**
- a **definition of done** for each CRUD endpoint
- a **security checklist** for validation/rate limiting/SSRF in your Spring app
- or a **week-by-week implementation plan** based on your current project structure

---

| #   | Proyecto         | Done | Next                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| --- | ---------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | boabsenses jsb   |      | Reqs para el inputValidator:<br>Consider separating concerns:<br>Keep basic input validation in validators<br>Move business validation (like uniqueness checks) to service layer<br>Add more validation rules (email format, length limits, etc.)<br><br>- Replicar modelo a OUType<br>                                                                                                                                                                                                                                                                                                                                                                                            |
| 2   | boabsenses react |      | - Testing del nuevo codebase<br><br>- codear el microfrontend del dashboard<br><br>- ajustar el digester de errors para los MFEs<br><br>-Integration testing de Companycontainer using Jest+React Testing Library<br><br>1. continuar con la dinamica de TDD+refactor<br>3. repository pattern (I like this suggestion)<br>4. react query hooks for caching, background updates<br>5. optimistic updates<br>6. error boundary integration<br>7. validation layer using zod                                                                                                                                                                                                         |
| 3   | devops           |      | - migrar la pipeline para abcenses                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 4   | reacttodo        |      | - integration testing con los containers                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 5   | vanillatstodo    |      | -verificar costos y todo el flow de la pipeline<br>- codear una pipeline que incluya: create IAM role, setup OIDC provider para el github actions, configurar la trust policies, creat el bucket S3 para el terraform state<br>-especializarse en administracion de grafana y de eks<br>- aplicar al cluster el plan de HPA<br>- elaborar un dashboard de verdadero uso<br>- continuar con el plan de getting in production pero usando otro proyecto: integrar vanillatstodo+goresttodo+pg<br><br><br>- GitHubActions: IAM+IaC+CSI driver+Monitoring<br>- ArgoCD: deploy codebase<br>- DevSecOps: SAST, DAST impacta a codebase, en segundo plano algunas medidas para el IAC<br> |

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


### Reorganizar

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
