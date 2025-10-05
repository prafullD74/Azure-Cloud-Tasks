# az
### Let's create a new Linux virtual machine

```bash
az vm create \
  --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
  --location westus \
  --name SampleVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --verbose
```
```bash
az vm list-ip-addresses --name SampleVM --output table
```
```bash
ssh azureuser@20.59.108.36
```
```bash
az vm image list --output table
```
```bash
az vm image list --sku Wordpress --output table --all
```
```bash
az vm image list --publisher Microsoft --output table --all
```
```bash
az vm image list --location eastus --output table
```
```bash
az vm list-sizes --location eastus --output table
```

```bash
az vm create \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --name SampleVM2 \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --verbose \
    --size "Standard_DS2_v2"
```

### must check to see if the desired size is available in the cluster our VM is part of.
```bash
az vm list-vm-resize-options \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --name SampleVM \
    --output table
```

```bash
az vm resize \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --name SampleVM \
    --size Standard_D2s_v3
```

```bash
az vm list --output table
```
```bash
az vm list-ip-addresses -n SampleVM -o table
```
```bash
az vm show --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 --name SampleVM
```

```bash
az vm show \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --name SampleVM \
    --query "osProfile.adminUsername"
```
```bash
az vm show \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --name SampleVM \
    --query hardwareProfile.vmSize
```
```bash
az vm show \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --name SampleVM \
    --query "networkProfile.networkInterfaces[].id"
```
```bash
az vm show \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --name SampleVM \
    --query "networkProfile.networkInterfaces[].id" -o tsv
```

```bash
az vm stop \
    --name SampleVM \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714
```

### To see the current running state of your VM:
```bash
az vm get-instance-view \
    --name SampleVM \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```

```bash
az vm start \
    --name SampleVM \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714
```

```bash
az vm restart -n SampleVM -g learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 --no-wait
```

```bash
Install nginx web server: sudo apt-get -y update && sudo apt-get -y install nginx
```

```bash
curl -m 80 <PublicIPAddress>
```


### To open port 80:
```bash
az vm open-port \
    --port 80 \
    --resource-group learn-d5755e53-3d8c-48da-9880-ba1f4f9d6714 \
    --name SampleVM
```

### set shell variables that contain our app's name, resource group name, plan name, sku, and location: 

1. export APPNAME=$(az webapp list --query [0].name --output tsv)
2. export APPRG=$(az webapp list --query [0].resourceGroup --output tsv)
3. export APPPLAN=$(az appservice plan list --query [0].name --output tsv)
4. export APPSKU=$(az appservice plan list --query [0].sku.name --output tsv)
5. export APPLOCATION=$(az appservice plan list --query [0].location --output tsv)

```bash
az webapp up --name $APPNAME --resource-group $APPRG --plan $APPPLAN --sku $APPSKU --location "$APPLOCATION"
```

### commands to create a username and generate a random password.
- USERNAME=azureuser
- PASSWORD=$(openssl rand -base64 32)

### command in Cloud Shell to open your VM's port 80 for web traffic:
```bash
az vm open-port \
  --port 80 \
  --resource-group learn-94cf94e4-5943-47c4-bee5-effcfdda741b \
  --name myVM
```

### command to start a PowerShell session:
```bash
pwsh
```

### command to create the CoreServicesVnet virtual network:
```bash
az network vnet create \
    --resource-group learn-a5028163-bd52-4af8-a222-20133e130625 \
    --name CoreServicesVnet \
    --address-prefixes 10.20.0.0/16 \
    --location westus
```
### create the subnets
```bash
az network vnet subnet create \
    --resource-group learn-a5028163-bd52-4af8-a222-20133e130625 \
    --vnet-name CoreServicesVnet \
    --name GatewaySubnet \
    --address-prefixes 10.20.0.0/27
```
```bash
az network vnet subnet create \
    --resource-group learn-a5028163-bd52-4af8-a222-20133e130625 \
    --vnet-name CoreServicesVnet \
    --name SharedServicesSubnet \
    --address-prefixes 10.20.10.0/24
```
```bash
az network vnet subnet create \
    --resource-group learn-a5028163-bd52-4af8-a222-20133e130625 \
    --vnet-name CoreServicesVnet \
    --name DatabaseSubnet \
    --address-prefixes 10.20.20.0/24
```
```bash
az network vnet subnet create \
    --resource-group learn-a5028163-bd52-4af8-a222-20133e130625 \
    --vnet-name CoreServicesVnet \
    --name PublicWebServiceSubnet \
    --address-prefixes 10.20.30.0/24
```
### Command to show all the subnets that we configured:
```bash
az network vnet subnet list \
    --resource-group learn-a5028163-bd52-4af8-a222-20133e130625 \
    --vnet-name CoreServicesVnet \
    --output table
```
##create the virtual network and subnet for the Sales systems:
```bash
az network vnet create \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --name SalesVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name Apps \
    --subnet-prefixes 10.1.1.0/24 \
    --location northeurope
```

### Command to view the virtual networks:
```bash
az network vnet list --query "[?contains(provisioningState, 'Succeeded')]" --output table
```

### command, replacing <password> with a password that meets the requirements for Linux VMs, to create an Ubuntu VM 
```bash
az vm create \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --no-wait \
    --name SalesVM \
    --location northeurope \
    --vnet-name SalesVNet \
    --subnet Apps \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password March19@2023
```
```bash
az vm create \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --no-wait \
    --name MarketingVM \
    --location northeurope \
    --vnet-name MarketingVNet \
    --subnet Apps \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password March20@2023
```
```bash
az vm create \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --no-wait \
    --name ResearchVM \
    --location westeurope \
    --vnet-name ResearchVNet \
    --subnet Data \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password March21@2023
```

### To confirm that the VMs are running, run the following command. 
```bash
watch -d -n 5 "az vm list \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --show-details \
    --query '[*].{Name:name, ProvisioningState:provisioningState, PowerState:powerState}' \
    --output table"
```


---
```bash
git init
git add .
git commit -m "Initial commit"
```
---
