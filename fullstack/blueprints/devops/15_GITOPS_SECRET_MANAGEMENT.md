
## Pre-requisite for external secret management:

- External Secrets Operator is installed in the cluster.
- The EC2/k3s node role can read the AWS secret.
- ArgoCD is actually applying this folder/path.

___

The Helm chart expects a Kubernetes Secret named absencesbocred.
That Secret does not exist in namespace absencesbo-dev.
AWS Secrets Manager does not automatically create Kubernetes Secrets.

Solution:
External Secrets Operator: This is usually the cleaner GitOps-native approach.

Flow:
AWS Secrets Manager
        |
        | External Secrets Operator
        v
Kubernetes Secret absencesbocred
        |
        v
backend/database pods

You would install External Secrets Operator into k3s, then define something like:
apiVersion: external-secrets.io/v1
kind: ExternalSecret
metadata:
  name: absencesbocred
  namespace: absencesbo-dev
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: aws-secrets-manager
    kind: SecretStore
  target:
    name: absencesbocred
  data:
    - secretKey: POSTGRES_USER
      remoteRef:
        key: absencesbocred
        property: POSTGRES_USER
    - secretKey: POSTGRES_PASSWORD
      remoteRef:
        key: absencesbocred
        property: POSTGRES_PASSWORD
    - secretKey: POSTGRES_DB
      remoteRef:
        key: absencesbocred
        property: POSTGRES_DB
    - secretKey: POSTGRES_HOST
      remoteRef:
        key: absencesbocred
        property: POSTGRES_HOST
    - secretKey: POSTGRES_PORT
      remoteRef:
        key: absencesbocred
        property: POSTGRES_PORT
        
This is probably the best long-term approach for ArgoCD.        

## Commands for testing how secrets are working on K8S cluster

```
kubectl get pods -n external-secrets
kubectl get secretstore -n absencesbo-dev
kubectl get externalsecret -n absencesbo-dev
kubectl get secret absencesbocred -n absencesbo-dev
kubectl get pods -n absencesbo-dev

```
