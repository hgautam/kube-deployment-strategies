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
$ ingress_ip=$(minikube ip); while sleep 1; do curl http://$ingress_ip -H "Host: go-app.com"; done

# To see the deployment in action, open a new terminal and run the following
# command:
$ watch kubectl get po

# Deploy version 2
$ kubectl apply -f app-ver2.yaml

# Create a canary ingress in order to split traffic: 90% to v1, 10% to v2
$ kubectl apply -f ingress-ver2-canary.yaml

# Validate application
$ while sleep 1; do curl http://$ingress_ip -H "Host: go-app.com"; done

# When you ready, delete the canary ingress
$ kubectl delete -f ingress-ver2-canary.yaml

# Finish the rollout by setting 100% traffic to version 2
$ kubectl apply -f ingress-ver2.yaml
```

### Clean up
```
kubectl delete all -l app=go-app
```