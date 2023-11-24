
Basado en este tutorial: https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app


el proyecto se llama gobankrestapi

export PROJECT_ID=gobankrestapi

gcloud config set project $PROJECT_ID

gcloud artifacts repositories create gobankrestapirepo \
   --repository-format=docker \
   --location=us-west2 \
   --description="Gobank RestAPI Docker repository"
   
   
   verificar si fue creado el repo: gcloud artifacts repositories list
   
   
   https://github.com/hftamayo/gobank.git
   
   cd gobank
     
   
   docker build -t us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestdocker/gobank:v1 .
   
   pero en mi caso uso un multicontainer asi que:
   
   docker-compose up -d 
   
   sin embargo GKE no tiene full soporte a docker-compose asi que toca hacer el decople con un manifiesto de Kubernetes o bien partirlo en Dockerfiles equivalentes al numero de contenedores. En mi caso use 2 Dockerfiles y los comandos fueron estos:
   
      docker build -f pg_Dockerfile -t us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankdb:v1 .
      docker build -f app_Dockerfile -t us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankapp:v1 .
   
   
   verificar si las imagenes se generaron:
   
   docker images
   
   creando el IAM policy:
   
   gcloud artifacts repositories add-iam-policy-binding gobankrestapirepo \
    --location=us-west2 \
    --member=serviceAccount:215836713221-compute@developer.gserviceaccount.com \
    --role="roles/artifactregistry.reader"
    
    
    Listar si este recurso IAM se creo satisfactoriamente:
    
    gcloud iam service-accounts list
    
    
    verificando si los containers corren:
    
    docker run --rm -p 5432:5432 us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankdb:v1
    docker run --rm -p 8001:8001 us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankapp:v1
    
    intenté correr los containers pero dice que no tengo que ser root (creo que la credencial inicia bajo dichos permisos) asi que seguiré adelante.
    
    
    gcloud auth configure-docker us-west2-docker.pkg.dev
    
    docker push us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankdb:v1
    docker push us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankapp:v1
    
    
    verificar si las imagenes fueron pusheadas: gcloud artifacts docker images list us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo
    
    
    Creando el GKE Cluster:
    -  gcloud config set compute/region us-west2
    -  gcloud container clusters create-auto gobankrestapi-cluster
    
    
    Deploy de los containers al cluster:
    
    El Pod puede tener uno varios containers pero de la misma aplicacion, el FrontEnd y BackEnd debe deployarse en pods separados:
    
    Verificando si estamos conectados al GKE cluster:
    - gcloud container clusters get-credentials gobankrestapi-cluster --region us-west2
    
    Deployando las imagenes al Kubernete:
    Un container: kubectl create deployment gobankdb-rest --image=us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankdb:v1
    Dos imagenes en pods separados: 
    - kubectl create deployment gobankrestdb --image=us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankdb:v1
    - kubectl create deployment gobankapprestapp --image=us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestapirepo/gobankapp:v1
    
    Despues de cada comando tira un warning pero abajo dice que el pod se creo
    
    Setear 3 replicas:
    
    - kubectl scale deployment gobankrestdb --replicas=3
    - kubectl scale deployment gobankapprestapp --replicas=3
    
    Crear un Horizontal AutoScaler:
    
    - kubectl autoscale deployment gobankrestdb --cpu-percent=80 --min=1 --max=5
    - kubectl autoscale deployment gobankapprestapp --cpu-percent=80 --min=1 --max=5    
    
    Ver los pods creados:
    
    - kubectl get pods
    
    
    En esta etapa ambos pods me quedaron en error the CrashLoopBackOff, segun ChatGPT esto significa:
    
    The `CrashLoopBackOff` status in Kubernetes indicates that a container within a pod is crashing immediately after starting, and Kubernetes is attempting to restart it, but it continues to fail repeatedly. This status suggests that the container is unable to start successfully, causing it to crash and enter a restart loop.

When you see the `CrashLoopBackOff` status in Google Kubernetes Engine (GKE) pods, it usually means there's an issue with the application or the container configuration. Here are some common reasons why this error might occur:

1. **Misconfigured Application:** There could be an error in the application code or configuration files, causing the application to crash when it starts.

2. **Missing Dependencies:** The container might rely on external services or dependencies that are not available or not properly configured within the cluster.

3. **Incorrect Environment Variables:** The application might depend on certain environment variables that are not set correctly in the pod's environment.

4. **Incorrect Ports:** The application might be trying to bind to a port that is already in use or a port that is not accessible from within the container.

5. **Volume Mount Issues:** If the container relies on mounted volumes, there could be issues with the volume mounts or permissions.

6. **Resource Constraints:** The container might be requesting more resources (CPU, memory) than the node can provide, causing it to crash.

To troubleshoot the `CrashLoopBackOff` status, you can:

- **Check Logs:** Use `kubectl logs POD_NAME` to view the logs of the failing container. This can provide valuable information about what's going wrong.

- **Review Configuration:** Double-check your pod and container configurations (including environment variables, ports, volume mounts) for correctness.

- **Check Resource Usage:** Ensure that the container's resource requests and limits are appropriate. You can inspect this in your pod's YAML configuration.

- **Look at Application Code:** If your application is custom, review the application code for errors, especially in the initialization process.

- **Examine External Dependencies:** If your application relies on external services, ensure they are accessible and properly configured.

By investigating these areas, you can often pinpoint the root cause of the `CrashLoopBackOff` error and take appropriate corrective actions.


verficando los errores:

kubectl logs "name del pod"

pg_ctl: cannot be run as root. Please log in (using, e.g., "su") as the (unprivileged) user that will own the server process

24 horas depues de tener estos pods en linea se me ha gastado $8:

![[Pasted image 20231014215325.png]]

lunes a las 8:15 am:

![[Pasted image 20231016081429.png]]

segun los logs y la consulta que hice en reddit los pods postgres deben ejecutarse por la cuenta de usuario postgres

Depues de un par de verificacion de recursos, identifico que puedo caer en un rabbit hole asi que daré de baja estos containers:

    =============
    CLEAN-UP
    =============
    
    si solo hice la imagen local sin pushearla: docker rmi <image id>
    borrar el proyecto: https://cloud.google.com/go/getting-started
    
    
    si termine todo el tutorial:
    
    
    gcloud artifacts docker images delete \
    us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestdocker/gobank:v1 \
    --delete-tags --quiet
gcloud artifacts docker images delete \
    us-west2-docker.pkg.dev/${PROJECT_ID}/gobankrestdocker/gobank:v2 \
    --delete-tags --quiet







Este tutorial levanta unos k8s de postgresl en HA: https://cloud.google.com/kubernetes-engine/docs/tutorials/stateful-workloads/postgresql