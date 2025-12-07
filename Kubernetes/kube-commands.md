
- [Node](#node)
- [Pod](#pod)
  
# Node

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
