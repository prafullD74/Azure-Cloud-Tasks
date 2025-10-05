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
### command to create the peering connection between the SalesVNet and MarketingVNet virtual networks. 
```bash
az network vnet peering create \
    --name SalesVNet-To-MarketingVNet \
    --remote-vnet MarketingVNet \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --vnet-name SalesVNet \
    --allow-vnet-access
```
### command to create a reciprocal connection from MarketingVNet to SalesVNet.
```bash
az network vnet peering create \
    --name MarketingVNet-To-SalesVNet \
    --remote-vnet SalesVNet \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --vnet-name MarketingVNet \
    --allow-vnet-access
```
### command to create the peering connection between the MarketingVNet and ResearchVNet virtual networks:
```bash
az network vnet peering create \
    --name MarketingVNet-To-ResearchVNet \
    --remote-vnet ResearchVNet \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --vnet-name MarketingVNet \
    --allow-vnet-access
```
### command to create the reciprocal connection between ResearchVNet and MarketingVNet:
```bash
az network vnet peering create \
    --name ResearchVNet-To-MarketingVNet \
    --remote-vnet MarketingVNet \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --vnet-name ResearchVNet \
    --allow-vnet-access
```
### command to check the connection between SalesVNet and MarketingVNet:
```bash
az network vnet peering list \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --vnet-name SalesVNet \
    --query "[].{Name:name, Resource:resourceGroup, PeeringState:peeringState, AllowVnetAccess:allowVirtualNetworkAccess}"\
    --output table
```bash
### command to check the peering connection between the ResearchVNet and MarketingVNet virtual networks:
```bash
az network vnet peering list \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --vnet-name ResearchVNet \
    --query "[].{Name:name, Resource:resourceGroup, PeeringState:peeringState, AllowVnetAccess:allowVirtualNetworkAccess}"\
    --output table
```
###command to check the peering connections for the MarketingVNet virtual network.
```bash
az network vnet peering list \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --vnet-name MarketingVNet \
    --query "[].{Name:name, Resource:resourceGroup, PeeringState:peeringState, AllowVnetAccess:allowVirtualNetworkAccess}"\
    --output table
```
### command to look at the routes that apply to the SalesVM network interface:
```bash
az network nic show-effective-route-table \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --name SalesVMVMNic \
    --output table
```
### command to look at the routes for MarketingVM:
```bash
az network nic show-effective-route-table \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --name MarketingVMVMNic \
    --output table
```
### command to look at the routes for ResearchVM:
```bash
az network nic show-effective-route-table \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --name ResearchVMVMNic \
    --output table
```
### command to list the IP addresses you'll use to connect to the VMs:
```bash
az vm list \
    --resource-group learn-7b4e4823-f309-4b4d-aba8-e604286a7339 \
    --query "[*].{Name:name, PrivateIP:privateIps, PublicIP:publicIps}" \
    --show-details \
    --output table
```

### command to create a route table.
```bash
az network route-table create \
        --name publictable \
        --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
        --disable-bgp-route-propagation false
```

### command to create a custom route.
```bash
az network route-table route create \
        --route-table-name publictable \
        --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
        --name productionsubnet \
        --address-prefix 10.0.1.0/24 \
        --next-hop-type VirtualAppliance \
        --next-hop-ip-address 10.0.2.4
```

### command to create the vnet virtual network and the publicsubnet subnet.
```bash
az network vnet create \
        --name vnet \
        --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
        --address-prefixes 10.0.0.0/16 \
        --subnet-name publicsubnet \
        --subnet-prefixes 10.0.0.0/24
```
### command to create the privatesubnet subnet.
```bash
az network vnet subnet create \
        --name privatesubnet \
        --vnet-name vnet \
        --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
        --address-prefixes 10.0.1.0/24
```

### command to create the dmzsubnet subnet.
```bash
az network vnet subnet create \
        --name dmzsubnet \
        --vnet-name vnet \
        --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
        --address-prefixes 10.0.2.0/24
```
### command to show all of the subnets in the vnet virtual network.
```bash
az network vnet subnet list \
        --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
        --vnet-name vnet \
        --output table
```

### command to associate the route table with the public subnet.	
```bash
az network vnet subnet update \
        --name publicsubnet \
        --vnet-name vnet \
        --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
        --route-table publictable
```

### Deploy the network virtual appliance
>command to deploy the appliance. Replace <password> with a suitable password of your choice for the azureuser admin account.
```bash
az vm create \
    --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
    --name nva \
    --vnet-name vnet \
    --subnet dmzsubnet \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password March20@2023
```

###Enable IP forwarding for the Azure network interface
>command to get the ID of the NVA network interface.
```bash
NICID=$(az vm nic list \
    --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
    --vm-name nva \
    --query "[].{id:id}" --output tsv)
```
```bash
echo $NICID
```
### Command to get the name of the NVA network interface.
```bash
NICNAME=$(az vm nic show \
    --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
    --vm-name nva \
    --nic $NICID \
    --query "{name:name}" --output tsv)
```
```bash
echo $NICNAME
```
### command to enable IP forwarding for the network interface.
```bash
az network nic update --name $NICNAME \
    --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
    --ip-forwarding true
```bash

### Enable IP forwarding in the appliance
>command to save the public IP address of the NVA virtual machine to the variable NVAIP.
```bash
NVAIP="$(az vm list-ip-addresses \
    --resource-group learn-238ad42d-c8a5-445c-a227-bff02b9a490b \
    --name nva \
    --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
    --output tsv)"
```
```bash
echo $NVAIP
```
### command to enable IP forwarding within the NVA.
```bash
ssh -t -o StrictHostKeyChecking=no azureuser@$NVAIP 'sudo sysctl -w net.ipv4.ip_forward=1; exit;'
```


---
```bash
git init
git add .
git commit -m "Initial commit"
```
---
