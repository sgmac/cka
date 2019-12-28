# Main components

- different roles: master/worker  nodes.
- controllers.
- services
- pods (group of container)
- namespaces/quotas
- network policies
- storage


## Master node

The kubernetes master node, runs:

- api-server
- kube-scheduler
- controllers (cloud-controller-manager)
- etcd (database)


### api-server

All connections, both external and internally are handle via the `api-server`. This is the only service that talks to `etcd.` Acts as a frontend for the whole cluster.

### kube-scheduler

Determines in what node a Pod is going to be scheduled, based on available resources.

### etcd

The state of the cluster, networking and other persistent information is kept in `etcd`.

### kube-controller-manager

It's a control loop daemon that interacts with the kube-aposerver to determine the state of the cluster. If the current state does not  match the desired state, will contact the appropiate controller to match the desired state. When using  
`cloud-controller-manager`, the `kubelet` needs to be started with `--cloud-provider-external`.


## Worker nodes

All nodes run

- **kubelet**
- **kube-proxy** manages network connectivity to the containers. (iptables/ipvs). It also has an userspace mode that monitors Services and Enpoints.


## Kubelet

Below are the main tasks 

- accepts API calls for Pod specifications (PodSpec, JSON/YAML describing a pod)
- access to storage, confimaps, secrets. It also sends back status to the api-server for eventual persistence.
- passes request to the local container engine (docker, rkt, cri-o)

## Services

- Type: ClusterIP, NodePort, LoadBalancer
- Handles policies for inbound requests.
- Connect Pods together
- Expose Pods to Internet


## Pods

Containers in a Pod are started in parallel. The use of `InitContainers` can order startup. Containers inside a PoD share the same network namespace and data volumes.

## Containers

In the `PodSpec` 

```yaml
resources:
  limits:
    cpu: "100m"
    memory "250Mi"
  requests: 
    cpu: "50m" 
    memory: "500Mi"
```

- **ResourceQuota** allows to define limits per `namespace`.
- A beta feature in `v1.12` use the `scopeSelector` to define priorities to Pods if they have set the `priorityClassName`.

### Init containers

Containers that must complete before the app container starts


## Component review

- Only `kube-apiserver` talks to `etcd`
- There are tools such as `etcdctl` or `calicoctl`. Calico use `bird` to communicate between `Calico Felix` in both, worker and master nodes.

## Node

- A `node` is an API object created outside the cluster representing an instance. Master must run on Linux, but worker can be Microsoft Windos Server 2019.

- If the `kube-apiserver` can't not communicate with the `kubelete` for 5 minutes, the default `NodeLease` will schedule the node for deletion and the NodeStatus will change from _ready_. You can create a kube master with `kubeadm init` and worker nodes with `kubeadm join` later.


**IMPORTANT: Removing a node form the cluster**

- first use `kubectl delete node <node-name>`
- then `kubeadm reset` to remove specific information.

## Networking setup

Main networking challenges to solve:

- coupled container-to-container (solved with pod concept)
- pod-to-pod communication.
- external-to-pod communication (solved by services)

## CNI Network Configuration

To provide container networking :

- CNI (Container Network Interface)
- Configuration file

## Pod-to-Pod communication

Basically all IPs involdes (nodes and pods) are routable without NAT. This can be achieved with

- Weave
- Flannel
- Calico
- Romana
