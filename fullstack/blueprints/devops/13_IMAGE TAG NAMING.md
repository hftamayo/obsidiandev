
This is related to migrating GITOPS_ARGOCD.

Use **GitOps image tag promotion**.

Your service pipeline should do this after pushing the image:

```plain text
backend repo CI
→ build image hftamayo/absencesbobe:abc1234
→ push to Docker Hub
→ update deployment repo values-dev.yaml
→ commit/PR change
→ aws_dev_deploy_codebase.yml validates
→ ArgoCD detects Git change
→ ArgoCD deploys image abc1234
```


So the “dynamic connection” is **not Docker Hub → ArgoCD directly**.  
It is:

```plain text
Docker image tag → written into Git → ArgoCD syncs Git
```


## Recommended approach

Update the **build-and-push pipeline of each microservice repo** to also update your deployment repo.

Example target file:

```plain text
absences_boffice_deployments/k8s/helm/helm-chart-app/values-dev.yaml
```


Example desired result:

```yaml
backend:
  image:
    repository: hftamayo/absencesbobe
    tag: "abc1234"
```


For frontend:

```yaml
frontend:
  image:
    repository: hftamayo/absencesbofe
    tag: "def5678"
```


For tools/nginx:

```yaml
nginx:
  image:
    repository: hftamayo/nginxk8s
    tag: "789abcd"
```


## What changes

### Old way

```plain text
Human copies Docker tag manually
→ runs deploy workflow with tag input
→ Ansible/Helm deploys
```


### New way

```plain text
Service pipeline calculates short SHA
→ pushes Docker image
→ writes same short SHA into values-dev.yaml
→ ArgoCD deploys
```


## Best options

### Option A — auto-commit to deployment repo

Fastest for dev.

```plain text
backend repo pipeline commits backend.image.tag=abc1234 directly to main
```


### Option B — open PR to deployment repo

Better for stg/apps.

```plain text
backend repo pipeline opens PR:
"Promote backend abc1234 to dev"
```


You review/merge, then ArgoCD deploys.

## Alternative

Use **ArgoCD Image Updater**, but for your case I’d start with the simpler GitOps approach:

```plain text
CI updates values-dev.yaml
```


because it is explicit, auditable, and avoids `latest`.


### No manual intervention: moving to ArgoCD Image Update


Yes. There are other ways, and for a more production-ready setup the best candidate is usually:

## Recommended alternative: Argo CD Image Updater

Instead of your service pipeline editing `values-dev.yaml`, you let **Argo CD Image Updater** watch your image registry and update the running Argo CD application when a new allowed image tag appears.

Flow:

```plain text
backend CI
→ build image hftamayo/absencesbobe:abc1234
→ push to Docker Hub
→ Argo CD Image Updater detects abc1234
→ updates Argo CD app image tag
→ Argo CD syncs deployment
```


So the service repo no longer needs to update the deployment repo directly.

---

## Important distinction

There are two modes for Argo CD Image Updater:

### 1. Git write-back mode

```plain text
Docker Hub → Image Updater → commits new tag to Git → Argo CD syncs Git
```


This still modifies Git, but automatically.

Pros:
- Still pure GitOps.
- Auditable.
- Rollback is easy.
- Production-friendly.

Cons:
- It still creates Git commits automatically.

This is the best balance for production.

---

### 2. Argo CD API write-back mode

```plain text
Docker Hub → Image Updater → updates Argo CD Application directly
```


Pros:
- No Git commit needed.
- Fully automatic.

Cons:
- Less GitOps-pure.
- Git no longer fully represents the live deployed version.
- Rollback/audit is weaker.

I would **not** choose this for production unless you accept Git not being the full source of truth.

---

## For your case, I would use this model

### Dev

Use full automation:

```plain text
merge to main
→ build image with short SHA
→ push image
→ Argo CD Image Updater auto-promotes latest allowed dev tag
→ Argo CD syncs
```


No manual PR.

### Stage

Use controlled automation:

```plain text
backend release tag v1.4.2
→ build image hftamayo/absencesbobe:v1.4.2
→ Image Updater opens/commits Git update
→ Argo CD syncs stage
```


You can require manual approval through branch protection if desired.

### Production/apps

Use release promotion, not every commit:

```plain text
approved release v1.4.2
→ image already exists
→ promote tag to production Git config
→ Argo CD syncs production
```


For production, I would avoid deploying every SHA automatically.

---

## Specific options ranked

### Option 1 — Argo CD Image Updater with Git write-back

**Best production-ready GitOps option.**

You annotate your Argo CD Application with image rules, for example conceptually:

```yaml
argocd-image-updater.argoproj.io/image-list: backend=hftamayo/absencesbobe
argocd-image-updater.argoproj.io/backend.update-strategy: newest-build
argocd-image-updater.argoproj.io/write-back-method: git
```


Then Image Updater updates the Helm value automatically.

Use this if you want:

```plain text
no human editing values files
but still Git-auditable deployments
```


---

### Option 2 — Argo CD Image Updater with semver tags

Better for stage/prod.

Use image tags like:

```plain text
v1.4.0
v1.4.1
v1.4.2
```


Then configure Image Updater to only accept semver tags:

```plain text
only deploy tags matching v1.x.x
```


This avoids random commit SHAs reaching production.

---

### Option 3 — Use mutable environment tags

Example:

```plain text
hftamayo/absencesbobe:dev
hftamayo/absencesbofe:dev
```


Pipeline does:

```plain text
build abc1234
push abc1234
also retag as dev
push dev
```


Then Helm always uses:

```yaml
tag: dev
```


But this is **not recommended** for serious GitOps.

Why?

Because Git says:

```yaml
tag: dev
```


but the actual image behind `dev` can change any time. That hurts traceability and rollback.

Acceptable for local/dev only, not stage/prod.

---

### Option 4 — CI calls Argo CD directly

Pipeline does:

```plain text
argocd app set absencesbo-dev --helm-set backend.image.tag=abc1234
argocd app sync absencesbo-dev
```


This is operationally simple but less GitOps.

I would avoid this if your goal is GitOps.

---

## My specific recommendation

Use this:

```plain text
Dev:
Argo CD Image Updater + Git write-back + SHA tags

Stage:
Argo CD Image Updater + Git write-back + semver/release tags + optional PR/approval

Apps/Prod:
manual release approval + Git write-back/PR promotion using semver tags
```


In short:

```plain text
Do not manually update values files.
Do not rely on latest.
Do not let production deploy every commit SHA automatically.
Use Argo CD Image Updater with Git write-back.
```


That gives you automation **and** keeps Git as the source of truth.