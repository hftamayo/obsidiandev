
## INTEGRATION TESTING

1. no me dio bola tratar de crear un maven configuration profile, tuve que usar la terminal
2. estos comandos si funcionaron

```

Correr test en especifico:
./mvnw clean verify -Ptest -Dspring.profiles.active=test -Dskip.tests=true -Dskip.integration.tests=false -Dskip.coverage.check=true -Dit.test=CompanyCommandRepositoryAdapterIT

./mvnw clean verify -Ptest -Dspring.profiles.active=test -Dskip.tests=true -Dskip.integration.tests=false -Dskip.coverage.check=true -Dit.test=CompanyQueryRepositoryAdapterIT

Correr test verificando los test coverage esperados en el setup de Jacoco:
./mvnw clean verify -Ptest -Dspring.profiles.active=test -Dskip.tests=true -Dskip.integration.tests=false -Dit.test=CompanyCommandRepositoryAdapterIT

Correr todos los tests:
./mvnw clean verify -Ptest -Dspring.profiles.active=test -Dskip.tests=true -Dskip.integration.tests=false

correrlos todos sin code coverage:
./mvnw clean verify -Ptest -Dspring.profiles.active=test -Dskip.tests=true -Dskip.integration.tests=false -Dskip.coverage.check=true


```

## CODE COVERAGE

```
Ejecutar Unit Tests + coverage:
- ./mvnw clean test -Ptest -Dspring.profiles.active=test -Dskip.integration.tests=true
  
Ejecutar Unit + Integrations Tests + coverage:
- ./mvnw clean test -Ptest -Dspring.profiles.active=test

ejecutar Unit only without coverage:
- ./mvnw clean test -Ptest -Dspring.profiles.active=test -Dskip.integration.tests=true -Dskip.coverage.check=true

Cuando solicito code coverage el repo esta en:ROOTDIR/target/site/jacoco/index.html


```


## Analisis del primer code coverage de Company:

### Resultados


![[Pasted image 20260312121151.png]]


una mejor vista:

![[Pasted image 20260312121238.png]]

### Interpretacion


Your coverage is **decent and meaningful**, especially in the core domain and some application pieces.

### Totals
- **Instruction coverage:** `64%`
- **Branch coverage:** `63%`
- **Lines covered:** `427`
- **Lines missed:** `135`
- **Methods covered:** `148`
- **Methods missed:** `63`
- **Classes covered:** `33 total`, `14` with misses

That’s not bad at all. It looks like you already test the **business core** much better than the surrounding plumbing, which is usually the right instinct.

---

## What looks strong

These areas are in very good shape:

### Excellent coverage
- `features.companies.domain` → **100%**
- `shared.application.result` → **100%**
- `shared.web.constants` → **100%**
- `shared.application.errors.UnknownErrorHandler` → **100%**
- `shared.web.correlation.CorrelationUtils` → **100% instructions**, **90% branches**
- `features.companies.adapters.web.mapper.CompanyResponseMapper` → **91%**
- `features.companies.adapters.persistence` package → **85%**
- `features.companies.adapters.web.query.CompanyQueryController` → **84%**
- `features.companies.application.usecases` package → **74%**

### Interpretation
This suggests:
- your **domain logic is properly exercised**
- **read/query paths** are healthier than write/command paths
- mapping and repository adapter coverage is quite solid

That’s a very respectable foundation.

---

## What looks weak / missing

Here’s where coverage is clearly thin.

### Completely untested or effectively untested
#### `shared.web.error` package → **0%**
Includes:
- `BusinessLogicError`
- `UnknownError`
- `ValidationError`
- `ResourceNotFoundError`
- `DuplicateResourceError`
- `ErrorLogEventDescriptor`

This is the biggest red flag in the report.  
Not because these are always “complex”, but because error handling is exactly where production likes to get creative.

#### `shared.web.jackson` package → **0%**
Includes:
- `TrimmingStringDeserializer`
- `JacksonTrimmingConfig`

If you rely on request trimming / sanitization, this deserves tests. These classes often look tiny... until whitespace breaks a controller test at 2 AM.

#### Other 0% areas
- `AbsencesbobeApplication` → **0%**
- `shared.infrastructure.audit.AuditorAwareConfig` → **0%**
- `shared.web.factory.ErrorLogEventFactory` → **0%**

These are not equally urgent, though:
- `AbsencesbobeApplication`: usually low priority unless you care about smoke tests
- `AuditorAwareConfig`: medium priority if auditing matters functionally
- `ErrorLogEventFactory`: medium, especially if error payload/logging consistency matters

