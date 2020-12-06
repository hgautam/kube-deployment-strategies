## Rolling Update Deployment Strategy

![Rolling update](../images/rollingUpdate.png)

A slow rollout or zero downtime rollout. The RollingUpdate strategy is the generally preferable strategy for any user-facing service. You start replacing existing pods until you have no pods to replace.

Main advantages of this approach are:
* Rollout can be paused and resumed
* Rollbacks are fairly fast
* Rollouts are slow and controlled

Main disadvantages are:
* Rollouts are slower
* No control over traffic
* Problematic if older and newer version of apps cannot function together

Anatomy: A rolling update uses readiness probes to check for the availability of new pods probe before it starts scaling down the old ones. If there is a problem, the rolling update or deployment can be aborted without bringing the whole cluster down. In this sense, it is a controlled way to release new features. 