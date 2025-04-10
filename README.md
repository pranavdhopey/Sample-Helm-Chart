# Sample Helm Chart

This repository contains a sample Helm chart used to demonstrate Helm packaging, templating, and deployment in Kubernetes clusters. It is a minimal Helm chart that deploys a basic APACHE and NGINX web server.

## 🧰 Prerequisites

- [Helm 3.x](https://helm.sh/docs/intro/install/)
- Access to a Kubernetes cluster (e.g., Minikube, GKE, EKS, etc.)
- `kubectl` configured for your cluster

## 🚀 Installation

Clone the repository and switch to the `test` branch:

```bash
git clone -b test https://github.com/pranavdhopey/Sample-Helm-Chart.git
cd Sample-Helm-Chart/SampleApp
helm install sampleapp -f dev-values.yaml .
```

## 📦 Chart Structure

```bash
SampleApp/
├── .helmignore
├── charts/
│   │── app1/
│   │   └── templates/
│   │      ├── _helpers.tpl
│   │      ├── backendconfig.yaml
│   │      ├── deployment.yaml
│   │      ├── hpa.yaml
│   │      ├── pdb.yaml
│   │      ├── service.yaml
│   │      └── serviceaccount.yaml
│   │── app1/
│   │   └── templates/
│   │      ├── _helpers.tpl
│   │      ├── backendconfig.yaml
│   │      ├── deployment.yaml
│   │      ├── hpa.yaml
│   │      ├── pdb.yaml
│   │      ├── service.yaml
│   │      └── serviceaccount.yaml
├── templates/
│   ├── _helpers.tpl
│   ├── cronjob.yaml
│   ├── frontendConfig.yaml
│   ├── ingress.yaml
│   ├── internal-ingress.yaml
│   ├── managed-certificate.yaml
│   └── serviceaccount.yaml
├── Chart.yaml
├── dev-values.yaml
├── prod-values.yaml
└── stage-values.yaml

```

## 🧪 Testing
To test the chart's rendering without installing:
```bash
helm template sampleapp -f dev-values.yaml .
```