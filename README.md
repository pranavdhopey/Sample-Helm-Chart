# Sample Helm Chart

This Helm chart deploys the **SampleApp** suite into a Kubernetes cluster. It supports multi-environment configurations (`dev`, `stage`, `prod`) and includes subcharts for individual sample services such as `app1`, `app2`. The chart is designed with flexibility in mind, supporting features like HPA, PDB, CronJobs, ingress management, and more.


## 🧰 Prerequisites

- [Helm 3.x](https://helm.sh/docs/intro/install/)
- Access to a Kubernetes cluster (e.g., Minikube, GKE, EKS, etc.)
- `kubectl` configured for your cluster

## 🚀 Installation

Clone the repository and switch to the `test` branch:

```bash
git clone -b test https://github.com/pranavdhopey/Sample-Helm-Chart.git
cd Sample-Helm-Chart/SampleApp
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

## 🧪 Testing for an Environment
You can install the chart for a specific environment using the corresponding values file:

```bash
helm install sampleapp ./ -f dev-values.yaml --namespace dev --create-namespace
```

Or for staging:

```bash
helm install sampleapp ./ -f stage-values.yaml --namespace staging --create-namespace
```

For production:

```bash
helm install sampleapp ./ -f prod-values.yaml --namespace prod --create-namespace
```


## 🔄 Uninstall
To uninstall the chart and release:

```bash
helm uninstall sampleapp
```