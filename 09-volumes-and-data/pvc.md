# Persistent Volumes and Claims

The backend storage is `StorageClass`

```
$ k get pv
$ k get pvc
```

## Persistent Volume

Volume using hostPath type. Each type has its own configuration.

**IMPORTANT** _persistent volumes_ are not namespaces object, whereas _persistent volumes claims_ are.

```yaml

kind: PersistentVolume
apiVersion: v1
metadata:
    name: 10Gpv01
    labels: 
        type: local 
spec:
    capacity: 
        storage: 10Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: "/somepath/data01"
```

## Persistent Volume Claim

First the PV is created, now you can claim the volume from a Pod.

```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
    name: myclaim
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
                storage: 8GI
```

And in the Pod

```yaml
spec:
    containers:
....
    volumes:
        - name: test-volume
          persistentVolumeClaim:
                claimName: myclaim
```