---

## The most important class to improve next

### `CompanyCommandController`
Coverage:
- **45% instruction**
- **70% branch**

This stands out because:
- controllers are part of the public contract
- command flows usually carry validation, status codes, error mapping, payload edge cases
- your query controller is already at **84%**, so command coverage is lagging behind

### `CompanyCommandService`
Coverage:
- **74% instruction**
- **64% branch**
- still has a meaningful amount of missed complexity

This is probably the **best ROI target** after the controller:
- likely contains core branching
- branch coverage suggests some negative flows are still missing

If I had to prioritize just two things:
1. **Add tests for `CompanyCommandController`**
2. **Add missing branch/negative-path tests for `CompanyCommandService`**

---

## Coverage patterns I noticed

### 1. Query side is healthier than command side
- `CompanyQueryController` → **84%**
- `CompanyCommandController` → **45%**

That usually means:
- happy-path read operations are covered
- write/update/delete validation and error scenarios are under-tested

### 2. Pure/domain code is covered better than framework glue
That’s actually good engineering behavior.

But some framework glue is still worth testing when it affects behavior:
- Jackson config/deserializer
- error response objects/factories
- auditing config if it changes stored metadata

### 3. Several DTO/config/error classes show 0%
Not all 0%s are equally bad.

#### Usually acceptable-ish to leave low:
- bootstrapping class
- simple DTOs with no logic
- Lombok-generated boilerplate carriers

#### Not great to leave low:
- custom serializers/deserializers
- error translation / API error models
- factories that shape API responses or log payloads
- controller command paths

---

## Practical recommendations

## Highest priority
### 1. Test `CompanyCommandController`
Focus on:
- valid create/update/delete requests
- invalid payloads
- validation failures
- duplicate resource scenarios
- not found scenarios
- expected HTTP status codes and response body shape

### 2. Add negative-path tests for `CompanyCommandService`
Focus on branches like:
- duplicate company
- missing company
- invalid transitions
- repository failure mapped into application result/error
- optional/nullable field handling

### 3. Cover `shared.web.error`
At least verify:
- produced error payload structure
- code/message/status consistency
- validation error formatting
- resource-not-found and duplicate-resource responses

These tests often provide a lot of confidence with relatively little code.

---

## Medium priority
### 4. Test `TrimmingStringDeserializer` and `JacksonTrimmingConfig`
Especially if you expect:
- leading/trailing spaces to be removed
- blank strings to stay blank or become null
- request JSON binding to behave consistently

A couple of focused JSON serialization/deserialization tests can wipe out most of that red quickly.

### 5. Test `ErrorLogEventFactory`
If it builds diagnostic objects, verify:
- mapped fields
- null handling
- formatting consistency

### 6. Test `AuditorAwareConfig`
Only if auditing affects persisted business data or compliance expectations.

---

## Low priority
### 7. `AbsencesbobeApplication`
I would not chase coverage here just to make the chart prettier.

A smoke test is fine if you want:
```java
@SpringBootTest
class ApplicationContextTest {
    @Test
    void contextLoads() {
    }
}
```


But this is a **nice-to-have**, not where your testing energy should go first.

---

## If your goal is to improve the number efficiently

### Fastest route to raise overall coverage
Target these in order:

1. `CompanyCommandController`
2. `shared.web.error`
3. `shared.web.jackson`
4. `CompanyCommandService` missing branches
5. `ErrorLogEventFactory`

That combination should improve both:
- **instruction coverage**
- **branch coverage**

without wasting effort on low-value boilerplate.

---

## My honest verdict

### Good news
Your report does **not** look like a neglected project.
It looks like a project where:
- the **domain is tested**
- query behavior is tested
- persistence mapping is tested
- but **API write/error/config edges** are still catching a free ride

### The main weakness
Your weakest area is not the business model — it’s the **boundary behavior**:
- command controllers
- error handling
- request serialization/deserialization
- supporting factories/config

That’s a very fixable situation.

---

## Short version

### Strong
- domain logic
- result abstraction
- query controller
- response mapper
- persistence adapters

### Weak
- command controller
- error package
- Jackson trimming config/deserializer
- auditing/bootstrap/factory support classes

### Best next move
Start with:
- **`CompanyCommandController` tests**
- **`CompanyCommandService` branch tests**
- **`shared.web.error` tests**

