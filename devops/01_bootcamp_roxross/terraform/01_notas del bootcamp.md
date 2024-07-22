
AWS Cloud Formation -> homologo de Terraform
GCP Pulumi -> homologo de Terraform

![[Pasted image 20240702125126.png]]

==escenario de uso ansible:
necesito replicar el siguiente contenido en 100 servidores: nginx+docker+docker compose + codigo
por medio de ansible hago un host master

==escenario de uso de terraform:
por medio de terraform yo voy a redirigir a los 100 servidores para que se conecten al master que es sostenido gracias a Ansible, esto se llama "aprovisionamiento de infraestructura"

==escenario de uso de AWS Cloud Formation:

![[Pasted image 20240702125651.png]]

==escenario de uso de Terraform:


![[Pasted image 20240702125746.png]]

![[Pasted image 20240702125945.png]]




==ejemplo de un flujo que puede automatizarse con Terraform:

![[Pasted image 20240702131022.png]]

==ejmplo de como podriamos descomponer cada uno de los servicios arriba descritos por medio de un playbook en terraform:

![[Pasted image 20240702131129.png]]

==Instalar Terraform;
https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli

github.com/tfutils/tfenv -> busca e instala una version especifica de Terraform


==herramientas para escaneo dinamico:
- Sonarqube
- Horusec
- Trivy
- Zap -> si, el mismo MIM Proxy de OWASP

Pra hacer la demo de DevSecOps Roxx usó el proyecto OWASP Juice Shop

==All in one solutions para DevSecOps:
- Faraday
- Defectdojo
- AWS Security hub
- semgrep

Ejemplo de Jenkinspipeline con DevSecOps:

![[Pasted image 20240717123638.png]]

