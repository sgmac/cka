# Deployments

- Creates a ReplicaSet (creates a )-> Pod.
- Updates to the deployment creates a new replicaset.


```
$ k create deployment test --image=nginx:1.13-7-alpine
```

- controllers or watch loops, running as trheads of the `kube-controller-manager`.
- each controller queries the `api-server` for current state of the object they track.
- the state of each object on a worker node is sent back from the local kubelet.


## Kubelet

- checks the pod specification asking the container engine (docker/cri-o) for the current status.
- kubelet then compares the specification with what the container engine replies.


## ReplicaSet

- manage the creation or termination of pods, always matching the specification.
- new versions of pods, first create a new replica set with pods, when they are ready, start shuting down pods from the old replicaset.
