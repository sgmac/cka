# Limit resources per namespace

Any new Pod (statefultset, daemonset, deployment) in the namespace will get this constraints by default
```
apiVersion: v1
kind: LimitRange
metadata:
  name: low-resource-range
spec:
  limits:
  - default:
      cpu: 100m
      memory: 500Mi
    defaultRequest:
      cpu: 100m
      memory: 500Mi
    type: Container
```

*Question*: If I modify a deployment to have certain resource limits, what takes preference?.


# Basic node maintenance 

```bash
$ k drain <NODE-NAME> --ignore-daemonsets --delete-local-data
$ k describe node | grep -i taint
$ k uncorden <NODE-NAME>
$ k describe node | grep -i taint
```
