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

3. First, create the Kubernetes service account with an annotation that references the GCP service account:

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: <service-account>
  namespace: <namepsace>
  annotations:
    iam.gke.io/gcp-service-account: [GCP_SA]@[PROJECT_ID].iam.gserviceaccount.com
```

4. Grant the Kubernetes service account the iam.workloadIdentityUser role on the GCP service account:

```
gcloud iam service-accounts add-iam-policy-binding \
  ${GCP_SA}@${PROJECT_ID}.iam.gserviceaccount.com \
  --role="roles/iam.workloadIdentityUser" \
  --member "serviceAccount:${PROJECT_ID}.svc.id.goog[${K8S_NAMESPACE}/${K8S_SA}]"
```


## 📘 Example values.yaml

For GCP ClusterSecretStore

```
clustersecretstore:
  enabled: true
  name: gcp-cluster-secret-store
  namespace: <namespace>
  provider:
    gcpsm:
      auth:
        workloadIdentity:
          clusterLocation: <cluster-location>
          clusterName: <cluster-name>
          serviceAccountRef:
            name: <service-account>
      projectID: <gcp-project-id>
```

Or for GCP SecretStore

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
            name: <service-account>
      projectID: <gcp-project-id>
```

For ExternalSecret

```
externalsecret:
  enabled: true
  name: <external-secret-name>
  namespace: <namespace>
  labels: {}
  annotations: {}
  secretStoreRef:
    name: gcp-secret-store
    kind: SecretStore       # Or ClusterSecretStore
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


## 📚 Reference Links

🌐 [Integration with Google Cloud Secret Manager (Official)](https://external-secrets.io/latest/provider/google-secrets-manager/)