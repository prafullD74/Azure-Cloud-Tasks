# [AZ-700 Design and Implement Microsoft Azure Network Solutions](https://learn.microsoft.com/en-us/training/paths/design-implement-microsoft-azure-networking-solutions-az-700/)


# [Azure Virtual Networks](https://learn.microsoft.com/en-us/training/modules/introduction-to-azure-virtual-networks/)

1. [Azure Virtual Networks (VNets)](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) are the fundamental building block of your private network in Azure.
2. All resources in a VNet can communicate outbound to the internet, by default. 
3. Communication between Azure resources: There are three key mechanisms through which Azure resource can communicate: VNets, VNet service endpoints, and VNet peering. 
4. Communication between on-premises resources using Point-to-site virtual private network (VPN), Site-to-site VPN, Azure ExpressRoute.
5. Filter network traffic between subnets using any combination of network security groups and network virtual appliances.
6. Azure routes traffic between subnets, connected virtual networks, on-premises networks, and the Internet, by default. You can implement route tables or border gateway protocol (BGP) routes to override the default routes Azure creates.

## Design considerations for Azure Virtual Networks
1. Virtual Networks
   - VNet use address ranges enumerated in RFC 1918.

   | Private Range                 | CIDR           | Size            |
   | ----------------------------- | -------------- | --------------- |
   | 10.0.0.0 – 10.255.255.255     | 10.0.0.0/8     | Large networks  |
   | 172.16.0.0 – 172.31.255.255   | 172.16.0.0/12  | Medium networks |
   | 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | Small networks  |
   - **Nonroutable address spaces** are private IP ranges defined in RFC 1918 that cannot be routed over the public internet. They are used for internal communication inside VNets, on-prem networks, and hybrid environments. External communication requires NAT or public IP translation.
   - Address ranges are reserved.
     - 224.0.0.0/4 (Multicast)
     - 255.255.255.255/32 (Broadcast)
     - 127.0.0.0/8 (Loopback)
     - 169.254.0.0/16 (Link-local)
     - 168.63.129.16/32 (Internal DNS)
   - When planning to implement virtual networks, you need to consider: [Considerations for virtual networks](https://learn.microsoft.com/en-us/training/modules/introduction-to-azure-virtual-networks/2-explore-azure-virtual-networks)
2. Subnets
   -  A subnet (subnetwork) is a logical subdivision of a larger network. A subnet divides a network (like a VNet) into smaller, manageable sections so resources can be organized, secured, and controlled properly.
   - All Azure resources in a virtual network are deployed into subnets within the virtual network.
   - Why We Use Subnets? For Network segmentation, Security isolation, Traffic control and Better IP management.
   - The smallest supported IPv4 subnet is /29, and the largest is /2 (using CIDR subnet definitions). IPv6 subnets must be exactly /64 in size, specified in **Classless Inter-Domain Routing (CIDR)** format.
   - Certain Azure services require their own subnet (Azure Kubernetes Service). Some Azure services like Azure Firewall, VPN Gateway, ExpressRoute Gateway, Bastion, Application Gateway, and Route Server require dedicated subnets because Azure enforces isolation and specific routing/security configurations for those services.
3. Public IP
   - Public IP addresses enable Internet resources to communicate with Azure resources and enable Azure resources to communicate outbound with Internet and public-facing Azure services. 
   - A public IP address in Azure is dedicated to a specific resource. A resource without a public IP assigned can communicate outbound through network address translation services, where Azure dynamically assigns an available IP address that isn't dedicated to the resource.
   - Dynamic and static public IP addresses
     - Resources you can associate a public IP address resource with: Virtual machine network interfaces, Virtual machine scale sets, Public Load Balancers, Virtual Network Gateways (VPN/ER), NAT gateways, Application Gateways, Azure Firewall, Bastion Host, Route Server.
     - A **dynamic public IP address** is an assigned address that can change over the lifespan of the Azure resource. In each Azure region, public IP addresses are assigned from a unique pool of addresses. The default allocation method is dynamic.
     - A **static public IP address** is an assigned address that is fixed over the lifespan of the Azure resource. To configure a static IP address, set the allocation method explicitly to static.
   - SKU for a public IP address: Standard & Basic. 
4. [M01 - Unit 4 Design and implement a Virtual Network in Azure](https://microsoftlearning.github.io/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/Instructions/Exercises/M01-Unit%204%20Design%20and%20implement%20a%20Virtual%20Network%20in%20Azure.html)
5. Name resolution for your virtual network
   - [Azure DNS]() is a cloud service that allows you to host and manage domain name system (DNS) domains, also known as DNS zones.
   - A DNS zone is a collection of DNS records. DNS records provide information about the domain.
   - Public DNS services
     - [Azure public DNS](https://learn.microsoft.com/en-us/azure/dns/public-dns-overview) is a hosting service for DNS domains that provides name resolution by using Microsoft Azure infrastructure. Azure public DNS is best for public and global solutions.
     - Azure DNS public zones host domain name zone data for records that you intend to be resolved by any host on the internet.
   - Delegate DNS Domains
     - Azure DNS allows you to host a DNS zone and manage the DNS records for a domain in Azure. In order for DNS queries for a domain to reach Azure DNS, the domain has to be delegated to Azure DNS from the parent domain. Keep in mind Azure DNS isn't the domain registrar.
     - To delegate your domain to Azure DNS, you first need to know the name server names for your zone. Each time a DNS zone is created Azure DNS allocates name servers from a pool. Once the Name Servers are assigned, Azure DNS automatically creates authoritative NS records in your zone.
     - Once the DNS zone is created, and you have the name servers, you need to update the parent domain. Each registrar has their own DNS management tools to change the name server records for a domain. In the registrar’s DNS management page, edit the NS records and replace the NS records with the ones Azure DNS created.
   - Child Domains
     - To set up a separate child zone, you can delegate a subdomain in Azure DNS. Setting up a subdomain follows the same process as typical delegation. The only difference is that NS records must be created in the parent zone contoso.com in Azure DNS, rather than in the domain registrar.
     - The parent and child zones can be in the same or different resource group.
   - Private DNS services
     - [Azure Private DNS](https://learn.microsoft.com/en-us/azure/dns/private-dns-overview) manages and resolves domain names in the virtual network without the need to configure a custom DNS solution.
     - Azure Private DNS zones allow you to configure a private DNS zone namespace for private Azure resources.
     - By using private DNS zones, you can use your own custom domain name instead of the Azure-provided names during deployment. It provides a naming resolution for virtual machines (VMs) within a virtual network and connected virtual networks.
     - Azure private DNS is best to manage domains for virtual machines or other resources within and across virtual networks.
   - Azure Private DNS Zones
     - Private DNS zones in Azure are available for internal resources only.
     - Private DNS zones are highly resilient, being replicated to regions all throughout the world. They aren't available to resources on the internet.
6. [M01 - Unit 6 Configure DNS settings in Azure](https://microsoftlearning.github.io/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/Instructions/Exercises/M01-Unit%206%20Configure%20DNS%20settings%20in%20Azure.html)
7. Cross-virtual network connectivity with peering
   - [Virtual network peering]() enables you to seamlessly connect separate VNets with optimal network performance, whether they are in the same Azure region (VNet peering) or in different regions (Global VNet peering).
   - Network traffic between peered virtual networks is private. The traffic between virtual machines in peered virtual networks uses the Microsoft backbone infrastructure, and no public Internet, gateways, or encryption is required in the communication between the virtual networks.
   - Virtual network peering enables you to seamlessly connect two Azure virtual networks.
     - **Regional VNet peering** connects Azure virtual networks in the same region.
     - **Global VNet peering** connects Azure virtual networks in different regions.
       ![Virtual network peering Diagram](https://learn.microsoft.com/en-in/training/wwl-azure/introduction-to-azure-virtual-networks/media/global-vnet-peering-2368962c.png)
     - Virtual network peering allows seamless communication between virtual networks across different Azure subscriptions, Microsoft Entra tenants, deployment models, and regions.
     -  virtual network peering enables you to seamlessly connect separate VNets with optimal network performance, whether they are in the same Azure region (VNet peering) or in different regions (Global VNet peering).
   - Gateway Transit and Connectivity
     - [Gateway Transit](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity) allows the virtual network to communicate to resources outside the peering.
     - Configure a VPN gateway in the peered virtual network as a [gateway transit](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-peering-gateway-transit) point. A peered virtual network uses the remote gateway to gain access to other resources. A virtual network can have only one gateway. Gateway transit is supported for both VNet Peering and Global VNet Peering.
       ![Diagram shows how gateway transit works with virtual network peering](https://learn.microsoft.com/en-us/azure/vpn-gateway/media/vpn-gateway-peering-gateway-transit/gatewaytransit.png)
     - Connectivity available on the VPN gateway, including S2S, P2S, and VNet-to-VNet connections, applies to all three virtual networks. The transit option can be used with all VPN Gateway SKUs except the Basic SKU.
     - Both virtual network peering and global virtual network peering support gateway transit.
       ![Diagram shows, the gateway should be either a local or remote gateway in the peered virtual network](https://learn.microsoft.com/en-us/azure/virtual-network/media/virtual-networks-peering-overview/local-or-remote-gateway-in-peered-virual-network.png)
8. [M01 - Unit 8 Connect two Azure Virtual Networks using global virtual network peering](https://microsoftlearning.github.io/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/Instructions/Exercises/M01-Unit%208%20Connect%20two%20Azure%20Virtual%20Networks%20using%20global%20virtual%20network%20peering.html)
   - Can resize the address space of Azure virtual networks that are peered without incurring any downtime on the currently peered address space.
9. Virtual network traffic routing
   - Azure automatically creates a route table for each subnet within an Azure virtual network. The route table has the default system routes and any user defined routes you require. Each route contains an address prefix and next hop type.
   - System routes
     - Azure automatically creates system routes and assigns the routes to each subnet in a virtual network. You can't create or remove system routes, but you can override some system routes with custom routes.
     - Azure automatically creates the following default system routes for each subnet within the virtual network.
       | Route Source   | Address Prefix (Destination CIDR)      | Next Hop Type    | Description                                                                       |
       | -------------- | -------------------------------------- | ---------------- | --------------------------------------------------------------------------------- |
       | System Default | VNet address space (e.g., 10.1.0.0/16) | Virtual network  | Enables intra-VNet communication between subnets (east-west traffic).             |
       | System Default | 0.0.0.0/0                              | Internet         | Default route for outbound internet traffic (north-south traffic).                |
       | System Default | 10.0.0.0/8                             | None (Blackhole) | Drops traffic to private RFC 1918 range not defined within the VNet.              |
       | System Default | 172.16.0.0/12                          | None (Blackhole) | Prevents unintended routing to private address space outside configured networks. |
       | System Default | 192.168.0.0/16                         | None (Blackhole) | Ensures isolation from unconfigured private networks.                             |
       | System Default | 100.64.0.0/10                          | None (Blackhole) | Blocks traffic to Carrier-Grade NAT (CGNAT) address space (RFC 6598).             |

     - Azure routes any traffic destined for its service directly to the service over the backbone network, rather than routing the traffic to the Internet. You can override Azure's default system route for the 0.0.0.0/0 address prefix with a custom route.
     - None: Traffic routed to the None next hop type is dropped, rather than routed outside the subnet.
     - Depending on the capability, Azure adds optional default routes to either specific subnets within the virtual network, or to all subnets within a virtual network.
       -  When you create a **virtual network peering** between two virtual networks, a route is added for each address range within the address space of each virtual network.
       - When you add a **virtual network gateway** to a virtual network, Azure adds one or more routes with Virtual network gateway as the next hop type.
       - Azure adds the public IP addresses for certain services to the route table when you enable a **service endpoint** to the service. Service endpoints are enabled for individual subnets within a virtual network, so the route is only added to the route table of a subnet a service endpoint is enabled for.
   - User defined routes (UDR)
     - [User-defined routes (UDR)](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview) useful when you want to ensure that **traffic between two subnets passes through a firewall appliance**. Internet is a valid next hop for a user defined custom route.
     - Specify the following next hop types when creating a user-defined route:
       - **Virtual appliance**: A virtual appliance is a virtual machine that typically runs a network application, such as a firewall. When you create a route with the virtual appliance hop type, you also specify a next hop IP address.
       - **Virtual network gateway**: Specify when you want traffic destined for specific address prefixes routed to a virtual network gateway. The virtual network gateway must be created with type VPN.
       - **None**: Specify when you want to drop traffic to an address prefix, rather than forwarding the traffic to a destination.
       - **Virtual network**: Specify when you want to override the default routing within a virtual network.
       - **Internet**: Specify when you want to explicitly route traffic destined to an address prefix to the Internet.
   - Azure Route Server
     - [Azure Route Server](https://learn.microsoft.com/en-us/azure/route-server/quickstart-configure-route-server-portal) simplifies dynamic routing between your network virtual appliance (NVA) and your virtual network.
     - Azure Route Server simplifies configuration, management, and deployment of your NVA in your virtual network.
       - No longer need to manually update the routing table on your NVA whenever your virtual network addresses are updated.
       - No longer need to update user defined routes manually whenever your NVA announces new routes or withdraws old ones.
   - Diagnose a routing problem by viewing the [effective routes](https://learn.microsoft.com/en-us/azure/virtual-network/diagnose-network-routing-problem#diagnose-using-azure-portal) for a virtual machine network interface.
10. Configure internet access with Azure Virtual NAT
    - [Azure Network Address Translation (NAT)](https://learn.microsoft.com/en-us/azure/nat-gateway/nat-overview) to let all instances in a subnet connect outbound to the internet while remaining fully private. Use a NAT service to map outgoing requests from internal resources to an external IP address. 
      ![Diagram shows outbound traffic flow from Subnet 1 through the NAT gateway to be mapped to a Public IP address or a Public IP prefix.](https://learn.microsoft.com/en-in/training/wwl-azure/introduction-to-azure-virtual-networks/media/nat-flow-map-e4870a4e.png)
    - NAT enables you to share a single public IPv4 address among multiple internal resources.(NAT enables internal resources to share an IP address for communication with Internet resources.)
    - NAT scales automatically to support dynamic workloads. NAT can support up to 16 public IP addresses. By using port network address translation (PNAT or PAT), NAT provides up to 64,000 concurrent flows for UDP and TCP. NAT is compatible with the following standard SKU resources: Load balancer, Public IP address and Public IP prefix.
    -  Only the IPv4 address family is supported. NAT doesn't interact with IPv6 address family. NAT can't span multiple virtual networks.
