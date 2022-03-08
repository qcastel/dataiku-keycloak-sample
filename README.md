# Keycloak deployments on k8s - Example

## Prerequisite

Deploy a managed Postgres into GCP
Create a K8s cluster

## Install SOPS

```
source <(curl -s https://raw.githubusercontent.com/viaduct-ai/kustomize-sops/master/scripts/install-ksops-archive.sh)
```


### Login with GCLOUD

```
gcloud auth login
```


## Encrypt secret

```
sops --encrypt --gcp-kms projects/dataiku-dev/locations/global/keyRings/sops/cryptoKeys/sops-key db.secrets.yml > db.secrets.enc.yml
sops --encrypt --gcp-kms projects/dataiku-dev/locations/global/keyRings/sops/cryptoKeys/sops-key keycloak.secrets.yml > keycloak.secrets.enc.yml
```

## Decrypt secret

```
sops --decrypt keycloak.secrets.enc.yml
sops --decrypt db.secrets.enc.yml
```


# Deploy to kubernetes cluster

```
kubectl apply -f namespace.yml
kubectl apply -f db.secrets.yml
kubectl apply -f keycloak.secrets.yml
kubectl apply -f sql-proxy.deployment.yml
kubectl apply -f keycloak.deployment.yml
kubectl apply -f keycloak.service.yml
```