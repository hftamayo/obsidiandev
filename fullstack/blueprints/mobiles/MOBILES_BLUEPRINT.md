## Mobile Application Blueprint (Android Native / Kotlin, Jetpack Compose, Offline Writes) — v1

This blueprint describes a **production-minded starting architecture** for an Android app that behaves like a frontend (like web), but supports **offline reads and offline writes**, syncing safely when connectivity returns.

---

## 1) Architecture (Compose + MVVM + Repository + Offline-first)
### 1.1 Recommended stack
- **UI**: Jetpack Compose
- **Architecture**: MVVM (ViewModel + UiState) with a simple Clean layering
- **Async/state**: Kotlin Coroutines + Flow/StateFlow
- **DI**: Hilt
- **Networking**: Retrofit + OkHttp
- **Local database**: Room (SQLite)
- **Background sync**: WorkManager
- **Serialization**: Kotlinx Serialization (or Moshi)

### 1.2 Layers (keep it testable and scalable)
- **Presentation**
    - Compose screens
    - ViewModels expose `StateFlow<UiState>`
- **Domain**
    - Use cases (business operations), pure Kotlin
- **Data**
    - Repositories orchestrate local DB + network
    - Local data source (Room)
    - Remote data source (API)
    - Mappers: DTO ↔ Domain ↔ Entity

**Rule:** UI never talks directly to Retrofit; it goes through ViewModel → use case → repository.

---

## 2) Offline-first data model (single source of truth)
### 2.1 Source of truth
- The **local Room database is the source of truth** for UI.
- Network sync updates local DB; UI reacts automatically via Flow queries.

### 2.2 Data categories (treat differently)
- **Server-owned read models**: lists/details fetched and cached (server wins)
- **User-owned writable data**: drafts/changes created offline (needs sync + conflict rules)

---

## 3) Offline writes: the Outbox pattern (required)
To support offline writes safely, implement a **local outbox** (this is the key piece).

### 3.1 Outbox table concept
Store “intents” to change server state:
- operation type: CREATE / UPDATE / DELETE
- target entity type + local entity id
- payload (or reference to entity snapshot)
- createdAt, attempts, lastError
- status: PENDING / IN_FLIGHT / SENT / FAILED

### 3.2 UX behavior (important)
When a user performs an action while offline:
1. Write/update the entity locally (optimistic UX).
2. Create an outbox entry (PENDING).
3. UI reflects the change immediately with a “sync pending” indicator if needed.

When online:
- WorkManager processes the outbox.
- Successful sync marks outbox SENT and reconciles local entity with server response.

### 3.3 Idempotency (prevents duplicates)
Offline retries can cause duplicate creates unless you design for it.

Minimum viable approach:
- Generate a **client_operation_id** (UUID) per write operation.
- Send it in API headers or request body.
- Server treats it as an **idempotency key** (same key ⇒ same result).

If the server can’t support idempotency yet:
- still keep operation IDs locally and implement best-effort dedupe client-side, but production-grade reliability improves a lot when the server supports idempotency keys.

---

## 4) Sync engine (WorkManager) — “how data converges”
### 4.1 Triggers
- Network becomes available
- App starts / returns to foreground (light refresh)
- Periodic sync (e.g., every 6–12 hours) if desired

### 4.2 Ordering rules
- Sync **outbox first** (push local writes)
- Then pull latest server state (refresh) to reconcile

### 4.3 Retry/backoff rules
- exponential backoff with jitter
- max attempts before marking FAILED
- FAILED operations surface in UI (“Tap to retry” / “Resolve conflict”)

### 4.4 Conflict strategy (must be explicit)
Pick one for v1:
- **Server wins** (simple, but can overwrite local edits)
- **Client wins** (dangerous if stale)
- **Manual resolution** for certain entities (best UX, more work)

Common v1 approach:
- server wins for most things
- manual resolution only for critical user-generated content

---

## 5) Network/API integration (mobile realities)
### 5.1 API client rules
- Timeouts tuned for mobile networks
- Centralized error mapping
- Automatic auth token refresh (single-flight to avoid multiple refresh calls)

### 5.2 Resilience
- Retry only safe operations (GET) and idempotent writes (with idempotency keys)
- Graceful offline detection:
    - show offline banner
    - enqueue writes instead of failing the user flow

### 5.3 Contract compatibility
Mobile releases lag; server must remain backward compatible:
- additive JSON fields are fine
- tolerate missing fields
- don’t rely on strict ordering of fields

---

