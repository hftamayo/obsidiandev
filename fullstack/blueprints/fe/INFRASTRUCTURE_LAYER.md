# Infrastructure Layer (Data & API)

## 🎯 Design Decision: Adapter Pattern (CQRS Lite)
We use a decoupled **Adapter Pattern** with a focus on separating Read and Write concerns.
- **Rationale**: Isolates the application from backend implementation details (like endpoint naming or specific HTTP headers).
- **Architecture**: For the Company entity, we use a Facade (`CompanyAdapter`) that delegates to a specialized `CompanyQueryAdapter` for all **READ** operations.

---

## 🏗️ Core Components

### 1. The Adapters (`libs/companies/infrastructure/src/adapters/`)
The specialized classes responsible for network communication.
*   **`CompanyQueryAdapter`**:
  *   **Responsibility**: Specifically handles all `GET` operations.
  *   **Resilience**: Implements `listTempCompanies` to bypass backend parameter issues (422 errors) by fetching raw lists when filters fail.
  *   **Convenience**: Provides `getActiveCompanies` which hardcodes the `isActive` and `isDeleted` flags for the API call.
*   **`BaseApiAdapter`**:
  *   The base class providing standard `get`, `post`, `put`, and `delete` methods.
  *   Handles internal caching logic and query string construction.

### 2. Common Infrastructure Utilities
Shared tools used across all entity adapters.
*   **`handleHybridPagination`**:
  *   A critical utility that normalizes backend responses.
  *   It ensures the Application layer always receives a consistent `ProcessedPaginatedResponse` regardless of whether the backend provides full metadata or a simple array.
*   **`API_BASE_PATH`**:
  *   Centralized configuration for endpoint versioning (e.g., `/api/v1`).

---

## 🔄 Technical Flow

1.  **Request Construction**: `CompanyQueryAdapter` receives validated parameters from the Repository.
2.  **Query Building**: `buildQueryString` converts JSON params into a URL-encoded string.
3.  **HTTP Call**: `BaseApiAdapter.get()` executes the request via the fetch client.
4.  **Normalization**: `handleHybridPagination` processes the raw response.
5.  **Return**: A standardized paginated object is returned to the Application layer.

---

## 🎨 Design Principles

1.  **Backend Agnostic**: The rest of the app should not know if the endpoint is `/company` or `/companies`; the Adapter hides these details.
2.  **Strict Typing**: Every adapter method is typed with Infrastructure-specific DTOs to ensure contract compliance with the Backend API.
3.  **Fail-Safe Mechanisms**: Temporary methods (like `listTempCompanies`) are allowed here to bridge gaps in backend stability without polluting the business logic.
4.  **Transparent Caching**: The infrastructure layer manages per-request caching options via the `useCache` flag.

---

## 🚀 Implementation Status

| Component | Status | Responsibility |
| :--- | :--- | :--- |
| **BaseApiAdapter** | ✅ Done | Global HTTP client & Caching |
| **CompanyQueryAdapter** | ✅ Done | Read operations & Endpoint mapping |
| **Hybrid Pagination** | ✅ Done | Response normalization |
| **Query String Builder**| ✅ Done | Object-to-URL conversion |

---

*This layer ensures that network-level complexities are handled in one place, providing a stable foundation for the Business Logic.*
