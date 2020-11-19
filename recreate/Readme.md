## Recreate startegy demo on Minikube

Version 1 is deleted and then version 2 is rolled out.

As part of this demo, we will delete the pods running current version of and then deploy a newer version. This implies there will be a downtime for users. The time of downtime will depend on shutdown and boot time of the application.

### Steps to follow

1. Check version 1 is active
2. Delete version 1
3. Deploy version 2
4. Validate version 2

### Requirements

1. Minikube installed. Instructions are found [here.](https://minikube.sigs.k8s.io/docs/start/)
2. kubectl installed. Instructions are found [here.](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
