Here is the streamlined documentation for the **Application Layer**, focusing on its role as the bridge between the raw infrastructure and the UI.

# Layer 2 & 3: Application Layer (Business Logic & State)

## 🎯 Design Decision: Repository & Query Pattern
We use a **Repository Pattern** decoupled from **React Query Hooks**.
- **Rationale**: Isolates business rules (validation, mapping) from the UI and server state management.
- **Benefits**: Allows for easy testing of logic without mocking hooks and ensures the UI always receives "clean" domain objects.

---

## 🏗️ Layer Components

### 1. The Repository (`libs/companies/application/src/repositories/`)
The single source of truth for "how" data is handled.
*   **`CompanyRepository`**:
  *   **Validation**: Uses **Zod** schemas to validate incoming parameters before they hit the network.
  *   **Orchestration**: Directs calls to the `CompanyAdapter` (Infrastructure).
  *   **Transformation**: Uses the `CompanyMapper` to turn DTOs into Domain Models.
  *   **Specialized Methods**: Includes `findActive` for common filtered lookups.

### 2. The Server State (`libs/companies/application/src/query/`)
Manages the lifecycle of asynchronous data using **TanStack Query (React Query)**.
*   **Hooks (`useCompanies`, `useCompany`, `useActiveCompanies`)**:
  *   Wraps repository methods in `useQuery`.
  *   Handles caching, background refetching, and loading/error flags.
*   **Query Keys (`companyKeys`)**:
  *   A centralized factory for all company-related keys.
  *   Ensures consistent cache invalidation (e.g., updating a company invalidates the `list` key).

### 3. The Mapper (`libs/companies/application/src/mappers/`)
A pure utility layer for data sanitization.
*   **`CompanyMapper`**:
  *   Converts backend strings (like ISO dates) into JavaScript `Date` objects.
  *   Handles default values for optional fields to prevent `undefined` crashes in UI components.
  *   Ensures the UI layer never deals with raw backend DTO formats.

---

## 🔄 Business Logic Flow

1.  **UI Request**: `useCompanies(params)` is called.
2.  **Validation**: Repository checks `params` against `CompanyQueryParamsSchema`.
3.  **Fetch**: Repository calls `CompanyAdapter.listCompanies()`.
4.  **Map**: `CompanyMapper` transforms the response.
5.  **State Update**: React Query caches the "mapped" domain objects and returns them to the UI.

---

## 🎨 Design Principles

1.  **Validation at the Boundary**: No invalid data should ever leave the Application layer toward the Infrastructure.
2.  **Domain Purity**: UI components should only consume models defined in the Domain/Application layer, never raw API responses.
3.  **Predictable Keys**: Query keys must be structured (e.g., `['companies', 'list', { filters }]`) to allow for granular cache management.
4.  **Error Propagation**: Errors from the repository are typed and passed through to the UI via React Query's `error` state.

---

## 🚀 Implementation Status

| Module | Status | Responsibility |
| :--- | :--- | :--- |
| **Zod Schemas** | ✅ Done | Request/Response validation |
| **CompanyRepository** | ✅ Done | Logic orchestration & `findActive` shortcut |
| **Query Hooks** | ✅ Done | Caching & Server state sync |
| **Mappers** | ✅ Done | DTO → Domain transformation |
| **Query Keys** | ✅ Done | Centralized cache key management |

--- 

*This architecture ensures that if the API changes, we only update the Mapper or Repository, leaving the UI components untouched.*
