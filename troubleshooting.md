# Troubleshooting

```
$ k get events
9s          Warning   SystemOOM           node/kube-node2             System OOM encountered, victim process: stress, pid: 4873
8s          Warning   BackOff             pod/hog-746dd84974-rbpwf    Back-off restarting failed container
0s          Normal    Pulling             pod/hog-746dd84974-rbpwf    Pulling image "vish/stress"
0s          Normal    Pulled              pod/hog-746dd84974-rbpwf    Successfully pulled image "vish/stress"
0s          Normal    Created             pod/hog-746dd84974-rbpwf    Created container stress
0s          Normal    Started             pod/hog-746dd84974-rbpwf    Started container stress
0s          Normal    Killing             pod/hog-746dd84974-rbpwf    Stopping container stress
```


 
