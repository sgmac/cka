# Secrets

It's a base64-encoded by default. You must create an EncryptionConfiguration with a key and proper identity.

Then the `kube-apiserver` needs the `--encryption-provider-config` flag set to a proviously configured provider, such as **aescbc or ksm**.



```
$ k get secrets
$ k create secret generic mysql --from-literal=password=root
```

## Secrets as ENV variables

- Max size for a secret 1MB

```yaml
spec: 
 
  containers:
  - image: mysql:5.5
    env:
    - name: MYSQL_ROOT_PASSWORD
      valueFrom:
        secretKeyRef:
          name: mysql
          key: password
```

They are store in the **tmpfs** storage on the host node. A secret must exist prior to being requested.

## Mounting secrets as volumes

```yaml
spec:
    containers:
    - image: busybox
      command:
        - sleep
        - "3600"
      volumeMounts:
      - mountPath: /mysqlpassword
        name: mysql
      name: busy
    volumes:
    - name: mysql
        secret:
            secretName: mysql
```