Got it: **Vault is external to the cluster**. That simplifies the Kubernetes backup plan a lot and makes the “don’t back up K8s Secrets” recommendation even stronger.

## Backup Strategy Blueprint (v1, with External Vault)

### 1) Core principles
- **Kubernetes backups are for cluster state + PVC data**, not for secrets source-of-truth.
- **Vault is the secrets SoT** and must be backed up using **Vault’s own procedures** (outside Kubernetes).
- A restore must be able to succeed even if Kubernetes Secrets are empty initially (External Secrets will repopulate them from Vault).

---

## 2) What gets backed up (scope)
### 2.1 Kubernetes state (Velero)
Back up Kubernetes objects for all application namespaces:
- Deployments, Services, Ingress, ConfigMaps
- External Secrets resources (e.g., `ExternalSecret`, `SecretStore` / `ClusterSecretStore`)
- Any CRDs you depend on (optional, but recommended if you install CRDs yourself)

**Default exclude**
- `Secret` resources (because they’re derived from Vault)

### 2.2 Persistent volumes (Velero)
Back up PVC data for stateful workloads:
- PostgreSQL PVCs (if DB runs in-cluster)
- Any other PVC-based state

Backup method:
- **Preferred:** CSI VolumeSnapshots (if supported)
- **Fallback:** file-level backups (Velero node-agent/restic approach)

### 2.3 Application-level database backups (Postgres)
Even if you snapshot PVs, schedule **DB-consistent backups**:
- Minimum viable: `pg_dump` daily to object storage
- Increase frequency if you need a tighter RPO

### 2.4 Vault (external)
Vault is outside the cluster, so it is **not** covered by Velero.
Blueprint requirement:
- Vault must have a **separate backup + restore plan** owned by the Vault/platform team.

What to state (vendor-neutral but Vault-correct):
- Backup method depends on Vault storage backend (e.g., integrated storage raft snapshots vs backing up the backend datastore).
- Backups must be encrypted, access-controlled, and tested with periodic restores.

---

## 3) Backup storage target
Use **S3-compatible object storage** (S3/MinIO/Ceph RGW):
- encryption at rest
- restricted access (Velero write, restore operators read)
- versioning / immutability if available (especially for production)

Recommended layout/prefixing:
- `velero/<cluster-name>/...`
- `postgres/<env>/...`
- `vault/<env or org>/...` (managed outside K8s)

---

## 4) Schedules & retention (minimum viable defaults)
You can tune later, but don’t leave this blank.

### 4.1 Velero (Kubernetes objects)
- **Daily** backup
- Retention: **14–30 days**

### 4.2 Velero (PVCs)
- **production:** daily
- **staging:** daily or weekly
- **experimental:** optional / short retention

Retention starter:
- daily kept 14 days
- weekly kept 4 weeks (optional)

### 4.3 Postgres logical backups
- **production:** daily minimum (consider 6h/12h if RPO matters)
- retention: 14–30 days (match Velero window or longer if cheap)

### 4.4 Vault backups
- At least daily (often more frequent depending on secret churn/operational needs)
- Retention: at least 30 days (common baseline)

---

## 5) Restore workflows (the “how” you must document)
### 5.1 Namespace restore (most common)
1. Restore namespace objects with Velero (Deployments/Services/Ingress/ExternalSecrets)
2. Restore PVCs (snapshot/file-level)
3. Confirm external-secrets controller is running
4. External Secrets reconcile → Kubernetes Secrets repopulated from Vault
5. Validate app readiness + key endpoints

### 5.2 Database restore
Preferred order:
1. Restore Postgres from **logical backup** (`pg_dump`) into a clean instance/PVC (most consistent)
2. Use PV snapshot restore when you explicitly accept DB snapshot consistency constraints

### 5.3 Full cluster recovery
1. Recreate cluster via IaC
2. Reinstall cluster add-ons (Velero + external-secrets operator + ingress, etc.)
3. Restore objects + PVCs via Velero
4. Ensure connectivity to external Vault (network/DNS/TLS/auth)
5. Let External Secrets repopulate Secrets
6. Validate services

---

## 6) Observability & alerting for backups (minimum)
Add alerts for:
- Velero backup failures / no successful backup in X hours
- Postgres backup job failures / missing new backup artifacts
- (Outside K8s) Vault backup job failure / last snapshot age too old

---

## 7) Definition of Done (v1)
This blueprint is “minimally viable” when:
- Velero is installed and performing daily backups of K8s objects
- PVC backups work (either CSI snapshots or file-level)
- Postgres logical backups run on schedule and are restorable
- K8s Secrets are excluded; External Secrets objects are included
- Vault has a documented and tested backup/restore plan (outside cluster)
- A restore drill has been executed at least once (staging is enough)

---
