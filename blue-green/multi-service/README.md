## Multi-service blue-green deployment

```bash
# Launch minikube cluster
$ minikube start --memory 2048 --cpus=2

# Install Traefik with Helm
# Add Helm repo
$ helm repo add traefik https://helm.traefik.io/traefik
# Install chart
$ helm install traefik traefik/traefik --version 9.11.0

# Install version 1 of apps a and b
$ kubectl apply -f app-a-ver1.yaml -f app-b-ver1.yaml -f ingress-ver1.yaml

# Validate the deployment
$ ingress=$(minikube service traefik --url | head -n1); curl $ingress -H 'Host: a.domain.com'
Host: go-app-a-v1-59b7b98958-5s2wg, Version: v1.0.0

$ curl $ingress -H 'Host: b.domain.com'
Host: go-app-b-v1-768ffd7c6b-j6dnk, Version: v1.0.0
```