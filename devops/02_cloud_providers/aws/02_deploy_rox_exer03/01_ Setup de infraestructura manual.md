****
==la idea será montar una EC2 que me servirá de estacion de trabajo y desde ahi voy a controlar mi Infraestructura, esto con el objetivo de no cargar mi estacion de trabajo dev, pero parece que el equipo DevOps si debería tener estaciones de trabajo para estas operaciones


este es el primer approach del objetivo a conseguir:
```
. **Deploy Jenkins on EKS**:
   - Set up an EKS cluster.
   - Deploy Jenkins as a Kubernetes deployment within the EKS cluster.
   - This approach allows you to leverage Kubernetes for managing Jenkins and its scalability, similar to your on-premise setup.

1. **Set Up EKS Cluster**:
   - Create an EKS cluster using the AWS Management Console, AWS CLI, or eksctl.
   - Ensure your EKS cluster is properly configured with worker nodes.

2. **Install Jenkins on EKS**:
   - Use Helm to deploy Jenkins on your EKS cluster. Helm is a package manager for Kubernetes that simplifies the deployment process.

   - First, install Helm on your local machine:
   - Add the Jenkins Helm chart repository:
   - Deploy Jenkins using Helm:
   - Retrieve the Jenkins admin password:
   - Access Jenkins via the service created by Helm. You may need to set up an Ingress controller or use a LoadBalancer service to expose Jenkins.
   - Access Jenkins using the external IP or DNS name provided by the LoadBalancer or Ingress.
   - Complete the initial setup and install necessary plugins.

4. **Migrate Your Pipelines**:
   - Export your existing Jenkins jobs and import them into the new Jenkins instance.
   - Update any configurations to point to AWS resources as needed.

```

1. instalar paquetes indispensables en mi equipo:

```
sudo apt install awscli
```


# ETAPA 1: CREACION DE CREDENCIALES


2. crear una cuenta de menor privilegio para estas operaciones:

![[Pasted image 20240814153908.png]]

![[Pasted image 20240814153942.png]]

![[Pasted image 20240814154115.png]]

![[Pasted image 20240814154539.png]]

![[Pasted image 20240814154609.png]]

![[Pasted image 20240814154642.png]]

![[Pasted image 20240814154841.png]]

![[Pasted image 20240814154917.png]]

![[Pasted image 20240814154959.png]]

Si uso el botón Downad csv file descargo las creds del usuario en formato CSV,

Para obtener el Access Key ID y el Secret Access Key presiono en el boton View user y voy a esta pantalla:

![[Pasted image 20240814155433.png]]

![[Pasted image 20240814155512.png]]

![[Pasted image 20240814155557.png]]

![[Pasted image 20240814155619.png]]

![[Pasted image 20240814155648.png]]

![[Pasted image 20240814155748.png]]

en el archivo descargado tengo la info que requiero para configurar mi awscli

3. configurar mi access key en awscli:
![[Pasted image 20240815124250.png]]

==para la seleccion de la region:
![[Pasted image 20240815124418.png]]


4.  Habilitando MFA para el nuevo usuario (usando un mismo dispositivo)

![[Pasted image 20240815125131.png]]

![[Pasted image 20240815125150.png]]

![[Pasted image 20240815125439.png]]

![[Pasted image 20240815125454.png]]

![[Pasted image 20240815125711.png]]


# ETAPA 2: ASIGNACION DE UN IAM ROLE PARA OPS CON EC2 INSTANCES 

Puesto que es más seguro usar IAM Roles para ciertos servicios pues no se requiere guardar llaves en dichas instancias, se sugiere crear roles como el siguiente:

![[Pasted image 20240815130147.png]]

![[Pasted image 20240815130254.png]]

![[Pasted image 20240815130325.png]]

![[Pasted image 20240815130400.png]]

![[Pasted image 20240815130642.png]]

![[Pasted image 20240815130657.png]]

![[Pasted image 20240815130756.png]]

![[Pasted image 20240815130820.png]]

# ETAPA 3: CREACION DE MI INSTANCIA EC2 QUE SERVIRA DE TERMINAL

![[Pasted image 20240815131747.png]]

![[Pasted image 20240815131834.png]]

![[Pasted image 20240815131945.png]]

![[Pasted image 20240815132138.png]]

![[Pasted image 20240815132228.png]]

==la siguienet configuracion es muy laxa:

![[Pasted image 20240815132509.png]]

![[Pasted image 20240815132535.png]]

![[Pasted image 20240815132639.png]]

![[Pasted image 20240815132806.png]]

# ETAPA 4:  CREANDO UNA ALERTA DE BILLING

![[Pasted image 20240815141247.png]]

