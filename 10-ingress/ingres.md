# Ingress

- Same function as services, but to expose an application outside the cluster.
- Route traffic based on the request host or path.
- It's a different controller, _does not run as part of the kube-controller-manager_.
- Uses rules to handle traffic.
- Two supported controllers, GCE and nginx.
- Create a `ClusterIP` service, use a rule from the ingress to target that internal service.
- Collection of rules that allow inbound connections to reach the cluster services.

## Ingress controller

- It's a daemon running in a Pod which watches the **/ingress** endpoint on the API server. 
- Multiple ingress controllers can be deployed. Traffic should use annotations to select the proper controller, the lack of it, would cause every controller to attempt to satisfy the ingress traffic.

## Ingress API

Example

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: wordpress-ingress
  namespace: default
  #annotations:
  #   nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: nvme0.net
    http:
      paths:
      - backend:
          serviceName: wordpress
          servicePort: 80
        path: /
  - host: nginx.nvme0.net
    http:
      paths:
      - backend:
          serviceName: hello-app
          servicePort: 80
        path: /
  tls:
  - hosts:
    - nvme0.net
    secretName: nginx-secret
```

But there are some API deprecations:


Ingress (in the extensions/v1beta1 API group)
Migrate to use the networking.k8s.io/v1beta1 API, serving Ingress since v1.14. Existing persisted data can be retrieved/updated via the networking.k8s.io/v1beta1 API.

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
    name: ghost
spec:
    rules:
    - host: ghost.customdomain.com
      http:
        paths:
        - path: /
          backend:
            serviceName: ghost
            servicePort: 2368
```
