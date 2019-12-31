# Dynamic provisioning

A cluster administartor still needs to create those volumes in the first place. In v1.4 Dynamyc provisiioning allowed for the cluster to request storage from an exterior, pre-configured source.

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast      # Could be any name
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd
```


## Rook for storage  orchestartion

Rook allows ochestration of storage using multiple storage providers.

Rooks uses a CRD and a custom operator.

- Cepth
- Cassandra
- CockroachDB
- EdgeFS
- Minio Objec Store
- NFS
