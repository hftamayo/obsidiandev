### 1. Test Management Strategy
In a modern monorepo, tests are treated as "first-class citizens" of the source code. They are not temporary artifacts; they are the insurance policy for your logic.

*   **Unit Tests (`.spec.ts/tsx`)**:
  *   **Placement**: **Side-by-side**. Place `CompanyTable.spec.tsx` in the same folder as `CompanyTable.tsx`.
  *   **Focus**: Isolated logic, component rendering, and hook behavior (e.g., `useCompanyData`).
  *   **Benefits**: High visibility for developers, easier imports, and instant feedback via Vitest.
*   **Integration Tests (`.test.ts/tsx`)**:
  *   **Placement**: Either side-by-side with the "parent" component (like `CompanyContainer`) or in a `__tests__` subfolder within the feature directory.
  *   **Focus**: The "Standard Data Flow"—how components, hooks, and state managers (Zustand/Query) work together.
*   **E2E (End-to-End) Tests**:
  *   **Placement**: In a dedicated Nx project (e.g., `apps/main-app-e2e`).
  *   **Focus**: Critical user journeys (e.g., "Full login flow to company creation") using tools like Playwright or Cypress.

---

### 2. Code Promotion: Old vs. Modern
The core difference lies in whether "cleaning" happens **manually by humans** or **automatically by tools**.

*   **The Old Approach (Code Promotion)**: Depended on physically separating or removing test files when moving from a Staging branch/environment to Production. The goal was to ensure Production remained "pure" and lean, but it carried the risk of deploying code that was structurally different from what was tested.
*   **The Modern Approach (Artifact Promotion)**: Uses **Build-Time Exclusion**. You keep code and tests together in Git (the "Source of Truth"). When you run a production build (`nx build`), the compiler (Vite/SWC) follows the dependency graph and **automatically ignores** all `.spec` and `.test` files. The resulting "Artifact" (the JS bundle) is lean and contains zero test code.

---

### 3. Final Recommendation
For your **Company** feature and future modules, I recommend the **Modern Artifact Promotion** model:

1.  **Merge Everything**: Merge both the codebase and all test suites into your `main` branch. This ensures that every developer who pulls the code can verify their changes against your requirements.
2.  **Staging as the "Proving Ground"**: Use your Staging environment to run the full suite (Unit, Integration, and E2E). This is your "QA Green Light" phase.
3.  **Production Gate**: Once Staging is verified, promote the **Build Artifact** to Production.
  *   **Why?** You are deploying the exact logic that passed the tests, but because of the build step, your production users never download a single byte of test code.
  *   **Security**: Ensure testing libraries (Vitest, Testing Library) are in `devDependencies` so they are never installed on your production server.

**The Bottom Line**: Don't be afraid to let your tests live in `main`. They are the "blueprint" of your application’s correctness, and your build tools are smart enough to keep them out of the hands of your end-users.
