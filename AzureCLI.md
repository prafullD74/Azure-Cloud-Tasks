# Azure Resource and Storage Account Creation Commands

This document outlines the Azure CLI commands to create a resource group and a storage account, and to fetch the storage account keys.

## 1. Create a Resource Group

This command creates an Azure resource group named `resgroup` in the `eastus` region.

```bash
az group create -n resgroup --location eastus
````

## 2\. Create a Storage Account

This command creates a storage account named `mystorageacc` within the `resgroup` resource group. It specifies the `eastus` location, a Standard LRS (Locally Redundant Storage) SKU, and enables blob encryption.

```bash
az storage account create -n mystorageacc --resource-group resgroup --location eastus --sku Standard_LRS --encryption blob
```

## 3\. Fetch Storage Account Keys

This command retrieves the access keys for the `mystorageacc` storage account located in the `resgroup` resource group.

```bash
az storage account keys list --account-name mystorageacc --resource-group resgroup
```
---
## AKS
```bash
az group creat -n aksrg --location eastus
```

```bash
az aks create -group aksrg --name myaks cluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```

```bash
az aks get-credentials -g aksrg -n myaks
```

```bash
az container create -g myaks --name mycontainer3 --image
```

```bash 
az container logs -g myaks --name mycontainer3
```

```bash
az container attach -g myaks --name mycontainer3
```

```bash
az storage account create -g my -n storageacc47 --sku Standard_LRS --location eastus
```

```bash
az storage account ket list -g my -n storageacc47 -o table
```

```bash
az storage share create -n myfs147 --account-name storageacc47 --account-key
--azure-file-volume-account-name  --azure-file-volume-account-key  --azure-file-volume-share-name  --azure-file-volume-mount-path /logs
```

---
## Azure Container registery:
```bash
az acr login -n myregistryname667
```

---
## Virtul Network:
```bash
az network vnet --help
```

```bash
az group list -o table
```

```bash
az network vnet check-ip-address -g myrg123 -n myvent123 --pi-address 10.2.24
```

```bash
az network vnet create -g myrg123 -name myvnet123
```

```bash
az network vnet create -g myrg123 -n  myvnet --address-prefix 10.0.0.0/16 --subnet-name mysubnet --subnet-prefix 10.0.0.0/24
```

```bash
az network vnet list-endpoint-services -o table -l eastus
```

```bash
az network vnet show -g myrg123 -n  myvnet
```

---
## Network Security Group : Firewall rules
```bash
az network nsg --help
```

```bash
az network nsg create -g myrg -n mynsg
```

---
## Azure SQL Database
## Cosmos DB

---
## SQL Database
```bash
az sql server create --name myserverdb -g myrg123 --location eastus --admin-user testuser --admin-password 123456789!@#
```

```bash
az sql server firewall-rule create -g myrg123 --server myserverdb -n AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

```bash
az sql db create -g myrg123 --server myserverdb -n mydb1db --sample-name Sqlstart --service-objective S0 --zone-redundant
```

```bash
az sql elastic-pool create -g myrg123 --server myserverbd -n myfirstpool --dtu 50
```

```bash
az sql db create -g myrg123 --server myserverdb --name mydb1db --elastic-pool myfirstpool
```
