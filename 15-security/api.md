# Accessing the API

Any action in a k8s cluster goes through 3 main steps:

- Authentication
- Authorization
- Admission control

The request first reaches the `api-server` it will go through any authentication module that has been configured. This request is encrypted with TLS. Then authorization the user can actually do what he is trying to do. Then admision controllers will check the actual content of the objects being created and validate them before admitting the request. 

# Authentication
It's done with

* X509 certificates, tokens or basic auth
* users are not created by the API
* system accounts are used by processes to access the API

Webhooks can be configured to verify Bearer tokens, and connection with an external OpenID provider

The type of authentication is defined in the `kube-apiserver` startup options.

# Authorization

Three main ways

* ABAC (Attribute Base Access Control)
* RBAC (Role Based Access Control)
* Webhooks (is an HTTP callback, and HTTP POST  that occurs  when something happens )

This is also configured with startup options on the  `kube-apiserver`.

# Admission controller

LimitRanger, ResourceQuota are examples of admission controllers. These are controllers that can access the content of the objects being created by the requests. They can modify the content or validate it, and potentially deny the request.

`--enable-admission-plugins=Initializers,NamespaceLifecycle,LimitRanger`

