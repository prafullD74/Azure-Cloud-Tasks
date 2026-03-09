# [Azure reliability](https://learn.microsoft.com/en-us/azure/reliability/overview)
1. [Reliability](https://learn.microsoft.com/en-us/azure/reliability/overview#what-is-reliability) is the ability of a system to consistently perform its intended functions without failure, ensuring high availability, fault tolerance, and rapid recovery during disruptions.
   - Reliability refers to the ability of a workload to perform consistently at an acceptable service level, and in accordance with business continuity requirements.
   - Two key approaches to achieving reliability in a workload are:
     1. Resiliency: the ability to withstand and continue operating when things go wrong, such as temporary errors, infrastructure outages, or unexpected spikes in demand. Resiliency helps you to avoid disruptions.
     2. Recoverability: the ability to restore normal operations after a disruption. If a disruption does occur, recoverability helps you to restore back to a reliable state.
   - The Azure platform and services offer a number of reliability features such as availability zones, multi-region support, data replication, and backup and restore.
2. A group of datacenters, which in Azure is called an **availability zone**


### [Redundancy](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#redundancy)
1. **Redundancy** is the ability to maintain multiple identical copies of a service component, and to use those copies in a way that prevents any one component from becoming a single point of failure.
2. **Redundancy** consists of deploying multiple instances of a component.
3. [**Stateless**](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#scenario-stateless-redundancy) components, are components that don't store any data. 
   - Example: Stateless redundancy >> a stateless API component is deployed to a virtual machine.
     - Deploys multiple copies of the API instance.
     - Implements a load balancer to distribute requests among API instances.
     - The load balancer monitors the health of each instance to send requests only to healthy instances.
4. Three ways to deploy redundant instances:
   - Local redundancy places instances in multiple parts of a single Azure datacenter and protects against hardware failures. Local redundancy typically provides the lowest cost and latency.
   - Zone redundancy spreads instances across multiple availability zones in a single Azure region. Zone redundancy protects against datacenter failures, in addition to hardware failures.
   - Geo-redundancy places instances across multiple Azure regions and provides protection against large-scale regional outages. Geo-redundancy comes at a higher cost


A [**fault domain**](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-fault-domains) is a fault isolation group within an availability zone or datacenter of hardware nodes that share the same power, networking, cooling, and platform maintenance schedule.


### [Health monitoring]() 
1. The health of each instance determines whether that instance can do its work, and health monitoring is important to enable fast recovery if there's a problem.

### [Replication: Redundancy for data](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#replication-redundancy-for-data)
- **Replication** or data redundancy is the ability to maintain multiple copies of data, called **replicas**.
- **Backup** is the ability to maintain a timestamped copy of data that can be used to restore data that has been lost.

### Each replication type affects two key metrics used in discussions of business continuity: 
- [**recovery time objective (RTO)**](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#replication-redundancy-for-data), which is the maximum amount of downtime you can tolerate in a disaster scenario.
  - How fast system must come back.
- [**recovery point objective (RPO)**](), which is the maximum amount of data loss you can tolerate in a disaster scenario.
  - How much data loss acceptable. 
  - ASR replication frequency directly affects RPO.
