# [Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)

## [Azure icons to use in architecture diagrams and documentation](https://learn.microsoft.com/en-us/azure/architecture/icons/)

### [Architecture styles](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/)

### [Browse Azure Architectures](https://learn.microsoft.com/en-us/azure/architecture/browse/)

---

### Virtual Network gateway routing

> Microsoft does not recommend using VPN or ExpressRoute gateways to enable communication between spokes.

#### [Configure a VNet-to-VNet VPN gateway connection](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-cli)

1. [Connect VNets that are in the same subscription](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-cli#samesub)


### [Virtual network connectivity options and spoke-to-spoke communication](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/virtual-network-peering?WT.mc_id=AZ-MVP-5004159#pattern-2-spokes-communicating-over-a-network-appliance)

1. Virtual network connection types
   - **Virtual network peering** connects two Azure virtual networks (500 peering limit).
   - **Subnet peering** connects specific subnets between virtual networks instead of peering entire virtual networks. (400 subnet limit per peering connection)
   - **VPN gateways** are specific types of virtual network gateways that send traffic between an Azure virtual network and a cross-premises location over the public internet.
2. Use **Gateway transit** to share an Azure ExpressRoute or VPN gateway with all peered virtual networks and manage connectivity in one place. This method saves money and simplifies management.
3. Virtual network peering and VPN gateways support the following connection types:
   - Virtual networks in different regions
   - Virtual networks in different Microsoft Entra tenants
   - Virtual networks in different Azure subscriptions
4. [Comparison of virtual network peering and VPN gateway](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/virtual-network-peering?WT.mc_id=AZ-MVP-5004159#comparison-of-virtual-network-peering-and-vpn-gateway)
5. [Spoke-to-spoke communication patterns](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/virtual-network-peering?WT.mc_id=AZ-MVP-5004159#spoke-to-spoke-communication-patterns)
   - **Inter-spoke** networking refers to direct communication between workloads. Inter-spoke networking provides architectural flexibility to optimize for performance and cost where it makes business sense, while maintaining centralized control for security and governance needs. Inter-spoke networking provides the following benefits:
     - **Better performanc**: Direct connections eliminate extra hops and bottlenecks.
     - **Lower costs**: Fewer peering connections and reduced hub infrastructure requirements.
     - **Easier management**: Less complex routing and fewer components to monitor.
     - **Regional flexibility**: Support for both single-region and cross-region communication patterns.
   - **Hub-and-spoke** architectures provide centralized control and security, but they can become performance bottlenecks and cost centers when all workload-to-workload traffic must traverse the hub.
   - **Virtual network peering** provides the highest performance option for direct spoke-to-spoke connectivity. Peer virtual networks across Azure regions, which is known as **global peering**.
     - Use virtual network peering for scenarios such as cross-region data replication and database failover, specifically where your data policies don't require inspection. This approach is often used between network-isolated components within a single workload. Because traffic stays private and travels only on the Microsoft backbone, virtual network peering supports strict data policies and avoids sending traffic over the public internet.
   - Use [network security groups](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview) to control traffic flow between peered networks.
6. [Virtual Network Manager](https://learn.microsoft.com/en-us/azure/virtual-network-manager/concept-connectivity-configuration#hub-and-spoke-topology) provides automated management for virtual network connectivity at scale.
   - Hub and spoke with spokes that don't connect to each other
   - Hub and spoke with spokes that directly connect to each other, without a hop in the middle
   - A meshed group of virtual networks that connect to each other
   ![Use Virtual Network Manager to build three types of topologies across subscriptions](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/images/virtual-network-manager-connectivity-options.svg)
   - Use Virtual Network Manager to statically or dynamically assign spoke virtual networks to specific [network groups](https://learn.microsoft.com/en-us/azure/virtual-network-manager/concept-network-groups). This assignment automatically creates virtual network connectivity.
7. Tune the route tables in each spoke to send private address traffic, such as traffic that uses RFC 1918 prefixes like `10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`, to an NVA. This appliance handles Azure-to-Azure and Azure-to-on-premises traffic, often known as **east-west traffic**.
8. Tune internet traffic, which has a `0.0.0.0/0` route, to a second NVA. This NVA manages Azure-to-internet traffic, also known as **north-south traffic**.
