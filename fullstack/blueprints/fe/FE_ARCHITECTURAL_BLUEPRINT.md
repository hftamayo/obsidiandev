## 📁 1. Repository Structure

```text
├── apps/              # Deployable applications (e.g., main-app)
├── libs/
│   ├── shared/        # Cross-cutting concerns (ui, domain, infra)
│   └── [feature]/     # Domain-specific libraries (e.g., companies)
├── nx.json            # Nx workspace configuration
└── package.json       # Workspace dependencies and pnpm configuration
```

## 🏗️ 2. Architectural Design: Monorepo

We use a **Monorepo** approach powered by Nx to manage multiple applications and shared libraries in a single repository.

### Why Monorepo?
- **Shared Code**: Easily share DTOs, UI components, and utility functions between features or apps.
- **Atomic Changes**: Update a shared library and its consumers in a single commit, ensuring consistency.
- **Unified Tooling**: Single configuration for linting (Biome), testing (Vitest), and CI/CD.

| Pros | Cons |
| :--- | :--- |
| Simplified dependency management | Larger initial repository size |
| Improved discoverability of shared code | Potential for complex build configurations |
| Standardized development workflow | Requires discipline to prevent "spaghetti" dependencies |

---

## 🛠️ 3. Technology Stack

*   **Nx**: Build system and monorepo management. Handles task orchestration, caching, and dependency graphing.
*   **Vite**: Next-generation frontend tooling providing fast Hot Module Replacement (HMR) and optimized production builds.
*   **pnpm**: Fast, disk space efficient package manager using a content-addressable store.
*   **React 19**: UI library utilizing the latest features for concurrent rendering.
*   **TypeScript**: Static typing for enhanced developer experience and runtime safety.
*   **Tailwind CSS & shadcn/ui**: Utility-first CSS framework combined with accessible, unstyled components via Radix UI.
*   **TanStack Query (v5)**: Asynchronous state management for data fetching and caching.
*   **Zustand**: Lightweight, client-side state management for global UI state.
*   **Zod**: Data validation library for TypeScript.
*   **Tanstack Form**: Form state management and validation library.
*   **Tanstack Table**: Data grid component for React.
*   **tRPC+React Query Data**: Data fetching and caching library for tRPC.
*   **date-fns**: Utility library for date and time manipulation.
*   **nuqs**: A lightweight search params.
*   **recharts**: A flexible charting library for React.
 
---

## 📐 4. Design Patterns & Principles

### 1. Hexagonal-ish Layering (libs/shared)
We organize code into distinct layers within libraries to decouple business logic from framework concerns:
- **Domain**: Pure business logic, types, and interfaces (no framework dependencies).
- **Application**: Use cases and orchestrators (e.g., specific business flows).
- **Infrastructure**: Concrete implementations of external services (API clients, storage).
- **UI/Feature**: React components and hooks focused on presentation and user interaction.

### 2. Web Design Layouts
Our applications follow established structural patterns to ensure a consistent user experience. Below are the most commonly used layouts in web design:

*   **Single Column Layout**: A simple, vertical arrangement where content is stacked in one main column. Often used for landing pages, blogs, or mobile-first designs.
*   **Two Column Layout**: Features a main content area paired with a sidebar (either left or right). Ideal for dashboards and documentation where navigation or filters need to remain accessible.
*   **Three Column Layout**: Contains a central content section flanked by two sidebars. Commonly used for complex social media platforms or news sites requiring multiple navigation and information panels.
*   **Grid (Card) Layout**: Organizes content into a series of uniform boxes. Best suited for portfolio sites, e-commerce product listings, and photo galleries.

**Our default standard for this project will be the Two Column Layout**, providing a persistent sidebar for primary navigation and a spacious main area for feature-specific content.

![Types of layouts](./web_layout.jpeg)

### 3. Entity-Specific Components
Instead of overly complex generic abstractions, we favor **Entity-Specific Components** (e.g., `CompanyTable`). This ensures perfect type safety and allows for rapid iteration without breaking unrelated features.

### 4. Container/Presenter (Smart/Dumb)
- **Containers/Hooks**: Manage side effects, data fetching (TanStack Query), and state.
- **Presentational Components**: Receive data via props and focus purely on rendering and user events.

### 5. Adapter Pattern
Used in the Infrastructure layer to transform raw API responses into domain-friendly models, shielding the UI from backend schema changes.

### 6. Best Practices:
- **Using Tailwind's tail selectors**: "[&_td]:px-3 [&_td]:py-2"

---

