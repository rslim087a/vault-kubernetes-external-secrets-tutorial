# Kubernetes CICD Tutorial

This repo contains all the code needed to follow along with our **[YouTube Tutorial](https://)** or **[Written Article](https://)**.

## Prerequisites

To follow along with this tutorial, you'll need:

- kubectl installed and configured ([https://youtu.be/IBkU4dghY0Y](https://youtu.be/IBkU4dghY0Y))
- Helm installed: ([https://kubernetestraining.io/blog/installing-helm-on-mac-and-windows](https://kubernetestraining.io/blog/installing-helm-on-mac-and-windows))

## Install ESO

```bash
# Add the External Secrets Helm repository
helm repo add external-secrets https://charts.external-secrets.io

# Update Helm repositories
helm repo update

# Install External Secrets Operator
helm install external-secrets external-secrets/external-secrets \\
  --namespace external-secrets \\
  --create-namespace \\
  --version 0.15.0
```

## Creating Authentication for Vault

```bash
# Create a namespace for our demo
kubectl create namespace demo

# Create a Kubernetes secret with the Vault token
kubectl create secret generic vault-token \\
  --namespace demo \\
  --from-literal=token=root
  ```
## Test using pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: api-consumer
  namespace: demo
spec:
  containers:
  - name: api-consumer
    image: busybox
    command: ['sh', '-c', 'echo "Using API Key: $API_KEY" && sleep 3600']
    env:
    - name: API_KEY
      valueFrom:
        secretKeyRef:
          name: dev-api-credentials
          key: api-key
```

## Kubernetes Training

If you found this guide helpful, check out our [Kubernetes Training course](https://kubernetestraining.io/)
