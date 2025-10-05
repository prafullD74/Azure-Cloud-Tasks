# Microsoft Azure Fundamentals: Describe cloud concepts

### Cloud computing is the delivery of computing services over the internet.
With the shared responsibility model, physical security, power, cooling, and network connectivity are the responsibility of the cloud provider. At the same time, the consumer is responsible for the data and information stored in the cloud.
The shared responsibility model is heavily tied into the cloud service types (covered later in this learning path): 
- infrastructure as a service (IaaS) : IaaS places the most responsibility on the consumer, with the cloud provider being responsible for the basics of physical security, power, and connectivity.
- platform as a service (PaaS)
- software as a service (SaaS)

### The cloud provider is always responsible for:
1. The physical datacenter
2. The physical network
3. The physical hosts

>The three main cloud models are: private, public, and hybrid.
- The general public availability is a key difference between public and private clouds.
- A hybrid cloud is a computing environment that uses both public and private clouds in an inter-connected environment.
- Azure Arc can help manage your cloud environment, whether it's a public cloud solely on Azure, a private cloud in your datacenter, a hybrid configuration, or even a multi-cloud environment running on multiple cloud providers at once.
- Azure VMware Solution lets you run your VMware workloads in Azure with seamless integration and scalability.

### This consumption-based model has many benefits, including:
    1. No upfront costs.
    2. No need to purchase and manage costly infrastructure that users might not use to its fullest potential.
    3. The ability to pay for more resources when they're needed.
    4. The ability to stop paying for resources that are no longer needed.

>To put it another way, cloud computing is a way to rent compute power and storage from someone else’s datacenter.

## The benefits of using cloud services

- High availability focuses on ensuring maximum availability, regardless of disruptions or events that may occur.
- Scalability refers to the ability to adjust resources to meet demand.
- Vertical scaling is focused on increasing or decreasing the capabilities of resources. With vertical scaling, if you were developing an app and you needed more processing power, you could vertically scale up to add more CPUs or RAM to the virtual machine.
- Horizontal scaling is adding or subtracting the number of resources.With horizontal scaling, if you suddenly experienced a steep jump in demand, your deployed resources could be scaled out (either automatically or manually). For example, you could add additional virtual machines or containers, scaling out.
- Reliability is the ability of a system to recover from failures and continue to function.
- Predictability in the cloud lets you move forward with confidence. Predictability can be focused on performance predictability or cost predictability.
- Performance predictability focuses on predicting the resources needed to deliver a positive experience for your customers. Autoscaling, load balancing, and high availability are just some of the cloud concepts that support performance predictability.
- Cost predictability is focused on predicting or forecasting the cost of the cloud spend. Use tools like the Total Cost of Ownership (TCO) or Pricing Calculator to get an estimate of potential cloud spend.
- Cloud-based auditing helps flag any resource that’s out of compliance with your corporate standards and provides mitigation strategies. Depending on your operating model, software patches and updates may also automatically be applied, which helps with both governance and security. Cloud providers are typically well suited to handle things like distributed denial of service (DDoS) attacks, making your network more robust and secure.

## Cloud service types

1. Infrastructure as a service (IaaS) is the most flexible category of cloud services, as it provides you the maximum amount of control for your cloud resources. In an IaaS model, the cloud provider is responsible for maintaining the hardware, network connectivity (to the internet), and physical security. You’re responsible for everything else: operating system installation, configuration, and maintenance; network configuration; database and storage configuration; and so on. With IaaS, you’re essentially renting the hardware in a cloud datacenter, but what you do with that hardware is up to you.
2. PaaS is well suited to provide a complete development environment without the headache of maintaining all the development infrastructure.
3. Software as a service (SaaS) is the most complete cloud service model from a product perspective. With SaaS, you’re essentially renting or using a fully developed application. In a SaaS environment you’re responsible for the data that you put into the system, the devices that you allow to connect to the system, and the users that have access. 

>Azure Fundamentals: Describe Azure architecture and services

## Azure compute and networking services

