# kube-scheduler

- determines which node will run a Pod, using a topology-aware algorithm.
- you can set priority to a pod, which allows preemtion of lower priority pods.
- scheduler tracks the set of nodes in your cluster, filters them based on a set of predicates, then uses priority functions to determine on which node each pode should be scheduled.

## Predicates

- the scheduler goes trhough a set of filters or predicates to find available nodes, then ranks each node using priority functions. The node with highest rank is selected to run the Pod.
- the scheduler can be updated by passing a configuration of `kind: Policy`, which can order predicates, give special weights to priorities, even **hardPodAffinitySymmetricWeight**, which deploys Pods such that if ew set Pod A to run with Pod B, then Pod B should automatically be run with Pod A.
