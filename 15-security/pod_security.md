# Security contexts

Eforces/limits what processes inside containers can do 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  securityContext:
    runAsNonRoot: true
  containers:
  - image: nginx
    name: nginx
```

But if the image was built to run  as root, it will fail:

```
$ k describe pod nginx
  Warning  Failed     2s (x4 over 33s)  kubelet, kube-node1  Error: container has runAsNonRoot and image will run as root
```

# Pod security Policies

These policies are cluster-level rules that govern what a pod can do, what they can access, what user they run as, etc.

For Pod Security Policies to be enabled, you need to configure the admission controller of the controller-manager to contain PodSecurityPolicy. 



```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted
spec:
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: MustRunAsNonRoot
  fsGroup:
    rule: RunAsAny
```

# Network security policies

- By default, all pods can reach each other; all ingress and egress traffic is allowed

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-egress-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
  - namespaceSelector:
      matchLabels:
        project: myproject
  - podSelector:
      matchLabels:
        role: frontend
  ports:
  - protocol: TCP
    port: 6379
egress:
- to:
  - ipBlock:
      cidr: 10.0.0.0/24
  ports:
  - protocol: TCP
    port: 5978
```

The egress rules have the `to` settings, in this case the 10.0.0.0/24 range TCP traffic to port 5978.

## Default policy

The empty braces will match all Pods not selected by other NetworkPolicy and will not allow ingress traffic. Egress traffic would be unaffected by this policy.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

Some network plugins, such as WeaveNet, may require annotation of the Namespace. The following shows the setting of a DefaultDeny for the myns namespace:

```yaml
kind: Namespace
apiVersion: v1
metadata:
  name: myns
  annotations:
    net.beta.kubernetes.io/network-policy: |
     {
        "ingress": {
          "isolation": "DefaultDeny"
        }
     }
```