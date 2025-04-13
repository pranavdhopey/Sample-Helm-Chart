## 🔐 External Secrets Helm Chart

This Helm chart deploys:

* `SecretStore` or `ClusterSecretStore` using External Secrets Operator (ESO)
* ExternalSecret to sync secrets from external providers like GCP Secret Manager, AWS Secrets Manager, etc.

## 🧰 Features

* Supports GCP Workload Identity or other provider-based secret management.
* Auto-creates Kubernetes secrets from external secret stores.
* Customizable via Helm values.yaml.

## 🗂️ Structure

This chart includes:

* ClusterSecretStore/SecretStore: Connects Kubernetes to an external secret manager (GCP Secret Manager).
* ExternalSecret: Defines which secrets to sync and where to store them inside Kubernetes.


## 🔧 Usage
1. Add the Helm Repo (optional if you’re managing it locally)

```bash
helm repo add my-secrets https://charts.external-secrets.io
helm repo update
```

2. Install the Chart

```bash
helm install external-secret external-secrets/external-secrets -n external-secret --create-namespace
```

## 📘 Example values.yaml

For GCP ClusterSecretStore

```
clustersecretstore:
  enabled: true
  name: gcp-cluster-secret-store
  namespace: external-secret
  provider:
    gcpsm:
      auth:
        workloadIdentity:
          clusterLocation: <cluster-location>
          clusterName: <cluster-name>
          serviceAccountRef:
            name: external-secret
      projectID: <gcp-project-id>
```

For GCP SecretStore

```
secretstore:
  enabled: true
  name: gcp-secret-store
  namespace: <namespace>
  provider:
    gcpsm:
      auth:
        workloadIdentity:
          clusterLocation: <cluster-location>
          clusterName: <cluster-name>
          serviceAccountRef:
            name: <namespace>
      projectID: <gcp-project-id>
```

For ExternalSecret

```
externalsecret:
  enabled: true
  name: <external-secret-name>
  labels: {}
  annotations: {}
  secretStoreRef:
    name: gcp-secret-store
    kind: SecretStore
  refreshInterval: "1m"
  target:
    name: k8s-secret
    creationPolicy: Owner
    deletionPolicy: Retain
  data:
    - secretKey: DB_USERNAME
      remoteRef:
        key: external-secret
        version: v1
        property: DB_USERNAME
        decodingStrategy: None

```
