# DaemonSets and labels

This controller ensures that a single pod exists on each node in the cluster.

# Labels

```
$ k get pods -l run=ghost
$ k get pods -L run

$ k label pods ghost-399394949-qek foo=bar
```

You could use (at containers level definition) the schedule of a Pod on a specific node. 

- first, add labels to your nodes. 
- then use `nodeSelector`

```yaml
spec:
    containers:
    - image: nginx
    nodeSelector:
        disktype: ssd
```
