
## BACKEND

### SAST:
- java source code: SonarCloud
- dep vulns: Synk
- secret scanning: Gitleaks
- Dockerfile: gitleaks
- container image: trivy

### DAST:
- runtime: OWASP ZAP
- APi sec testing: ZAP Api Scan
- auth/session/rate-limit: custom integration tests


ci.yml passes
        |
        v
docker_build_push.yml runs


### ci.yml:
checkout
setup Java 21
cache Maven dependencies
run unit + integration tests
generate coverage
run dependency/security checks
optionally run CodeQL/Semgrep/Trivy filesystem scan

### docker_build_push:
  - build image
  - scan image
  - push image


### dast:
 - deploy/start app
 - run OWASP ZAP
 - docker compose up app + postgres
- wait for /actuator/health
- run OWASP ZAP baseline scan against localhost:8080

___

## STEPS:

### phase 1: CI Foundation
  Owner: ci.yml

  - Maven verify
  - Unit tests
  - Integration tests with Testcontainers
  - Coverage check

### phase 2: SAST/dep scan:
  Owner: ci.yml

  - Trivy filesystem scan
  - GitHub CodeQL
  - OWASP Dependency-Check
  - Gitleaks

Starts with:
./mvnw -Pci clean verify
Trivy filesystem scan
Gitleaks scan

then:

CodeQL
OWASP Dependency-Check

### phase 3: container security:
  Owner: docker_build_push.yml

  - Build image locally
  - Scan image with Trivy
  - Push only if scan passes

### phase 4: DAST:
  Owner: dast.yml or later deployment workflow

  - Start/deploy the app
  - Wait for health endpoint
  - Run OWASP ZAP against running app
  - Publish report
