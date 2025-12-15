
- [Certificate](#certificate)
- [Config](#config)
- [ConfigMap](#configmap)
- [ETCD](#etcd)
- [HPA](#hpa)
- [Logs](#logs)
- [Node](#node)
- [Pod](#pod)
- [Service](#service)
- [Secrets](#secrets)
- [User Creation](https://github.com/SeshadriRC/Devops-Learnings/blob/main/Kubernetes/User-management/User-creation.md)


# Certificate

```
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
k get csr
k certificate approve <user-name>
k certificate deny <user-name>
k delete csr <user-name>
echo "keywords" | base64 --decode
```



# Config

```
k config view
k config use-context research --kubeconfig <config-file-location>
```

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

# HPA

```
k describe hpa ,hpa-name>
k get hpa
kubectl get hpa --watch
k autoscale deployment <dep-name>

```
# Logs

```
k logs <pod-name> -c <initcontainer>
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

- describe

```
k describe pod etcd-controlplane -n kube-system | grep Image

kubectl -n kube-system describe pod etcd-controlplane | grep '\--cert-file'
      --cert-file=/etc/kubernetes/pki/etcd/server.crt

```
- exec into the pod

```
kubectl -n elastic-stack exec -it app -- cat /log/app.log
```
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

- test pod

```
kubectl run testpod --rm -it --image=ubuntu -- bash
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