- An image is a template used to create a VM and may already include an OS and other software, like development tools or web hosting environments.
- Azure can also manage the grouping of VMs for you with features such as scale sets and availability sets.
- Virtual machine scale sets let you create and manage a group of identical, load-balanced VMs.
- Virtual machine availability sets are another tool to help you build a more resilient, highly available environment.
- Availability sets do this by grouping VMs in two ways: update domain and fault domain.
- Availability sets stagger VM updates based on their update and fault domains.
- VMs are also an excellent choice when you move from a physical server to the cloud (also known as lift and shift).
```bash
az vm create \
  --resource-group [sandbox resource group name] \
  --name my-vm \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```
```bash
az vm extension set \
  --resource-group learn-6e73edbd-07dd-422e-ba95-281e77f498ff \
  --vm-name my-vm \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
  --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
```
```bash
#!/bin/bash

# Update apt cache.
sudo apt-get update

# Install Nginx.
sudo apt-get install -y nginx

# Set the home page.
echo "<html><body><h2>Welcome to Azure! My name is $(hostname).</h2></body></html>" | sudo tee -a /var/www/html/index.html
```
```bash
IPADDRESS="$(az vm list-ip-addresses \
  --resource-group learn-6e73edbd-07dd-422e-ba95-281e77f498ff \
  --name my-vm \
  --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
  --output tsv)"
```
```bash
curl --connect-timeout 5 http://$IPADDRES
```
```bash
az network nsg list \
  --resource-group learn-6e73edbd-07dd-422e-ba95-281e77f498ff \
  --query '[].name' \
  --output tsv
```
```bash
az network nsg rule create \
  --resource-group learn-6e73edbd-07dd-422e-ba95-281e77f498ff \
  --nsg-name my-vmNSG \
  --name allow-http \
  --protocol tcp \
  --priority 100 \
  --destination-port-range 80 \
  --access Allow
```
```bash
az network nsg rule list \
  --resource-group learn-6e73edbd-07dd-422e-ba95-281e77f498ff \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table
```
---

1. Azure Virtual Desktop is a desktop and application virtualization service that runs on the cloud. It enables you to use a cloud-hosted version of Windows from any location.Azure Virtual Desktop lets you use Windows 10 or Windows 11 Enterprise multi-session, the only Windows client-based operating system that enables multiple concurrent users on a single VM.
Containers are a virtualization environment. Much like running multiple virtual machines on a single physical host, you can run multiple containers on a single physical or virtual host.
2. Azure Container Instances offer the fastest and simplest way to run a container in Azure.
Containers are often used to create solutions by using a microservice architecture. This architecture is where you break solutions into smaller, independent pieces. For example, you might split a website into a container hosting your front end, another hosting your back end, and a third for storage. This split allows you to separate portions of your app into logical sections that can be maintained, scaled, or updated independently.
3. Azure Functions is an event-driven, serverless compute option that doesn’t require maintaining virtual machines or containers.With Azure Functions, an event wakes the function, alleviating the need to keep resources provisioned when there are no events.Functions scale automatically based on demand, so they may be a good choice when demand is variable.In this model, you're only charged for the CPU time used while your function runs.
Functions can be either stateless or stateful. When they're stateless (the default), they behave as if they're restarted every time they respond to an event. When they're stateful (called Durable Functions), a context is passed through the function to track prior activity.

VMs give you maximum control of the hosting environment and allow you to configure it exactly how you want. VMs also may be the most familiar hosting method if you’re new to the cloud. Containers, with the ability to isolate and individually manage different aspects of the hosting solution, can also be a robust and compelling option.

4. Azure App Service is a robust hosting option that you can use to host your apps in Azure. Azure App Service lets you focus on building and maintaining your app, and Azure focuses on keeping the environment up and running.App Service enables you to build and host web apps, background jobs, mobile back-ends, and RESTful APIs in the programming language of your choice without managing infrastructure. It offers automatic scaling and high availability. App Service supports Windows and Linux. It enables automated deployments from GitHub, Azure DevOps, or any Git repo to support a continuous deployment model.

Azure virtual networks provide the following key networking capabilities:
Isolation and segmentation
Internet communications
Communicate between Azure resources
Communicate with on-premises resources
Route network traffic : Route tables allow you to define rules about how traffic should be directed.
Filter network traffic : Network security groups are Azure resources that can contain multiple inbound and outbound security rules.
Connect virtual networks

