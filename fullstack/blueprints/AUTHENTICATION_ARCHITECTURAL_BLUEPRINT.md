### 1. Project Structure (The Gatekeeper Layout)
We follow the Clean Architecture approach in Go, ensuring that the business logic (Identity) is decoupled from the delivery mechanisms (REST/JWT).

```textmate
auth-service/
├── cmd/
│   └── server/             # Entry point (main.go)
├── internal/
│   ├── api/                # REST Handlers (Login, Signup, MFA)
│   ├── auth/               # JWT logic, Password Hashing, Claims
│   ├── provider/           # Strategy Pattern for Auth (Local, OAuth, OIDC)
│   ├── repository/         # DB Layer (Postgres) & Token Blacklist (Redis)
│   ├── service/            # Core logic (The "Identity" service)
│   └── middleware/         # Rate Limiting, RBAC Enforcement, Audit Logging
├── pkg/
│   └── audit/              # Client for our Logger Service
├── web/                    # Frontend (Dashboard & Login Pages)
├── .env                    # Secrets (JWT_KEY, DB_URL, SMTP_CONFIG)
└── go.mod
```


### 2. The Token & Identity Contract (`models.go`)
This defines the structure of our identity and the claims that will travel across our microservices ecosystem.

```textmate
// CustomClaims travels inside the JWT
type CustomClaims struct {
    UserID   string   `json:"uid"`
    Email    string   `json:"email"`
    Roles    []string `json:"roles"`       // e.g., ["admin", "reporter_user"]
    MFA      bool     `json:"mfa_verified"` 
    jwt.RegisteredClaims
}

// User represents the stored identity
type User struct {
    ID           string    `json:"id"`
    Email        string    `json:"email"`
    PasswordHash string    `json:"-"` // Never export
    MFASecret    string    `json:"-"`
    IsActive     bool      `json:"isActive"`
    CreatedAt    time.Time `json:"createdAt"`
}
```


### 3. Architectural Highlights
*   **Stateless JWT Authentication:** We issue short-lived **Access Tokens** (15 mins) and long-lived **Refresh Tokens** (7 days). Services like the *Reporting Service* will validate these tokens locally using a shared public key or secret without hitting the Auth DB.
*   **MFA (Multi-Factor Authentication):** To prevent account takeover via basic auth brute-force, we implement **TOTP** (Time-based One-Time Password). A user is only fully "authenticated" once the MFA challenge is completed.
*   **Dual-Layer Token Management:**
    *   *Access Tokens:* Stored in memory/cookies on the frontend.
    *   *Refresh Tokens:* Stored in a HttpOnly, Secure cookie to prevent XSS.
*   **Audit Streaming:** Every security event (failed login, password change, MFA skip) is streamed to our **Logger Service** as an "Audit Log" for SIEM/SOC tools to analyze.
*   **Rate Limiting & Throttling:** Using Redis `Fixed Window` or `Token Bucket` algorithms to prevent brute-force attacks on the `/login` and `/reset-password` endpoints.

### 4. Technology Stack (The Security Suite)
*   **Bcrypt:** For secure password hashing (Cost factor 12+).
*   **Golang-JWT (v5):** For generating and parsing RS256/HS256 tokens.
*   **PostgreSQL:** The source of truth for user identities and roles.
*   **Redis:** To handle session blacklisting (revoking tokens before they expire) and rate-limiting counters.
*   **Echo or Gin:** A high-performance web framework for the REST API.

### 5. Suggested Design Patterns
*   **Provider Pattern (Strategy):** We define an `AuthProvider` interface. This allows us to start with `LocalAuthProvider` (Email/Pass) and easily plug in `GoogleAuthProvider` (OAuth2) or `OktaProvider` (OIDC) without changing the `/login` handler.
*   **Middleware Chain:** A sequence of decorators that handle:
    1.  *Rate Limiting* (First line of defense).
    2.  *HTTPS Enforcement* (No tokens over plain text).
    3.  *Audit Logger* (Record the attempt).
    4.  *Logic* (The actual login).
*   **Repository Pattern:** Abstracts the database, allowing us to swap Postgres for any other storage (like LDAP) if the infrastructure grows.

### 6. Operational Considerations (SOC & Compliance)
*   **SIEM Integration:** By using our custom **Logger Service**, we format logs in JSON with `CorrelationID`. A SIEM (like Splunk or ELK) can track a single user’s journey from "Login" to "Report Generated" across services.
*   **RBAC (Role-Based Access Control):** The Auth service is the "Issuer." It decides what roles a user has. The downstream services are the "Enforcers"—they read the `roles` slice in the JWT and decide if the user can `POST /reports`.
*   **Statelessness vs. Revocation:** To remain stateless but still support "Logout," we use a **Token Blacklist** in Redis. If a user logs out, their token ID is added to Redis until its natural expiration time.

Excellent choice. Go gives us that "mechanical sympathy" where the code runs close to the metal, perfectly fitting the Cybertruck's performance profile.

Here is the tech stack breakdown for the **Authentication Service**, mapped to the legendary robustness of the Java Security ecosystem but executed with Go's efficiency:

