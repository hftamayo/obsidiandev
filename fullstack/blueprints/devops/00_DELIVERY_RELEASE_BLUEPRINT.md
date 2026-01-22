## Delivery & Release (API behind Ingress) — How to Think About It

### 1) Pipeline concerns (what the pipeline is responsible for)
A delivery pipeline isn’t just “deploying”—it’s a chain of responsibilities. A clear mental model is:

- **Build & test**: compile, unit/integration tests, linting, security scans.
- **Package**: build and publish the container image, version the Helm chart (or at least produce a chart artifact), and record exactly what will be deployed.
- **Deploy**: apply the desired state to Kubernetes (for you: `helm upgrade --install ...` into a target namespace).
- **Release strategy**: control *how* the new version is rolled out and *how traffic is shifted* (rolling/blue-green/canary), including rollout speed and rollback rules.
- **Verification**: automated checks after (and sometimes during) rollout—smoke tests, API probes, and SLO-like signals (error rate/latency) to decide whether to continue, pause, or roll back.

The strategies you listed (Rolling, Blue/Green, Canary, etc.) are mainly about the **Deploy + Release strategy + Verification** portion—i.e., what happens once a new version is ready to be rolled out.

---

### 2) Deployment strategies are not “GitOps-only”
These strategies are **independent** of whether you deploy via:
- **Helm in CI** (your current plan), or
- **GitOps** (future evolution like Argo CD/Flux).

GitOps changes *how changes are applied* (pull-based reconciliation, drift detection, auditability). It does **not** restrict you to a specific deployment strategy. Rolling, recreate, canary, and blue/green can be done in both worlds—the difference is usually how cleanly and safely you can automate them.

---

### 3) What Helm-in-CI can do well (and what needs extra patterns)
With **Helm directly in CI**, you can do:

- **Rolling deployment** (best default): this is the Kubernetes Deployment default behavior. It’s stable, well understood, and requires minimal extra infrastructure.
- **Recreate deployment**: Kubernetes can do this too, but it causes downtime because it stops the old version before starting the new one. It’s typically reserved for non-critical services or special cases.
- **Blue/Green and Canary**: possible, but usually require **traffic control** patterns in front of your pods:
    - two Deployments or two “versions” running side by side
    - a controlled switch or split of traffic using **Ingress rules**, a **Gateway/API gateway**, or a **service mesh**
    - stronger verification (metrics-driven) to decide whether to continue or revert  
      In practice, teams often adopt a **progressive delivery controller** later (or implement careful routing patterns) because it reduces the amount of custom scripting in CI.

---

### 4) Practical mapping to your current setup (1 cluster, 3 namespaces, Helm-in-CI)
Given your environment model (single cluster, stable namespaces, feature branches deploy only to experimental) the recommended progression is:

- **Default for all namespaces (experimental/staging/production): Rolling**
    - Minimal complexity
    - Works great for APIs behind Ingress
    - Rollback is straightforward if you detect problems early

- **Recreate**: only when you explicitly accept downtime
    - Usually not appropriate for production APIs behind Ingress
    - Can be acceptable for internal tools, jobs, or early prototypes

- **Canary / Blue-Green**: introduce later, mainly for **production**
    - Adopt when production risk is higher (more users, stricter uptime)
    - Requires mature **observability** (see next section) and some form of controlled traffic shifting
    - Typically used when you want to reduce rollout risk beyond what rolling updates provide

---

### 5) Why “API behind Ingress” matters for release strategy
For an API, the key question is: **how do we control and observe user traffic during a rollout?**

- With **Rolling**, you’re effectively shifting traffic gradually because pods are replaced over time, but traffic routing is still relatively “simple” (requests go to whichever healthy pods exist at that moment).
- With **Canary** and **Blue/Green**, you explicitly control traffic:
    - **Canary**: send a small percentage of Ingress traffic to the new version first, watch metrics, then increase.
    - **Blue/Green**: run two complete versions and switch traffic “all at once” after validation.

Ingress is often the natural place to implement traffic switching/splitting (directly or via additional components), which is why these strategies become more achievable once you invest in routing and metrics.

---

### 6) Observability and verification: the “unlock” for safer releases
Progressive strategies (and even safe rolling updates) are only as good as your ability to detect problems quickly. For an API behind Ingress, verification typically focuses on:

- **Golden signals**: request rate, error rate (HTTP 5xx/4xx patterns), latency (p95/p99), saturation (CPU/memory).
- **Kubernetes health**: readiness probes (do not send traffic until ready), liveness probes (restart stuck pods), and rollout status.
- **Automated smoke tests**: call a few critical endpoints after deploy to confirm basic functionality.
- **Rollback conditions**: if error rate/latency crosses a threshold, stop/pause the rollout and roll back.

This is why canary/blue-green is usually adopted **after** you have reliable metrics and alerting: it lets you make rollout decisions based on evidence, not vibes.

---

### 7) Recommended “Phase 1 → Phase 2” adoption path (fits your blueprint)
**Phase 1 (now, Helm-in-CI, minimal complexity):**
- Use **Rolling** everywhere (experimental/staging/production).
- Add strong **verification** steps (readiness probes, smoke tests, basic metrics checks).
- Use manual approval gates for production if needed.

**Phase 2 (later, when you want lower prod risk):**
- Introduce **Canary** or **Blue/Green** for production.
- Add/standardize traffic management at the Ingress layer and strengthen metrics-driven rollout decisions.
- Optionally move to GitOps for better auditability and reconciliation—without changing your conceptual model.

This keeps your current setup simple and stable while giving you a clear evolutionary path to safer, more sophisticated releases.