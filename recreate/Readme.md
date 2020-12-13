## Recreate startegy demo on Minikube

Version 1 is deleted and then version 2 is rolled out.

As part of this demo, we will delete the pods running current version of and then deploy a newer version. This implies there will be a downtime for the users. The time of downtime will depend on shutdown and boot time of the application.

### Requirements

1. Minikube installed. Instructions are found [here.](https://minikube.sigs.k8s.io/docs/start/)
2. kubectl installed. Instructions are found [here.](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Demo Steps

1. Deploy version 1
2. Validate version 1
3. Delete version 1 by deploying version 2
4. Validate version 2
5. Clean up

### Demo Details
```bash
# Start minikube
$ minikube start --memory 2048 --cpus=2

# Deploy version 1 of the application
$ kubectl apply -f app-ver1.yaml

# Test if the deployment was successful
$ curl $(minikube service demo-app --url)
Host: demo-app-698f8b9646-24rv2, Version: v1.0.0

# See the deployment in action. In a different terminal window type the following command
$ watch kubectl get po
# Instructions to install watch on mac can be found at https://osxdaily.com/2010/08/22/install-watch-command-on-os-x/

# Now, deploy version 2 of the application
$ kubectl apply -f app-ver2.yaml

# Test the second deployment progress
$ service=$(minikube service demo-app --url); while sleep 1; do curl "$service"; done
```

### Clean up
```
# Delete the app
$ kubectl delete all -l app=demo-app

# Delete minikube cluster
$ minikube delete
```
