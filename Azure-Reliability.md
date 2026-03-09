# [Azure reliability](https://learn.microsoft.com/en-us/azure/reliability/overview)
1. [Reliability](https://learn.microsoft.com/en-us/azure/reliability/overview#what-is-reliability) is the ability of a system to consistently perform its intended functions without failure, ensuring high availability, fault tolerance, and rapid recovery during disruptions.
   - Reliability refers to the ability of a workload to perform consistently at an acceptable service level, and in accordance with business continuity requirements.
   - Two key approaches to achieving reliability in a workload are:
     1. **Resiliency**: the ability to withstand and continue operating when things go wrong, such as temporary errors, infrastructure outages, or unexpected spikes in demand. Resiliency helps you to avoid disruptions.
     2. **Recoverability**: the ability to restore normal operations after a disruption. If a disruption does occur, recoverability helps you to restore back to a reliable state.
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
   - **Local redundancy** places instances in multiple parts of a single Azure datacenter and protects against hardware failures. Local redundancy typically provides the lowest cost and latency.
   - **Zone redundancy** spreads instances across multiple availability zones in a single Azure region. Zone redundancy protects against datacenter failures, in addition to hardware failures.
   - **Geo-redundancy** places instances across multiple Azure regions and provides protection against large-scale regional outages. Geo-redundancy comes at a higher cost
5. A [**fault domain**](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-fault-domains) is a fault isolation group within an availability zone or datacenter of hardware nodes that share the same power, networking, cooling, and platform maintenance schedule.

### [Replication: Redundancy for data](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#replication-redundancy-for-data)
- **Replication** or data redundancy is the ability to maintain multiple copies of data, called **replicas**.
  - Replication synchronizes all changes among multiple replicas and doesn't maintain old copies of data.
- [Synchronous and asynchronous replication](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#synchronous-and-asynchronous-replication)
  1. **Synchronous replication** requires updates to take place on multiple replicas before the update is considered complete. Synchronous replication can guarantee consistency, which means it can support an RPO of zero.
  2. **Asynchronous replication** happens in the background. However, if you need to fail over to another replica, it might not have the latest data, and so your RPO must be greater than zero.
- [Replica roles](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#replica-roles)
  1. **Active-passive replication** means that you have one active replica, which is responsible for acting as the source of truth. Any changes made to the data must be applied to that replica. Any other replicas act in a passive role, which means they receive updates to the data from the active replica, but they don't process changes directly from clients. Passive replicas aren't used for live traffic unless a failover occurs and the replicas' roles change.
     - ![Diagram shows an active-passive system with one passive replica](https://learn.microsoft.com/en-us/azure/reliability/media/concept-redundancy-replication-backup/replica-roles-active-passive.svg)
     - RTO for an active-passive system is measured in minutes.
     - **read-only replicas**, which enables you to read (but not write) data from the passive replicas. Several Azure services support read-only replicas, including [Azure Storage with the read access GRS (RA-GRS) replication type](https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy#read-access-to-data-in-the-secondary-region), and [Azure SQL Database active geo-replication](https://learn.microsoft.com/en-us/azure/azure-sql/database/active-geo-replication-overview?view=azuresql&preserve-view=true).
  2. **Active-active replication** enables using multiple active replicas for live traffic simultaneously, and any of the replicas can process requests.
     - ![Active-active replication](https://learn.microsoft.com/en-us/azure/reliability/media/concept-redundancy-replication-backup/replica-roles-active-active.svg)
     - Active-active replication can support an RTO of zero in some situations.
- If you use virtual machines, you can use [Azure Site Recovery](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-overview) to replicate virtual machines and their disks between availability zones or to another Azure region.
  
### Each replication type affects two key metrics used in discussions of business continuity: 
- [**recovery time objective (RTO)**](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#replication-redundancy-for-data), which is the maximum amount of downtime you can tolerate in a disaster scenario.
  - How fast system must come back.
- [**recovery point objective (RPO)**](), which is the maximum amount of data loss you can tolerate in a disaster scenario.
  - How much data loss acceptable. 
  - ASR replication frequency directly affects RPO.

### [Backup](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#backup)
- **Backup** takes a copy of your data at a specific point in time `OR` is the ability to maintain a timestamped copy of data that can be used to restore data that has been lost.
- Backup can protect you from a variety of risks, including: Catastrophic losses of hardware or other infrastructure, Data corruption and deletion, Cyberattacks, such as ransomware.
- As part of a disaster recovery strategy, backups typically support an RTO and RPO
  1. **RTO** is influenced by the time it takes for you to initiate and complete your recovery processes, including restoring a backup and validating that the restoration completed successfully.
  2. **RPO** is influenced by the frequency of your backup process. If you take backups more frequently, it means you lose less data if you have to restore from a backup.
- [Azure Backup](https://learn.microsoft.com/en-us/azure/backup/backup-overview) is a dedicated backup solution for several key Azure services, including virtual machines, Azure Storage, and Azure Kubernetes Service (AKS).

### Backup vs. replication
1. Replication supports day-to-day resiliency and is commonly used in a high availability strategy. However, replication doesn't protect you against risks that result in data loss or corruption.
2. In contrast, backup is often a last line of defense against catastrophic risks. A total restore from a backup is often part of a disaster recovery plan.

### [Manage capacity with over-provisioning](https://learn.microsoft.com/en-us/azure/reliability/concept-redundancy-replication-backup#manage-capacity-with-over-provisioning)
1. Over-provisioning allows the solution to tolerate some degree of capacity loss and still continue to function without degraded performance.

### [Health monitoring]() 
1. The health of each instance determines whether that instance can do its work, and health monitoring is important to enable fast recovery if there's a problem.
