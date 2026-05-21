
## Current DNS and deployment architecture

Our current setup separates **domain registration**, **DNS management**, **TLS certificate management**, and **traffic routing** across the appropriate services.

The domain is still **registered at name.com**, which means name.com remains the place where the domain is purchased, renewed, and administratively owned. However, DNS is now delegated to **AWS Route 53**. In name.com, we replaced the domain’s authoritative nameservers with the Route 53 nameservers generated for the hosted zone. Because of that, public DNS queries for the domain are now answered by Route 53 instead of name.com.

In AWS, Terraform manages a **Route 53 hosted zone** for the domain and creates DNS records for the application. For the experimental environment, Route 53 contains an Alias A record such as:

```plain text
exp.absencesbo.tamayo.dev -> AWS Network Load Balancer
```


This Alias record points directly to the AWS Load Balancer instead of hardcoding a CNAME to the long AWS load balancer DNS name. That gives us a stable public URL while allowing the underlying AWS Load Balancer DNS name to change if the load balancer is recreated.

## TLS and ACM certificate handling

HTTPS is handled with **AWS Certificate Manager ACM**. The application certificate was issued for:

```plain text
exp.absencesbo.tamayo.dev
```


The ACM certificate is attached to the AWS Load Balancer HTTPS listener. This means TLS terminates at the AWS Load Balancer. Users connect to:

```plain text
https://exp.absencesbo.tamayo.dev
```


The request reaches Route 53, resolves to the AWS Load Balancer, and the Load Balancer presents the ACM certificate during the TLS handshake.

The ACM validation CNAME was originally created in name.com to prove ownership of the domain. Since DNS is now delegated to Route 53, that validation CNAME also needs to exist in Route 53. This is important because ACM uses the validation CNAME not only for the initial certificate issuance but also for future certificate renewals.

So the Route 53 hosted zone should contain both:

```plain text
exp.absencesbo.tamayo.dev
A Alias -> AWS Load Balancer
```


and:

```plain text
_d98172c7089c8756135db1835ab212d0.exp.absencesbo.tamayo.dev
CNAME -> _30d9dddeb5fc02660b4454fec3bad374.jkddzztszm.acm-validations.aws
```


## Traffic flow

The runtime traffic flow is:

```plain text
User browser
  -> https://exp.absencesbo.tamayo.dev
  -> Route 53 DNS resolution
  -> AWS Network Load Balancer
  -> TLS termination using ACM certificate
  -> Target group on NodePort 32322
  -> EC2 instance running k3s
  -> nginx Kubernetes Service
  -> nginx pod
  -> frontend/backend services inside the cluster
```


Terraform manages the AWS infrastructure pieces:

- EC2 instance
- security groups
- Network Load Balancer
- target group
- HTTP and HTTPS listeners
- Route 53 hosted zone
- Route 53 Alias record
- outputs such as load balancer DNS name, application URL, hosted zone ID, and Route 53 nameservers

The GitHub Actions deployment pipeline applies the Terraform configuration and prints the relevant outputs, including:

```plain text
Route53 Zone ID
Route53 Zone Name
Route53 Name Servers
Application DNS URLs
Load Balancer DNS
```


These outputs allow us to verify the DNS setup and confirm what nameservers need to be configured in name.com.

## Role of name.com

After the migration, name.com is no longer managing individual DNS records for the application. It remains the **domain registrar only**.

The important setting in name.com is the domain’s nameservers. Those nameservers now point to Route 53:

```plain text
ns-1307.awsdns-35.org
ns-168.awsdns-21.com
ns-1713.awsdns-22.co.uk
ns-793.awsdns-35.net
```


That delegation tells the internet:

```plain text
For tamayo.dev DNS records, ask Route 53.
```


Once this is configured, DNS records inside name.com are no longer authoritative for the public domain. Any app records, validation records, or future environment records should be managed in Route 53.

## Why we do not need to update nginx or k3s

We do **not** need to update the nginx container or the k3s cluster because TLS and public DNS are handled outside the cluster.

The nginx service inside k3s is still exposed through a Kubernetes `NodePort` on port `32322`. The AWS Network Load Balancer, created by Terraform, forwards traffic to that NodePort. The Load Balancer is responsible for accepting public traffic on ports `80` and `443`. For HTTPS traffic, the Load Balancer uses the ACM certificate and terminates TLS before forwarding the request to nginx over plain HTTP.

Because of that, nginx does not need to know about ACM, Route 53, name.com, or AWS Load Balancer certificates. From nginx’s perspective, it continues serving HTTP traffic on port `80` inside the cluster. The cluster does not need AWS Load Balancer Controller annotations, Kubernetes `Service type: LoadBalancer`, or container-level TLS configuration for this current architecture.

Those Kubernetes AWS load balancer annotations would only be needed in a different architecture where Kubernetes itself creates and manages the AWS Load Balancer, such as with EKS and the AWS Load Balancer Controller. In our current setup, Terraform owns the Load Balancer, Route 53 owns DNS, ACM owns the certificate, and k3s/nginx simply receives forwarded HTTP traffic.

## Summary

The final architecture is:

```plain text
name.com
  -> domain registrar
  -> delegates DNS to Route 53 nameservers

Route 53
  -> authoritative DNS provider
  -> manages exp.absencesbo.tamayo.dev
  -> Alias A record points to AWS Load Balancer
  -> stores ACM validation CNAME

ACM
  -> issues and renews TLS certificate for exp.absencesbo.tamayo.dev

AWS Network Load Balancer
  -> public traffic entrypoint
  -> terminates HTTPS using ACM certificate
  -> forwards traffic to EC2/k3s NodePort

k3s + nginx
  -> serves internal HTTP traffic
  -> routes requests to frontend and backend services
```


This gives us a stable public HTTPS URL, AWS-managed DNS, ACM-managed TLS, and a simple k3s runtime that does not need to manage public certificates directly.