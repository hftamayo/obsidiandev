## Observability Blueprint (OSS, Grafana Stack: Prometheus + Grafana + Loki + Tempo)

This is a vendor-neutral, open-source observability baseline centered on the “Grafana stack”: **Prometheus** (metrics), **Grafana** (dashboards), **Loki** (logs), **Tempo** (traces), plus **Alertmanager** and **OpenTelemetry** for clean ingestion and correlation.

---

## 1) Components & responsibilities (what each thing is for)

### Metrics
- **Prometheus**: scrapes metrics endpoints and stores time-series data.
- **kube-state-metrics**: exposes Kubernetes object state (Deployments, Pods, HPA, etc.).
- **node-exporter**: node-level CPU/mem/disk/network metrics.
- **Alertmanager**: routes alerts (Slack/email/etc), groups/dedupes, supports silences.

### Dashboards
- **Grafana**: single UI for dashboards across metrics + logs + traces.

### Logs
- **Loki**: log store optimized for label-based search + cheap-ish storage.
- **Promtail** (or Grafana Agent/Alloy): log shipper that tails container logs and sends to Loki.

### Traces
- **Tempo**: trace store (high scale, low cost) that integrates tightly with Grafana.
- **OpenTelemetry Collector**: receives traces (and optionally metrics/logs), processes/batches them, exports to Tempo.

> Strong starting rule: **Prometheus scrapes metrics directly**, **OTel Collector receives traces**, **Promtail ships logs**.

---

## 2) Data conventions (this is what keeps it usable at scale)

### 2.1 Common labels/tags everywhere
Use these consistently in metrics/log labels and traces:
- `env`: experimental | staging | production
- `namespace`
- `service`: stable service name (e.g., `backend`, `frontend`)
- `version`: git sha or semver

Avoid:
- user IDs, request IDs, full URLs as metric labels (cardinality explosion)

### 2.2 Correlation IDs (must-have)
- **request_id**: generated at the edge (ingress) or at the backend if missing.
- **trace_id**: from OpenTelemetry tracing.

Goal: from a Grafana panel, you can pivot:
**metric spike → related logs → specific trace**.

---

## 3) What you instrument in apps (minimum viable)
### Backend (required)
- Metrics endpoint: `/metrics` (Prometheus format)
- RED metrics:
    - request rate
    - error rate (5xx + important 4xx)
    - duration histogram (p50/p95/p99)
- Tracing:
    - OTel SDK instrumentation for HTTP server + outbound HTTP + DB client
    - propagate `traceparent`
- Logs:
    - JSON structured logs
    - include `service`, `env`, `version`, `request_id`, `trace_id`

### Frontend / Nginx (recommended)
- Ingress metrics will cover the user-facing side.
- Keep access/error logs; include request_id and (if possible) trace propagation to backend.

---

## 4) Dashboards to create/import on Day 1
### 4.1 Cluster Overview
- Node readiness, CPU/mem/disk pressure
- Pod restarts, CrashLoopBackOff count
- Pending pods / scheduling failures

### 4.2 Namespace / Environment Overview
- CPU/memory usage vs requests/limits
- Top pods by CPU/mem
- CPU throttling, OOMKills

### 4.3 Ingress Overview
- RPS by host/path
- 4xx/5xx
- latency p95/p99
- upstream errors/timeouts

### 4.4 Service Overview (per service)
- RED panels
- replicas desired/available
- HPA status (current vs max)
- log panel (Loki) filtered to service+env
- trace exemplars / trace links (when enabled)

---

## 5) Alerts (small set, high signal)
Start with alerts that are almost always actionable.

### 5.1 Platform/K8s alerts (high priority in prod)
- Node NotReady (sustained)
- Pods pending too long (capacity/scheduling)
- PVC usage > 80% (warn) and > 90% (page)
- DNS/Ingress/Prometheus components down (your “platform is blind” alert)

### 5.2 Workload alerts (service-owned)
- CrashLoopBackOff / high restart rate
- Deployment replicas unavailable / rollout stuck
- High 5xx rate (ingress + backend)
- Latency p95/p99 above threshold for key endpoints

### 5.3 Saturation alerts
- CPU throttling high (often indicates too-low CPU limits)
- Memory near limit / OOMKills
- HPA pinned at max replicas (sustained)

Add runbook links to every paging alert.

---

## 6) Logging blueprint (Loki-specific guidance)
### 6.1 Label strategy (important)
Keep Loki labels low-cardinality:
- good labels: `env`, `namespace`, `service`, `pod`, `container`
- avoid labels like `request_id`, `user_id`, `path`, `full_url`

Put high-cardinality fields **inside the log line JSON**, not as labels. You can still filter/search them, but they won’t blow up indexing.

### 6.2 Log content standards
Backend JSON log fields (suggested minimum):
- `ts`, `level`, `msg`
- `service`, `env`, `version`
- `request_id`, `trace_id`
- `http_method`, `route` (route template, not raw URL), `status`, `duration_ms`