![[Pasted image 20240815141347.png]]

# ETAPA 5: ASIGNANDO EL ROL IAM A LA EC2

![[Pasted image 20240815142019.png]]

![[Pasted image 20240815142422.png]]


![[aws_modify_iam.gif]]


seleccion del role:

![[Pasted image 20240815144634.png]]

![[Pasted image 20240815145110.png]]

# ETAPA 6: PROBAR LA CONEXION A LA EC2

agregando los permisos al pem file:

![[Pasted image 20240816151045.png]]

==aca me dio error porque la cuenta default ec2-user es para amazon linux, para debian es admin


![[Pasted image 20240816151416.png]]

==verificando el IAM role y si puedo hacer uso de comandos aws-cli sin instalar keys:

![[Pasted image 20240816151707.png]]

# ETAPA 7: CONFIGURANDO LA VM CON LA PAQUETERIA NECESARIA

![[Pasted image 20240816152153.png]]

![[Pasted image 20240816152420.png]]

![[Pasted image 20240816153748.png]]

==por contingencia he agregado un usuario con privilegios estandar:

htues@ devopstest

![[Pasted image 20240819080933.png]]

==ahora bien, intentando conectarme a la VM usando htues encontré esto:

![[Pasted image 20240819082511.png]]

y tiene logica pues esta key esta reservada para el admin de la cuenta

# ETAPA 8:  USANDO AWS SESSION MANAGER PARA GESTION DE CONEXIONES DE MANERA CENTRALIZADA

==agregando el repo:

![[Pasted image 20240819083430.png]]

![[Pasted image 20240819083537.png]]

![[Pasted image 20240819083946.png]]

aca es bien interesante porque parece que el repo no lo encuentra, ==estuve leyendo y parece que toda esa paqueteria si ya esta previamente instalada en los Amazon Linux VM, entonces para evitarme futuros malestares voy a eliminar esta VM y pasarme a un Amazon Linux. Aca tiene total razon el uso de IaC.


# ETAPA 9: MIGRANDO HACIA UNA AMAZON LINUX VM

a continuacion resumo los parametros reutilizados:

```
nombre: aldevopsterminal
login key: awsdevopsdebian pem
user account: htues


```

![[Pasted image 20240820155110.png]]

# ETAPA 10: ACTIVANDO AWS SYSTEMS MANAGER

agregando el siguiente permiso al IAM Role ec2instancesmanager:

![[Pasted image 20240821131425.png]]

relacionar este Role con la ALinux VM:

![[Pasted image 20240821131826.png]]

dentro de la misma pestaña de seguridad verificar si el security group permite trafico saliente por el puerto 443 y via HTTPS:

![[Pasted image 20240821132225.png]]

![[Pasted image 20240821132253.png]]

conectarse via Session Manager:

![[Pasted image 20240821132909.png]]

![[Pasted image 20240821132946.png]]

==antes de llegar aca la VM EC2 ya debe estar ejecutándose:

![[Pasted image 20240821133353.png]]

===click en el boton Start Session y esperamos:

![[Pasted image 20240821133553.png]]

==investigar si es posible mejorar la terminal porque es un tanto borrosa, cuando ya quiera terminar la sesion en la esquina superior derecha hay un boton de Terminate.

# ETAPA 11: agregando mi usuario estandar a sudo e instalando paqueteria

```
sudo visudo
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin

curl --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C .
sudo mv ./eksctl /usr/local/bin

curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

Agregar Jenkins al Helm repo:
     helm repo add jenkins https://charts.jenkins.io
     helm repo update
```

==IMPORTANTE:
puesto que estoy usando AWS Session Manager no necesito configurar una key para AWS Configure, cualquier ampliacion de funciones debo hacerlo en el rol.


==carateristicas de un cluster EKS production ready:

1. **High Availability**: Ensure that your EKS cluster and Jenkins deployment are highly available. This includes:
   - **EKS Cluster**: Use multiple Availability Zones for your worker nodes.
   - **Jenkins**: Deploy Jenkins with multiple replicas and use a LoadBalancer or Ingress to distribute traffic.

2. **Persistence**: Use Persistent Volumes (PVs) for Jenkins data to ensure data persistence across pod restarts and upgrades.
   - **AWS EBS**: Use Amazon EBS volumes for Jenkins home directory and other critical data.

3. **Security**: Implement robust security measures.
   - **IAM Roles**: Use IAM roles for service accounts to grant Jenkins the necessary permissions to interact with AWS services.
   - **Network Policies**: Use Kubernetes Network Policies to control traffic between pods.
   - **Secrets Management**: Use Kubernetes Secrets or AWS Secrets Manager to manage sensitive information.

