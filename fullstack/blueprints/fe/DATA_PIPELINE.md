# Data Management Pipeline

This document describes the flow of data for the **Company** entity, starting from the UI layer (`CompanyContainer`) down to the infrastructure layer (`CompanyQueryAdapter`).

## Overview

The data pipeline follows a layered architecture, moving through:
1. **Presentation Layer (React Components & Hooks)**
2. **Application Layer (React Query & Repositories)**
3. **Infrastructure Layer (Adapters & HTTP Client)**

---

## 1. Presentation Layer

### `CompanyContainer`
*   **Path:** `absenses/src/components/company/CompanyContainer.tsx`
*   **Responsibility:** The main entry point for the Company UI. It handles the display logic, including tables, search, and pagination.
*   **Data Hook:** It delegates data fetching and state management to the `useCompanyData` hook.

### `useCompanyData` (Hook)
*   **Path:** `absenses/src/components/company/hooks/useCompanyData.ts`
*   **Responsibility:** Orchestrates local UI state (sorting, filters, pagination) and calls the application-level data hook.
*   **Key Operation:**
```typescript
const { data, isLoading, error } = useCompanies(fetchParams);
```

---

## 2. Application Layer

### `useCompanies` & `useActiveCompanies` (Query Hooks)
*   **Path:** `libs/companies/application/src/query/hooks/`
*   **Responsibility:** React Query wrappers that manage caching and query state.
*   **Key Features:**
  *   **Cache Management:** Uses a centralized `companyKeys` factory for consistent query key generation.
  *   **Specialized Hooks:** `useActiveCompanies` provides a shortcut for common views by automatically filtering for non-deleted, active companies.
*   **Key Operation:** Calls `companyRepository.findAll(params)` or `findActive(params)` as the `queryFn`.

### `CompanyRepository`
*   **Path:** `libs/companies/application/src/repositories/CompanyRepository.ts`
*   **Responsibility:** Acts as the business logic layer. It performs:
  *   **Validation:** Uses Zod schemas (e.g., `CompanyQueryParamsSchema`) to validate incoming request parameters.
  *   **Orchestration:** Calls the infrastructure adapter.
  *   **Mapping:** Transforms raw infrastructure data into domain objects using `CompanyMapper`.

### `CompanyMapper`
*   **Path:** `libs/companies/application/src/mappers/CompanyMapper.ts`
*   **Responsibility:** Transforms raw DTOs from the infrastructure into UI-friendly models (e.g., converting date strings to `Date` objects).

---

## 3. Infrastructure Layer

### `CompanyAdapter` (Facade)
*   **Path:** `libs/companies/infrastructure/src/adapters/CompanyAdapter.ts`
*   **Responsibility:** Provides a unified facade for Company operations. For read operations, it delegates to the `CompanyQueryAdapter`.

### `CompanyQueryAdapter`
*   **Path:** `libs/companies/infrastructure/src/adapters/CompanyQueryAdapter.ts`
*   **Responsibility:** Specifically handles **READ** operations (CQRS pattern).
*   **Key Tasks:**
  *   Inherits from `BaseApiAdapter`.
  *   **Endpoint:** `GET /api/v1/companies/list` (Note: plural "companies").
  *   **Pagination Handling:** Uses `handleHybridPagination` to ensure the UI receives a consistent `ProcessedPaginatedResponse` regardless of whether the backend provides metadata.
  *   **Resilience:** Includes `listTempCompanies` to fetch data without parameters as a fallback for specific backend validation (422) errors.

---

## Data Flow Summary

`CompanyContainer`
→ `useCompanyData` (UI State/Hook)
→ `useCompanies` / `useActiveCompanies` (React Query + `companyKeys`)
→ `CompanyRepository` (Validation & Orchestration)
→ `CompanyAdapter` (Facade)
→ `CompanyQueryAdapter` (Request Builder + Hybrid Pagination)
→ `BaseApiAdapter` (HTTP Client)
→ **BACKEND API**

---