---

## 7) Tracing blueprint (Tempo + OTel)
### 7.1 Sampling (don’t bankrupt yourself)
- Start with **probabilistic sampling** (e.g., 1–10% in production; higher in staging)
- Keep error traces with higher priority if you later add tail sampling (optional)

### 7.2 Trace-to-logs and trace-to-metrics
- Ensure logs include `trace_id`
- Configure Grafana so a trace view can link to Loki logs for that trace_id/time window
- Use “exemplars” where supported (nice-to-have)

---

## 8) Retention & scale knobs (set expectations early)
- Prometheus retention: 7–15 days to start
- Loki retention: start 7–14 days; increase for prod later
- Tempo retention: start short (3–7 days) unless you really need longer trace history
- Plan for later:
    - long-term metrics: Thanos/Mimir
    - log storage scaling: object storage backend for Loki
    - multi-tenancy: separate by namespace/env or Grafana orgs if needed

---

## 9) Phased rollout plan (practical)
**Phase 1: Metrics + Alerting + Grafana dashboards**
- Prometheus + Alertmanager + Grafana
- kube-state-metrics + node-exporter
- Ingress dashboards + core alerts

**Phase 2: Central logs**
- Promtail → Loki
- Standardize backend JSON logs + request_id

**Phase 3: Tracing**
- OTel SDK in backend + OTel Collector → Tempo
- Add trace_id to logs; enable Grafana correlations

---
## Observability topology (single stack, cluster-wide coverage)

### 1) Namespace & ownership
- Create a dedicated namespace: **`observability`**
- One shared stack runs there:
  - Prometheus + Alertmanager + Grafana
  - Loki (+ Promtail/Agent)
  - Tempo (+ OpenTelemetry Collector)

**Ownership rule**
- Platform team owns the observability stack lifecycle (install/upgrade/config).
- App teams own:
  - app instrumentation (metrics/log fields/tracing)
  - service dashboards (or at least dashboards-as-code PRs)
  - service alerts + runbooks

---

## 2) Data separation (how you avoid “prod drowned by experimental”)
Even with one stack, you keep environments separated logically:

### 2.1 Mandatory tags everywhere
Enforce consistent identifiers:
- `env`: experimental | staging | production
- `namespace`
- `service`
- `version`

These must exist in:
- Prometheus labels (as much as feasible)
- Loki labels (at least env/namespace/service)
- Tempo resource attributes (service.name + env)

### 2.2 Grafana access control
- Use Grafana **folders + teams/roles**:
  - “Production” dashboards folder restricted
  - Staging/Experimental can be wider
- Make prod alerts configurable only by restricted roles.

---

## 3) Collection model (how “one stack watches all namespaces”)
### 3.1 Metrics (Prometheus)
- Prometheus runs in `observability` and scrapes:
  - cluster components (kube-state-metrics, node-exporter)
  - ingress metrics
  - application `/metrics` endpoints across namespaces

Best practice:
- Use **ServiceMonitors/PodMonitors** so teams onboard services declaratively.
- Require that each app exposes `/metrics` on a dedicated port.

### 3.2 Logs (Loki)
- Run Promtail/Agent as a **DaemonSet** (cluster-wide) so it collects logs from all nodes/pods.
- Loki runs centrally in `observability`.
- Keep Loki labels low-cardinality:
  - `env`, `namespace`, `service`, `pod`, `container` (good)
  - avoid request_id/user_id/path as labels (bad)

### 3.3 Traces (Tempo)
- Apps send traces to an **OpenTelemetry Collector** service in `observability`.
- Collector exports to Tempo (also in `observability`).
- Standardize trace propagation (`traceparent`) and include `trace_id` in logs to enable correlations.

---

## 4) Alerting model (central Alertmanager, env-aware routing)
- One Alertmanager instance handles everything.
- Alerts must include: `severity`, `env`, `service`, and a `runbook_url`.

Routing rule of thumb:
- `production` + `severity=page` → on-call/pager
- `staging` → ticket/Slack channel
- `experimental` → low-noise (only “platform is broken” alerts)

Also add inhibition:
- If a node is NotReady, suppress the “pod down” spam from that node.

---

## 5) Reliability & “don’t lose observability during incidents”
Because it’s one shared stack, protect it:
- Give observability workloads sane **requests/limits**
- Prefer **PDBs** for Grafana/Alertmanager/Prometheus where applicable
- Store data on persistent volumes where needed (Prometheus, Loki indexes/chunks depending on mode)

---

## 6) Definition of Done (for the shared stack)
The stack is “ready” when:
- Grafana has:
  - Cluster overview dashboard
  - Namespace/environment overview dashboards
  - Ingress dashboard
- Alertmanager is wired to notification channels and has silencing configured
- Loki is receiving logs from all namespaces, queryable by `env` and `service`
- Tempo is receiving traces (at least from backend), and Grafana can pivot:
  - dashboard panel → logs (Loki) → trace (Tempo)
