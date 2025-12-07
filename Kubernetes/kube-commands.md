
- [Node](#node)
- [Pod](#pod)
- [Service](#service)
  
# Node

- drain the node

```
kubectl drain worker-node01 --ignore-daemonsets --force --delete-emptydir-data
```

- show the labels of node

```
kubectl get nodes --show-labels
```

- label the node

```
kubectl label node <node-name> disktype=ssd
```

# Pod

- delete the pod by using yaml

```
kubectl delete -f pod.yaml
```


```
kubectl scale deploy myapp --replicas=4
```

# Service

- to list the service based on the selector

```
kubectl get svc -o jsonpath='{range .items[?(@.spec.selector.app=="myapp")]}{.metadata.name}{"\n"}{end}'
```