Azure ExpressRoute provides a dedicated private connectivity to Azure that doesn't travel over the internet. ExpressRoute is useful for environments where you need greater bandwidth and even higher levels of security.
Site-to-site virtual private networks
Point-to-site virtual private network
A network virtual appliance carries out a particular network function, such as running a firewall or performing wide area network (WAN) optimization.
You can link virtual networks together by using virtual network peering. Peering allows two virtual networks to connect directly to each other.
User-defined routes (UDR) allow you to control the routing tables between subnets within a virtual network or between virtual networks. This allows for greater control over network traffic flow.

SSH (Secure Shell) is a protocol that's used on Linux to allow administrators to access the system remotely.

A virtual private network (VPN) uses an encrypted tunnel within another network. VPNs are typically deployed to connect two or more trusted private networks to one another over an untrusted network (typically the public internet). 
A VPN gateway is a type of virtual network gateway. Azure VPN Gateway instances are deployed in a dedicated subnet of the virtual network and enable the following connectivity:
Connect on-premises datacenters to virtual networks through a site-to-site connection.
Connect individual devices to virtual networks through a point-to-site connection.
Connect virtual networks to other virtual networks through a network-to-network connection.

VPN type: either policy-based or route-based.
Use a route-based VPN gateway if you need any of the following types of connectivity:
Connections between virtual networks
Point-to-site connections
Multisite connections
Coexistence with an Azure ExpressRoute gateway

In regions that support availability zones, VPN gateways and ExpressRoute gateways can be deployed in a zone-redundant configuration. This configuration brings resiliency, scalability, and higher availability to virtual network gateways.

Azure ExpressRoute lets you extend your on-premises networks into the Microsoft cloud over a private connection, with the help of a connectivity provider.This connection is called an ExpressRoute Circuit.There are several benefits to using ExpressRoute as the connection service between Azure and on-premises networks.
Connectivity to Microsoft cloud services across all regions in the geopolitical region.
Global connectivity to Microsoft services across all regions with the ExpressRoute Global Reach.
Dynamic routing between your network and Microsoft via Border Gateway Protocol (BGP).
Built-in redundancy in every peering location for higher reliability.
ExpressRoute supports four models :
CloudExchange colocation
Point-to-point Ethernet connection
Any-to-any connection
Directly from ExpressRoute sites : ExpressRoute Direct provides dual 100 Gbps or 10-Gbps connectivity, which supports Active/Active connectivity at scale.

Azure DNS is a hosting service for DNS domains that provides name resolution by using Microsoft Azure infrastructure.Azure DNS can manage DNS records for your Azure services and provide DNS for your external resources as well. You can't use Azure DNS to buy a domain name. Azure DNS also supports alias record sets. You can use an alias record set to refer to an Azure resource, such as an Azure public IP address, an Azure Traffic Manager profile, or an Azure Content Delivery Network (CDN) endpoint. If the IP address of the underlying resource changes, the alias record set seamlessly updates itself during DNS resolution. The alias record set points to the service instance, and the service instance is associated with an IP address.

<Azure storage services>

Data in an Azure Storage account is always replicated three times in the primary region.
Azure Storage offers two options for how your data is replicated in the primary region, locally redundant storage (LRS) and zone-redundant storage (ZRS).
Azure Storage offers two options for copying your data to a secondary region: geo-redundant storage (GRS) and geo-zone-redundant storage (GZRS). GRS is similar to running LRS in two regions, and GZRS is similar to running ZRS in the primary region and LRS in the secondary region.

List of redundancy options :
Locally redundant storage (LRS) : replicates your data three times within a single data center in the primary region.LRS provides at least 11 nines of durability (99.999999999%) of objects over a given year.
Geo-redundant storage (GRS) : GRS copies your data synchronously three times within a single physical location in the primary region using LRS. It then copies your data asynchronously to a single physical location in the secondary region (the region pair) using LRS. GRS offers durability for Azure Storage data objects of at least 16 nines (99.99999999999999%) over a given year.
Read-access geo-redundant storage (RA-GRS)
Zone-redundant storage (ZRS) : replicates your Azure Storage data synchronously across three Azure availability zones in the primary region. ZRS offers durability for Azure Storage data objects of at least 12 nines (99.9999999999%) over a given year.Microsoft recommends using ZRS in the primary region for scenarios that require high availability. ZRS is also recommended for restricting replication of data within a country or region to meet data governance requirements.
Geo-zone-redundant storage (GZRS) : Data in a GZRS storage account is copied across three Azure availability zones in the primary region (similar to ZRS) and is also replicated to a secondary geographic region, using LRS, for protection from regional disasters. Microsoft recommends using GZRS for applications requiring maximum consistency, durability, and availability, excellent performance, and resilience for disaster recovery. 16 nines (99.99999999999999%)
Read-access geo-zone-redundant storage (RA-GZRS) 

