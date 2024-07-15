
https://labs.iximiuz.com --> labs y challenges
https://validkube.com  --> validador de yamls
awseducate.com
go.aws/46x4Gmw -> Cloud Practitioner Challenge
https://workshops.aws
https://ecsworkshop.com
https://eksworkshop.com
https://skillbuilder.aws -> insignias de diveras tecnologias
https://www.localstack.cloud -> unit testing de deploys en la nube para manejar costos del provider y sólo subir infras ya testeadas
portal.cloud.hashicorp.com  -> tiene una capa gratuita donde puedo usar los productos hashicorp

Plugins para Jenkins sugeridos:
- Blue Ocean, Common API for Blue Ocean, REST API for Blue Ocean


Articulos sobre proyectos DevOps:

1. E2E DevSecOps Netflix Clone: https://medium.com/@bhandariajay006/project-8-end-to-end-devsecops-netflix-clone-application-deployment-7f918d0b8f81
2. Deploy of Swiggy Applications using github actions: 
https://www.linkedin.com/feed/update/urn:li:activity:7208442052924108800/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7208442052924108800%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29

Step 1 → Setup Terraform and configure aws on your local machine  
  
Step 2 → Building a simple Infrastructure from code using terraform  
Step 3 → IAM Role for EC2  
Step 4 →Setup github actions with ec2  
Step 5 →setup sonarqube and dockerhub for github actions  
Step 6→ Elastic kubernetes service or Eks cluster setup  
Step 7 → build and push docker image  
Step 8 →Monitering via Prmotheus and grafana  
Step 9 → Destroy all the things

3. Monitoring a K8S using prometeus and Grafana:
https://www.linkedin.com/feed/update/urn:li:activity:7208293341271138304/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7208293341271138304%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29

4. Mastering K8S security: 
https://www.linkedin.com/feed/update/urn:li:activity:7204452021158993920/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7204452021158993920%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29

5. Diez ejercicios para implementar proyectos DevOs en AWS:
https://www.linkedin.com/feed/update/urn:li:activity:7207351865762537472/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7207351865762537472%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29

6. Ejercicios DevOps On-Premise y con Provider:
https://www.linkedin.com/feed/update/urn:li:activity:7113100075467161602/


7. Ejercicios Serverless:
https://www.linkedin.com/feed/update/urn:li:activity:7216830387610603521?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7216830387610603521%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29

8. Automating Tetris deploy with DevSecOps:
https://www.linkedin.com/feed/update/urn:li:activity:7218135802961158144?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7218135802961158144%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29

https://www.linkedin.com/feed/update/urn:li:activity:7214499687519657984/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7214499687519657984%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29

9. Lambda exercise con AWS:
https://www.linkedin.com/feed/update/urn:li:activity:7217354498736807936/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7217354498736807936%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29

10. 


kubernetes:
=

gestor de multi clusters: https://k8slens.dev


kubectl get nodes

kubectl get nodes -o wide

kubectl get pod -A : ver los pods del core del cluster

kubectl describe pod podname

kubectl apply -f name.yaml

kubectl exec -it podname -- bash

kubectl logs podname

kubectl delete pod podname -> no se vuelve a regenerar

kubectl get pod --show-labels

kubectl get pod -l owner=hftamayo

kubectl get pod -Lowner -> para crear una columnar con el valor la var owner

![[Pasted image 20240606134210.png]]

![[Pasted image 20240606134230.png]]

![[Pasted image 20240606134022.png]]

![[Pasted image 20240606134329.png]]

==COMO AGREGAR RELEASES A LOS YAML FILES:
=

![[Pasted image 20240610141123.png]]

VERIFICANDO EL DEPLOY Y LOS OBJETOS CREADOS ALREDEDOR:

![[Pasted image 20240610141226.png]]

==PUESTO QUE LA ETIQUETA ANNOTATIONS SE LA PUSE AL YAML DEL DEPLOY ENTONCES PARA VERLO:

![[Pasted image 20240610141413.png]]

==AHORA, SI MANUALMENTE YO HAGO EL CAMBIO A version 1.0.1 y nuevamente hago el deploy y despliego los objetos para saber si todo funciono entonces ahi me aparece el update:

![[Pasted image 20240610141531.png]]

==ahora bien, si necesto hacer un rollback del ultimo deploy y nuevamente verifico:

![[Pasted image 20240610141649.png]]

![[Pasted image 20240610141710.png]]


![[Pasted image 20240610143032.png]]

![[Pasted image 20240610144345.png]]

![[Pasted image 20240610144453.png]]

![[Pasted image 20240610144510.png]]

==Hostpath no se recomienda para produccion, puede ir bien con minikube o kind para los devs


==esquema de la arquitectura del juego pacman desplegado en K8S:

![[Pasted image 20240611140454.png]]


==ejemplo de K8s gitpos:

![[Pasted image 20240611140803.png]]

==en este flow, es el operador -argo o flux- quien es el encargado de desplegar la aplicacion en la infraestructura, es asi que al CI/CD se le quita carga.

