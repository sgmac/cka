# HA

- new feature of `kubeadm`  is the integrated ability to join multiple master nodes with collocated etcd databases.
- Three instances of `etcd` are required to be able to determine quorum.
- You can collocate the database with the control plane or use and external `etcd` database cluster.


# Collocated DBs

- Easiest way is to join at least 2 more master servers to the cluster, almost as the worker node but providing `--control-plane` and a `certificate-key`.
- The key probably needs to be regenerated if the join  does not happen within the 2hours of the cluster creation.
- Should a node fail, you would lose both a control plane and a database.

# Non-collocated databases

- Using an external cluster of etcd allows for less interruption should a node fail
- `kubeadm-config.yaml` needs to be populated with the `etcd` set to external, endpoints and the certificate location. Once the first control plane is fully initialized, the redundant control planes need to be added one at a time, each fully initialized before the next is added.