# Deploying apps


## Deployments
```
$ k create deployment nginx --image=nginx
$ k get events
LAST SEEN   TYPE     REASON              OBJECT                        MESSAGE
8s          Normal   Killing             pod/nginx-86c57db685-2zwnx    Stopping container nginx
<unknown>   Normal   Scheduled           pod/nginx-86c57db685-dfrrc    Successfully assigned default/nginx-86c57db685-dfrrc to kube-node4
4s          Normal   Pulling             pod/nginx-86c57db685-dfrrc    Pulling image "nginx"
2s          Normal   Pulled              pod/nginx-86c57db685-dfrrc    Successfully pulled image "nginx"
2s          Normal   Created             pod/nginx-86c57db685-dfrrc    Created container nginx
2s          Normal   Started             pod/nginx-86c57db685-dfrrc    Started container nginx
8s          Normal   Killing             pod/nginx-86c57db685-g7lcw    Stopping container nginx
8s          Normal   Killing             pod/nginx-86c57db685-jd8k4    Stopping container nginx
5s          Normal   SuccessfulCreate    replicaset/nginx-86c57db685   Created pod: nginx-86c57db685-dfrrc
5s          Normal   ScalingReplicaSet   deployment/nginx              Scaled up replica set nginx-86c57db685 to 1

# $ k edit deployment nginx (replicas set to 3)

2m12s       Normal   SuccessfulCreate    replicaset/nginx-86c57db685   Created pod: nginx-86c57db685-dfrrc
8s          Normal   SuccessfulCreate    replicaset/nginx-86c57db685   Created pod: nginx-86c57db685-bm577
8s          Normal   SuccessfulCreate    replicaset/nginx-86c57db685   Created pod: nginx-86c57db685-f26l9
2m12s       Normal   ScalingReplicaSet   deployment/nginx              Scaled up replica set nginx-86c57db685 to 1
8s          Normal   ScalingReplicaSet   deployment/nginx              Scaled up replica set nginx-86c57db685 to 3
```


- Output of a deployment in yaml `$ k get deployment nginx -o yaml`
- To change an object configuration one can use subcommands apply, edit or patch for non-disruptive updates. 
- A disruptive update could be done using the `replace --force` option.


## Services

- A deployment can be exposed, but if the containerPort is not defined in the manifest, it will fail

```
$ k expose deployment nginx
error: couldn't find port via --port flag or introspection
See 'kubectl expose -h' for help and examples

$ k get deployment nginx -o yaml > nginx-deployment.yaml 

# Add:
# ports:
# - containerPort: 80
    protocol: TCP

$ k replace -f nginx-deployment.yaml
$ k expose deployment nginx
service/nginx exposed
```
