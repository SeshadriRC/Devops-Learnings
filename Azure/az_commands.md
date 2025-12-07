- [SSh-key](#ssh-key)
- [Resource-group](#resource-group)

# ssh-key

```
az sshkey list --query "[].name" --output table

az sshkey create \
  --name nautilus-kp \
  --resource-group <your-rg> \
  --ssh-key-name nautilus-kp
```

# Resource-group
```
az group list -o table
az group create --name myResourceGroup --location eastus
```
