# Overview


## Monitoring

- if a shell is not available in the affected Pod, create a Pod with busybox
- use tools such as tcpdump or dig depending on the issue
- **metrics server** exposes a standard API, _/apis/metrics/k8s.io/_.

## Logging
- fluentd can be used as data collector.

# Troubleshooting

- Use a shell to troubleshooting DNS problemes.

```
$ kubectl create deploy busybox --image=busybox --command sleep 3600
$ k exec -it busybox -- /bin/sh
$ k logs pod-name
```

## Ephemeral containers

- In v1.16 you can attach containers to a pod
- They are handled via API call not via podSpec

```
$ k debug buggypod --image debian --attach
```

## Cluster start squence

```
$ systemctl status kubelet.service
```

If you installled the node with `kubeadm` uses _/etc/systemd/system/kubelet.service.d/10-kubeadm.conf_

Inside of the config.yaml file you will find several settings for the binary, including the staticPodPath which indicates the directory where kubelet will read every yaml file and start every pod. If you put a yaml file in this directory, it is a way to troubleshoot the scheduler, as the pod is created with any requests to the scheduler.

Uses 
- /var/lib/kubelet/config.yaml configuration file
- **staticPodPath** is set to /etc/kubernetes/manifests/

## Labs

More reading: https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/ and 
https://kubernetes.io/docs/tasks/debug-application-cluster/determine-reason-pod-failure

