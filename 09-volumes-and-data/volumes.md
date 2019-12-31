# Volumes
- A kubernetes volume shares the Pod lifetime, no the container within.
- If you want persistance, use Persistent Volumes.

## Introduction

Volumes require:

- name
- type
- mountpoint

This volume can be available to multiple containers inside a Pod. There is no concurrency checking.

**Access mode**

- RWO (ReadWriteOnce) read-write by a single node.
- ROX (ReadOnlyMany) read-only by multiple nodes.
- RWX (ReadWriteMany) read-write many nodes.

## Volume spec

**emptyDir** the kubelet will create the directory in the container, but not mount any storage. Any data created is written to the shared container space. Pod deleted, data deleted.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: busybox
    namespace: default
spec:
    containers:
    - image: busybox
      name: busy
      command:
        - sleep
        - "3600"
      volumeMounts:
      - mountPath: /scratch
        name: scratch-volume
    volumes:
    - name: scratch-volume
            emptyDir: {}
```

## Volumes types

- **GCE/AWS** you can use GCEpersistentDisk/awsElasticBlockStore.
- **emptyDir** delete when the Pod dies.
- **hostPath** mounts a resource from the host node filesystem. The resource could be a 
  - directory
  - file socket, character or block device
- **NFS, CephFS, GlusterFS**

## Shared Volumes

Example of a Pod  with 2 containers sharing a volume

```yaml
containers:
       - image: busybox
     volumeMounts:
       - mountPath: /busy
     name: test
     name: busy
       - image: busybox
     volumeMounts:
       - mountPath: /box
     name: test
     name: box
     volumes:
       - name: test
       emptyDir: {}
```
