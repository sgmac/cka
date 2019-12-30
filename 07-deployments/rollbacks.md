# Commands && rollbacks

```
$ kubectl scale deploy/nginx --replicas=4
$ kubectl edit deployment nginx  # change nginx version, triggers rolling update
```


```
$ kubectl create deploy ghost --image=ghost --record

####
# change version of ghost
####

$ kubectl set image deployment/ghost ghost=ghost:09 --all
$ kubectl rollout history deployment/ghost deployments "ghost"
$ kubectl rollout undo deployment/ghost ; kubectl get pods

## roll back to a specific version with --to-revision=2
$ kubectl rollout pause deployment/ghost
kubectl rollout resume deployment/ghost



