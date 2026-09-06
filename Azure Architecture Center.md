# [Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)

## [Azure icons to use in architecture diagrams and documentation](https://learn.microsoft.com/en-us/azure/architecture/icons/)

### [Architecture styles](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/)

1. An architecture style is a family of architectures that share specific characteristics. 

2. [N-tier](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/n-tier)

   ![N-tier](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/n-tier-logical.svg)
   - N-tier is a traditional architecture for enterprise applications that divides an application into logical layers and physical tiers.
   - Traditional layered architecture for enterprise applications, suitable for existing applications with minimal changes needed for Azure migration.
3. [Web-Queue-Worker](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/web-queue-worker)

   ![Web-Queue-Worker](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/web-queue-worker-logical.svg)
   - Web-Queue-Worker is an architecture that consists of a web front end, a message queue, and a back-end worker.
   - The web front end handles HTTP requests and user interactions, while the worker performs resource-intensive tasks, long-running workflows, or batch operations. Communication between the front end and worker occurs through an asynchronous message queue.
   - This architecture is ideal for applications with relatively simple domains that have some resource-intensive processing requirements. It's easy to understand and deploy with managed Azure services like App Service and Azure Functions.
4. [Microservices](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices)

   ![Microservices](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/microservices-logical.svg)
   - The Microservices architecture decomposes applications into a collection of small, autonomous services.
   - Microservices enable teams to work autonomously and support frequent updates with higher release velocity. This architecture is well-suited for complex domains that require frequent changes and innovation.
5. [Event-driven architecture](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven)

   ![Event-driven architecture](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/event-driven.svg#lightbox)
   - Event-driven architectures use a [publish-subscribe model](https://learn.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber) where event producers generate streams of events, and event consumers respond to those events in near real time.
   - Uses a publish-subscribe model for real-time processing, suitable for IoT and high-volume data applications.
6. [Big data](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/big-data)

   ![Big data](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/big-data-logical.svg#lightbox)
   - Big data architectures handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems
   - Manages large datasets with batch and real-time processing capabilities, essential for analytics and machine learning.
7. [Big compute](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/big-compute)

   ![Big compute](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/big-compute-logical.png)
   - Big compute architectures support large-scale workloads that require hundreds or thousands of cores for computationally intensive operations.
   - The work can be split into discrete tasks that run across many cores simultaneously, with each task taking input, processing it, and producing output.
   - Big compute is essential for simulations, financial risk modeling, scientific computing, engineering stress analysis, and 3D rendering.
   - Supports high-performance computing workloads, enabling efficient task distribution across many cores.

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
