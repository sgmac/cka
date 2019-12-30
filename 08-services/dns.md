# DNS

- CoreDNS by default as of v1.13
- For troubleshooting create a pod and verify resolution is working properly.

```
apiVersion: v1
kind: Pod
metadata:
    name: busybox
    namespace: default
spec:
    containers:
    - image: busybox
      name: busy
      command:
        - sleep
        - "3600"
$kubectl exec -ti busybox -- nslookup nginx
```

If there are still errors check logs for `coredns` pods  in the `kube-system` namespace.

