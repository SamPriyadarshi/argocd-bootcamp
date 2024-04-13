# ArgoCD Bootcamp

## Description


## Clone this repository
```
git clone https://github.com/SamPriyadarshi/argocd-bootcamp.git
```

## Create ArgoCD GKE Cluster
```
gcloud container clusters create  argocd-cluster --region us-central1 --workload-pool=<project-id>.svc.id.goog
```

## Install and Configure ArgoCD

### Install ArgoCD
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Install Nginx Ingress
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx --create-namespace=true
```

### Install Cert-Manager
```
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.14.4 \
  --set installCRDs=true
```

### Deploy ArgoCD Ingress
```
kubectl apply -f [setup/ingress.yaml](setup/ingress.yaml)
```

### Update A Record in CloudDNS
```
kubectl get svc -n ingress-nginx
```
Go to CloudDNS and select the Public zone and add an A record with the IP that you just got from the above output.

### Deploy Cluster Issuer
```
kubectl apply -f [setup/clusterissuer.yaml](setup/clusterissuer.yaml)
```

### Update ArgoCD Password
```
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
echo "<encoded-password>" | base64 -d
argocd login <argocd-url>
argocd account update-password
```
