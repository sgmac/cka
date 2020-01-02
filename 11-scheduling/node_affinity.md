# Node affinity rules

**nodeAffinity** allows Pod scheduling Pod based on node labels, whereas **affinity/anti-affinity** has to do with other Pods. 

In this case it's used `nodeSelector`.


```yaml
spec:
  affinity:
    nodeAffinity: 
      requiredDuringSchedulingIgnoredDuringExecution:  
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/colo-tx-name
            operator: In
            values:
            - tx-aus
            - tx-dal
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: disk-speed
            operator: In
            values:
            - fast
            - quick
```

he second rule gives extra weight to nodes with a key of disk-speed with a value of fast or quick.
