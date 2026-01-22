# 🚀 DevOps Pipeline Blueprint: IaC & Infrastructure

## Default inventory

### Network Layer (`01_network`)
- **VPC:** 1 Dedicated VPC with DNS Hostnames/Support enabled.
- **Subnets:**
    - 2 Public Subnets (AZ-a, AZ-b) for Load Balancers and NATs.
    - 2 Private Subnets (AZ-a, AZ-b) for EKS Worker Nodes and Data Layer.
- **Gateways:**
    - 1 Internet Gateway (IGW) for egress.
    - 2 NAT Gateways (High Availability) for private subnet internet access.
- **Routing:**
    - 1 Public Route Table (pointing to IGW).
    - 2 Private Route Tables (pointing to respective NAT Gateways).

### Compute Layer (`02_eks`)
- **Cluster:** 1 EKS Managed Cluster (v1.31).
- **Node Group:** Managed Node Group using `t3.medium` instances.
- **Access:** EKS Access Entries enabled for GitHub Actions OIDC integration.

### Management & Observability
- **Storage:** 1 S3 Bucket for Terraform Remote State (with versioning & locking).
- **Logging:** CloudWatch Log Group for VPC Flow Logs.
    - **Retention:** 30 Days.
- **Monitoring:** CloudWatch Alarms for EKS Node Count monitoring.

### ☸️ EBS CSI Driver: Terraform Management
To avoid "Split Brain" state (where GitHub Actions and Terraform fight for control), we are moving to a **Terraform-Native** lifecycle using the following guidelines:

1. **IAM Role for Service Accounts (IRSA):**
    - Create a dedicated IAM Role with the `AmazonEBSCSIDriverPolicy` AWS-managed policy.
    - Establish a **Trust Relationship** using the EKS Cluster's OIDC Provider URL to allow the `ebs-csi-controller-sa` service account to assume the role.

2. **Add-on Resource Implementation:**
    - Use the `aws_eks_addon` resource within the `02_eks` module.
    - **Key Attributes:**
        - `addon_name`: Set to `aws-ebs-csi-driver`.
        - `service_account_role_arn`: Link the ARN of the IAM Role created in step 1.
        - `resolve_conflicts_on_update`: Set to `PRESERVE` or `OVERWRITE` depending on whether you want Terraform to override manual changes.

3. **StorageClass Provisioning:**
    - Use the `kubernetes_storage_class` resource (via the Kubernetes provider) to define a `gp3` storage class.
    - Mark it as the `default` storage class to ensure any PersistentVolumeClaim (PVC) without a specified class uses the efficient EBS gp3 volumes.

4. **Lifecycle Management:**
    - **Importing:** If the add-on was previously created via CLI or Console, use `terraform import` to bring it under management without downtime.
    - **Updates:** Control driver versions via the `addon_version` attribute to ensure compatibility with EKS v1.31.

### 💾 Database Evolution: Why & When RDS?
While running databases inside Kubernetes (StatefulSets) is possible, we are migrating to **AWS RDS** for managed reliability.

**Why use RDS?**
- **Offloaded Management:** AWS handles patching, OS maintenance, and automated backups.
- **High Availability:** Multi-AZ deployments provide failover in seconds, which is harder to orchestrate manually in K8s.
- **Performance:** Dedicated compute resources that don't compete with application pods for memory or CPU.

**When to use RDS?**
- When the data is "Production Critical" and requires Point-in-Time Recovery (PITR).
- When the team wants to reduce operational overhead (No DBAs needed for hardware/OS scaling).
- **Current Plan:** Deploy RDS in `05_rds` within private subnets, restricted to EKS Security Group traffic.

## 🎯 Infrastructure Suitability & Workload Boundaries

This infrastructure blueprint is optimized for a specific profile of applications. Understanding these boundaries ensures operational stability:

**Best Suited For:**
- **Containerized Microservices:** Ideal for lightweight APIs (Go, Node.js, Python) and frontend applications.
- **Burstable Workloads:** The use of `t3.medium` instances is perfect for applications with variable traffic patterns thanks to AWS "CPU Credits."
- **Production-Grade Security:** Applications requiring high compliance, as all compute and data reside in Private Subnets with restricted egress.

**Architectural Boundaries:**
- **Memory Constraints:** With `t3.medium` nodes (4GB RAM), memory-intensive applications (e.g., large Java/JVM heaps or heavy Data Processing) may require a horizontal scaling strategy or a future upgrade to `m` or `r` instance families.
- **Stateless Preference:** While EBS CSI is available, the architecture strongly favors **stateless application logic**, delegating long-term state to the managed RDS layer.
- **Regional High Availability:** This setup provides high availability across two Availability Zones (AZs). It is resilient to a single AZ failure but is not designed for multi-region disaster recovery in its current iteration

## Head's Up
In the use of t3.medium instances, keep an eye on your CPU Credit balance in CloudWatch. If your app is constantly "pegged" at high CPU, those credits might run out, and AWS will throttle the performance. If you notice your app getting sluggish during peak hours, it might be a sign to move from t (burstable) to m (general purpose) instances!

## Helm in CI Blueprint
Run a **single Kubernetes cluster** with three **stable namespaces**—`experimental`, `staging`, and `production`—where the namespace represents the environment boundary, while **Helm values represent environment-specific configuration**. Keep one Helm chart and maintain **three values files in Git** (for example `values-exp.yaml`, `values-staging.yaml`, `values-prod.yaml`) covering safe differences like replicas/autoscaling, resource requests/limits, ingress hostnames, and feature flags; keep secrets out of plain values files unless you have an explicit encryption/secret-management workflow.

Use a simple promotion flow: **feature branches deploy only to `experimental`**, **`main` deploys to `staging`**, and **tagged releases (or an approval-gated job) deploy to `production`**. Since you merge one feature branch at a time, you can use a **single fixed Helm release name** per environment (no per-branch release isolation needed).

Manage **namespaces and baseline guardrails via IaC** (Namespace objects plus RBAC boundaries, ResourceQuotas/LimitRanges, and—ideally—NetworkPolicies), and make CI/Helm deploys fail if the namespace is missing (avoid relying on Helm “create namespace” behavior) to prevent drift and split-brain ownership. Use **separate CI credentials and RBAC scopes per environment** (broadest in experimental, deploy-only in staging, most restrictive in production) and protect production with an explicit approval gate and restricted triggers.

To keep a future GitOps migration painless, ensure deploys are **deterministic** (same chart + same values + same image = same output), keep environment config **in Git**, and **separate build from deploy** so the exact same artifact (preferably an immutable image digest) is promoted from experimental → staging → production.

### Graph to represent the interaction between branches and environments:

feature/* branch
|
|  (push commits)
v
CI deploys to:  experimental
|
|  (PR merged into main)
v
main branch
|
v
CI deploys to:  staging
|
|  (tag + approval)
v
release/tag
|
v
CI deploys to:  production

### What “interact” means in practice (your assumptions)

- feature/ branches only ever touch experimental
- main is the only branch that touches staging
- production is only deployed from a tagged release (and/or approval step)
- Because you merge one feature branch at a time, experimental is a shared lane: latest feature deployment wins (by design)

## Suggested project structure

![Project Structure](./proj_struct.png)

