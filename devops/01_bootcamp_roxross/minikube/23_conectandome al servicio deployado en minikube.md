despues de 5 días el servicio sigue activo:

![[Pasted image 20240424123319.png]]

==pero desde mi equipo no puedo conectarme a esa direccion, en efecto, minikube levanta una VM desde donde es accesible pero exclusivamente desde ese host.

METODOS PARA ACCEDER:
=

==METODO 1: USANDO UN SSH PORT FORWARDING

![[Pasted image 20240424125106.png]]

el resultado del comando anterior es que se abrirá una sesion en el remoteHost:

![[Pasted image 20240424125224.png]]

==probando si la conexion funciona:

![[Pasted image 20240424125305.png]]

![[Pasted image 20240424125325.png]]

==METODO 2: USANDO NGINX AS LOAD BALANCER

Yes, you can use NGINX as a load balancer for your on-premise Kubernetes cluster. However, it's important to note that this setup involves installing and configuring an NGINX Ingress Controller, which is a bit more complex than using a NodePort service.

Here's a high-level overview of the steps you would need to follow:

1. **Install the NGINX Ingress Controller**: You can install the NGINX Ingress Controller using Helm, a package manager for Kubernetes. Here's an example command:

   ```bash
   helm install nginx stable/nginx-ingress
   ```

2. **Create an Ingress resource**: You would need to create a new Kubernetes resource of kind `Ingress`. This resource would define how requests to your service should be routed. Here's an example based on your existing service:

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: jenkins-ingress
   spec:
     rules:
     - http:
         paths:
         - pathType: Prefix
           path: "/"
           backend:
             service:
               name: jenkins-service
               port:
                 number: 80
   ```

3. **Access your service**: Once the Ingress resource is created, you should be able to access your service at the IP address of your NGINX Ingress Controller. You can get this IP address with the following command:

   ```bash
   kubectl get ingress jenkins-ingress
   ```

Remember to replace `jenkins-ingress` with the name of your Ingress resource.

This setup allows you to expose multiple services on the same IP address and port, and it provides features like SSL termination and name-based virtual hosting. However, it requires more setup and maintenance than a NodePort service.

==deberia instalar nginx en el mismo namespace?

Yes, it's generally a good practice to deploy the NGINX Ingress Controller in the same namespace as your application. This makes it easier to manage your resources and apply policies consistently.

You can specify the namespace when you install the NGINX Ingress Controller with Helm. Here's an example command:

```bash
helm install nginx stable/nginx-ingress --namespace jenkinsvit
```

This command will install the NGINX Ingress Controller in the `jenkinsvit` namespace.

After you've installed the NGINX Ingress Controller, you can create an Ingress resource in the same namespace to route traffic to your Jenkins service. Here's an example based on your existing service:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: jenkins-ingress
  namespace: jenkinsvit
spec:
  rules:
  - http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: jenkins-service
            port:
              number: 80
```

This Ingress resource will route traffic from the NGINX Ingress Controller to your Jenkins service. Remember to replace `jenkins-ingress` and `jenkins-service` with the names of your Ingress resource and service.