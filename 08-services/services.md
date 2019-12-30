# Services

- Different type of service.
- Each can be exposed internally or externally to the cluster.
- `kube-proxy` agent watches the k8s API for new services and endpoints being created on each node.

## Pattern

- Labels are used to determiend which pods should receive the traffic.
- Create a deployment with label v2, then update the service pointing to this new label.


```
$ k expose deployment/nginx --port=80 --type=NodePort
$ k get svc nginx -o yaml
```

## Service Types

- **ClusterIP** internal use, only available, from inside the cluster. IP Range is defined via an API server startup option.
- **NodePort**  opens a port on each node from range 30000-32767 (defined in the cluster configuration). Uses the static IP address of the node.
- **LoadBalancer** cloud-controler-manager creates a  LB on the cloud provider.
- **ExternalName** it has no selectors, nor does it define ports or endpoints. 


`kubectl proxy` creates a local service to access ClusterIP.

## Diagram

- `kube-proxy` monitors the API service for changes in Service/Endpoints.
- use `Readiness Probe` to ensure all containers are functional prior to connection.
- `iptables mode` allows up to 5000 nodes. (since version v1.2)
- `ipvs mode` still in beta, is available in ipvs.
- `kube-proxy` configured via flag  during initialization, such as **mode=iptables** (could be IPVS or userspace)

