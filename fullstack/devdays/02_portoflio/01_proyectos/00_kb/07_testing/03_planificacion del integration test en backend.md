
### Why integration tests before a new entity

1. Validates the full stack: unit tests cover logic; integration tests cover the full flow (controller → service → repository → database).

2. Catches integration issues early: database interactions, transaction boundaries, Spring context wiring.

3. Completes the testing pyramid: unit tests (base) + integration tests (middle) = solid foundation.

4. Builds confidence: moving to OUType is safer when Company is fully tested end-to-end.


### Suggested workflow:

Phase 1: Complete Company Testing (Current)
├── ✅ Unit Tests (DONE - 99% coverage!)
├── ⚠️  Fix Branch Coverage (TODO - notification package)
└── 🔄 Integration Tests (NEXT)

Phase 2: Apply to OUType (After Integration Tests)
├── Unit Tests (TDD approach)
└── Integration Tests (reuse patterns)


### Integration test strategy:

### What to test

1. Full CRUD operations:

- POST /api/v1/companies - Create
- GET /api/v1/companies/{id} - Read
- PUT /api/v1/companies/{id} - Update
- DELETE /api/v1/companies/{id} - Delete

2. Business rules:

- Creating company with duplicate name
- Deleting company with active OUs (if applicable)
- Invalid state transitions

3. Error handling:

- 404 Not Found
- 409 Conflict (duplicate)
- 422 Validation errors
- 400 Business logic errors


### Integration test structure:

src/test/java/.../company/
├── CompanyCommandControllerIT.java  (Integration tests)
└── CompanyQueryControllerIT.java    (Integration tests)


### Data Layer: Testcontainers

Recommendation: use Testcontainers with PostgreSQL for integration tests.

Why:

- Matches production (same DBMS)
- Catches PostgreSQL-specific issues (constraints, functions, JSON)
- No manual setup (containerized)
- Works in CI/CD
- Isolated per test

1. Testcontainers dependencies (PostgreSQL + JUnit integration)
2. Updated application-test.yml with PostgreSQL configuration
3. Created BaseIntegrationTest.java — base class for integration tests

### Recommendation

Use Testcontainers for integration tests because:

- Matches production (PostgreSQL)
- Catches database-specific issues
- No manual setup
- Works in CI/CD (if Docker is available)
