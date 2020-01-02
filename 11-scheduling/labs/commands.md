# commands

```
$ k get nodes
$ k label node kube-node1 status=vip
$ k label node kube-node2 status=other
$ k get nodes --show-labels
$ k drain node kube-node4
node/kube-node4 cordoned
evicting pod "taint-deployment-7cf4d757c8-h5s6h"
evicting pod "calico-kube-controllers-6bbf58546b-76xpj"
evicting pod "taint-deployment-7cf4d757c8-x5zv6"
evicting pod "taint-deployment-7cf4d757c8-b9z7z"
pod/taint-deployment-7cf4d757c8-b9z7z evicted
pod/taint-deployment-7cf4d757c8-x5zv6 evicted
pod/calico-kube-controllers-6bbf58546b-76xpj evicted
pod/taint-deployment-7cf4d757c8-h5s6h evicted
$ k get nodes | grep node4
kube-node4    Ready,SchedulingDisabled   <none>   5d19h   v1.16.1
$ k uncordon kube-node4
```


