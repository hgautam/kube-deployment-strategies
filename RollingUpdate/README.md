## Rolling Update Deployment Strategy demo on Miniukbe

Performs zero downtime upgrade by incrementally upgrading application pods.

### Requirements

1. Minikube installed. Instructions are found [here.](https://minikube.sigs.k8s.io/docs/start/)
2. kubectl installed. Instructions are found [here.](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Execution steps

1. Deploy version 1
2. Validate version 1
3. Start rolling update
4. Pause the rollout
6. Resume the rollout
7. Validate version 2