For read access to the secondary region, enable read-access geo-redundant storage (RA-GRS) or read-access geo-zone-redundant storage (RA-GZRS).
The data is available to be read only if the customer or Microsoft initiates a failover from the primary to secondary region. However, if you enable read access to the secondary region, your data is always available, even when the primary region is running optimally.

following table shows the endpoint format for Azure Storage services.
Storage service 	Endpoint
Blob Storage 		https://<storage-account-name>.blob.core.windows.net
Data Lake Storage Gen2 	https://<storage-account-name>.dfs.core.windows.net
Azure Files 		https://<storage-account-name>.file.core.windows.net
Queue Storage 		https://<storage-account-name>.queue.core.windows.net
Table Storage 		https://<storage-account-name>.table.core.windows.net

The factors that help determine which redundancy option you should choose include:
How your data is replicated in the primary region.
Whether your data is replicated to a second region that is geographically distant to the primary region, to protect against regional disasters.
Whether your application requires read access to the replicated data in the secondary region if the primary region becomes unavailable.

The RPO (recovery point objective) indicates the point in time to which data can be recovered. Azure Storage typically has an RPO of less than 15 minutes, although there's currently no SLA on how long it takes to replicate data to the secondary region.

Azure Storage platform includes the following data services:
Azure Blobs: A massively scalable object store for text and binary data. Also includes support for big data analytics through Data Lake Storage Gen2.
Azure Files: Managed file shares for cloud or on-premises deployments.
Azure Queues: A messaging store for reliable messaging between application components.
Azure Disks: Block-level storage volumes for Azure VMs.

Azure portal and Azure Storage Explorer offer easy visual solutions for working with your data.
Azure Blob Storage is an object storage solution for the cloud.Blob Storage is unstructured, meaning that there are no restrictions on the kinds of data it can hold.

Azure provides several access tiers, which you can use to balance your storage costs with your access needs.
Hot access tier: Optimized for storing data that is accessed frequently (for example, images for your website).
Cool access tier: Optimized for data that is infrequently accessed and stored for at least 30 days (for example, invoices for your customers).
Archive access tier: Appropriate for data that is rarely accessed and stored for at least 180 days, with flexible latency requirements (for example, long-term backups).
Azure Files offers fully managed file shares in the cloud that are accessible via the industry standard Server Message Block (SMB) or Network File System (NFS) protocols. Azure Files file shares can be mounted concurrently by cloud or on-premises deployments.
Azure Queue Storage is a service for storing large numbers of messages.Queue storage can be combined with compute functions like Azure Functions to take an action when a message is received.
Disk storage, or Azure managed disks, are block-level storage volumes managed by Azure for use with Azure VMs. 

Azure Migrate is a service that helps you migrate from an on-premises environment to the cloud. Azure Migrate hub also includes the following tools to help with migration:
Azure Migrate: Discovery and assessment. Discover and assess on-premises servers running on VMware, Hyper-V, and physical servers in preparation for migration to Azure.
Azure Migrate: Server Migration. Migrate VMware VMs, Hyper-V VMs, physical servers, other virtualized servers, and public cloud VMs to Azure.
Data Migration Assistant. Data Migration Assistant is a stand-alone tool to assess SQL Servers. It helps pinpoint potential problems blocking migration. It identifies unsupported features, new features that can benefit you after migration, and the right path for database migration.
Azure Database Migration Service. Migrate on-premises databases to Azure VMs running SQL Server, Azure SQL Database, or SQL Managed Instances.
Web app migration assistant. Azure App Service Migration Assistant is a standalone tool to assess on-premises websites for migration to Azure App Service. Use Migration Assistant to migrate .NET and PHP web apps to Azure.
Azure Data Box. Use Azure Data Box products to move large amounts of offline data to Azure.maximum usable storage capacity of 80 terabytes. Data Box can be used to export data from Azure.Once the data from your import order is uploaded to Azure, the disks on the device are wiped clean in accordance with NIST 800-88r1 standards. For an export order, the disks are erased once the device reaches the Azure datacenter.

