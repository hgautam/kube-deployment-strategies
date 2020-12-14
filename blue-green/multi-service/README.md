## Multi-service blue-green deployment
In blue-green deployment strategy, version 2 (green) is deployed alongside version 1 (blue) with exactly the same amount of instances. After testing that version 2 meets all the requirements, the traffic is switched from version 1 to version 2.

### Requirements
1. Minikube installed. Instructions are found [here.](https://minikube.sigs.k8s.io/docs/start/)
2. kubectl installed. Instructions are found [here.](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
3. Helm 3 installed. Instructions can be found [here](https://helm.sh/docs/helm/helm_install/)

### Demo Steps
1. version 1 is serving the traffic
2. deploy version 2 on separate set of pods
3. wait until version 2 is ready
4. switch incoming traffic from version 1 to version 2
5. delete version 1

### Demo Details
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

# To see the deployment in action, open a new terminal and run the following command
$ watch kubectl get po

# Deploy version 2 of both applications
$ kubectl apply -f app-a-ver2.yaml -f app-b-ver2.yaml

# Validate the deployment
$ kubectl rollout status deploy go-app-a-v2 -w
deployment "go-app-a-v2" successfully rolled out

$ kubectl rollout status deploy go-app-b-v2 -w
deployment "go-app-b-v2" successfully rolled out

# Lets manually test one of the newer pods by port-forwarding it to our local cluster
# Select one of the newly created pods and run the following command on a terminal window:
$ kubectl port-forward pod/go-app-a-v2-7b5d95b4b4-crknl 8081:8080

# On a separate terminal window, type the following command:
$ curl http://localhost:8081
Host: go-app-a-v2-7b5d95b4b4-crknl, Version: v2.0.0

# Once all the new pods are ready, you can update the ingress
$ kubectl apply -f ingress-ver2.yaml

# Test if the deployment was successful
$ curl $ingress -H 'Host: a.domain.com'
Host: go-app-a-v2-7b5d95b4b4-crknl, Version: v2.0.0

$ curl $ingress -H 'Host: b.domain.com'
Host: go-app-b-v2-5c6759b9d4-mrgp7, Version: v2.0.0

# In case of a rollback to the previous version, run the following command:
$ kubectl apply -f ingress-v1.yaml

# On successful upgrade, delete the v1.0.0 deployment
$ kubectl delete -f app-a-ver1.yaml -f app-b-ver1.yaml
```

### Delete the resources
```
$ kubectl delete all -l app=go-app
$ helm del traefik
```