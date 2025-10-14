# [Get started with Office 365 Management API](https://learn.microsoft.com/en-us/office/office-365-management-api/get-started-with-office-365-management-apis)
>The Office 365 Management APIs use Microsoft Entra ID to provide authentication services that you can use to grant rights for your application to access them.

### Register a new application in MS Azure
1. Sign into the Azure portal then navigation pane, select Microsoft Entra ID.
2. On the Microsoft Entra ID page, In Manage select App registrations, and then select New registration.
3. On the Register an application page, do the following things:
	- Name your app.
	- Choose who can use the app and access the API. (Accounts in this organizational directory only (onevizioncustomertest1.onmicrosoft.com only - Single tenant))
	- Provide a redirect URL for user redirect after authentication, if needed.
4. Click Register to register the new app.

### Generate a new key for your application : 
client secrets, are used when exchanging an authorization code for an access token.
1. On the Microsoft Entra ID page in the Azure portal, select App registrations, and then select your application.
2. select Certificates & secrets in the left pane,  and create new client secrets.
3. select New client secret, type a description and select the duration for your key, and then select Add.
4. Click the clipboard icon to copy the client secret value to the clipboard. Azure only displays the client secret value at the time you initially generate it.

### API difference between Office 365 exchange online vs Office 365 managemnet Apis
[Authenticate an IMAP, POP or SMTP connection using OAuth](https://learn.microsoft.com/en-us/exchange/client-developer/exchange-web-services/how-to-authenticate-an-ews-application-by-using-oauth)

### To enable your EWS Managed API applications to access Exchange Online in Office 365. 
[Authenticate an IMAP, POP or SMTP connection using OAuth](https://learn.microsoft.com/en-us/exchange/client-developer/legacy-protocols/how-to-authenticate-an-imap-pop-smtp-application-by-using-oauth#add-the-pop-imap-or-smtp-permissions-to-your-microsoft-entra-application)
Specify the permissions your app requires to access the Office 365 Management APIs
1. In the Azure Portal, go to App registrations > All applications, select your application, and then select API Permissions in the left pane.
2. Click Add a permission to display the Request API permission flyout page.
3. On the Microsoft APIs tab, Select the APIs my organization uses tab and search for "Office 365 Exchange Online".
4. On the flyout page, Click Application permissions and then for IMAP access, choose the IMAP.AccessAsApp permission.
5. Once you've chosen the type of permission, select Add permissions.
6. Select Grant admin consent for "tenant name" to consent to the permissions given to your app.

### Get tenant admin consent 
>To access Exchange mailboxes via POP or IMAP, your Entra application must get tenant admin consent for each tenant. 

### Alternative
>The [Client Credentials Grant Flow](https://learn.microsoft.com/en-us/previous-versions/azure/dn645543(v=azure.100)) allows your application to request subsequent access tokens as old ones expire, without requiring the tenant admin to sign in and explicitly grant consent. This method must be used for applications that run continuously in the background calling the APIs once the initial tenant admin consent has been granted.

### Navigate to the Enterprise Applications and select the newly registered application
- [Register your Entra application's service](https://learn.microsoft.com/en-us/exchange/client-developer/legacy-protocols/how-to-authenticate-an-imap-pop-smtp-application-by-using-oauth#register-service-principals-in-exchange) in Exchange via Exchange Online PowerShell. This registration is enabled by the [New-ServicePrincipal cmdlet](https://learn.microsoft.com/en-us/powershell/module/exchange/new-serviceprincipal).
- To use the New-ServicePrincipal cmdlet, install ExchangeOnlineManagement and connect to your tenant.
```powershell
Install-Module -Name ExchangeOnlineManagement
Import-module ExchangeOnlineManagement 
Connect-ExchangeOnline -Organization <tenantId>
```
- Copy The APPLICATION_ID & OBJECT_ID is the Object ID/Application ID from the Overview page of the Enterprise Application node (Azure Portal) for the application registration.
- Registration of an Microsoft Entra application's service principal in Exchange
```powershell
New-ServicePrincipal -AppId <APPLICATION_ID> -ObjectId <OBJECT_ID> [-Organization <ORGANIZATION_ID>]
```
- Using the [`Get-ServicePrincipal | fl`](https://learn.microsoft.com/en-us/powershell/module/exchange/get-serviceprincipal) cmdlet, can get your registered service principal's identifier.
- The tenant admin can now add the specific mailboxes that your application can access using the [`Add-MailboxPermission`](https://learn.microsoft.com/en-us/powershell/module/exchange/add-mailboxpermission) cmdlet.
```powershell
Add-MailboxPermission -Identity "john.smith@contoso.com" -User 
<SERVICE_PRINCIPAL_ID> -AccessRights FullAccess
```
- Find the service principals associated with an application object
```powershell
Get-MgServicePrincipal -Filter "appId eq '{AppId}'"
```
---

[Identity](https://learn.microsoft.com/en-us/powershell/module/exchangepowershell/get-mailbox?view=exchange-ps&redirectedfrom=MSDN#-identity)
The Identity parameter specifies the mailbox that you want to view. You can use any value that uniquely identifies the mailbox.
"Add-MailboxPermission -Identity "@onevizion.com" -User "EXO ServicePrincipal for AzureAD App" -AccessRights FullAccess"

---
1. [Application and service principal objects in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity-platform/app-objects-and-service-principals?tabs=browser)
2. [Connect to Exchange Online by using remote PowerShell](https://learn.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell)
3. [Get-EXOMailboxPermission](https://learn.microsoft.com/en-us/powershell/module/exchangepowershell/get-exomailboxpermission?view=exchange-ps)
4. [Microsoft identity platform and the OAuth 2.0 client credentials flow](https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-client-creds-grant-flow)
5. [Securing service principals in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/architecture/service-accounts-principal)
6. [Develop web service clients for Exchange](https://learn.microsoft.com/en-us/exchange/client-developer/exchange-web-services/develop-web-service-clients-for-exchange)
7. [Authenticate an IMAP, POP or SMTP connection using OAuth](https://learn.microsoft.com/en-us/exchange/client-developer/legacy-protocols/how-to-authenticate-an-imap-pop-smtp-application-by-using-oauth#register-your-application)
8. [Overview of permissions and consent in the Microsoft identity platform](https://learn.microsoft.com/en-us/entra/identity-platform/permissions-consent-overview)

---
### [Service to Service Calls Using Client Credentials](https://learn.microsoft.com/en-us/previous-versions/azure/dn645543(v=azure.100))
[Client credentials to request app-only access tokens](https://learn.microsoft.com/en-us/office/office-365-management-api/get-started-with-office-365-management-apis#configure-an-x509-certificate-to-enable-service-to-service-calls)
- Here are instructions for using the Visual Studio or Windows SDK makecert tool to create a self-signed certificate and export the public key to a base64-encoded file.
```powershell
makecert -r -pe -n "CN=MyCompanyName MyAppName Cert" -b 03/15/2015 -e 03/15/2017 -ss my -len 2048
```
---
### [Create a self-signed public certificate to authenticate your application](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-self-signed-certificate)

- [Create and upload a self-signed certificate]()
- To create a self-signed certificate, open Windows PowerShell and run [`New-SelfSignedCertificate`](https://learn.microsoft.com/en-us/powershell/module/pki/new-selfsignedcertificate) with the following parameters to create the certificate in the user certificate store on your computer
```powershell
$cert=New-SelfSignedCertificate -Subject "CN=DaemonConsoleCert" -CertStoreLocation "Cert:\CurrentUser\My"  -KeyExportPolicy Exportable -KeySpec Signature
```
- Export this certificate to a file using the [Manage User Certificate](https://learn.microsoft.com/en-us/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) MMC snap-in accessible from the Windows Control Panel.

---
## [Expand virtual hard disks Azure]()
>Allow up to 10 minutes for the correct size to be reflected in Windows VMs and Linux VMs.

1. [Linux]()
   - Expanding operating system (OS) disks and data disks for a Linux. Default OS disk is 30 GB on a Linux. [Master boot record (MBR)](https://wikipedia.org/wiki/Master_boot_record) limits the usable size to 2 TiB.
   - [Increase the size of the OS disk](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/expand-disks?tabs=ubuntu#increase-the-size-of-the-os-disk)
   - `df -Th` command to identifying the relationship between disk utilization, mount point, and device. Note the Type column, which indicates filesystem format.
   - To locate Azure logical unit numbers (LUNs) correlates to Filesystem `sudo ls -alF /dev/disk/azure/scsi1/`
   - Expand an Azure managed disk without downtime (Expanding Ultra Disks and Premium SSD v2 disks without downtime)
   - With [`az vm deallocate`](https://learn.microsoft.com/en-us/cli/azure/vm#az-vm-deallocate) VM must be deallocated to expand the virtual hard disk. Stopping the VM with `az vm stop` doesn't release the compute resources..
   ```azurecli
   az vm deallocate -g MyResourceGroup -n MyVm
   ```
   - View a list of managed disks in a resource group with [`az disk list`](https://learn.microsoft.com/en-us/cli/azure/disk#az-disk-list).
   ```azurecli
   az disk list --resource-group myResourceGroup --query '[*].{Name:name,size:diskSizeGB,Tier:sku.tier}' --output table
   ```
   - Expand the required disk with [`az disk update`](https://learn.microsoft.com/en-us/cli/azure/disk#az-disk-update).
   ```azurecli
   az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 200
   ```
   - Start your VM with [`az vm start`](https://learn.microsoft.com/en-us/cli/azure/vm#az-vm-start).
   ```azurecli
   az vm start -g MyResourceGroup -n MyVm
   ```
   - Expand a disk partition and filesystem
		1. Detect a changed disk size : Identify the currently recognized size and 
		```bash
		sudo fdisk -l /dev/sda
		```
		```bash
		echo 1 | sudo tee /sys/class/block/sda/device/rescan
		```
   - [Add data disks](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/add-disk) to provide more storage space
2. [Windows](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/expand-disks)
   - Expand the partition by using [DiskPart](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/expand-disks#use-diskpart) or [Disk Manager](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/expand-disks#use-disk-manager).
   - Start a remote desktop session with the VM. Open Disk Management.
   - Right-click an existing C: drive partition and select Extend Volume. Follow the steps in the wizard to see the disk with updated capacity.
3. [Increase Azure Linux RedHat(8.8) data disk size bt 100 GB.](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/expand-disks?tabs=rhellvm#tabpanel_1_rhellvm)
   - In the Azure portal, go to the VM then On the left menu, under Settings, select Disk. **Note the LUN(Logical Unit  Number)**
   - Under Disk name, select the disk that you want to expand.
   - On the left menu, under Settings, select Size + performance. In Custome Disk Size, enter the size of disk. Select Resize.
   - Now ssh to VM and run the below command one by one.(pvresize, lvextend and resize2fs to expand the LVM & Filesystem)
   ```bash
   sudo -i
   lsscsi
   lsblk /dev/sdl
   pvresize /dev/sdl1
   lvextend -r -l +100%FREE /dev/rhel_data/lv_data
   df -hT /
   ```
   - `lsscsi` List all disks and their LUNs. example, LUN 10 = /dev/sdl
   - `ls -l /dev/disk/azure/scsi` Compare the LUN number from the portal to the last number of the HTCL portion of the output, which is the LUN.
   - `lsblk /dev/sda4` Verify the size of the partition and use `lsblk` Verify LVM setup
   - `pvresize /dev/sda4` Resize the Physical Volume and run `pvscan` verify that the new size of the PV.
   - To verify VG (Volume Group) and LV (Logical Volume) names, use `vgdisplay` `lvdisplay` `vgs` `lvs` and `lsblk -f` to determine which logical volume (LV) is mounted on the root of the filesystem (/).
   - `lvextend` only to increase (extend) the size of a logical volume.
   - `-r` resize filesystem automatically and `-l +100%FREE` use all available space.

   - `lvresize` can increase OR decrease the size of a logical volume.
