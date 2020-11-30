## Rolling Update Deployment Strategy demo on Miniukbe

Performs zero downtime upgrade by incrementally upgrading application pods.

### Requirements

1. Minikube installed. Instructions are found [here.](https://minikube.sigs.k8s.io/docs/start/)
2. kubectl installed. Instructions are found [here.](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Steps to follow

1. Deploy version 1
2. Validate version 1
3. Start rolling update
4. Rollback the deployment
5. Start a new deployment
6. Pause the deployment
7. Resume the deployment
8. Validate version 2

### Execution steps

1. Create a minikube cluster
```
$ minikube start --memory 2048 --cpus=2
```
2. Deploy version 1 of the application
```
$ kubectl apply -f app-ver1.yaml
```
3. Validate the deployment

$ curl $(minikube service go-app --url)

Host: go-app-d89d6dc79-grdlg, Version: v1.0.0

4. To see the deployment in action, open a new terminal and run the following command:

$ watch kubectl get po

5. Deploy version 2 of the application

$ kubectl apply -f app-ver2.yaml

6. Validate the new deployment

$ service=$(minikube service go-app --url)
$ while sleep 1; do curl "$service"; done

6. Rollback this deployment

$ kubectl rollout undo deploy go-app

7. Validate the rollback

$ while sleep 1; do curl "$service"; done