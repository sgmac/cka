# Helm

You can see it as a package manager, packages are called (charts)

## Helm v2 and Tiller

Helm v2 is made of two components.

- A server called Tiller, runs inside the Kubernetes cluster.
- Client `helm`

Helm v2 deploys tiller on a Pod

## Helm v3

- You must now provide a name with `--generated-name`.
- Tiller Pod gone.


## Chart Contents

- the **Chart.yaml** contains some metadata about the chart. It's like name, version, keywords and  so on.
- the **values.yaml** keys and values that are used to generate the release in your cluster. The values are replace in the resource manifests using the Go templating syntax.
- the **templates** contains resource manifest to make the application live.


```yaml
├── Chart.yaml
├── README.md
├── templates
│   ├── NOTES.txt
│   ├── _helpers.tpl
│   ├── configmap.yaml
│   ├── deployment.yaml
│   ├── pvc.yaml
│   ├── secrets.yaml
│   └── svc.yaml
└── values.yaml
```

An example of a template:

```yaml
apiVersion: v1
kind: Secret
metadata:
    name: {{ template "fullname" . }}
    labels:
        app: {{ template "fullname" . }}
        chart: "{{ .Chart.Name }}-{{ .Chart.Version }}"
        release: "{{ .Release.Name }}"
        heritage: "{{ .Release.Service }}"
type: Opaque
data:
    mariadb-root-password: {{ default "" .Values.mariadbRootPassword | b64enc | quote }}
    mariadb-password: {{ default "" .Values.mariadbPassword | b64enc | quote }}
```

## Initializing Helm v2

```
$ helm init
$ k get deployments --namespace=kube-system
```

## Char repositories 

```
$ helm repo add testing http://storage.googleapis.com/kubernetes-charts-testing
$ helm repo list
$ helm search redis
$ helm install testing/redis-standalone
$ helm list



