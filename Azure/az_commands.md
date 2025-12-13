- [Basic](#basic)
- [Configure](#configure)
- [SSh-key](#ssh-key)
- [Resource-group](#resource-group)
- [VM](#vm)

# Basic

```
az account show
az login
```

# Configure

```
az configure --list-defaults
az configure --defaults group=kml_rg_main-ddf1062158e543a8
```

# ssh-key

```
ssh -i <private-key-file-path> azureuser@40.112.236.237

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

# VM

```
az vm list -o table
```
