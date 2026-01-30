A **Postgres StatefulSet** usually means: **running PostgreSQL on Kubernetes using a `StatefulSet` controller**, because PostgreSQL is a *stateful* workload (it needs durable storage and stable identity).

## What a StatefulSet is (in Kubernetes terms)

A **StatefulSet** is a Kubernetes workload type designed for apps that need:

1. **Stable network identity**  
   Pods get predictable names like:
    - `postgres-0`, `postgres-1`, …  
      and stable DNS like:
    - `postgres-0.postgres.<namespace>.svc.cluster.local`

2. **Stable, per-pod persistent storage**  
   Each pod gets its own **PersistentVolumeClaim (PVC)**, commonly created via `volumeClaimTemplates`.  
   If `postgres-0` restarts or gets rescheduled to another node, it **re-attaches the same volume**.

3. **Ordered startup/shutdown (and controlled rolling updates)**  
   Kubernetes can start/stop pods in a defined order, which matters for clustered systems and some replication setups.

For databases, these three properties are the big reason you pick StatefulSet over Deployment.

## What “Postgres StatefulSet” typically looks like

When someone says “Postgres StatefulSet”, they usually mean a setup that includes:

- A `StatefulSet` for PostgreSQL pods
- A `Service` (often “headless”) for stable DNS to each pod
- PVCs for data (one per pod)
- Config + secrets for:
    - `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD` (or better: mounted secrets)
- Health checks (`livenessProbe` / `readinessProbe`)
- Backup strategy (logical backups or volume snapshots)

## Important clarification: StatefulSet ≠ HA by itself

A **StatefulSet alone does not automatically make PostgreSQL highly available**.

- If you run **1 replica**: you get persistence + stable identity, but it’s still a single instance.
- If you run **multiple replicas**: PostgreSQL replication/failover is not “free”; you need tooling/logic (e.g., an operator) to manage:
    - primary/replica roles
    - replication configuration
    - failover and leader election
    - re-syncing replicas

That’s why in real production setups people often use a **Postgres Operator** (CrunchyData PGO, Zalando/Spilo, CloudNativePG, etc.) which *internally uses StatefulSets* (or similar primitives) but adds the database-specific brains.

## Why it’s a good fit for SQL data-layer “blueprints”

If your blueprint includes “Postgres on Kubernetes”, StatefulSet is the standard primitive because it provides:

- **Durability** (via PV/PVC)
- **Predictable addressing** (for replication clients, monitoring, etc.)
- **More controlled lifecycle** (start/stop order)

---
## Database on Kubernetes (PostgreSQL StatefulSet)

### Current baseline (single-instance PostgreSQL)
We run PostgreSQL as a **single-instance StatefulSet** (1 replica) per environment.

Key rules:
- **DB per service**: each service owns its database (no shared DB between services).
- **Environment isolation**: dev, staging, and production use separate PostgreSQL instances.
- **Schema management**: Flyway is the source of truth for the schema.
- **Hibernate**: `ddl-auto=validate` in dev/staging/prod (Hibernate must not mutate schema).

### Connectivity
The application connects to PostgreSQL through a Kubernetes **Service DNS name** (for example `absencesdb`).

All DB connection details must be injected via environment variables (ConfigMap/Secret):
- `DB_HOST`
- `DB_PORT`
- `DB_NAME`
- `DB_USERNAME`
- `DB_PASSWORD`

The application configuration should build the JDBC URL from these values (no hardcoded cluster DNS names).

### Migration strategy (Flyway)
We support two strategies; the default depends on the environment.

**Strategy A — Flyway on application startup**
- The application runs Flyway migrations during startup.
- Recommended for: **development**
- Notes: simplest setup; relies on Flyway locking to prevent concurrent migrations.

**Strategy B — Flyway via a dedicated migration Job (recommended for staging/prod)**
- A Kubernetes Job runs Flyway migrations before the application rollout/upgrade.
- Recommended for: **staging and production**
- Benefits:
  - clean control of rollout order
  - avoids multiple application replicas attempting migrations during deployments

### Operational notes (single instance)
- **Persistence**: PostgreSQL must use a PersistentVolumeClaim (PVC).
- **Backups**: define and test a backup + restore procedure early (scheduled backups + verified restore).
- **Probes**: use `startupProbe`/`readinessProbe` appropriately (PostgreSQL can take time to become ready).
- **Capacity**: keep connection pools conservative and monitor DB resource usage.

---

## Road ahead: HA (High Availability) PostgreSQL

When uptime requirements grow (node failures, maintenance windows, stricter SLAs), the next step is migrating from a single-instance StatefulSet to an **operator-managed HA PostgreSQL** setup.

### What changes with HA
- Primary + replicas with automated failover.
- A stable write endpoint (Service) that always points to the current primary.
- Short write interruptions can occur during failover (applications must tolerate brief DB reconnects).

### Recommended direction
Use a PostgreSQL operator (operator-managed clusters), and ensure:
- automated backups + PITR (point-in-time recovery)
- monitoring/alerting (replication lag, disk, WAL growth)
- staging-first failover drills

### Flyway + HA note
In HA environments, prefer **Strategy B (migration Job)** so migrations happen once in a controlled way, independent of application replica rollout and failover behavior.


## 3 questions to lock your blueprint
1. Is this **domain separation** or **multi-tenancy**?
2. Do you need **hard isolation** (separate backups/restores, different SLAs), or is logical separation enough?
3. Expected scale: **how many schemas** (rough order of magnitude)?
