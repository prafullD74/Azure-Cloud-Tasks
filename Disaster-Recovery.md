#

### . Disaster Recovery based on business RTO and RPO requirements.
> For production workloads, we implement a layered DR strategy: to ensures business continuity and SLA compliance.”
1. **High Availability within Region**
   - Deploy workloads across Availability Zones
   - Use Zone-redundant services wherever possible
2. **Cross-Region Disaster Recovery**
   - Use Azure Site Recovery for VM replication
   - Enable Geo-redundant storage (GRS) for storage accounts
   - Configure Geo-replication for Azure SQL
   - Use Azure Traffic Manager for DNS-based failover
3. **Infrastructure as Code**
   - DR infrastructure defined in Terraform
   - Same modules reused for primary and DR region
   - Allows faster environment recreation
4. **Testing & Validation**
   - Conduct planned failover tests
   - Validate RTO/RPO
   - Maintain DR runbooks

### . What are the DR Components in Azure?
1. Virtual Machines – Azure Site Recovery (ASR)
   - Service: Azure Site Recovery
     - ASR installs a replication agent in VM
     - Data continuously replicates to secondary region
     - Replication is asynchronous
     - In disaster: Trigger failover > VM boots in DR region > Networking mapping happens > Public IP & DNS updated
     - Supports test failover & planned failover, Maintains recovery points.
2. Storage – Geo Redundant Storage (GRS)
   - Azure Storage offers redundancy types: Local Redundant(LRS), Zone Redundant(ZRS), Geo Redundant(GRS), Read Access Geo Redundant(RA-GRS)
   - Data stored in primary region
   - Asynchronously replicated to paired region
   - Microsoft manages replication
3. Azure SQL – Geo Replication
   - Active Geo-Replication
     - Primary database in Region A
     - Readable secondary in Region B
     - Asynchronous replication
     - Manual or automatic failover
   - Auto-Failover Group
     - Automatically fails over if region goes down
     - Updates connection string endpoint
4. Kubernetes (If Using AKS)
   - Deploy cluster in multiple zones
   - Backup etcd, Use Azure Backup
   - Maintain secondary cluster in DR region
   - Use container registry replication
5. DNS-Based Failover – Azure Traffic Manager
   - Traffic Manager works at DNS level.
     - It monitors health of primary endpoint
     - If unhealthy → routes traffic to secondary
     - TTL (Time to Live) should be low (like 30–60 seconds)
   - Routing methods: Priority, Weighted, Performance, Geographic
   - For DR → we use Priority routing

### . Tell DR Strategy Types in Azure (From Low to High Cost)?

| Strategy         | Description               | RTO      | Cost      |
| ---------------- | ------------------------- | -------- | --------- |
| Backup & Restore | Restore infra from backup | High     | Low       |
| Pilot Light      | Core services replicated  | Medium   | Medium    |
| Warm Standby     | Scaled-down infra running | Low      | Higher    |
| Active-Active    | Both regions live         | Very Low | Very High |
