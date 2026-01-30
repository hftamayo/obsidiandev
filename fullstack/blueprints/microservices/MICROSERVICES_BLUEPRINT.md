## Microservices Blueprint (v1) — gRPC + Kafka (default) + RabbitMQ (optional)

This blueprint is a practical guide for building **production-ready microservices** with:
- **gRPC** as the default synchronous internal protocol
- **Kafka** as the default asynchronous backbone (events + integration)
- **RabbitMQ** as an optional component for queue/worker patterns when Kafka is not a great fit

Structured flow: **contracts → communication → data → consistency → config → caching → resilience → ops/DoD**.

---

## 0) Principles & boundaries
- A microservice is **independently deployable**, owned by a single team, and aligned to a **bounded context**.
- Default architecture choices:
    - **REST at the edge**, **gRPC internally**
    - **Kafka for events** and async integration
    - **DB per service**
    - **Saga pattern** for multi-service workflows
    - **Resilience by design** (timeouts, retries, circuit breakers)

---

## 1) API Contracts (non-negotiable)
### 1.1 Contract-first artifacts
- **Public REST APIs**: OpenAPI
- **Internal APIs**: gRPC + Protobuf IDL
- **Async events**: schema-managed payloads (Avro/Protobuf/JSON Schema) + AsyncAPI (optional but recommended)

### 1.2 Compatibility rules
- **gRPC/Protobuf**
    - add fields as optional
    - never reuse field numbers
    - keep backward compatibility (clients may lag behind)
- **Events**
    - additive changes only
    - version schemas explicitly
    - maintain compatibility checks in CI

### 1.3 Contract testing
- CI gates for:
    - Protobuf breaking changes
    - Event schema compatibility
    - Consumer-driven contract tests where appropriate

---

## 2) Service frameworks (Java + Go)
### 2.1 Java: Spring Boot (+ selective Spring Cloud)
- Spring Boot: web, actuator, metrics, security, tracing integration
- Spring Cloud: use only what you need (config client, resilience integration, gateway patterns)

### 2.2 Go: “Spring analogue” toolkit
- HTTP (edge only) or gRPC server (`grpc-go`)
- Config: env-first + lightweight config loader (Viper optional)
- Observability: OpenTelemetry SDK
- Resilience: small libraries (retries/circuit breakers) or platform-level later
- Prefer explicit wiring over framework magic

---

## 3) Communication model (the core decision)
### 3.1 Synchronous: **gRPC (default for internal service-to-service)**
Use when:
- immediate response is required (queries, commands)
- strong typing and low latency matter

Rules:
- deadlines/timeouts on every call
- propagate `traceparent` + request IDs
- idempotency for command-style calls (see resilience section)

**REST at the edge**
- browsers and external clients talk REST/JSON
- API gateway/ingress can translate REST → gRPC (optional pattern)

### 3.2 Asynchronous: **Kafka (default backbone)**
Use Kafka for:
- domain events and integration
- sagas (choreography or orchestration triggers)
- audit/replay and rebuilding read models

Production rules:
- assume **at-least-once delivery**
- consumers must be **idempotent**
- use **outbox pattern** for publishing events from DB state changes
- define **partition key strategy** (usually aggregate ID like `orderId`)
- use DLQ/retry topics + replay procedure

### 3.3 Asynchronous: **RabbitMQ (optional, queue/worker patterns)**
Introduce RabbitMQ when you specifically need:
- classic **work queues** / worker pools
- complex routing patterns (topics/headers)
- broker-oriented message ACK workflows for job processing

Rules stay the same:
- idempotent consumers
- DLQ
- observability headers (trace/correlation)

**Blueprint rule:** Kafka-first. Add RabbitMQ only with a documented use case.

---

## 4) Async message/event standards (Kafka + RabbitMQ)
### 4.1 Message envelope (standard fields)
All async messages/events should include:
- `message_id` / `event_id`
- `type` (event name), `version`
- `timestamp`
- `producer` (service name)
- `correlation_id` / `trace_id` (for end-to-end tracing)

### 4.2 Delivery semantics + idempotency
- Design for **duplicate delivery**:
    - store processed message IDs (or use transactional/idempotent patterns)
    - make handlers safe to run twice

### 4.3 Failure handling
- bounded retries with backoff
- send poison messages to DLQ
- provide a replay/runbook (manual or automated)

---

## 5) Data architecture: DB per service
### 5.1 Rule
- Each service owns its data store and schema.
- No cross-service DB reads/writes.

