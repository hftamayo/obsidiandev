# Presentation Layer (UI) - Architecture

## 🎯 Design Decision: Entity-Specific Components
We prioritize **Entity-Specific Components** over generic abstractions.
- **Rationale**: Faster implementation, perfect type safety, and easier maintenance.
- **Pattern**: The "Company" implementation serves as the gold-standard blueprint for future entities.

---

## 🏗️ Component & Hook Hierarchy

### 1. Presentation Components (`libs/shared/ui/src/components/company/`)
Dedicated UI for Company data, built on top of **shadcn/ui** and **Radix UI**.

*   **`CompanyTable`**: The primary list view. Integrates with `@monorepo/shared/ui`'s `DataTable`.
  *   *Features*: Action menus (View/Edit/Delete), dynamic columns, and integrated sorting.
*   **`CompanyDetails`**: A read-only card view for single-entity inspection.
*   **`CompanyForm`**: (Planned) Managed form for Create/Update operations using `react-hook-form` and Zod validation.
*   **`CompanyPagination`**: Standardized controls for navigating paginated server data.

### 2. State & Logic Hooks (`absences/src/components/company/hooks/`)
The "brains" behind the UI, bridging the Application layer with React components.

*   **`useCompanyData`**: The main orchestrator.
  *   *Logic*: Manages UI filters, debounced search, and sort states.
  *   *Data*: Connects to the `useCompanies` query hook.
  *   *Error Handling*: Automatically reports frontend errors to the Error Microservice.
*   **`useSort` & `useFilters`**: Reusable utility hooks for managing UI query parameters.

---

## 🔄 Standard Data Flow (List View)

1.  **`CompanyContainer`**: The page-level entry point.
2.  **`useCompanyData`**: Fetches data and manages local state (e.g., "Is the detail modal open?").
3.  **`CompanyTable`**: Receives raw data and renders the `DataTable`.
4.  **`Actions`**: User clicks "View Details" → `handleViewDetails` updates state → `CompanyDetails` renders in a Modal/Drawer.

---

## 🎨 Design Principles

1.  **Type Safety**: All props extend domain-driven DTOs (e.g., `CompanyResponseDto`).
2.  **Hybrid Pagination**: Components handle both full metadata and raw lists via `handleHybridPagination` at the adapter level.
3.  **Resilience**: Loading skeletons and error boundaries are mandatory for all entity views.
4.  **Composition**: Complex views are built by composing small, single-responsibility components (e.g., `Table` + `FilterBar` + `Pagination`).

---

## 🚀 Implementation Status

| Component | Status | Responsibility |
| :--- | :--- | :--- |
| **CompanyTable** | ✅ Done | Sorting, Row Actions, Data Mapping |
| **CompanyDetails** | ✅ Done | Data visualization, Status badges |
| **useCompanyData** | ✅ Done | Filter logic, Error reporting, Selection state |
| **CompanyForm** | ⏳ In Progress | Zod validation, Mutation integration |
| **Common UI** | ✅ Done | `LoadingSpinner`, `ErrorMessage`, `EmptyState` |

---

*This document serves as the implementation standard for all future feature modules.*
