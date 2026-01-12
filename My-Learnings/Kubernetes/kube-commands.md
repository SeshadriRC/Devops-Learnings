**Config**

```
kubectl config set-context --current --namespace=alpha
```

---

**Logs**

```
k logs <pod-name> -f --previous
```

---
**Replace**

```
kubectl replace --force -f file.yaml
```
