# Pod specification

Inside the Pod specfication, we can include:

- **nodeName**  assign to a single node
- **nodeSelector** assigne to a group of nodes (this node must be labeled beforhand) 
- **affinity/anti-affinity** require or prefer which node is used by the scheduler.
- **schedulerName** use a custom scheduler.
- **taints/tolerations** taints allow a node to be labeled such that Pods will not be scheduled for some reason, such as the master node after initialiation. A toleration allows a Pod to ignore the taint and be scheduled assuming other requirements are met.

```yaml
spec:
  containers:
  - name: redis
    image: redis
  nodeSelector:
    net: fast
```
