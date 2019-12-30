# Deployment configuration metadata

- **annotations**
- **labels**
- **selfLink** how the `kube-apiserver` will ingest this information to the API


# Configuration Spec

- Two spec declaration:
 * first spec, ReplicaSet
 * second spec, Pod


## Specs

- **progressDeadlineSeconds** time in seconds until a progress error is reported during a change. Reasons could be quotas, images issues, or limit ranges.
- **matchLabels** requirements of the Pod selector.
- **selector** 
- **strategy** how to update pods. _RollingUpdate_ you can control how many pods are deleted at a time.
	- **maxSurge** default 25%, number of  new pods to create  before deleting old ones.
	- **maxUnavilable** percentage of Pods which can be iun a state other than `ready` during the update process.
	- **type** RollingUpdate



