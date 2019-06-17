# GitOps Sample

## Components
- [Argo CD](https://github.com/argoproj/argo-cd)
- [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)

## Local Kubernetes Environments
- [minikube](https://github.com/kubernetes/minikube)
- [docker-machine-driver-hyperkit](https://github.com/machine-drivers/docker-machine-driver-hyperkit)
- [helm](https://github.com/helm/helm)

## Setup
```bash
minikube start --vm-driver=hyperkit

kubectl create namespace argocd
kubectl create namespace sealed-secrets
kubectl create namespace app

# We use minikube for running kubernetes. So we can't setup Argo CD HA.
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v1.0.2/manifests/install.yaml

helm template setup \
  --namespace argocd \
  --values setup/values-dev.yaml | kubectl apply -f -
```

## Secrets
```bash
mkdir tmp
kubeseal --fetch-cert \
  --controller-namespace=sealed-secrets \
  --controller-name=sealed-secrets \
  > tmp/cert.pem
kubectl create secret generic nginx-top --namespace app --dry-run --from-literal=index.html=Hello -o yaml > tmp/secret.yaml
kubeseal --format=yaml --cert=cert.pem < tmp/secret.yaml > nginx-app/sealedsecret.yaml
```

## Links
- [GitOps - Operations by Pull Request](https://www.weave.works/blog/gitops-operations-by-pull-request)
- [Cluster Bootstrapping (Application Of Applications Pattern) - Argo CD](https://argoproj.github.io/argo-cd/operator-manual/cluster-bootstrapping/#application-of-applications-pattern)
- [Declarative Setup (Manage Argo CD Using Argo CD) - Argo CD](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#manage-argo-cd-using-argo-cd)
