
## INTEGRATION TESTING

1. no me dio bola tratar de crear un maven configuration profile, tuve que usar la terminal
2. estos comandos si funcionaron

```

Correr test en especifico:
./mvnw clean verify -Ptest -Dspring.profiles.active=test -Dskip.tests=true -Dskip.integration.tests=false -Dskip.coverage.check=true -Dit.test=CompanyCommandRepositoryAdapterIT

./mvnw clean verify -Ptest -Dspring.profiles.active=test -Dskip.tests=true -Dskip.integration.tests=false -Dskip.coverage.check=true -Dit.test=CompanyQueryRepositoryAdapterIT

Correr test verificando los test coverage esperados en el setup de Jacoco:
./mvnw clean verify -Ptest -Dspring.profiles.active=test -Dskip.tests=true -Dskip.integration.tests=false -Dit.test=CompanyCommandRepositoryAdapterIT

Correr todos los unit tests:
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

#### Other 0% areas
- `AbsencesbobeApplication` → **0%**
- `shared.infrastructure.audit.AuditorAwareConfig` → **0%**
- `shared.web.factory.ErrorLogEventFactory` → **0%**

These are not equally urgent, though:
- `AbsencesbobeApplication`: usually low priority unless you care about smoke tests
- `AuditorAwareConfig`: medium priority if auditing matters functionally

---

## Practical recommendations

### 6. Test `AuditorAwareConfig`
Only if auditing affects persisted business data or compliance expectations.

---

## Despues de mejorar el code coverage

![[Pasted image 20260316105728.png]]