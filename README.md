# GitOps Sample

## Components
- [Argo CD]()
- [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)

## Local Kubernetes Environments
- [minikube](https://github.com/kubernetes/minikube)
- [docker-machine-driver-hyperkit](https://github.com/machine-drivers/docker-machine-driver-hyperkit)
- [helm](https://github.com/helm/helm)

## Setup
```bash
minikube start --vm-driver=hyperkit

kubectl create namespace argocd
# We use minikube for running kubernetes. So we can't setup Argo CD HA.
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v1.0.2/manifests/install.yaml
helm template setup \
  --namespace argocd \
  --values setup/values-dev.yaml | kubectl apply -f -
helm template setup \
  --namespace argocd \
  --values setup/values-prd.yaml | kubectl apply -f -
```

## Links
- [GitOps - Operations by Pull Request](https://www.weave.works/blog/gitops-operations-by-pull-request)
- [Cluster Bootstrapping (Application Of Applications Pattern) - Argo CD](https://argoproj.github.io/argo-cd/operator-manual/cluster-bootstrapping/#application-of-applications-pattern)
- [Declarative Setup (Manage Argo CD Using Argo CD) - Argo CD](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#manage-argo-cd-using-argo-cd)