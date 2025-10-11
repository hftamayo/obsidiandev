
## Periodo de refresh de los github actions

![[Pasted image 20250612111700.png]]

en la primera imagen se despliegan los actions disponibles en la rama default

## Descripcion de las pipelines:

1. deploy_tfiac_aws -> deploya toda la IaC

Preflight para pasar a la siguiente fase:

```
1. debo estar logeado con una cuenta que no sea root
   
aws sts assume-role --role-arn arn:aws:iam::482619384845:role/vanillatstodo-deployer --role-session-name test-session

aws eks update-kubeconfig --name vanillatstodo-cluster --region us-east-2 --role-arn arn:aws:iam::482619384845:role/vanillatstodo-deployer --alias vanillatstodo-admin
kubectl config use-context vanillatstodo-admin

kubectl auth can-i get pods -A
kubectl get nodes -o wide   

```

2. install_EBS_CSI Add on -> installation de un driver CSI que grafana requiere

Pasos de verificacion para en efecto confirmar que todo esta listo:

```
Autenticacion:
aws eks update-kubeconfig --name vanillatstodo-cluster --region us-east-2 --role-arn arn:aws:iam::482619384845:role/vanillatstodo-deployer --alias vanillatstodo-admin

kubectl config use-context vanillatstodo-admin

kubectl get nodes -o wide

Verificacion del driver:

kubectl get csinodes

kubectl get csidriver

# Controller deployment and pods
kubectl -n kube-system get deploy | grep -i ebs

kubectl -n kube-system get pods -l app=ebs-csi-controller

# If nothing, try the add-on labels:

kubectl -n kube-system get pods -l 'app.kubernetes.io/name=aws-ebs-csi-driver,

```

3. Deploy CodeBase to AWS -> deploya el codigo

4. Deploy Monitoring Stack to EKS -> deploya un grafana board

