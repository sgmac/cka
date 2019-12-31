# Google Load Balancer controller (GLBC)
-  Quotas are not evaluated prior to creation
- GLBC controller must be created and started first
- TLS ingress only supports 443 and assumes TLS termination
- TLS secret must contain keys named `tls.crt`
