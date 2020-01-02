# Taints

A node with a particular taint will repel Pods without tolerations for that taint. A taint is expressed as key=value:effect. The key and the value value are created by the administrator.

**NoScheduling** the scheduler will not schedule a Pod on this node, unless the Pod has this toteration.

**PreferNoSchedule** the scheduler will avoid using this node, unless there are no untainted nodes for the Pods toleration. Existing pods are unaffected.

**NoExecute** This taint will cause existing Pods to be evacuated and no future Pods scheduled. If a Pod sets `tolerationSeconds`, they will remain for that many seconds, then evicted. Certain node issues will cause the kubelet to add 300 second to tolerations to avoid unnecessary evictions.


If a node has multiple taints, the scheduler ignores those with matching tolerations. 

# Tolerations

Tolerations are used to scheduled Pods on tainted nodes. Only those Pods with a particular toleration will be scheduled.

```yaml
tolerations:
- key: "server"
  operator: "Equal"
  value: "ap-east"
  effect: "NoExecute"
  tolerationSeconds: 3600
```

The Pod will remain on the **server** with a key of server and value **ap-east** for 3600 seconds, after the node has been tainted with **NoExecute**. When the time runs out, the Pod will be evicted.
