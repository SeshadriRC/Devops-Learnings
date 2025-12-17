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

- variable set

```
RG="kml_rg_main-5e34bbaec41e49cf"
VM="devops-vm"
LOC="westus"
IMAGE="Ubuntu2204"
SIZE="Standard_B2s"
DISK=30
```

- vm create

```
az vm create \
  --resource-group $RG \
  --name $VM \
  --image Ubuntu2204 \
  --size $SIZE \
  --location $LOC \
  --os-disk-size-gb $DISK \
  --storage-sku Standard_LRS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --nsg-rule ssh
```
- to check the specific instance

```
az vm get-instance-view \
  --name devops-vm \
  --resource-group $RG \
  --query "instanceView.statuses[?starts_with(code,'PowerState/')].displayStatus" \
  --output table
  
az vm show \
  --name devops-vm \
  --resource-group $RG \
  --show-details \
  --query "powerState"
```

- to check publicIps of specific instance

```
az vm show \
  --resource-group $RG \
  --name devops-vm \
  --show-details \
  --query "publicIps" \
  --output tsv
```
