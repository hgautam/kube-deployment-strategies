## Manual Canary Deployment strategy

In this approach, we will use native Kubernetes commands to do a canary deployment.

```bash
# # Create minikube cluster
$ minikube start --memory 2048 --cpus=2

# Deploy the first application
$ kubectl apply -f app-ver1.yaml

# Validate the deployment
$ curl $(minikube service go-app --url)
Host: go-app-v1-658588dc46-tm2vz, Version: v1.0.0

# To see the deployment in action, open a new terminal and run the following
# command:
$ watch kubectl get po

# Next, deploy version 2 and scale down version 1 to 9 replicas at same time
$ kubectl apply -f app-ver2.yaml
$ kubectl scale --replicas=9 deploy go-app-v1

# Only one pod with the new version should be running.
# Lets test if the second deployment was successful
$ service=$(minikube service go-app --url); while sleep 1; do curl "$service"; done

# Next, scale up version 2 to 3 replicas and scale down version 1 to 7 replicas
$ kubectl scale --replicas=3 deploy go-app-v2
$ kubectl scale --replicas=7 deploy go-app-v1

# More pods should now show version 2. Let's validate
$ while sleep 1; do curl "$service"; done

# Finally, lets scale up version 2 to 10 and delete version 1
$ kubectl scale --replicas=10 deploy go-app-v2
$ kubectl delete deploy go-app-v1

# Now all pods should show version 2. Let's validate
$ while sleep 1; do curl "$service"; done
```

### Clean up
```
$ kubectl delete all -l app=go-app
$ minikue delete
```