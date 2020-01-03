# Kubernetes dashboard

```
$ kubectl create -f https://bit.ly/2OFQRMy
$ kubectl create clusterrolebinding dashaccess --clusterrole=cluster-admin  --serviceaccount=kubernetes-dashboard:kubernetes-dashboard
$ kubectl -n kubernetes-dashboard edit svc kubernetes-dashboard # NodePort
$ k -n kubernetes-dashboard get secrets
NAME                               TYPE                                  DATA   AGE
default-token-tm4xn                kubernetes.io/service-account-token   3      24m
kubernetes-dashboard-certs         Opaque                                0      24m
kubernetes-dashboard-csrf          Opaque                                1      24m
kubernetes-dashboard-key-holder    Opaque                                2      24m
kubernetes-dashboard-token-nk8vn   kubernetes.io/service-account-token   3      24m
```	

