#  TLS

- You can check the kubelet process on both the master and worker nodes. `/etc/kubernetes/manifest`
- Check the numbers of secrets   `kubectl -n kube-system get secrets` 


**IMPORTANT** making a type can remove access to the cluster. **Do a backup of KUBECONFIG** before changing anything.

```
$ kubectl config view
$ kubeadm  config view

# explore this command
$ kubeadm config print init-defaults
$ kubeadm token <TAB>
```
