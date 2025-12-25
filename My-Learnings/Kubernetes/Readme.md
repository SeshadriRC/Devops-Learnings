- [Commands](#commands)
- [Notes](#notes)


# Notes
- Check application is accessible using curl 
- Service selector shud match pod label
- If you are checking service, service endpoint should match POD ip.
- First check pod events, then you can check log.

# Commands

**Config**

```
kubectl config set-context --current --namespace=alpha
```

**Logs**

```
k logs <pod-name> -f --previous
```
**Replace**

```
kubectl replace --force -f file.yaml
```