### 7. Technology Stack (The Go Security Suite)
To match the "Grille Guard" reliability of `java.security`, we will utilize a curated selection of high-performance tools from the Go ecosystem:
*   **Echo or Gin (The Web Engine):** Acting as our **Servlet Container**, these frameworks provide the high-performance HTTP plumbing. We'll leverage their middleware chains to implement **Interceptors** for security headers (HSTS, CSP, XSS protection).
*   **Argon2 or Bcrypt (The Password Vault):** While Java uses `MessageDigest` or `SecretKeyFactory`, we will use the `golang.org/x/crypto/argon2` package. Argon2 is the winner of the Password Hashing Competition (PHC) and provides superior resistance to GPU-based brute-force attacks compared to standard PBKDF2.
*   **Golang-JWT/v5 (The Token Mint):** This replaces the `io.jsonwebtoken (JJWT)` stack. It will handle the generation and cryptographic signing of **RS256** (Asymmetric) tokens, ensuring that our downstream services can verify identity using a public key without needing access to the Auth Service's private secrets.
*   **Goth or ODR (The Provider Bridge):** To achieve the flexibility of **Spring Security OAuth2**, we’ll use `markbates/goth`. It provides a consistent multi-provider interface for OIDC and OAuth2 (Google, GitHub, AzureAD), allowing us to swap authentication strategies without touching the core business logic.
*   **Paseto (Optional Hardening):** For environments requiring even higher security than JWT, we can implement **Platform-Agnostic Security Tokens (PASETO)**, which eliminates the "alg: none" vulnerability and header-tampering risks found in the traditional JWT spec.
*   **Ent or GORM (The Identity Mapping):** Serving as our **JPA/Hibernate** layer, these ORMs will manage our PostgreSQL identity store with support for automated migrations and strict type-safety for our User and Role schemas.
*   **Redis (The State Guard):** Replaces the distributed `HttpSession`. Redis will manage our **Token Blacklist** and **Rate Limiting** buckets, ensuring that "Credential Stuffing" attacks are throttled before they even hit the database.

Here are the details for **Section 8: Communication Protocols**, designed to ensure the Auth Service is both accessible to the public and lightning-fast for internal service-to-service communication.

### 8. Communication Protocols (The "Dual-Port" Strategy)

The Authentication Service acts as a central hub. To minimize latency and maximize compatibility, we implement a hybrid communication layer:

#### **A. External Layer: RESTful API (The Public Gate)**
*   **Purpose:** User-facing operations (Login, Signup, MFA, Password Reset).
*   **Implementation:** Using the **Echo Framework** to handle JSON-over-HTTPS.
*   **Protocol:** HTTP/2 (standard in Go) to allow multiplexing of requests.
*   **Security:**
    *   **SameSite/HttpOnly Cookies:** For secure Refresh Token storage.
    *   **CORS Policies:** Strict origin validation to prevent unauthorized frontend access.
    *   **Standardized Errors:** RFC 7807 compliant error responses (Problem Details for HTTP APIs) to avoid leaking sensitive system info.

#### **B. Internal Layer: gRPC (The High-Speed Express Lane)**
*   **Purpose:** **Token Introspection & Authorization Checks.**
*   **Why:** When the *Reporting Service* receives a request, it needs to verify if the user has `permission: "generate_report"`. Doing this over REST creates "Architectural Friction." gRPC uses **Protocol Buffers (Protobuf)**—a binary format that is significantly faster and uses less CPU than JSON.
*   **Implementation:** A gRPC server running on a dedicated internal port (e.g., `:50051`).
*   **Contract Example (`auth.proto`):**
```protobuf
service IdentityService {
      rpc VerifyToken (TokenRequest) returns (TokenResponse);
      rpc GetUserPermissions (UserRequest) returns (PermissionResponse);
    }
```

#### **C. Push Layer: WebSockets (The Live Signal)**
*   **Purpose:** **Instant Session Revocation & MFA Notifications.**
*   **Why:** If a security event is detected (e.g., account locked by an admin), we use a WebSocket connection to the frontend to trigger an immediate "Hard Logout."
*   **Integration:** Shared logic with our **Logger Service**'s WebSocket Hub to maintain architectural consistency across the project.

#### **D. Event-Driven Layer: Webhooks (The SOC Messenger)**
*   **Purpose:** Integration with SOC/SIEM tools.
*   **Why:** For high-severity events (e.g., 50 consecutive failed logins from one IP), the service can fire a Webhook to an external security tool or our own **Logger Service** for immediate alerting.

---

### Revised Operational Flow:
1.  **Frontend** sends credentials via **REST** (`POST /login`).
2.  **Auth Service** validates, sets a cookie, and returns a **JWT**.
3.  **Frontend** requests a report from **Reporting Service** using that JWT.
4.  **Reporting Service** calls **Auth Service** via **gRPC** to verify the user’s roles in real-time.
5.  **Auth Service** logs the entire interaction to **Logger Service** via **WebSocket/REST**.

### WatchList:
1.  **The Proto File** (Defining the gRPC contract)?
2.  **The JWT Mint** (The logic for signing tokens)?
3.  **The Database Schema** (Postgres user tables)?

---
