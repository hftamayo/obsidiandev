
### Listado de comandos para un cluster para el proyecto boabsences

```
aws sts assume-role \
  --role-arn arn:aws:iam::482619384845:role/websystem_administrator \
  --role-session-name eks-admin-session
  
aws eks associate-access-policy \
  --cluster-name absencesbo-cluster \
  --principal-arn arn:aws:iam::482619384845:role/websystem_administrator \
  --policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSClusterAdminPolicy \
  --access-scope type=cluster \
  --region us-east-2  
  
aws sts get-caller-identity  
  
aws eks update-kubeconfig --name absencesbo-cluster --region us-east-2

aws eks list-access-entries --cluster-name absencesbo-cluster --region us-east-2

kubectl cluster-info

aws eks list-access-entries --cluster-name absencesbo-cluster --region us-east-2

aws eks list-associated-access-policies --cluster-name absencesbo-cluster --principal-arn arn:aws:iam::482619384845:role/websystem_administrator --region us-east-2


aws eks describe-cluster --name absencesbo-cluster --region us-east-2
kubectl cluster-info
kubectl get namespaces

kubectl get pods
kubectl get pods -A
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl get pods -w
kubectl config view --minify --output 'jsonpath={..namespace}'
kubectl logs absencesbo-experimental-database-0 -n absencesbo-experimental
kubectl logs absencesbo-experimental-backend-7975b5759f-97t7g -n absencesbo-experimental

```
