
- [ConfigMap](#configmap)
- [ETCD](#etcd)
- [Node](#node)
- [Pod](#pod)
- [Service](#service)
- [Secrets](#secrets)


# ConfigMap

- To create a configmap
```
k create cm --help
k create cm <cm-name> --from-literal=<key>=<value>

k get cm
k describe cm <config-name>

```

# Etcd

- Install etcd-client

```
apt-get install etcd-client
```
  

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

- scale the replica
```
kubectl scale deploy myapp --replicas=4
```

- replace the pod

```
kubectl replace --force -f file.yaml
```

# Service

- to list the service based on the selector

```
kubectl get svc -o jsonpath='{range .items[?(@.spec.selector.app=="myapp")]}{.metadata.name}{"\n"}{end}'
```

# Secrets

- To create a secret

```
k create secret generic <secret-name> --from-literal=<key>=<value>

k get secrets
k describe secret <secret-name>
```

- Decode and encode a secret

```
echo -n "my-password" | base64        --> encode

echo "encoded-pass" | base64          --> decode
```
