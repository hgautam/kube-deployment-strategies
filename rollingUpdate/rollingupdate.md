## Rolling Update Deployment Strategy

![Rolling update](../images/rollingUpdate.png)

A slow rollout or zero downtime rollout. You start replacing existing pods until you have no pods to replace. This approach can slow down deployments and can be effective if you want to do slower deployment.

Main advantages of this approach are:
* Rollout can be paused and resumed
* Rollbacks are fairly fast
* Rollouts are slow and controlled

Main disadvantages are:
* Rollouts are slower
* No control over traffic
* Problematic if application APIs are not backward compatible