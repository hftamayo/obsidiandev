
Based on your situation, I'd recommend setting up a lightweight Kubernetes development environment that can serve as both a testing ground and a stepping stone to your production setup. Here's what I suggest:

1. Use your cloud provider credits to set up a small managed Kubernetes cluster (like EKS on AWS, GKE on Google Cloud, or AKS on Azure). This is preferable to Heroku or standalone containers since you've indicated Kubernetes will be part of your production environment.
    
2. Implement a GitOps approach using tools like ArgoCD or Flux, which will:
    
    - Allow you to version control your infrastructure configurations
    - Provide automatic synchronization between your Git repos and the cluster state
    - Give you a foundation for your future CI/CD pipeline
3. For CI/CD without managing Jenkins, consider GitHub Actions or GitLab CI if you're already using these platforms for source control. These are cloud-hosted options that require minimal setup and maintenance.
    
4. Set up infrastructure as code using Terraform or Pulumi to create reproducible environments that can be easily destroyed when not in use, saving your credits.
    
5. Start implementing basic DevSecOps practices early:
    
    - Integrate container scanning in your build process
    - Implement network policies in your Kubernetes configurations
    - Use secrets management from the beginning

This approach gives you several advantages:

- Resembles your production environment closely
- Minimizes maintenance overhead so you can focus on code
- Allows you to develop deployment automation iteratively
- Provides easy teardown when not in use to save credits
- Builds toward your DevSecOps goals from the start

Would you like me to elaborate on any specific part of this recommendation?