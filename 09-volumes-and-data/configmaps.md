# ConfigMaps

- Pod env variables from single or multiple configmaps
- Volume from ConfigMap
- ConfigMaps in Pod commands
- Set file names and access mode in Volume from ConfigMap data
- Can be used by system components and controllers

```yaml
env:
- name: SPECIAL_LEVEL_KEY
  valueFrom:
    configMapKeyRef:
      name: special-config
      key: special.how

---
...
volumes:
    - name: config-volume
      configMap:
        name: special-config
```