AzCopy is a command-line utility that you can use to copy blobs or files to or from your storage account.
Azure Storage Explorer is a standalone app that provides a graphical interface to manage files and blobs in your Azure Storage Account.
Azure File Sync is a tool that lets you centralize your file shares in Azure Files and keep the flexibility, performance, and compatibility of a Windows file server.

<Azure identity, access, and security>

Azure Active Directory (Azure AD) is a directory service that enables you to sign in and access both Microsoft cloud applications and cloud applications that you develop. Azure AD can also help you maintain your on-premises Active Directory deployment.Azure AD is Microsoft's cloud-based identity and access management service.
When you secure identities on-premises with Active Directory, Microsoft doesn't monitor sign-in attempts. When you connect Active Directory with Azure AD, Microsoft can help protect you by detecting suspicious sign-in attempts at no extra cost. For example, Azure AD can detect sign-in attempts from unexpected locations or unknown devices.
Azure AD provides services such as: Authentication, Single sign-on, Application management, Device management- Registration enables devices to be managed through tools like Microsoft Intune.

Azure Active Directory Domain Services (Azure AD DS) is a service that provides managed domain services such as domain join, group policy, lightweight directory access protocol (LDAP), and Kerberos/NTLM authentication.
A managed domain is configured to perform a one-way synchronization from Azure AD to Azure AD DS.In a hybrid environment with an on-premises AD DS environment, Azure AD Connect synchronizes identity information with Azure AD, which is then synchronized to the managed domain.

Single sign-on (SSO) enables a user to sign in one time and use that credential to access multiple resources and applications from different providers.

Passwordless authentication methods are more convenient because the password is removed and replaced with something you have, plus something you are, or something you know.Three passwordless authentication options that integrate with Azure Active Directory (Azure AD):
Windows Hello for Business :  biometric and PIN credentials are directly tied to the user's PC
Microsoft Authenticator app : App turns any iOS or Android phone into a strong, passwordless credential. Users can sign-in to any platform or browser by getting a notification to their phone, matching a number displayed on the screen to the one on their phone, and then using their biometric (touch or face) or PIN to confirm.
FIDO2 security keys : (Fast IDentity Online) FIDO allows users and organizations to leverage the standard to sign-in to their resources without a username or password by using an external security key or a platform key built into a device.These FIDO2 security keys are typically USB devices, but could also use Bluetooth or NFC.

Azure AD External Identities refers to all the ways you can securely interact with users outside of your organization. Business to business (B2B) collaboration, B2B direct connect , Azure AD business to customer (B2C) 
Conditional Access is a tool that Azure Active Directory uses to allow (or deny) access to resources based on identity signals. These signals include who the user is, where the user is, and what device the user is requesting access from.

Azure role-based access control (Azure RBAC) provides built-in roles that describe common access rules for cloud resources.Role-based access control is applied to a scope, which is a resource or set of resources that this access applies to. Azure RBAC is hierarchical, in that when you grant access at a parent scope, those permissions are inherited by all child scopes. 
Resource Manager is a management service that provides a way to organize and secure your cloud resources.
Zero Trust is a security model that assumes the worst case scenario and protects resources with that expectation.Limit user access with Just-In-Time and Just-Enough-Access (JIT/JEA)

A defense-in-depth strategy uses a series of mechanisms to slow the advance of an attack that aims at acquiring unauthorized access to data.Each layer provides protection so that if one layer is breached, a subsequent layer is already in place to prevent further exposure. This approach removes reliance on any single layer of protection. It slows down an attack and provides alert information that security teams can act upon, either automatically or manually.
Here's a brief overview of the role of each layer:
The physical security layer is the first line of defense to protect computing hardware in the datacenter.
The identity and access layer controls access to infrastructure and change control.
The perimeter layer uses distributed denial of service (DDoS) protection to filter large-scale attacks before they can cause a denial of service for users.
The network layer limits communication between resources through segmentation and access controls.
The compute layer secures access to virtual machines.
The application layer helps ensure that applications are secure and free of security vulnerabilities.
The data layer controls access to business and customer data that you need to protect.

Defender for Cloud is a monitoring tool for security posture management and threat protection. It monitors your cloud, on-premises, hybrid, and multi-cloud environments to provide guidance and notifications aimed at strengthening your security posture. Defender for Cloud provides the tools needed to harden your resources, track your security posture, protect against cyber attacks, and streamline security management. When necessary, Defender for Cloud can automatically deploy a Log Analytics agent to gather security-related data. Cloud security posture management (CSPM) features are extended to multi-cloud machines without the need for any agents. To extend protection to on-premises machines, deploy Azure Arc and enable Defender for Cloud's enhanced security features.
Defender for Cloud fills three vital needs as you manage the security of your resources and workloads in the cloud and on-premises:
Continuously assess – Know your security posture. Identify and track vulnerabilities.
Secure – Harden resources and services with Azure Security Benchmark.
Defend – Detect and resolve threats to resources, workloads, and services.

Defender for Cloud's threat protection includes fusion kill-chain analysis, which automatically correlates alerts in your environment based on cyber kill-chain analysis, to help you better understand the full story of an attack campaign, where it started, and what kind of impact it had on your resources.

<Core architectural components of Azure>

The Azure free account includes:
Free access to popular Azure products for 12 months.
A credit to use for the first 30 days.
Access to more than 25 products that are always free.
Commands>>
pwsh
bash
az interactive

The core architectural components of Azure may be broken down into two main groupings: the physical infrastructure, and the management infrastructure.
The physical infrastructure for Azure starts with datacenters. Datacenters are grouped into Azure Regions or Azure Availability Zones that are designed to help you achieve resiliency and reliability for your business-critical workloads.
https://infrastructuremap.microsoft.com/
global Azure services such as Azure Active Directory, Azure Traffic Manager, and Azure DNS.
Availability zones are physically separate datacenters within an Azure region.Availability zones are connected through high-speed, private fiber-optic networks.
You can use availability zones to run mission-critical applications and build high-availability into your application architecture by co-locating your compute, storage, networking, and data resources within an availability zone and replicating in other availability zones. Keep in mind that there could be a cost to duplicating your services and transferring data between availability zones.
Most Azure regions are paired with another region within the same geography (such as US, Europe, or Asia) at least 300 miles away.
may need to use a sovereign region for compliance or legal purposes.

The management infrastructure includes Azure resources and resource groups, subscriptions, and accounts. Resource groups can't be nested, meaning you can’t put resource group B inside of resource group A. Resource groups provide a convenient way to group resources together.
Azure, subscriptions are a unit of management, billing, and scale.There are two types of subscription boundaries that you can use: Billing boundary & Access control boundary.

Resources are gathered into resource groups, and resource groups are gathered into subscriptions.
You organize subscriptions into containers called management groups and apply governance conditions to the management groups. All subscriptions within a management group automatically inherit the conditions applied to the management group, the same way that resource groups inherit settings from subscriptions and resources inherit from resource groups.Management groups can be nested.
Important facts about management groups:
10,000 management groups can be supported in a single directory.
A management group tree can support up to six levels of depth. This limit doesn't include the root level or the subscription level.
Each management group and subscription can support only one parent.

< Describe features and tools in Azure for governance and compliance >

Azure Blueprints lets you standardize cloud subscription or environment deployments.with Azure Blueprints you can define repeatable settings and policies that are applied as new subscriptions are created.
Each component in the blueprint definition is known as an artifact. It is possible for artifacts to have no additional parameters (configurations). An example is the Deploy threat detection on SQL servers policy, which requires no additional configuration.You can specify a parameter's value when you create the blueprint definition or when you assign the blueprint definition to a scope.

Azure Blueprints deploy a new environment based on all of the requirements, settings, and configurations of the associated artifacts. Artifacts can include things such as:
Role assignments
Policy assignments
Azure Resource Manager templates
Resource groups

Azure Blueprints are version-able, With Azure Blueprints, the relationship between the blueprint definition (what should be deployed) and the blueprint assignment (what was deployed) is preserved.

Azure Policy is a service in Azure that enables you to create, assign, and manage policies that control or audit your resources. These policies enforce different rules across your resource configurations so that those configurations stay compliant with corporate standards.
Azure Policies are inherited, so if you set a policy at a high level, it will automatically be applied to all of the groupings that fall within the parent.
Azure Policy comes with built-in policy and initiative definitions for Storage, Networking, Compute, Security Center, and Monitoring.
If you have a specific resource that you don’t want Azure Policy to automatically fix, you can flag that resource as an exception – and the policy won’t automatically fix that resource. Azure Policy also integrates with Azure DevOps

