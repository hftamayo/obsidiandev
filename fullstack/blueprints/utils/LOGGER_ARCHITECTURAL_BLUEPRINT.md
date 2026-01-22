### 1. Project Structure (The Layout)
Following Go best practices, we will separate the communication (WebSockets), the logic (Contract), and the storage (Redis/JSON).

```plain text
logger-service/
├── cmd/
│   └── server/             # Entry point
│       └── main.go
├── internal/
│   ├── auth/               # ApiKey & Session validation
│   │   └── middleware.go
│   ├── contract/           # The "Master Contract" (Structs)
│   │   └── models.go
│   ├── storage/            # Interface for JSON/DB/Redis
│   │   ├── storage.go      # Interface definition
│   │   ├── redis.go        # Redis session management
│   │   └── json_file.go    # Initial JSON output
│   └── websocket/          # Hub and Client management
│       ├── hub.go          # Manages connections
│       └── client.go       # Individual connection logic
├── pkg/
│   └── utils/              # Helpers (Timestamps, UUIDs)
├── .env                    # Config (Redis URL, API Keys)
└── go.mod
```


### 2. The Master Contract (`models.go`)
This is the "Law" of your data flow. We use JSON tags to ensure the output format matches your requirements.

```textmate
package contract

import "time"

// LogLevel defines the severity
type LogLevel string
const (
	Info    LogLevel = "info"
	Warning LogLevel = "warning"
	Error   LogLevel = "error"
)

// Session represents the active user data
type Session struct {
	User      string    `json:"user"`
	Timestamp time.Time `json:"timestamp"`
	IPAddress string    `json:"ipaddress"`
}

// LogEntry is the primary contract for all incoming/outgoing data
type LogEntry struct {
	Timestamp     time.Time `json:"timestamp"`
	CorrelationID string    `json:"correlationId"`
	Level         LogLevel  `json:"level"`
	Project       string    `json:"project"` // e.g., "absences"
	System        string    `json:"system"`  // e.g., "frontend", "backend"
	Module        string    `json:"module"`  // e.g., "AddCompanies"
	Action        string    `json:"action"`  // e.g., "saveHandler"
	ErrorType     string    `json:"errorType,omitempty"` // unauth, validation, etc.
	ErrorCode     int       `json:"errorCode,omitempty"`
	ErrorMessage  string    `json:"errorMessage,omitempty"`
	ActiveSession Session   `json:"activeSession"`
}
```


### 3. State Management with Redis
Since the project is stateful, Redis will store the `ActiveSession` mapping against the `ApiKey`.

```textmate
package storage

import (
	"context"
	"encoding/json"
	"github.com/go-redis/redis/v8" // Standard Go Redis client
	"logger-service/internal/contract"
)

type RedisStore struct {
	client *redis.Client
}

// GetSession retrieves the session linked to an API Key
func (r *RedisStore) GetSession(ctx context.Context, apiKey string) (*contract.Session, error) {
	val, err := r.client.Get(ctx, apiKey).Result()
	if err != nil {
		return nil, err
	}

	var session contract.Session
	err = json.Unmarshal([]byte(val), &session)
	return &session, err
}
```


### 4. WebSocket Architecture (The Hub)
The **Hub** will manage all active connections. It acts as the traffic controller for incoming logs.

```textmate
package websocket

import "logger-service/internal/contract"

type Hub struct {
	// Registered clients.
	clients map[*Client]bool
	// Inbound messages from the clients.
	broadcast chan contract.LogEntry
	// Register requests from the clients.
	register chan *Client
	// Unregister requests from clients.
	unregister chan *Client
}

func NewHub() *Hub {
	return &Hub{
		broadcast:  make(chan contract.LogEntry),
		register:   make(chan *Client),
		unregister: make(chan *Client),
		clients:    make(map[*Client]bool),
	}
}

func (h *Hub) Run() {
	for {
		select {
		case client := <-h.register:
			h.clients[client] = true
		case client := <-h.unregister:
			if _, ok := h.clients[client]; ok {
				delete(h.clients, client)
				close(client.send)
			}
		case log := <-h.broadcast:
			// Here is where you would call storage.Write(log)
			// and also broadcast to any monitoring dashboard
		}
	}
}
```


### 5. Architectural Highlights

*   **Security (ApiKey):** When a service (Frontend or Backend) connects via WebSocket, they must provide the `ApiKey` in the header or as a query param. The service will check Redis:
    *   If `ApiKey` exists: Inject the `ActiveSession` data into the `LogEntry`.
    *   If `ApiKey` is invalid: Reject the connection.
    *   **Concurrency:** Go routines will handle the `Hub.Run()` loop, allowing thousands of logs to be processed without blocking the main thread.
    *   **Graceful Shutdown:** The service will listen for termination signals (SIGINT/SIGTERM). Upon receiving one, it will stop accepting new WebSocket connections, flush the remaining logs in the `Hub`'s channel to storage, and close Redis connections cleanly to prevent data loss.
    *   **Health Checks & Observability:** We will expose a `/health` endpoint for orchestrators (like Docker/K8s). Additionally, the service will track "Internal Metrics" (e.g., total logs processed, active connections, and circuit breaker status) to ensure we can monitor the health of the logging pipeline itself.
    *   **Test-Driven Development (TDD):** We will adopt a TDD approach by utilizing Go's built-in `testing` package and `testify` for assertions.

### 6. Suggested Design Patterns

*   To maintain a scalable and maintainable codebase, we will lean on two primary patterns:
    *   **Strategy Pattern (Storage):** By defining a `Storage` interface, we implement the Strategy pattern. This allows the service to swap "strategies" for data persistence (e.g., switching from `JSONFileStorage` to `ElasticSearchStorage`) at runtime or via configuration, ensuring the core logic remains untouched.
    *   **Circuit Breaker Pattern:** To ensure High Availability (HA), we will implement circuit breakers on our storage providers. If Redis or the primary database becomes unresponsive, the breaker "trips" to prevent system-wide latency, allowing the service to fail fast or redirect logs to a fallback local buffer.

### 7. Technology Stack
*    We will utilize the following tools from the Go ecosystem to ensure high performance and reliability:
    *   **Gorilla WebSocket:** The de-facto standard for implementing the WebSocket protocol in Go, providing robust handling of the handshake and connection lifecycle.
    *   **Go-Redis (v8/v9):** A type-safe Redis client that supports context-based timeouts and connection pooling, essential for managing our session state.
    *   **Zerolog or Zap:** For internal service logging (logging the logger itself), we'll use one of these structured, zero-allocation loggers to maintain maximum throughput.
    *   **Godotenv:** To manage environment variables (`.env`) for sensitive configuration like API keys and database credentials, keeping our "Master Contract" clean.


### Next Steps for Implementation:
1.  **Initialize the Go module** and install dependencies (`gorilla/websocket`, `go-redis`).
2.  **Implement the Handshake:** Create the logic that upgrades HTTP to WS and validates the ApiKey.
3.  **The Processor:** Write the logic that takes the raw WS message, validates it against the `LogEntry` struct, and saves it.

How does this blueprint look to you? Should we start deep-diving into the **ApiKey handshake logic**?