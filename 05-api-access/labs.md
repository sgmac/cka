# API calls using curl

- Basically get the information from the `kube config`, CA, cert and key.

```
$ curl --cert ./client.pem --key ./client-key.pem --cacert ./ca.pem https://kube-master:6443/api/v1/namespaces/default/pods/maint-5bcc549596-5s62j
```
