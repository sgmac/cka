# v1 API group

There are eight v1 groups (i.e: storage.k8s.io/v1, rbac.authorization.io/v1)

Objects:

- _node_ part of your k8s cluster. You can turn on/off scheduling to a node with `kubectl cordon/uncordon`.
- _service account_ provides an identifier for processes running in a pod to access the API server.
- _resource quota_ if you want to limit a specific namesapce to only run a given number of pods.
- _endpoint_ represent the set of IPs for pods that match a particular service. **If and endpoint is empty, it means there are no matching pods and something is wrong with your service definition.**


## Deploying an Application

- Deployment is a controller which manages the state of ReplicaSets and the pods within.
- ReplicaSet orchestrates individual pod lifecycle and updates. Replication controllers.

## DaemonSets

The controller ensures only a Pod of the same type runs on each node of the cluster. These are often used for:

- logging
- metrics
- security pods

## StatefulSets

StatefulSet considers each Pod as unique, and provides ordering to Pod deployment. They are not deployed in parallel.

## HPA - autoscaling 

**Horizontal Pod Autoscaling**, automatically scale Replication controllers, replicasets or deployments based on target 50% CPU usage by default.

 The usage is checked by the kubelet every 30 seconds, and retrieved by the Metrics Server API call every minute. 


The **cluster autoscaler** adds or removes nodes to the cluster, based on the inability to deploy a Pod or having nodes with low utilization for at least 10 minutes. Decisions are maded on a node every 10 minutes.

**Vertical Pod Autoscaler** still under development, adjust the amount of CPU and memory requested by Pods.

## Jobs

Part of the **batch** API group. They are used to run a set number of pods to completion, if a pod fails, it will be restarted until the number of completion is reached. They can be used to run on-off pods.

**CronJobs** work in a similar manner to Linux jobs, with the same syntax. 

`spec.concurrencyPolicy` how to handle existing jobs. If set to

- Allow (default) another concurrent job will be run.
- Forbid, current job continues and the new job is skipped.
- Cancel, a value of replace cancels the current job and starts a new job in its place.

## RBAC

**rbac.authorization.k8s.io** has 4 resources: ClusterRole, Role, ClusterRoleBinding, and Rolebinding.




