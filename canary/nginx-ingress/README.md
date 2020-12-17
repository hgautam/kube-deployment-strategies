## Canary deployment with Nginx Ingress

```bash
# Create minikube cluster
$ minikube start --memory 2048 --cpus=2

# Enalbe nginx ingress controller
$ minikube addons enable ingress

# Deploy version 1 and expose the service via an ingress
$ kubectl apply -f app-ver1.yaml -f ingress-ver1.yaml

# Get ingress ip address
$ kubectl get ingress
NAME     CLASS    HOSTS        ADDRESS         PORTS   AGE
go-app   <none>   go-app.com   192.168.64.77   80      7m45s

# Validate application
$ curl http://192.168.64.77 -H "Host: go-app.com"
Host: go-app-v1-658588dc46-7nchl, Version: v1.0.0
```