### 5.2 Sharing data across services
- sync via gRPC for immediate reads
- async via Kafka events to build local read models (CQRS-style)
- avoid distributed joins; prefer replication via events

### 5.3 Migrations
- service-owned migrations
- backward compatible migrations where possible
- controlled rollout plan (expand/contract pattern)

---

## 6) Distributed transactions: Saga (default)
### 6.1 Why
No ACID across services with DB-per-service.

### 6.2 Patterns
- **Choreography** (event-driven): simpler start, can sprawl
- **Orchestration** (workflow coordinator): clearer control and observability

### 6.3 Required mechanics
- compensating actions
- timeouts
- idempotent steps
- persistent saga state
- audit trail (Kafka helps here)

---

## 7) Centralized configuration management
- Baseline env config: ConfigMaps/Helm values (non-secrets)
- Runtime toggles: feature flags (OpenFeature recommended as a standard interface)
- Secrets: external secret manager (Vault) + External Secrets operator pattern

---

## 8) Caching layers (latency + load relief)
- Edge caching/CDN for static content
- Service caching:
    - in-memory cache (fast, per-instance)
    - distributed cache (Redis-like) for shared caching
      Rules:
- define TTLs + staleness tolerance
- avoid caching sensitive data unless explicitly designed
- protect DB with cache-aside + request coalescing to avoid thundering herd

---

## 9) Resilience patterns (must be designed, not hoped for)
### 9.1 Timeouts (mandatory)
- gRPC deadlines everywhere
- bounded queues and connection pools

### 9.2 Retries (careful)
- retry only safe/idempotent operations
- exponential backoff + jitter
- cap retries to prevent retry storms

### 9.3 Circuit breakers + fallbacks
- fail fast when downstream is unhealthy
- fallbacks: cached data, degraded responses, async deferral, or clear error

### 9.4 Bulkheads
- isolate dependencies (separate pools/limits per downstream)
- prevent cascading failures

### 9.5 Rate limiting
- gateway-level limits (primary)
- optional in-service limits for expensive endpoints

---

## 10) Add-ons that make this “real production”
### 10.1 Identity & service-to-service auth
- External: OIDC/OAuth2 for users
- Internal: service identity (mTLS and/or JWT with short TTLs)
- Define what identity is propagated (user vs service)

### 10.2 Observability baked into the platform
- tracing propagation across gRPC + Kafka/RabbitMQ headers
- standard RED metrics per service
- logs include request_id + trace_id

---

## 11) Definition of Done (new microservice)
A service is production-ready when it has:
- Protobuf contract + compatibility checks in CI
- gRPC client/server timeouts and error handling
- Kafka integration uses outbox (when publishing from DB changes)
- consumers idempotent + DLQ + replay runbook
- DB migrations + rollback plan
- dashboards/alerts + runbook
- resilience policies (timeouts/retries/circuit breakers) documented and tested

---

### Summary of the final architectural choices (explicit)
- **Internal sync:** gRPC (Protobuf contracts)
- **Async backbone:** Kafka (default)
- **Optional async queue:** RabbitMQ (only for worker/queue-specific needs)

## Add-on Section A: Topic/Queue Naming Conventions + Ownership

### A.1 Naming goals
A good naming scheme makes it obvious:
- what the message is about (domain + event/command)
- who owns it (producer/team)
- what environment it’s in (if you separate infra by env)
- whether it’s a retry/DLQ stream

Keep names **stable**; don’t encode implementation details that might change.

---

### A.2 Kafka topic naming (recommended)
**Pattern**
```
<domain>.<bounded-context>.<message-type>.<name>.v<major>
```


Examples:
- `orders.checkout.event.OrderCreated.v1`
- `payments.billing.event.PaymentAuthorized.v1`
- `orders.checkout.command.ReserveInventory.v1` (if you model commands on Kafka)
- `orders.checkout.event.OrderCreated.v1.dlq`
- `orders.checkout.event.OrderCreated.v1.retry.5m`

**Conventions**
- Prefer **events** as facts in past tense: `OrderCreated`, `PaymentFailed`
- If you use “commands,” name them as verbs: `ReserveInventory`, `CapturePayment`
- Version only when you introduce a breaking change (`v1`, `v2`), otherwise evolve compatibly

**Partitioning rule**
- Choose a partition key that preserves ordering where needed, usually:
    - `orderId`, `userId`, `accountId` depending on the aggregate
- Document the key per topic (“Ordering guarantee is per key”)

---

### A.3 RabbitMQ exchange/queue naming (recommended)

RabbitMQ is best when you want explicit queue/worker semantics.

**Exchanges**

```
<domain>.<bounded-context>.<purpose>.x
```

Examples:
- `orders.checkout.events.x`
- `notifications.email.commands.x`

**Queues**
```
<domain>.<bounded-context>.<consumer>.<purpose>.q
```

Examples:
- `orders.checkout.inventory-worker.process.q`
- `notifications.email.sender.send.q`

**DLQ**
```
<original-queue>.dlq
```

Example:
- `notifications.email.sender.send.q.dlq`

---

### A.4 Ownership model (must be explicit)
**Kafka**
- The **producer owns the topic contract** (schema + semantics + versioning policy).
- The producer must publish:
    - schema, example payloads, versioning rules
    - partition key definition
    - retention expectations
- Consumers own their processing logic and must be idempotent.

**RabbitMQ**
- The **consumer typically owns the queue** (because queues are consumer-facing).
- The **publisher owns the message contract** (payload schema + headers).

**Cross-cutting ownership**
- Platform team owns:
    - broker clusters, security, quotas, observability
- Domain teams own:
    - schemas, compatibility, DLQs, replay procedures

---

### A.5 Required metadata in every async message (headers/envelope)
- `message_id` / `event_id`
- `type` + `version`
- `produced_at` timestamp
- `producer` (service name)
- `correlation_id` and `traceparent` (or equivalent) for observability
- (optional) `tenant_id` if multi-tenant

---

## Add-on Section B: gRPC Error Model + Status Code Mapping

### B.1 Goals
- Make error handling consistent across all services.
- Ensure callers can distinguish:
    - retryable vs non-retryable errors
    - client mistakes vs server faults
    - auth vs validation vs not-found

---

### B.2 Standard gRPC statuses to use (baseline)
Recommended mapping:

- **INVALID_ARGUMENT**  
  Validation errors, malformed requests, missing required fields.

- **NOT_FOUND**  
  Resource does not exist.

- **ALREADY_EXISTS**  
  Create conflicts (idempotent create returning “already exists”).

- **FAILED_PRECONDITION**  
  Request is valid but cannot be executed in current state  
  (e.g., “order cannot be canceled because it’s shipped”).

- **PERMISSION_DENIED** / **UNAUTHENTICATED**  
  AuthN/AuthZ failures.

- **RESOURCE_EXHAUSTED**  
  Rate limiting, quota exceeded, too many requests.

- **UNAVAILABLE**  
  Downstream dependency unavailable / transient outage.  
  Typically retryable (with backoff).

- **DEADLINE_EXCEEDED**  
  Timeout. Usually retryable, but only if operation is idempotent.

- **INTERNAL**  
  Unexpected server error (bug). Not something clients can fix.

> Avoid using `UNKNOWN` unless you genuinely have no better classification.

---

### B.3 Error payload standard (structured details)
Define a consistent error detail schema (returned via gRPC error details):
- `error_code` (stable, service-defined string; e.g., `ORDER_STATE_INVALID`)
- `message` (human readable)
- `field_violations[]` for validation issues (field, description)
- `retryable` boolean (optional but helpful)
- `correlation_id` / `trace_id` (so support can find it)

This makes clients easier to implement and reduces “string matching” on errors.

---

### B.4 Idempotency + retries (how errors interact with resilience)
Rules:
- Clients may retry only when:
    - status is `UNAVAILABLE` or `DEADLINE_EXCEEDED` (sometimes `RESOURCE_EXHAUSTED`)
    - and the operation is **idempotent** or uses an **idempotency key**
- For non-idempotent operations:
    - require an idempotency key in the request
    - server must store and reuse the original result when the key repeats

This prevents “double charge / double create” incidents.

---

### B.5 Mapping gRPC errors to edge REST (if you expose REST externally)
If you have a REST gateway at the edge, map consistently:
- INVALID_ARGUMENT → 400
- NOT_FOUND → 404
- ALREADY_EXISTS → 409
- FAILED_PRECONDITION → 409 or 422 (choose one standard)
- UNAUTHENTICATED → 401
- PERMISSION_DENIED → 403
- RESOURCE_EXHAUSTED → 429
- DEADLINE_EXCEEDED → 504
- UNAVAILABLE → 503
- INTERNAL → 500

Pick a single mapping and document it to avoid different services behaving differently.

---

## Heads up

![Check List](./ms_design_checklist.jpeg)

### To Do