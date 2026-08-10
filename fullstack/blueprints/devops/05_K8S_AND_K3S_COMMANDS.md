
### Listado de comandos para un cluster para el proyecto boabsences

``` 
aws eks create-access-entry \
  --cluster-name absencesbo-cluster \
  --principal-arn arn:aws:iam::482619384845:role/websystem_administrator \
  --region us-east-2  
  
aws eks associate-access-policy \
  --cluster-name absencesbo-cluster \
  --principal-arn arn:aws:iam::482619384845:role/websystem_administrator \
  --policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSClusterAdminPolicy \
  --access-scope type=cluster \
  --region us-east-2

aws eks list-access-entries --cluster-name absencesbo-cluster --region us-east-2

aws eks list-associated-access-policies --cluster-name absencesbo-cluster --principal-arn arn:aws:iam::482619384845:role/websystem_administrator --region us-east-2
  
aws sts get-caller-identity
  
aws eks update-kubeconfig \
  --name absencesbo-cluster \
  --region us-east-2 \
  --role-arn arn:aws:iam::482619384845:role/websystem_administrator

kubectl cluster-info

aws eks describe-cluster --name absencesbo-cluster --region us-east-2
kubectl get namespaces

kubectl get pods -A
kubectl get pods -n <namespace_name>

kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl get pods -w
kubectl config view --minify --output 'jsonpath={..namespace}'
kubectl logs absencesbo-experimental-database-0 -n absencesbo-experimental

Comandos para ejecutar y verificar si el perfil websystem_administrator esta trabajando como se espera:

aws eks describe-cluster \
  --name absencesbo-cluster \
  --region us-east-2
  

aws eks describe-access-entry \
  --cluster-name absencesbo-cluster \
  --principal-arn arn:aws:iam::482619384845:role/websystem_administrator \
  --region us-east-2
  

aws eks list-access-entries \
  --cluster-name absencesbo-cluster \
  --region us-east-2
  

aws eks list-associated-access-policies \
  --cluster-name absencesbo-cluster \
  --principal-arn arn:aws:iam::482619384845:role/websystem_administrator \
  --region us-east-2
  
aws iam get-role \
  --role-name websystem_administrator
  
aws eks update-kubeconfig \
  --name absencesbo-cluster \
  --region us-east-2 \
  --role-arn arn:aws:iam::482619384845:role/websystem_administrator
  
kubectl cluster-info
kubectl get namespaces
kubectl get pods -A

VERIFICAR LOS LOGS DE PODS EN EJECUCION:  

kubectl logs absencesbo-experimental-backend-7975b5759f-97t7g -n absencesbo-experimental

kubectl logs absencesbo-experimental-nginx-5b64bb576b-4kd7d -n absencesbo-experimental

kubectl logs absencesbo-experimental-frontend-6897b76979-jd8w2 -n absencesbo-experimental

```

### Comandos utiles para debuggear un pod que se esta reiniciando de manera constante:

```
kubectl logs absencesbo-experimental-frontend-f796498f6-lppn2 -n absencesbo-experimental --previous

kubectl logs absencesbo-experimental-frontend-f796498f6-jdbfk -n absencesbo-experimental --previous

kubectl describe pod absencesbo-experimental-frontend-f796498f6-lppn2 -n absencesbo-experimental

```

## comandos para verificar la instalacion del EBS CSI Driver

```
aws eks describe-addon \
  --cluster-name absencesbo-cluster \
  --addon-name aws-ebs-csi-driver \
  --region us-east-2
  
aws eks list-addons \
  --cluster-name absencesbo-cluster \
  --region us-east-2
  
kubectl -n kube-system get sa ebs-csi-controller-sa -o yaml

kubectl -n kube-system get pods -o wide | grep -Ei 'ebs|csi'

kubectl -n kube-system get deploy,ds | grep -Ei 'ebs|csi'

kubectl get storageclass

aws eks describe-addon \
  --cluster-name absencesbo-cluster \
  --addon-name aws-ebs-csi-driver \
  --region us-east-2 \
  --query '{status:addon.status,health:addon.health,role:addon.serviceAccountRoleArn,version:addon.addonVersion}' \
  --output json  

```

### COMANDOS PARA LA ADMINISTRACION DE K3S

```
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

kubectl get ns

helm uninstall absencesbo-experimental \
  -n absencesbo-experimental \
  --wait \
  --timeout 5m || true

kubectl delete namespace absencesbo-experimental \
  --wait=true \
  --timeout=5m || true
  
helm list -n absencesbo-dev --all || true

kubectl get namespace absencesbo-dev || true

kubectl get pods -A

kubectl get svc -n absencesbo-dev

kubectl describe svc absencesbo-dev-backend-d75744846-6k5ql -n absencesbo-dev

kubectl get svc -A

kubectl get nodes -o wide

kubectl get svc <nginx-service-name> -n <namespace> -o jsonpath='{.spec.ports[0].nodePort}{"\n"}'kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(@.type=="ExternalIP")].address}{"\n"}'
  
From the IAC pipeline:
[](https://github.com/htues/absencesbo-devops/actions/runs/25077643529/job/73474435656#step:13:1)Run set -euo pipefail

INSTANCE_ID=i-05ada1300e068cdf4

PUBLIC_IP=3.13.103.176

INSTANCE_DNS=::error::Terraform exited with code 1.

LOAD_BALANCER_DNS_NAME=experimental-absencesbo-dev-nlb-d863a47af4aa088e.elb.us-east-2.amazonaws.com

LOAD_BALANCER_URL=[http://experimental-absencesbo-dev-nlb-d863a47af4aa088e.elb.us-east-2.amazonaws.com](http://experimental-absencesbo-dev-nlb-d863a47af4aa088e.elb.us-east-2.amazonaws.com)

APP_NODEPORT=32322

curl -I http://3.13.103.176:32322
  

Acceder por medio del AWS Network Load Balander DNS:
http://experimental-absencesbo-dev-nlb-d863a47af4aa088e.elb.us-east-2.amazonaws.com

  
```

### COMANDOS RELACIONADOS A HELM

```
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

- helm list -n absencesbo-experimental
- helm history absencesbo-experimental -n absencesbo-experimental
  

Kubernetes only replaces Pods when the Deployment’s spec.template changes.

CONFIRMAR IMAGEN VERSION DE UN POD:
kubectl get deployment absencesbo-dev-frontend \
  -n absencesbo-dev \
  -o jsonpath='{.spec.template.spec.containers[0].image}{"\n"}'
  
kubectl get rs -n absencesbo-experimental | grep nginx

kubectl rollout history deployment/absencesbo-experimental-nginx \
  -n absencesbo-experimental
  
FORZAR UN RESTART:
kubectl rollout restart deployment/absencesbo-experimental-nginx \
  -n absencesbo-experimental
  
VERIFICAR:
kubectl get pods -n absencesbo-experimental -w  

```