## 6) State management in Compose (MVVM done right)
### 6.1 UiState design
Each screen’s ViewModel should expose something like:
- `data`
- `isLoading`
- `error`
- `isOffline`
- `syncStatus` (e.g., SYNCED / PENDING / FAILED)
- `lastSyncedAt`

### 6.2 One-off events
Use a separate event channel for:
- navigation events
- snackbars/toasts
- “sync failed” prompts

Avoid mixing transient events into persistent UiState.

---

## 7) Local storage rules (Room + caching discipline)
- Store:
    - entities for offline browsing
    - outbox operations
    - sync metadata (lastUpdated, lastSyncedAt, version/etag if available)
- Eviction policy:
    - “keep last N items” or “keep last N days”
- Sensitive data:
    - store minimally
    - consider encryption if storing anything beyond low-risk cached content

---

## 8) Security baseline
- TLS everywhere
- Store tokens in **Android Keystore / EncryptedSharedPreferences**
- Never log PII/tokens
- Consider:
    - biometric gate for sensitive actions (optional)
    - screenshot prevention for sensitive screens (optional)

---

## 9) Testing strategy (focus on offline correctness)
Minimum viable tests:
- Repository tests:
    - offline write → DB updated → outbox created
    - online sync → outbox drained → DB reconciled
- WorkManager tests:
    - retry behavior + failure marking
- ViewModel tests:
    - UiState transitions: loading → data, offline banner, sync pending
- API tests with a mock server (timeouts, 401 refresh, 500 errors)

---

## 10) Observability (mobile side)
Define signals you need (vendor choice later):
- crashes + non-fatal exceptions
- network failure rates and latency
- sync metrics:
    - outbox size
    - last successful sync time
    - number of failed operations

Also propagate:
- `request_id` / `traceparent` headers where possible for backend correlation.

---

## 11) Release realities (mobile != web)
- Server must support backward compatibility because users won’t update instantly.
- Use feature flags / remote config to disable risky features without waiting for store updates.
- Rollout strategy:
    - internal → beta → staged → full

---

## 12) Definition of Done (v1 mobile app)
A feature/service integration is “done” when:
- UI implemented in Compose with MVVM + UiState
- Data stored in Room and readable offline
- Offline writes queue via Outbox
- Sync worker reliably retries and surfaces failures
- Idempotency approach defined (prefer server-supported keys)
- Basic tests cover offline write and sync
- Crash + sync telemetry hooks exist

---

## Add-on Section C: API Support for Idempotency Keys (Required for Mobile Offline Writes)

Mobile offline writes + retries mean the backend must safely handle duplicate submissions. This section defines how services (edge REST and/or internal gRPC) implement **idempotent command handling**.

---

### C.1 What problem this solves
Without idempotency, these are common in production:
- duplicate resource creation (double orders, duplicate payments)
- retries after timeouts causing the same mutation twice
- offline “outbox replay” producing duplicates when connectivity flaps

Idempotency ensures: **“same operation key ⇒ same result”**.

---

### C.2 When idempotency is required
Idempotency keys MUST be supported for any operation that is:
- **non-idempotent by nature** (create, charge, submit, finalize)
- callable by **mobile clients** that can retry offline/outbox operations
- subject to network timeouts or client retries

Examples:
- Create Order
- Submit Application
- Upload Final Confirmation
- Capture Payment (especially)

---

### C.3 Contract: how the client sends the idempotency key

#### REST (edge API)
- Client sends header: **`Idempotency-Key: <uuid>`**
- Optional but recommended headers:
    - `X-Request-Id: <uuid>` (for correlation)
    - `Traceparent: ...` (W3C trace context)

The idempotency key is **per operation attempt** (not per session). One write action = one UUID.

#### gRPC (internal)
Pass the idempotency key via one of:
- a request field: `string idempotency_key = ...;` (preferred; explicit in proto)
- or metadata header: `idempotency-key: <uuid>`

Also propagate tracing metadata (`traceparent`) as you already do.

---

### C.4 Server-side behavior (required semantics)
When receiving a request with an idempotency key:

1) **First time key is seen**
- execute the operation once
- store an idempotency record:
    - key
    - request hash/signature (to detect misuse)
    - resulting status + response payload (or a reference)
    - createdAt, expiresAt (TTL)

2) **Key is seen again**
- if the request matches the original (same “shape”): return the **same response** as the first execution
- if the request differs materially: return a **conflict error** (client is reusing keys incorrectly)

3) **In-flight protection**
- if the same key arrives while the first is still processing:
    - either block briefly and return the final result, or
    - return a “try again” response (less ideal for mobile UX)