4. **Backup and Recovery**: Implement a backup and recovery strategy for Jenkins data.
   - **Persistent Volume Snapshots**: Regularly snapshot your EBS volumes.
   - **Configuration Backup**: Regularly back up Jenkins configuration and job definitions.

5. **Monitoring and Logging**: Set up monitoring and logging to keep track of the health and performance of your Jenkins deployment.
   - **Prometheus and Grafana**: Use Prometheus for monitoring and Grafana for visualization.
   - **CloudWatch**: Integrate with AWS CloudWatch for centralized logging and monitoring.

6. **Scaling**: Ensure your Jenkins deployment can scale to handle varying workloads.
   - **Horizontal Pod Autoscaler (HPA)**: Use HPA to automatically scale Jenkins agents based on CPU/memory usage or custom metrics.
   - **Cluster Autoscaler**: Use the Cluster Autoscaler to automatically adjust the number of worker nodes in your EKS cluster based on the resource requirements.

# ETAPA 12: AMPLIANDO ROLE Y CREANDO UN CLUSTER EN EKS:

![[Pasted image 20240822151258.png]]

En esta etapa surge esta consulta:
==debería desplegar Jenkins en el mismo EKS Cluster que la App del Cliente?, debería desplegarlo en uno paralelo?, debería hacer el despliegue de mi jenkins de manera local? Aca las consideraciones de la IA:

```
In a production-ready environment, the deployment strategy for Jenkins can vary based on several factors, including security, resource isolation, scalability, and operational requirements. Here are some considerations for each approach:

### 1. **Running Jenkins in the Same Cluster**

**Pros:**
- **Simplified Management**: Easier to manage a single cluster.
- **Resource Sharing**: Can share resources and configurations between Jenkins and the client application.

**Cons:**
- **Resource Contention**: Jenkins and the client application might compete for resources, potentially impacting performance.
- **Security Risks**: Increased risk if Jenkins is compromised, as it shares the same environment as the client application.

### 2. **Running Jenkins in a Separate Cluster**

**Pros:**
- **Resource Isolation**: Jenkins and the client application have dedicated resources, reducing the risk of resource contention.
- **Enhanced Security**: Isolating Jenkins in a separate cluster reduces the risk of compromising the client application if Jenkins is attacked.

**Cons:**
- **Increased Complexity**: Managing multiple clusters can be more complex and require additional operational overhead.
- **Cost**: Potentially higher costs due to running and maintaining multiple clusters.

### 3. **Running Jenkins on a Local VM or Separate Infrastructure**

**Pros:**
- **Complete Isolation**: Jenkins is entirely isolated from the client application, providing the highest level of security and resource isolation.
- **Flexibility**: Easier to manage Jenkins upgrades and maintenance without affecting the client application.

**Cons:**
- **Network Latency**: Potential network latency issues when Jenkins interacts with the Kubernetes cluster.
- **Operational Overhead**: Managing separate infrastructure for Jenkins can add operational overhead.

### Recommendations

1. **For Small to Medium Environments**:
   - **Separate Namespace**: You can run Jenkins in the same cluster but in a separate namespace to provide some level of isolation.
   - **Resource Quotas**: Use Kubernetes resource quotas and limits to ensure Jenkins does not consume resources needed by the client application.

2. **For Larger or Security-Sensitive Environments**:
   - **Separate Cluster**: Run Jenkins in a separate Kubernetes cluster to ensure complete resource and security isolation.
   - **Dedicated Infrastructure**: Consider running Jenkins on dedicated infrastructure (e.g., a separate VM or a dedicated Jenkins server) if you require the highest level of isolation and security.

### Summary

The decision to run Jenkins in the same cluster, a separate cluster, or on a separate VM depends on your specific requirements for security, resource management, and operational complexity. For production environments, it is generally recommended to isolate Jenkins from the client application, either by using a separate Kubernetes cluster or dedicated infrastructure, to ensure better security and resource management.

```


==puesto que se me hace lo más logico: desplegar jenkins en un EKS Cluster aparte y luego crearé el EKS para la aplicacion del cliente, en este caso la voting app

==Cluster para devops y despliegue de Jenkins:

```
eksctl create cluster --name tamayodevops-cluster --region us-west-2 --nodegroup-name standard-workers --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 4 --managed

configurar kubectl para usar el cluster de trabajo:
aws eks --region us-west-2 update-kubeconfig --name votingapp-cluster

kubectl create namespace jenkins

helm repo add jenkins https://charts.jenkins.io
helm repo update
helm install jenkins jenkins/jenkins --namespace jenkins



```



==cluster para voting app




# ETAPA 13:

# ETAPA 14:

# ETAPA 15:


