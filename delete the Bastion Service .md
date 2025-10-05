# Runbook execution in Azure Automation: 
### Delete the Bastion Service using Powershell automation.
1.	Azure portal, go to the Automation account overview and create. 
2.	Fill the all the details depends on the requirement, important one enable “System assigned” in the managed Identities pane.
3.	Once the service is created, inside account settings select Identity.
4.	Add “Azure role assignments” or create a new role assignment (Scope: Subscription, Role: Contributor).
5.	Under Process Automation, select Runbooks. [Create PowerShell runbook](https://learn.microsoft.com/en-us/azure/automation/learn/powershell-runbook-managed-identity#create-powershell-runbook)
6.	Select Create a runbook.
    1. Name the runbook StopBastion.
    2. From the Runbook type drop-down, select PowerShell.
    3. From the Runtime version drop-down, select either 7.1 (preview) or 5.1.
    4. Enter an applicable Description.
7.	Click Create to create the runbook [Remove-AzBastion](https://learn.microsoft.com/en-us/powershell/module/az.network/remove-azbastion?view=azps-12.0.0).
8.	In the runbook editor, paste the following code (Example):
```bash
$automationAccount = "Account_Name"

# Ensures you do not inherit an AzContext in your runbook
$null = Disable-AzContextAutosave -Scope Process

# Connect using a Managed Service Identity
try
{
    "Logging in to Azure..."
    Connect-AzAccount -Identity
}
catch {
    Write-Error -Message $_.Exception
    throw $_.Exception
}
"Selecting subscription"
Select-AzSubscription <SUBSCRIPTIONID>
try
{
    try {
		#get info about bastion
		$nic = Get-AzBastion -Name "<BastionName" -ResourceGroup "<ResourceGroupName>"
		#get id from public ip
		$id = $nic.IpConfigurations.publicipaddress.id
		#extract the name of the id 
		$name = $id.Split("/")[-1]
	} catch {
		#Do nothing
	}
	"Removing bastion..."
	Remove-AzBastion -ResourceGroupName "<ResourceGroupName>" -Name "<BastionName>" -Force
	"Bastion removed"
	if (-not ([string]::IsNullOrEmpty($name)))
	{
		"Removing public IP..."
		Remove-AzPublicIpAddress -ResourceGroupName "<ResourceGroupName>" -Name $name -Force
		"Public IP removed"
	}
}
catch {  
	Write-Error -Message $_.Exception
    throw $_.Exception
}  
```
9.	Select Save and then Test pane.
10.	Under Resources, select Schedules.
11.	Use “Link a schedule to your runbook” and specify the parameters according on your needs in order to run it.
12.	Access the "Automation account" and view the completed jobs' statuses.

