# nginx
- Deploy the nginx controller form [nginx ingress](https://github.com/kubernetes/ingress-nginx/tree/master/deploy)
- Customization via _ConfigMaps, Annoations_
  * Easy integration with RBAC (meaning?)
  * Uses the annoation **kubernetes.io/ingress.class: "nginx"**
  * L7 traffic requires `proxy-real-ip-cidr`
  * Bypasses kube-proxy to allow session affinity
  * Does not use **contrack** entries for iptables DNAT
  * TLS requires the host field to be defined