An Azure Policy initiative is a way of grouping related policies together.

A resource lock prevents resources from being accidentally deleted or changed.Resource locks can be applied to individual resources, resource groups, or even an entire subscription. Resource locks are inherited. 
two types of resource locks
Delete means authorized users can still read and modify a resource, but they can't delete the resource.
ReadOnly means authorized users can read a resource, but they can't delete or update the resource. Applying this lock is similar to restricting all authorized users to the permissions granted by the Reader role.

The Microsoft Service Trust Portal is a portal that provides access to various content, tools, and other resources about Microsoft security, privacy, and compliance practices.

< features and tools for managing and deploying Azure resources > 

Azure PowerShell is a shell with which developers, DevOps, and IT professionals can run commands called command-lets (cmdlets). These commands call the Azure REST API to perform management tasks in Azure.
The Azure CLI is functionally equivalent to Azure PowerShell, with the primary difference being the syntax of commands. While Azure PowerShell uses PowerShell commands, the Azure CLI uses Bash commands.

Azure Arc provides a centralized, unified way to:
Manage your entire environment together by projecting your existing non-Azure resources into ARM.
Manage multi-cloud and hybrid virtual machines, Kubernetes clusters, and databases as if they are running in Azure.
Use familiar Azure services and management capabilities, regardless of where they live.
Continue using traditional ITOps while introducing DevOps practices to support new cloud and native patterns in your environment.
Configure custom locations as an abstraction layer on top of Azure Arc-enabled Kubernetes clusters and cluster extensions.

Azure Arc allows you to manage the following resource types hosted outside of Azure: 
Servers
Kubernetes clusters
Azure data services
SQL Server
Virtual machines (preview)

Azure Resource Manager (ARM) is the deployment and management service for Azure. It provides a management layer that enables you to create, update, and delete resources in your Azure account.
Infrastructure as code is a concept where you manage your infrastructure as lines of code. By using ARM templates, you can describe the resources you want to use in a declarative JSON format. With an ARM template, the deployment code is verified before any code is run. This ensures that the resources will be created and connected correctly. The template then orchestrates the creation of those resources in parallel. Needs only to define the desired state and configuration of each resource in the ARM template, and the template does the rest. Templates can even execute PowerShell and Bash scripts before or after the resource has been set up.

When possible, Azure Resource Manager deploys resources in parallel, so your deployments finish faster than serial deployments.

Azure Blueprints applies policies in an automated fashion and ARM Templates allow you to deploy your resource as code. Using the two together helps ensure that you’re deploying consistent, compliant resources.

< monitoring tools in Azure > 

Azure Advisor evaluates your Azure resources and makes recommendations to help improve reliability, security, and performance, achieve operational excellence, and reduce costs. Azure Advisor is designed to help you save time on cloud optimization. The recommendation service includes suggested actions you can take right away, postpone, or dismiss.The five recommendation categories for Azure Advisor are: Reliability, Security, Performance, Operational Excellence, and Cost.

The recommendations are divided into five categories:
Reliability is used to ensure and improve the continuity of your business-critical applications.
Security is used to detect threats and vulnerabilities that might lead to security breaches.
Performance is used to improve the speed of your applications.
Operational Excellence is used to help you achieve process and workflow efficiency, resource manageability, and deployment best practices.
Cost is used to optimize and reduce your overall Azure spending.

Azure Service Health helps you keep track of Azure resource, both your specifically deployed resources and the overall status of Azure. Azure service health does this by combining three different Azure services:
Azure Status is a broad picture of the status of Azure globally. The page is a global view of the health of all Azure services across all Azure regions.
Service Health provides a narrower view of Azure services and regions.
Resource Health is a tailored view of your actual Azure resources, provides information about the health of your individual cloud resources.

Azure Monitor is a platform for collecting data on your resources, analyzing that data, visualizing the information, and even acting on the results. Azure Monitor can monitor Azure resources, your on-premises resources, and even multi-cloud resources like virtual machines hosted with a different cloud provider. You can view high-level reports on the Azure Monitor Dashboard or create custom views by using Power BI and Kusto queries.

Azure Log Analytics is the tool in the Azure portal where you’ll write and run log queries on the data gathered by Azure Monitor. Log Analytics is a robust tool that supports both simple, complex queries, and data analysis.

