- [SSh-key](#ssh-key)

# ssh-key

```
az sshkey list --query "[].name" --output table

az sshkey create \
  --name nautilus-kp \
  --resource-group <your-rg> \
  --ssh-key-name nautilus-kp
```
