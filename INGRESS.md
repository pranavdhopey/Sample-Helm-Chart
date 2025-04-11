## 📘 Ingress Setup with ManagedCertificate and FrontendConfig

This Helm chart deploys GKE-compatible ingress resources including:

* A Kubernetes Ingress resource
* ManagedCertificate for automatic TLS
* FrontendConfig to enable features like HTTPS redirection and SSL policies

## 📁 Templates Used

* templates/ingress.yaml
* templates/internal-ingress.yaml
* templates/managed-certificate.yaml
* templates/frontendconfig.yaml

## ✅ Notes

* Make sure you have enabled the GKE Ingress controller in your cluster.
* Use a pre-reserved global static IP and reference it in annotations.kubernetes.io/ingress.global-static-ip-name.
* TLS can be either:
  * Automatically managed via ManagedCertificate
  * Or via your own Kubernetes Secret using .ingress.tls.secretName
* FrontendConfig is optional and lets you enforce SSL, tweak headers, etc.

## 📎 Additional Tips
* If using ManagedCertificate, your domain must have a valid DNS record pointing to the Load Balancer IP.
* `ManagedCertificate` and `FrontendConfig` resources are specific to GKE and won’t work on other Kubernetes distributions.