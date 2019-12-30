# Commands
```
$ k delete rs rs-one --cascade=false
```

You can set the `updateStrategy` to `type: OnDelete`, only when you delete a pod, a new changes to the manifest
will be applied.


```
$ k rollout history ds ds-one --revision=2
daemonset.apps/ds-one with revision #2
Pod Template:
  Labels:	system=DaemonSetOne
  Containers:
   nginx:
    Image:	nginx:1.12.1-alpine
    Port:	80/TCP
    Host Port:	0/TCP
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>

$ k rollout undo ds ds-one --to-revision=1
```

If we `rollback` to a previous revision, it will not take effect until we delete that Pod.