---

### C.5 TTL / retention policy for idempotency records
Define a minimum retention window so mobile can safely retry:
- baseline: **24 hours**
- if users may stay offline longer: **3–7 days**

Store size is typically small (key + response metadata), so longer TTLs are feasible.

---

### C.6 Storage and implementation pattern
Recommended implementation approach per service:
- Store idempotency records in the **service’s own database** (DB-per-service consistent with your architecture).
- If the operation also writes domain data, store the idempotency record **transactionally** with the domain change.

This prevents “domain write succeeded but idempotency record didn’t,” which would re-open the duplicate risk.

---

### C.7 Error handling (standardize)
If the same idempotency key is reused with a different request:
- REST: return **409 Conflict**
- gRPC: return **ALREADY_EXISTS** or **FAILED_PRECONDITION** (pick one standard; I recommend **FAILED_PRECONDITION** for “key reuse mismatch”)

Also include a structured error code like:
- `IDEMPOTENCY_KEY_REUSE_MISMATCH`

---

### C.8 Mobile outbox alignment (why this matters)
The mobile outbox should:
- generate one UUID per queued operation
- reuse the same key across retries until it succeeds definitively
- treat “same result returned” as success (even if it was a retry)

---
## Idempotency Key Checklist (API + Clients)

Use this checklist as the “Definition of Done” for any endpoint/RPC that can be triggered by **mobile offline outbox**, retries, flaky networks, or user double-taps.

---

### 1) Identify endpoints that require idempotency
- [ ] All “create/submit/finalize/capture” operations are flagged as **idempotency-required**
- [ ] Any operation that can be retried after timeout is assessed (especially payments, orders, submissions)
- [ ] The API documentation clearly marks which endpoints accept/require idempotency keys

---

### 2) Contract requirements (REST and/or gRPC)
**REST**
- [ ] Endpoint accepts `Idempotency-Key` header (UUID)
- [ ] Endpoint behavior is defined when the header is missing (reject vs best-effort)

**gRPC**
- [ ] Request includes `idempotency_key` field in proto (preferred) or accepts `idempotency-key` metadata
- [ ] Documentation states which RPCs require it

---

### 3) Server-side semantics (correctness)
- [ ] First request with a key executes **exactly once**
- [ ] Subsequent requests with the same key return the **same result**
- [ ] Requests with same key but different payload return a clear **conflict** error
- [ ] In-flight duplicate requests are handled (wait for completion or return a controlled retry response)
- [ ] The idempotency record is persisted **transactionally** with the domain change (same DB transaction if possible)

---

### 4) Storage & retention (practicality)
- [ ] Idempotency records are stored in the service’s own DB (DB-per-service aligned)
- [ ] TTL/retention is defined (minimum 24h; 3–7 days if offline periods are longer)
- [ ] Storage growth is controlled (cleanup job / TTL index)

---

### 5) Security & abuse prevention
- [ ] Idempotency key is scoped correctly (per endpoint + per authenticated principal where applicable)
- [ ] Request signature/hash is stored to detect key reuse with altered payload
- [ ] Rate limits apply even for idempotent retries (to prevent abuse)

---

### 6) Client behavior (mobile/web)
- [ ] Client generates **one UUID per user action**
- [ ] Client reuses the same key across retries until it succeeds definitively
- [ ] Client treats “same response returned” as success (not an error)
- [ ] Client does not reuse keys across different logical actions

---

### 7) Observability (you will need this in prod)
- [ ] Logs include: idempotency key, request_id, trace_id (redacted/safe handling)
- [ ] Metrics exist for:
    - idempotency hits (duplicate key seen)
    - idempotency conflicts (key reuse mismatch)
    - deduped requests count (useful to see retry patterns)
- [ ] Dashboards/alerts exist for unusual spikes in retries/conflicts (can indicate client bugs or outages)

---

### 8) Testing (must prove it works)
- [ ] Unit test: same key twice → same response, no duplicate side effects
- [ ] Integration test: timeout + retry with same key → no double create
- [ ] Concurrency test: two simultaneous requests with same key → only one effect
- [ ] Negative test: same key + different payload → conflict response
- [ ] Soak test (optional): many retries during downstream instability

---

### 9) Documentation & runbook
- [ ] API docs include idempotency requirements and error modes
- [ ] Runbook documents how to investigate:
    - duplicate reports
    - conflict errors
    - high retry periods
- [ ] Support knows which IDs to ask for (idempotency key / request_id)

---
