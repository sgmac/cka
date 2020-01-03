# Custom Resources

- In order to add a CRD to the cluster neeeds to be a controller or operator.
- Aggregated APIs (AA) which requires a new API server to be written and added to the cluster.
- If you are using RBAC you will need to greant access to the new CRD resource and controller.

## CRDs

- watcher loops or controllers interrogating the `kube-apiserver`.
- a CRD will be added to the cluster API path, under `apiextensions.k8s.io/v1beta1`.
- only existing API functionalily can be used.
- a resource can be namespaced or cluster.

```yaml
apiVersion: apiextensions.k8s.io/v1beta1
# inserted in the kube-apiserver
kind: Custo-mResourceDefinition
metadata:
  # the name must match the spec field <plural name>.<group>.
  name: backups.stable.linux.com
spec:
  # /apis/<group>/<version> or /apis/stable/v1 
  group: stable.linux.com
  version: v1
  # cluster wide or namespace
  scope: Namespaced
  names:
     # defines the last part of the API URL, such as apis/stable/v1/backups.
    plural: backups
    singular: backup
    # make CLI usage easier
    shortNames:
    - bks
    # A CamelCased singular type used in resource manifests.
    kind: BackUp
```

Then we can define a new object that will be evaluated by the controller.

```yaml
apiVersion: "stable.linux.com/v1"
kind: BackUp
metadata:
  name: a-backup-object
spec:
  timeSpec: "* * * * */5"
  image: linux-backup-image
replicas: 
```

## Optional Hooks

- Finalizer (pre-delete hook), If an API delete request is received, the object metadata field metadata.deletionTimestamp is updated. The controller then triggers whichever finalizer has been configured. When the finalizer completes, it is removed from the list. 

```yaml
metadata:
  finalizers:
  - finalizer.stable.linux.com
```
## Aggregated APIs

- Allows you add kubernetes-type API servers to the cluster
- The aggregation layer is easy to enable. Edit the flags passed during startup of the kube-apiserver to include --enable-aggregator-routing=true.