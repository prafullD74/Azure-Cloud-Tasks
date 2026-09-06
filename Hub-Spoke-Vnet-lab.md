# [Hub and Spoke Network Demo]()

> VNet peering is non-transitive - if VNet A peers with B and B with C, then A and C not connected. Mesh network is fine for small networks.

1. Allow traffic to and from remote network.
2. Use the remote gateway or Route Server.
3. Configure routes at the spoke subnet to forward traffic to the gateway.

- [CLI Network Vnet Commands](https://learn.microsoft.com/en-us/cli/azure/network/vnet?view=azure-cli-latest)

## [Create a virtual network (VNet) and deploy a virtual machine (VM) to the VNet with the Azure CLI](https://learn.microsoft.com/en-us/cli/azure/azure-cli-vm-tutorial-1?view=azure-cli-latest&tabs=bash)

1. [Create a resource group](https://learn.microsoft.com/en-us/cli/azure/azure-cli-vm-tutorial-1?view=azure-cli-latest&tabs=bash#create-a-resource-group)

>Interactive mode offers new AI functionalities that allow the user to run and search for commands more efficiently by running the `az interactive` command. 

```bash
# create Bash shell variables
resourceGroup=VMTutorialResources
location=eastus
```

```bash
az group create --name $resourceGroup --location $location
```

2. [Create a virtual network](https://learn.microsoft.com/en-us/cli/azure/azure-cli-vm-tutorial-2?view=azure-cli-latest&tabs=bash)

```bash
# create Bash shell variables
vnetName=HubVNet1
subnetName=HubSubnet1
vnetAddressPrefix=10.0.0.0/16
subnetAddressPrefix=10.0.0.0/24
```

```bash
az network vnet create --name $vnetName --resource-group $resourceGroup --address-prefixes $vnetAddressPrefix --subnet-name $subnetName --subnet-prefixes $subnetAddressPrefix
```

```bash
vnetName1=SpokeVNet1
subnetName1=SpokeSubnet1
vnetAddressPrefix1=10.1.0.0/16
subnetAddressPrefix1=10.1.0.0/24
```

```bash
az network vnet create --name $vnetName1 --resource-group $resourceGroup --address-prefixes $vnetAddressPrefix1 --subnet-name $subnetName1 --subnet-prefixes $subnetAddressPrefix1
```

```bash
vnetName2=SpokeVNet2
subnetName2=SpokeSubnet2
vnetAddressPrefix2=10.2.0.0/16
subnetAddressPrefix2=10.2.0.0/24
```

```bash
az network vnet create --name $vnetName2 --resource-group $resourceGroup --address-prefixes $vnetAddressPrefix2 --subnet-name $subnetName2 --subnet-prefixes $subnetAddressPrefix2
```

```bash
az network vnet list
```

```bash
az network vnet list -g $resourceGroup
```

3. [Create a virtual machine on a virtual network](https://learn.microsoft.com/en-us/cli/azure/azure-cli-vm-tutorial-3?view=azure-cli-latest&tabs=bash) & [`az vm`](https://learn.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az_vm_create)

> Use `--public-ip-address ""` for VM with a private IP address.

```bash
az vm create -n SpokeVM1 -g $resourceGroup --image Ubuntu2204 --vnet-name SpokeVNet1 --subnet SpokeSubnet1 --size "Standard_B1s" --admin-username admin1 --generate-ssh-keys --public-ip-address "" --output json --verbose
```

```bash
az vm create -n SpokeVM2 -g $resourceGroup --image Ubuntu2204 --vnet-name SpokeVNet2 --subnet SpokeSubnet2 --size "Standard_B1s" --admin-username admin2 --generate-ssh-keys --public-ip-address "" --output json --verbose
```

4. [Get VM information with queries](https://learn.microsoft.com/en-us/cli/azure/azure-cli-vm-tutorial-4?view=azure-cli-latest&tabs=bash)

```bash
az vm show -n SpokeVM1 -g $resourceGroup
```

```bash
az vm show -n SpokeVM2 -g $resourceGroup
```

5. [Test connectivity to a virtual machine](https://learn.microsoft.com/en-us/azure/network-watcher/connection-troubleshoot-manage?tabs=cli#test-connectivity-to-a-virtual-machine)

> Use [`az network watcher test-connectivity`](https://learn.microsoft.com/en-us/cli/azure/network/watcher#az-network-watcher-test-connectivity) command to run connection troubleshoot

```bash
az network watcher test-connectivity -g $resourceGroup --source-resource 'SpokeVM1' --dest-resource 'SpokeVM2' --protocol 'TCP' --dest-port '22'
```

- As of now, the VNets have not been peered. Therefore, the connectivity test between the VMs is expected to fail, as they are hosted in different VNets.
- [`az serial-console`](https://learn.microsoft.com/en-us/cli/azure/serial-console?view=azure-cli-latest) and [Azure Serial Console for Linux](https://learn.microsoft.com/en-us/troubleshoot/azure/virtual-machines/linux/serial-console-linux)

## [Create a Basic SKU VPN gateway](https://learn.microsoft.com/en-us/azure/vpn-gateway/create-gateway-basic-sku-powershell)

   - [Create the VPN gateway (Generation 2 VpnGw2AZ SKU)](https://learn.microsoft.com/en-us/azure/vpn-gateway/create-routebased-vpn-gateway-cli#CreateGateway)
   - Creating a gateway can often take 45 minutes or more, depending on the selected gateway SKU.
   
1. [Add a gateway subnet](https://learn.microsoft.com/en-us/azure/vpn-gateway/create-gateway-basic-sku-powershell#gwsubnet)

```bash
az network vnet subnet create -g $resourceGroup -n "GatewaySubnet"--vnet-name 'HubVNet1'  --address-prefix "10.0.1.0/24"
```

2. [Create a public IP address for your VPN gateway](https://learn.microsoft.com/en-us/azure/vpn-gateway/create-gateway-basic-sku-powershell#PublicIP)

```bash
az network public-ip create -g $resourceGroup -n "HubGateway-IP" --sku "Standard" --allocation-method "Static"
```

3. [Create the VPN gateway](https://learn.microsoft.com/en-us/azure/vpn-gateway/create-gateway-basic-sku-powershell#CreateGateway) and [`az network vnet-gateway create`](https://learn.microsoft.com/en-us/cli/azure/network/vnet-gateway)

```bash
az network vnet-gateway create -g $resourceGroup -n HubGateway --public-ip-address HubGateway-IP --vnet HubVNet1 --gateway-type "Vpn" --vpn-type "RouteBased"  --sku "VpnGw1" --no-wait
```

```bash
az network vnet-gateway show -g $resourceGroup -n HubGateway --query "provisioningState" -o tsv
```

## [Configure VPN gateway transit for virtual network peering](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-peering-gateway-transit)

> Use `--allow-gateway-transit`, If gateway links can be used in remote virtual networking to link to this virtual network. 

1. Use [`az network vnet peering`](https://learn.microsoft.com/en-us/cli/azure/network/vnet/peering?view=azure-cli-latest) Manage peering connections between Azure Virtual Networks.
2. Spoke to Hub VNet peering

```bash
az network vnet peering create -g "VMTutorialResources" --vnet-name "SpokeVNet1" --name "peer-spoke1-to-hub" --remote-vnet "HubVNet1" --allow-vnet-access --allow-forwarded-traffic --use-remote-gateways
```

```bash
az network vnet peering create -g "VMTutorialResources" --vnet-name "SpokeVNet2" --name "peer-spoke2-to-hub" --remote-vnet "HubVNet1" --allow-vnet-access --allow-forwarded-traffic --use-remote-gateways
```

3. Hub to spoke Vnet peering

```bash
az network vnet peering create -g "VMTutorialResources" --vnet-name "HubVNet1" --name "peer-hub-to-spoke1" --remote-vnet "SpokeVNet1" --allow-vnet-access --allow-forwarded-traffic --allow-gateway-transit 
```

```bash
az network vnet peering create -g "VMTutorialResources" --vnet-name "HubVNet1" --name "peer-hub-to-spoke2" --remote-vnet "SpokeVNet2" --allow-vnet-access --allow-forwarded-traffic --allow-gateway-transit
```

4. [Create, change, or delete Azure virtual network peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering?tabs=peering-cli)

5. Diagram

                       ┌──────────────────────┐
                       │      HubVNet1        │
                       │                      │
                       │ VPN/ER Gateway       │
                       │ Shared Services      │
                       └──────────┬───────────┘
                                  │
                     ┌────────────┴────────────┐
                     │                         │
          Hub → Spoke1                 Hub → Spoke2
          AllowGatewayTransit          AllowGatewayTransit
                     │                         │
                     ▼                         ▼
             ┌──────────────┐          ┌──────────────┐
             │ SpokeVNet1   │          │ SpokeVNet2   │
             │              │          │              │
             │ UseRemote    │          │ UseRemote    │
             │ Gateways     │          │ Gateways     │
             └──────────────┘          └──────────────┘

## [Route Table and its association with Vnet]()

1. [Create, change, or delete a route table](https://learn.microsoft.com/en-us/azure/virtual-network/manage-route-table)
2. Create a route table

```bash
az network route-table create -g "VMTutorialResources" -n Spoke1RT
```

```bash
az network route-table create -g "VMTutorialResources" -n Spoke2RT
```

3. Use [`az network route-table route`](https://learn.microsoft.com/en-us/cli/azure/network/route-table/route?view=azure-cli-latest) Manage routes in a route table.

```bash
az network route-table route create -g "VMTutorialResources" --route-table-name "Spoke1RT" --name "Spoke2-Traffic-to-Hub" --address-prefix "10.2.0.0/16" --next-hop-type "VirtualNetworkGateway"
```

```bash
az network route-table route create -g "VMTutorialResources" --route-table-name "Spoke2RT" --name "Spoke1-Traffic-to-Hub" --address-prefix "10.1.0.0/16" --next-hop-type "VirtualNetworkGateway"
```

4. To create association with Vnet subnet [`az network vnet subnet`](https://learn.microsoft.com/en-us/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update)

```bash
az network vnet subnet update -g "VMTutorialResources"
  --vnet-name "SpokeVNet1" --name "default" --route-table "Spoke1RT"
```

```bash
az network vnet subnet update -g "VMTutorialResources"
  --vnet-name "SpokeVNet2" --name "default" --route-table "Spoke2RT"
```

## Final Step [Test connectivity to a virtual machine](https://learn.microsoft.com/en-us/azure/network-watcher/connection-troubleshoot-manage?tabs=cli#test-connectivity-to-a-virtual-machine)

> Use [`az network watcher test-connectivity`](https://learn.microsoft.com/en-us/cli/azure/network/watcher#az-network-watcher-test-connectivity) command to run connection troubleshoot

```bash
az network watcher test-connectivity -g $resourceGroup --source-resource 'SpokeVM1' --dest-resource 'SpokeVM2' --protocol 'TCP' --dest-port '22'
```

- It Will work now.

## [Cleanup]()

> Use `az group wait --name $resourceGroup --deleted` to wait until the deletion is complete or watch it progress.

```bash
az group delete -n $resourceGroup --no-wait
```

```PowerShell
Remove-AzResourceGroup -Name VMTutorialResources
```
