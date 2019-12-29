# Rest APIS

`kubectl` makes API calls on your behalf, corresponding with verbs `GET,POST, DELETE`

```
curl --cert sgm.pem --key sgm.pem \  
--cacert /path/to/ca.pem \   
https://kube-master:6443/api/v1/pods
```
- Impersonating other users/groups helps to debug authorization policies of other users.

## Checking access

```
$ k auth can-i create deployments  --as sgm
yes
$ k auth can-i create namespaces  --as sgm
no
```

- `SelfSubjectAccessReview` (review for any user, helpful for delegating others)
- `LocalSubjectAccessReview` (stricted to a specific namespace)
- `SelfSubjectRulesReview` (allowed actions for given user within a namespace)

- `resourceVersion`  409 CONFLICT

## Annotations

- **Labels** work with objects or collections of objetcs.
- **Annotations** metadata to include with an object that may be helpful outside of the Kubernetes object interaction.

```
$ kubectl annotate --overwrite pods description="Old Production Pods" -n prod
```

## Simple Pod 

- The `apiVersion` must match the existing API group.
```
apiVersion: v1
kind: Pod
metadata:
  name: firspod
spec:
  containers:
  - image: nginx
    name: nginx
```

As we can see, Kubernetes exposes resources via RESTful API calls. The state of the resources can be changed using standard HTTP verbs (GET, POST, PATCH, DELETE).

```
$ k --v=10 delete pod maint-5bcc549596-dv79j

I1228 10:34:29.578343   51194 round_trippers.go:438] DELETE https://kube-master:6443/api/v1/namespaces/default/pods/maint-5bcc549596-dv79j 200 OK in 173 milliseconds
```

## Access outside the cluster

```
$ k config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://kube-master:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
```

## Kube config

The `kube config` has three main sections:

- _clusters_  where to send the API requests, `cetificate-authority-data` is passed to authenticate the curl request.
- _context_  helps to manage access to multiple clusters, you can define `cluster, user and namespace`.
- _users_ nickname associated with the client, auth can be Client Key/Certificate, username and password and a token. The last two, user/pass and token are mutually exclusive.

## Namespaces

- Every api call includes a namespace, using `default` if not otherwise included. `https://10.128.0.3:6443/api/v1/namespaces/default/pods.`

The are 4 namespaces when a cluster is first created:

- `default`  all resources assigned to this namespace if not otherwise explicitly declared.
- `kube-node-lease` worker node lease information is kept.
- `kube-public` a namespace readable by all, general information included here.
- `kube-system` infrastructure pods.

### Working with namespaces

```
$ k create namespace linuxcon
$ k describe ns linuxcon
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: "2019-12-28T09:56:54Z"
  name: linuxcon
  resourceVersion: "152036"
  selfLink: /api/v1/namespaces/linuxcon
  uid: 9852f648-6ba8-4fb5-bf6a-af9b99b7bca2
spec:
  finalizers:
  - kubernetes
status:
  phase: Active
$ k delete ns/linuxcon
```

## Additional Resource Methods

Below example would be the same as running `$ k logs firstpod` 

```
curl --cert /tmp/client.pem --key /tmp/client-key.pem \
--cacert /tmp/ca.pem -v -XGET \ 
 https://10.128.0.3:6443/api/v1/namespaces/default/pods/firstpod/log
```

Other API calls:

```
GET /api/v1/namespaces/{namespace}/pods/{name}/exec
GET /api/v1/namespaces/{namespace}/pods/{name}/log
GET /api/v1/watch/namespaces/{namespace}/pods/{name}
```

## API maturity

- **alpha** can be buggy, features can change or dissapear at any time. Test in cluster that are often rebuild.
- **beta**  you can expect some bugs and issues. It has better tested code. backwards compatibility between versions.
- **stable** denoted by only an integer which may be preceded by the letter v, is for stable APIs.