Azure Monitor Alerts are an automated way to stay informed when Azure Monitor detects a threshold being crossed. You set the alert conditions, the notification actions, and then Azure Monitor Alerts notifies when an alert is triggered. Depending on your configuration, Azure Monitor Alerts can also attempt corrective action.
Azure Monitor Alerts use action groups to configure who to notify and what action to take. An action group is simply a collection of notification and action preferences that you associate with one or multiple alerts. Azure Monitor, Service Health, and Azure Advisor all use actions groups to notify you when an alert has been triggered.

Application Insights, an Azure Monitor feature, monitors your web applications. Is capable of monitoring applications that are running in Azure, on-premises, or in a different cloud environment. You can either install an SDK in your application, or you can use the Application Insights agent.Application Insights help you monitor the performance of your application. 

< cost management in Azure >

Operational expense (OpEx) cost can be impacted by many factors. Some of the impacting factors are:
Resource type
Consumption
Maintenance
Geography
Subscription type
Azure Marketplace

Pay-as-you-go has been a consistent theme throughout, and that’s the cloud payment model where you pay for the resources that you use during a billing cycle.
Azure also offers the ability to commit to using a set amount of cloud resources in advance and receiving discounts on those “reserved” resources, in some cases up to 72 percent.
Network traffic is also impacted based on geography. For example, it’s less expensive to move information within Europe than to move information from Europe to Asia or South America. 
Bandwidth refers to data moving in and out of Azure datacenters. Some inbound data transfers (data going into Azure datacenters) are free. For outbound data transfers (data leaving Azure datacenters), data transfer pricing is based on zones.
Azure Marketplace lets you purchase Azure-based solutions and services from third-party vendors. When you purchase products through Azure Marketplace, you may pay for not only the Azure services that you’re using, but also the services or expertise of the third-party vendor. Billing structures are set by the vendor.

The pricing calculator is designed to give you an estimated cost for provisioning resources in Azure. 
Total cost of ownership (TCO) calculator is designed to help you compare the costs for running an on-premises infrastructure compared to an Azure Cloud infrastructure. With the TCO calculator, you enter your configuration, add in assumptions like power and IT labor costs, and are presented with an estimation of the cost difference to run the same environment in your current datacenter or in Azure.

Cost Management provides the ability to quickly check Azure resource costs, create alerts based on resource spend, and create budgets that can be used to automate management of resources.

Cost analysis is a subset of Cost Management that provides a quick visual for your Azure costs. Using cost analysis, you can quickly view the total cost in a variety of different ways, including by billing cycle, region, resource, and so on.

Cost alerts provide a single location to quickly check on all of the different alert types that may show up in the Cost Management service. The three types of alerts that may show up are:
Budget alerts : Budget alerts support both cost-based and usage-based budgets.Cost Management budgets are created using the Azure portal or the Azure Consumption API.
Credit alerts : notify you when your Azure credit monetary commitments are consumed. Credit alerts are generated automatically at 90% and at 100% of your Azure credit balance.
Department spending quota alerts : notify you when department spending reaches a fixed threshold of the quota.

A budget is where you set a spending limit for Azure. A more advanced use of budgets enables budget conditions to trigger automation that suspends or otherwise modifies resources once the trigger condition has occurred.

A resource tag consists of a name and a value. You can assign one or more tags to each Azure resource. Resource tags are another way to organize resources. Tags provide extra information, or metadata, about your resources. Can use Azure Policy to enforce tagging rules and conventions.
Tags allow you to associate metadata with a resource to help keep track of resource management, costs and optimization, security, and so on.
An SLA is an uptime or performance guarantee between you and your users.

END--------------------------------------------------------------------------------------------------------------------------------------------------------------------

< Microsoft Azure Fundamentals Certification Course (AZ-900) - Pass the exam in 3 hours! >




Thank you for your email. I’m out of the office and will be back at (Date of Return). During this period I will have limited access to my email.
Thank you for your message. I am currently out of the office, with no email access. I will be returning on 16 December.
If you need immediate assistance before then, you may reach me at my mobile – 9004719919.

Kind Regards,

Hi All,

Greetings for the day,

As I have informed earlier, I am taking planned leaves from 27/November to 4/December, I will be back to work on 05/December/2022   


