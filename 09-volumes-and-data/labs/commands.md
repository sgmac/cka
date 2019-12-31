# commands

```
$ k create configmap colors --from-literal=text=black --from-file=./favorite --from-file=./primary
```

## NFS
- **nfs-kernel-server** must be installed in the master.
	- `/opt/sfw/ *(rw,sync,no_root_squash,subtree_check)` /etc/exporfs
- **nfs-common** all the worker nodes need to install this.
```
$ Volumes:
  nfs-vol:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  pvc-one
    ReadOnly:   false
```

## Quotas

```
$ mor pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-one
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 200Mi


$ k describe ns small
Name:         small
Labels:       <none>
Annotations:  <none>
Status:       Active

Resource Quotas
 Name:                   storagequota
 Resource                Used  Hard
 --------                ---   ---
 persistentvolumeclaims  0     10
 requests.storage        0     100Mi

Resource Limits
 Type       Resource  Min  Max  Default Request  Default Limit  Max Limit/Request Ratio
 ----       --------  ---  ---  ---------------  -------------  -----------------------
 Container  memory    -    -    100Mi            500Mi          -
 Container  cpu       -    -    500m             500m           -

$ k create -n small -f pvc.yaml
Error from server (Forbidden): error when creating "pvc.yaml": persistentvolumeclaims "pvc-one" is forbidden: exceeded quota: storagequota, requested: requests.storage=200Mi, used: requests.storage=0, limited: requests.storage=100Mi
```

## NFS

By default the PersistentVolumeReclaimPolicy is set to `Retain`, **NFS does not suppport delete**.
