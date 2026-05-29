
Hey old friend — for this feature, you need to connect **three pieces** cleanly:

1. **Each repo builds and pushes its Docker image when it changes**
2. **Images are tagged in a way your deployment can identify the newest version**
3. **Your k3s deployment/Helm release pulls and uses that new image**

Given your current Helm values, you are already close: each component has a `repository`, `tag`, and `pullPolicy`. The main thing missing is an automated image versioning + deployment update flow.

---

## Recommended target flow

For each repo:

```plain text
git push / merge to main
        ↓
CI pipeline builds Docker image
        ↓
CI tags image with commit SHA and/or semantic version
        ↓
CI pushes image to Docker Hub
        ↓
Deployment pipeline updates Helm image tag
        ↓
k3s pulls the new image during helm upgrade
```


---

## 1. Add Docker Hub credentials to each repo CI

In each repository, add secrets for Docker Hub.

For GitHub Actions, for example:

```plain text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```


Use a Docker Hub **access token**, not your account password.

---

## 2. Build and push image when each repo changes

Each repo should have its own pipeline that runs on changes to the main branch.

Example GitHub Actions workflow:

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Set image tag
        run: echo "IMAGE_TAG=${GITHUB_SHA::7}" >> "$GITHUB_ENV"

      - name: Build and push Docker image
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: |
            hftamayo/absencesbobe:${{ env.IMAGE_TAG }}
            hftamayo/absencesbobe:latest
```


For each repo, change the image name:

```plain text
Backend:  hftamayo/absencesbobe
Frontend: hftamayo/absencesbofe
Nginx:    hftamayo/nginxk8s
Database: hftamayo/pgdevelopment
```


If the repos are separate, each one should have its own workflow with the correct Docker Hub repository.

---

## 3. Avoid relying only on `latest`

You *can* push `latest`, but I would not use only `latest` in Kubernetes.

Better:

```plain text
hftamayo/absencesbobe:latest
hftamayo/absencesbobe:a1b2c3d
```


Use the commit SHA tag for deployment:

```yaml
backend:
  image:
    repository: hftamayo/absencesbobe
    tag: "a1b2c3d"
```


Why?

Because Kubernetes may not redeploy correctly if the tag stays the same. A new immutable tag forces a clean rollout.

---

## 4. Update your Helm values during deployment

Right now your chart uses fixed tags like:

```yaml
backend:
  image:
    tag: "0.0.2"
```


For automatic deployments, your deployment pipeline should override these at deploy time instead of manually editing the file every time.

Example:

```shell script
helm upgrade --install absencesbo ./chart \
  --namespace absencesbo \
  --create-namespace \
  --set backend.image.tag="$BACKEND_IMAGE_TAG" \
  --set frontend.image.tag="$FRONTEND_IMAGE_TAG" \
  --set nginx.image.tag="$NGINX_IMAGE_TAG"
```


If only the backend changed:

```shell script
helm upgrade --install absencesbo ./chart \
  --namespace absencesbo \
  --reuse-values \
  --set backend.image.tag="$BACKEND_IMAGE_TAG"
```


That lets one repo update only its own image without changing the others.

---

## 5. Change image pull policy strategy

Your current values use:

```yaml
pullPolicy: IfNotPresent
```


That is fine when you use immutable tags like commit SHAs or versions.

If you insist on using `latest`, then use:

```yaml
pullPolicy: Always
```


Recommended setup:

```yaml
tag: "a1b2c3d"
pullPolicy: IfNotPresent
```


Less recommended setup:

```yaml
tag: "latest"
pullPolicy: Always
```


For k3s deployments, immutable tags are more predictable.

---

## 6. Make sure k3s can pull private images if needed

You already have:

```yaml
global:
  imagePullSecrets:
    - name: regcred
```


If your Docker Hub repositories are private, you need this secret in the target namespace:

```shell script
kubectl create secret docker-registry regcred \
  --namespace absencesbo \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username="$DOCKERHUB_USERNAME" \
  --docker-password="$DOCKERHUB_TOKEN" \
  --docker-email="your-email@example.com"
```


If the images are public, `imagePullSecrets` is optional.

---

## 7. Deployment options

You have two main architecture choices.

---

### Option A — Each repo deploys itself to k3s

Each repo pipeline does:

```plain text
build image → push to Docker Hub → helm upgrade only that component tag
```


Example backend repo:

```shell script
helm upgrade --install absencesbo ./chart \
  --namespace absencesbo \
  --reuse-values \
  --set backend.image.repository=hftamayo/absencesbobe \
  --set backend.image.tag="${IMAGE_TAG}"
