## Blue-green deployments

In blue-green deployment strategy, version 2 (green) is deployed alongside version 1 (blue) with exactly the same amount of instances. After testing that version 2 meets all the requirements, the traffic is switched from version 1 to version 2. 

### Requirements
1. Minikube installed. Instructions are found [here.](https://minikube.sigs.k8s.io/docs/start/)
2. kubectl installed. Instructions are found [here.](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Demo steps
1. version 1 is serving the traffic
2. deploy version 2 on separate set of pods
3. wait until version 2 is ready
4. switch incoming traffic from version 1 to version 2
5. delete version 1

### Demo Details

```bash
# Create minikube cluster
$ minikube start --memory 2048 --cpus=2

# Deploy the first application
$ kubectl apply -f app-ver1.yaml

# Validate the deployment
$ curl $(minikube service go-app --url)
Host: go-app-v1-658588dc46-tm2vz, Version: v1.0.0

# To see the deployment in action, open a new terminal and run the following
# command:
$ watch kubectl get po

# Then deploy version 2 of the application
$ kubectl apply -f app-ver2.yaml

# Wait for all the version 2 pods to be running
$ kubectl rollout status deploy go-app-v2 -w
deployment "go-app-v2" successfully rolled out

# Side by side, 3 pods are running with version 2 but the service object still sending
# traffic to version. Lets validate:
$ service=$(minikube service go-app --url); while sleep 1; do curl "$service"; done


# Lets manually test one of the pods by port-forwarding it to our local cluster
# Select one of the newly created pods and run the following command on a terminal window:
$ kubectl port-forward go-app-v2-58f7cbc964-vrd5q 8081:8080

# On a separate terminal window, type the following command:
$ curl http://localhost:8081
Host: go-app-v2-58f7cbc964-vrd5q, Version: v2.0.0

# After successfully validating version 2, we can switch the traffic to version 2 by patching
# the service to send traffic to all pods with label version=v2.0.0
$ kubectl patch service go-app -p '{"spec":{"selector":{"version":"v2.0.0"}}}'

# Validate service object is directing traffic to version 2.
$ while sleep 1; do curl "$service"; done

# In case you need to rollback to the previous version
$ kubectl patch service go-app -p '{"spec":{"selector":{"version":"v1.0.0"}}}'

# If everything is working as expected, you can then delete the v1.0.0
# deployment
$ kubectl delete deploy go-app-v1
```

### Clean up
```
$ kubectl delete all -l app=go-app
```
