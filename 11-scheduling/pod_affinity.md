# Pod Affinity

- Pods that share data may operate best if co-locate, that's affinity.
- If you want tolerance, you want Pod to be separeted that's anti-affinity.
- use of **In, NotIn, Exists and DoesNotExist**

## Affinity Rules

**requiredDuringSchedulingIgnoredDuringExecution** the Pod will not be scheduled on a node unless following operator is true. I.E: you don't want to schdule Pod A if Pod X is already running. _hard rule_

**preferredDuringSchedulingIgnoredDuringExecution** will choose a Node with the desired setting before those without, but if there is not option, it will run. _soft rule_

**podAffinity** to schedule Pods together.

**podAntiAffinity** keep pods on different nodes.

**topologyKey** allows a general grouping of Pod deployments. 

If using requiredDuringScheduling and the admission controller LimitPodHardAntiAffinityTopology setting, the topologyKey must be set to kubernetes.io/hostname.

f using PreferredDuringScheduling, an empty topologyKey is assumed to be all, or the combination of kubernetes.io/hostname, failure-domain.beta.kubernetes.io/zone and failure-domain.beta.kubernetes.io/region.


## Examples

**podAffinity** Pod can be scheduled on a node running Pod with a key label of security and value of S1. If this is not posible, the Pod will remain in a **Pending** state.

```yaml
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: security
            operator: In
            values:
            - S1
      topologyKey: failure-domain.beta.kubernetes.io/zone
```

**podAntiAffinity** The scheduler will prefer to avoid a node with a key set to security and value of S2. 

```yaml
podAntiAffinity:
  preferredDuringSchedulingIgnoredDuringExecution:
  - weight: 100
    podAffinityTerm:
      labelSelector:
        matchExpressions:
        - key: security
          operator: In
          values:
          - S2
    topologyKey: kubernetes.io/hostname
```

