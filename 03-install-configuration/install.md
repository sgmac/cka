# Kubernetes install

**master-node** 4Gb
**worker-node** 2Gb


## Manual install 


##  Nodes

```
# apt update && apt-get upgrade -y
# apt install docker.io vim -y

# cat <<EOF>> /etc/apt/sources.list.d/kubernetes.list
deb  http://apt.kubernetes.io/  kubernetes-xenial  main
EOF

# curl -s \
      https://packages.cloud.google.com/apt/doc/apt-key.gpg \
      | apt-key add -

# apt update

#  apt-get install -y \
           kubeadm=1.16.1-00 kubelet=1.16.1-00 kubectl=1.16.1-00

# apt-mark hold kubelet kubeadm kubectl
```



## Calico network

```
wget https://tinyurl.com/yb4xturm -O rbac-kdd.yaml
wget https://tinyurl.com/y2vqsobb -O calico.yaml
```

**calico.yaml** _CALICO_IPV4POOL_CIDR_ must match the value given to `kubeadm init`


## Add master to /etc/hosts


```
IP_KUBE_MASTER  kube-master
```

## Create kubeadm-config.yaml

```yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
kubernetesVersion: 1.16.1 #<-- Use the word stable for newest version
controlPlaneEndpoint: "kube-master:6443" #<-- Use the node alias not the IP
networking:
  podSubnet: 192.168.0.0/16 # same as CALICO_IPV4POOL_CIDR
```

# Adding a node

# Taints

By default the master node is not used to schedule pods.

```
$ kubectl describe node kube-mater
k describe node kube-master
Name:               kube-master
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=kube-master
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    projectcalico.org/IPv4Address: 104.238.176.110/23
                    projectcalico.org/IPv4IPIPTunnelAddr: 192.168.221.192
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Fri, 27 Dec 2019 10:16:16 +0100
Taints:             <none>
Unschedulable:      false
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Fri, 27 Dec 2019 10:59:26 +0100   Fri, 27 Dec 2019 10:59:26 +0100   CalicoIsUp                   Calico is running on this node
  MemoryPressure       False   Fri, 27 Dec 2019 14:59:57 +0100   Fri, 27 Dec 2019 10:16:09 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Fri, 27 Dec 2019 14:59:57 +0100   Fri, 27 Dec 2019 10:16:09 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Fri, 27 Dec 2019 14:59:57 +0100   Fri, 27 Dec 2019 10:16:09 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Fri, 27 Dec 2019 14:59:57 +0100   Fri, 27 Dec 2019 10:24:26 +0100   KubeletReady                 kubelet is posting ready status. AppArmor enabled
Addresses:
  InternalIP:  104.238.176.110
  Hostname:    kube-master
Capacity:
 cpu:                2
 ephemeral-storage:  82533764Ki
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             4039484Ki
 pods:               110
Allocatable:
 cpu:                2
 ephemeral-storage:  76063116777
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             3937084Ki
 pods:               110
System Info:
 Machine ID:                 3983df4fe3654649807804cdc61dcb08
 System UUID:                3983DF4F-E365-4649-8078-04CDC61DCB08
 Boot ID:                    cbb08e90-d214-4ae2-9478-b58ae936673e
 Kernel Version:             4.15.0-66-generic
 OS Image:                   Ubuntu 18.04.3 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://18.9.7
 Kubelet Version:            v1.16.1
 Kube-Proxy Version:         v1.16.1
PodCIDR:                     192.168.0.0/24
Non-terminated Pods:         (8 in total)
  Namespace                  Name                                   CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                   ------------  ----------  ---------------  -------------  ---
  kube-system                calico-node-5sgbg                      250m (12%)    0 (0%)      0 (0%)           0 (0%)         4h1m
  kube-system                coredns-5644d7b6d9-bgs44               100m (5%)     0 (0%)      70Mi (1%)        170Mi (4%)     4h44m
  kube-system                coredns-5644d7b6d9-plt76               100m (5%)     0 (0%)      70Mi (1%)        170Mi (4%)     4h44m
  kube-system                etcd-kube-master                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         4h43m
  kube-system                kube-apiserver-kube-master             250m (12%)    0 (0%)      0 (0%)           0 (0%)         4h43m
  kube-system                kube-controller-manager-kube-master    200m (10%)    0 (0%)      0 (0%)           0 (0%)         4h43m
  kube-system                kube-proxy-npb4f                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         4h44m
  kube-system                kube-scheduler-kube-master             100m (5%)     0 (0%)      0 (0%)           0 (0%)         4h43m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                1 (50%)     0 (0%)
  memory             140Mi (3%)  340Mi (8%)
  ephemeral-storage  0 (0%)      0 (0%)
Events:              <none>
```

In order to allow scheduling, we have to remove the `taint`.

```
$ kubectl taint nodes --all "node-role.kubernetes.io/master-"
```

If we want to taint back:


```
$ k taint nodes kube-master node-role.kubernetes.io=master:NoSchedule
```