```


Pros:

- Simple
- Fast
- Each service deploys independently

Cons:

- Each repo needs access to the cluster
- Harder to coordinate frontend/backend compatibility

---

### Option B — Central deployment repo handles k3s

Each app repo does:

```plain text
build image → push to Docker Hub → trigger deployment repo
```


The deployment repo does:

```plain text
helm upgrade with new image tags
```


Pros:

- Cleaner separation
- Only deployment repo has k3s credentials
- Better audit trail
- Easier to promote dev/staging/prod

Cons:

- Slightly more setup

For your case, I would recommend **Option B** if you want this to grow cleanly.

---

## 8. Example backend pipeline with deployment

If the backend repo can access your k3s cluster, the workflow would look like this:

```yaml
name: Build Push Deploy Backend

on:
  push:
    branches:
      - main

env:
  IMAGE_REPOSITORY: hftamayo/absencesbobe
  RELEASE_NAME: absencesbo
  NAMESPACE: absencesbo

jobs:
  build-push-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout backend repo
        uses: actions/checkout@v4

      - name: Set image tag
        run: echo "IMAGE_TAG=${GITHUB_SHA::7}" >> "$GITHUB_ENV"

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push backend image
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: |
            ${{ env.IMAGE_REPOSITORY }}:${{ env.IMAGE_TAG }}
            ${{ env.IMAGE_REPOSITORY }}:latest

      - name: Configure kubeconfig
        run: |
          mkdir -p "$HOME/.kube"
          echo "${{ secrets.KUBE_CONFIG }}" > "$HOME/.kube/config"
          chmod 600 "$HOME/.kube/config"

      - name: Deploy backend to k3s
        run: |
          helm upgrade --install "$RELEASE_NAME" ./chart \
            --namespace "$NAMESPACE" \
            --create-namespace \
            --reuse-values \
            --set backend.image.repository="${IMAGE_REPOSITORY}" \
            --set backend.image.tag="${IMAGE_TAG}"
```


This assumes the Helm chart is available in that repo. If the chart is in a separate repo, add a checkout step for the chart repo.

---

## 9. If chart is in a separate deployment repo

Use two checkouts:

```yaml
- name: Checkout app repo
  uses: actions/checkout@v4
  with:
    path: app

- name: Checkout deployment repo
  uses: actions/checkout@v4
  with:
    repository: your-org/your-deployment-repo
    token: ${{ secrets.DEPLOY_REPO_TOKEN }}
    path: deploy
```


Then build from `app` and deploy from `deploy`:

```yaml
- name: Build and push image
  uses: docker/build-push-action@v6
  with:
    context: ./app
    push: true
    tags: |
      hftamayo/absencesbobe:${{ env.IMAGE_TAG }}
      hftamayo/absencesbobe:latest

- name: Deploy with Helm
  run: |
    helm upgrade --install absencesbo ./deploy/chart \
      --namespace absencesbo \
      --create-namespace \
      --reuse-values \
      --set backend.image.tag="${IMAGE_TAG}"
```


---

## 10. Important Kubernetes detail: force rollout when needed

If the image tag changes, Kubernetes will roll out automatically.

If the tag does **not** change, for example if you keep using `latest`, Kubernetes may not restart pods unless the pod template changes.

If you use `latest`, you may need:

```shell script
kubectl rollout restart deployment/absencesbo-backend -n absencesbo
```


But again, better solution: **use unique image tags**.

---

## 11. Recommended final setup for you

I would do this:

### In each app repo

- Build image on merge/push to `main`
- Push:
  - `hftamayo/component:<short-sha>`
  - `hftamayo/component:latest`
- Trigger deployment

### In deployment

- Run `helm upgrade --install`
- Override only the changed component tag
- Keep `pullPolicy: IfNotPresent`
- Use Docker Hub pull secret only if images are private

### In Helm values

Keep default stable tags, but allow CI to override them:

```yaml
backend:
  image:
    repository: hftamayo/absencesbobe
    tag: "0.0.2"
    pullPolicy: IfNotPresent
```


Then CI deploys:

```shell script
--set backend.image.tag="$IMAGE_TAG"
```


---

## Minimal checklist

You need to add/update:

- [ ] Dockerfile in each repo
- [ ] GitHub Actions/GitLab CI pipeline in each repo
- [ ] Docker Hub token secret in each repo
- [ ] Image tagging strategy, preferably commit SHA
- [ ] Helm deployment command using `--set component.image.tag=...`
- [ ] k3s kubeconfig secret for deployment pipeline
- [ ] Docker registry pull secret in k3s if Docker Hub images are private
- [ ] Optional central deployment repo for cleaner automation

The most important change is: **do not deploy a static tag forever**. Build with a new tag per repo update, push it, and pass that exact tag into your Helm upgrade.