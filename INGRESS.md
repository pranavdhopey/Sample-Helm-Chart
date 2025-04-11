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

## 🔧 Sample values.yaml for External Ingress

```
managedCertificateEnabled: true
managedCertificate:
  - name: sampleapp-cert
    domains:
      - example.com

frontendConfigEnabled: true
frontendConfig:
  name: sampleapp-frontendconfig
  redirectToHttps:
    enabled: true
    responseCodeName: "PERMANENT_REDIRECT"
  sslPolicy: "gke-ssl-policy"

ingress:
  enabled: true
  name: sampleapp-ingress
  className: "gce"  # or "gce-internal" for internal load balancer
  annotations:
    kubernetes.io/ingress.global-static-ip-name: "my-static-ip"
    networking.gke.io/managed-certificates: "sampleapp-cert"
    networking.gke.io/frontend-config: "sampleapp-frontendconfig"
  tls:
    - hosts:
        - example.com
      secretName: tls-secret
  hosts:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: ImplementationSpecific
            backend:
              service:
                name: sampleapp-service
                port:
                  number: 80

```

## 🔧 Sample values.yaml for Internal Ingress

```
ingress:
  enabled: true
  name: sampleapp-ingress
  className: "gce-internal" # for internal load balancer
  annotations:
    kubernetes.io/ingress.global-static-ip-name: "my-static-ip"
    kubernetes.io/ingress.allow-http: 'true' # or 'false'
  tls:
    - hosts:
        - example.internal
      secretName: tls-secret
  hosts:
    - host: example.internal
      http:
        paths:
          - path: /
            pathType: ImplementationSpecific
            backend:
              service:
                name: sampleapp-service
                port:
                  number: 80

```



## 📚 Reference Links

🌐 [GKE Ingress Overview (Official)](https://cloud.google.com/kubernetes-engine/docs/how-to/ingress-configuration)
🔐 [ManagedCertificate in GKE](https://cloud.google.com/kubernetes-engine/docs/how-to/managed-certs)
⚙️ [FrontendConfig Customization](https://cloud.google.com/kubernetes-engine/docs/how-to/ingress-configuration#configuring_ingress_features_through_frontendconfig_parameters)
🧠 [Internal HTTP(S) Load Balancing on GKE](https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balance-ingress#https_between_client_and_load_balancer)
