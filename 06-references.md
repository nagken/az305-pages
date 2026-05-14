# AZ-305 Concept & Reference Index

> A single index of every concept used in this study guide, with links back to the **official Microsoft Learn documentation** that supports it. Use it to verify facts, dive deeper on any decision tree, or hand to a teammate who needs the source-of-truth article.
>
> Anchor: [Microsoft Learn AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305) - [Exam AZ-305 page](https://learn.microsoft.com/credentials/certifications/exams/az-305/) - [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/) - [Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/) - [Azure Well-Architected Framework](https://learn.microsoft.com/azure/well-architected/).

---

## Skill 1 - Identity, Governance & Monitoring (~25-30%)

### Microsoft Entra ID and identity

| Concept | Microsoft Learn |
|---|---|
| Microsoft Entra ID overview | [What is Microsoft Entra ID](https://learn.microsoft.com/entra/fundamentals/whatis) |
| Tenants, subscriptions, directories | [What is Microsoft Entra ID](https://learn.microsoft.com/entra/fundamentals/whatis) |
| Entra ID editions (Free / P1 / P2) | [Compare editions](https://learn.microsoft.com/entra/fundamentals/licensing) |
| External ID (B2C and B2B) | [Microsoft Entra External ID](https://learn.microsoft.com/entra/external-id/external-identities-overview) |
| B2B guest collaboration | [B2B collaboration](https://learn.microsoft.com/entra/external-id/what-is-b2b) |
| Conditional Access | [Conditional Access overview](https://learn.microsoft.com/entra/identity/conditional-access/overview) |
| Multifactor authentication (MFA) | [Microsoft Entra MFA](https://learn.microsoft.com/entra/identity/authentication/concept-mfa-howitworks) |
| Privileged Identity Management (PIM) | [PIM overview](https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure) |
| Hybrid identity - Entra Connect Sync | [Entra Connect Sync](https://learn.microsoft.com/entra/identity/hybrid/connect/whatis-azure-ad-connect) |
| Hybrid identity - Entra Cloud Sync | [Cloud Sync](https://learn.microsoft.com/entra/identity/hybrid/cloud-sync/what-is-cloud-sync) |
| Password Hash Sync vs Pass-Through Auth vs Federation | [Choose authentication method](https://learn.microsoft.com/entra/identity/hybrid/connect/choose-ad-authn) |
| Managed identities | [What are managed identities](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview) |
| Service principals vs managed identities | [Service principals](https://learn.microsoft.com/entra/identity-platform/app-objects-and-service-principals) |
| Identity Protection | [Identity Protection](https://learn.microsoft.com/entra/id-protection/overview-identity-protection) |
| Entra ID Governance (access reviews, entitlement) | [ID Governance](https://learn.microsoft.com/entra/id-governance/identity-governance-overview) |

### Authorization and governance

| Concept | Microsoft Learn |
|---|---|
| Azure RBAC | [Azure RBAC overview](https://learn.microsoft.com/azure/role-based-access-control/overview) |
| Built-in vs custom RBAC roles | [Custom roles](https://learn.microsoft.com/azure/role-based-access-control/custom-roles) |
| Entra ID roles vs Azure RBAC | [Classic vs Entra vs Azure roles](https://learn.microsoft.com/entra/identity/role-based-access-control/custom-overview) |
| Management groups | [Management groups](https://learn.microsoft.com/azure/governance/management-groups/overview) |
| Subscriptions and resource groups | [Resource hierarchy](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-setup-guide/organize-resources) |
| Azure Policy | [Azure Policy overview](https://learn.microsoft.com/azure/governance/policy/overview) |
| Policy effects (Audit, Deny, DeployIfNotExists, Modify) | [Policy effects](https://learn.microsoft.com/azure/governance/policy/concepts/effects) |
| Initiatives (policy sets) | [Initiative definitions](https://learn.microsoft.com/azure/governance/policy/concepts/initiative-definition-structure) |
| Resource locks | [Lock resources](https://learn.microsoft.com/azure/azure-resource-manager/management/lock-resources) |
| Tags and tagging strategy | [Tagging strategy](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/resource-tagging) |
| Azure landing zones | [Landing zone design](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/) |
| Microsoft Defender for Cloud | [Defender for Cloud](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-cloud-introduction) |
| Microsoft Sentinel | [Sentinel overview](https://learn.microsoft.com/azure/sentinel/overview) |

### Monitoring and observability

| Concept | Microsoft Learn |
|---|---|
| Azure Monitor | [Azure Monitor overview](https://learn.microsoft.com/azure/azure-monitor/overview) |
| Log Analytics workspace | [Log Analytics overview](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-overview) |
| Data Collection Rules (DCRs) | [Data collection rules](https://learn.microsoft.com/azure/azure-monitor/essentials/data-collection-rule-overview) |
| Application Insights | [App Insights overview](https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview) |
| Metric vs log alerts | [Alert types](https://learn.microsoft.com/azure/azure-monitor/alerts/alerts-types) |
| Action groups | [Action groups](https://learn.microsoft.com/azure/azure-monitor/alerts/action-groups) |
| Workbooks and dashboards | [Workbooks](https://learn.microsoft.com/azure/azure-monitor/visualize/workbooks-overview) |
| KQL (Kusto Query Language) | [KQL quick reference](https://learn.microsoft.com/azure/data-explorer/kql-quick-reference) |
| Network Watcher | [Network Watcher](https://learn.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) |

---

## Skill 2 - Data Storage Solutions (~20-25%)

### Relational databases

| Concept | Microsoft Learn |
|---|---|
| Azure SQL deployment options | [SQL: IaaS vs PaaS](https://learn.microsoft.com/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview) |
| Azure SQL Database service tiers | [Purchasing models](https://learn.microsoft.com/azure/azure-sql/database/purchasing-models) |
| SQL Database Hyperscale | [Hyperscale tier](https://learn.microsoft.com/azure/azure-sql/database/service-tier-hyperscale) |
| Elastic pools | [Elastic pools](https://learn.microsoft.com/azure/azure-sql/database/elastic-pool-overview) |
| SQL Managed Instance | [SQL MI overview](https://learn.microsoft.com/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview) |
| SQL on Azure VM | [SQL Server on Azure VMs](https://learn.microsoft.com/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) |
| Auto-failover groups | [Failover groups](https://learn.microsoft.com/azure/azure-sql/database/auto-failover-group-overview) |
| Azure Database for PostgreSQL Flexible Server | [PostgreSQL Flexible](https://learn.microsoft.com/azure/postgresql/flexible-server/overview) |
| Azure Database for MySQL Flexible Server | [MySQL Flexible](https://learn.microsoft.com/azure/mysql/flexible-server/overview) |

### NoSQL and Cosmos DB

| Concept | Microsoft Learn |
|---|---|
| Azure Cosmos DB | [Cosmos DB introduction](https://learn.microsoft.com/azure/cosmos-db/introduction) |
| Cosmos DB APIs (NoSQL, Mongo, Cassandra, Gremlin, Table) | [Choose an API](https://learn.microsoft.com/azure/cosmos-db/choose-api) |
| Consistency levels | [Consistency levels](https://learn.microsoft.com/azure/cosmos-db/consistency-levels) |
| Partitioning and partition keys | [Partitioning](https://learn.microsoft.com/azure/cosmos-db/partitioning-overview) |
| Multi-region writes | [Multi-region writes](https://learn.microsoft.com/azure/cosmos-db/how-to-multi-master) |
| Request units (RUs) | [Request units](https://learn.microsoft.com/azure/cosmos-db/request-units) |

### Storage and big data

| Concept | Microsoft Learn |
|---|---|
| Azure Storage account | [Storage introduction](https://learn.microsoft.com/azure/storage/common/storage-introduction) |
| Blob access tiers (Hot / Cool / Cold / Archive) | [Access tiers](https://learn.microsoft.com/azure/storage/blobs/access-tiers-overview) |
| Lifecycle management | [Lifecycle management](https://learn.microsoft.com/azure/storage/blobs/lifecycle-management-overview) |
| Storage redundancy (LRS, ZRS, GRS, RA-GRS, GZRS) | [Redundancy options](https://learn.microsoft.com/azure/storage/common/storage-redundancy) |
| Azure Files (SMB / NFS / Azure File Sync) | [Azure Files](https://learn.microsoft.com/azure/storage/files/storage-files-introduction) |
| Azure NetApp Files | [Azure NetApp Files](https://learn.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) |
| Data Lake Storage Gen2 | [ADLS Gen2](https://learn.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) |
| Azure Synapse Analytics | [Synapse overview](https://learn.microsoft.com/azure/synapse-analytics/overview-what-is) |
| Azure Data Factory | [Data Factory](https://learn.microsoft.com/azure/data-factory/introduction) |
| Azure Databricks | [Databricks overview](https://learn.microsoft.com/azure/databricks/introduction/) |
| Microsoft Fabric | [Microsoft Fabric](https://learn.microsoft.com/fabric/get-started/microsoft-fabric-overview) |

### Messaging

| Concept | Microsoft Learn |
|---|---|
| Choose a messaging service | [Messaging services comparison](https://learn.microsoft.com/azure/event-grid/compare-messaging-services) |
| Azure Service Bus | [Service Bus messaging](https://learn.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) |
| Azure Event Hubs | [Event Hubs](https://learn.microsoft.com/azure/event-hubs/event-hubs-about) |
| Azure Event Grid | [Event Grid](https://learn.microsoft.com/azure/event-grid/overview) |
| Storage Queues vs Service Bus Queues | [Compare queues](https://learn.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted) |

### Data security

| Concept | Microsoft Learn |
|---|---|
| Encryption at rest | [Encryption at rest](https://learn.microsoft.com/azure/security/fundamentals/encryption-atrest) |
| Customer-managed keys (CMK) | [CMK in Key Vault](https://learn.microsoft.com/azure/storage/common/customer-managed-keys-overview) |
| Always Encrypted | [Always Encrypted](https://learn.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) |
| Transparent Data Encryption (TDE) | [TDE for Azure SQL](https://learn.microsoft.com/azure/azure-sql/database/transparent-data-encryption-tde-overview) |
| Dynamic Data Masking | [Dynamic Data Masking](https://learn.microsoft.com/azure/azure-sql/database/dynamic-data-masking-overview) |
| Azure Key Vault | [Key Vault overview](https://learn.microsoft.com/azure/key-vault/general/overview) |

---

## Skill 3 - Business Continuity (~15-20%)

### Backup and restore

| Concept | Microsoft Learn |
|---|---|
| Azure Backup | [Backup overview](https://learn.microsoft.com/azure/backup/backup-overview) |
| Recovery Services vault vs Backup vault | [Vault types](https://learn.microsoft.com/azure/backup/backup-vault-overview) |
| Backup Center | [Backup Center](https://learn.microsoft.com/azure/backup/backup-center-overview) |
| Soft delete for backups | [Soft delete](https://learn.microsoft.com/azure/backup/backup-azure-security-feature-cloud) |
| SQL backups in Azure | [Azure SQL backups](https://learn.microsoft.com/azure/azure-sql/database/automated-backups-overview) |

### Disaster recovery and HA

| Concept | Microsoft Learn |
|---|---|
| Azure Site Recovery | [ASR overview](https://learn.microsoft.com/azure/site-recovery/site-recovery-overview) |
| ASR Recovery Plans | [Recovery plans](https://learn.microsoft.com/azure/site-recovery/recovery-plan-overview) |
| Availability zones | [Availability zones](https://learn.microsoft.com/azure/reliability/availability-zones-overview) |
| Region pairs and cross-region replication | [Region pairs](https://learn.microsoft.com/azure/reliability/cross-region-replication-azure) |
| VM availability - sets, zones, scale sets | [VM availability](https://learn.microsoft.com/azure/virtual-machines/availability) |
| Azure SQL high availability | [SQL HA](https://learn.microsoft.com/azure/azure-sql/database/high-availability-sla) |
| Cosmos DB business continuity | [Cosmos BCDR](https://learn.microsoft.com/azure/cosmos-db/high-availability) |
| Storage redundancy options | [Storage redundancy](https://learn.microsoft.com/azure/storage/common/storage-redundancy) |

### Global routing and load balancing

| Concept | Microsoft Learn |
|---|---|
| Choose a load-balancing option | [Load balancing decision tree](https://learn.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) |
| Azure Front Door | [Front Door overview](https://learn.microsoft.com/azure/frontdoor/front-door-overview) |
| Azure Traffic Manager | [Traffic Manager](https://learn.microsoft.com/azure/traffic-manager/traffic-manager-overview) |
| Azure Application Gateway (with WAF) | [App Gateway](https://learn.microsoft.com/azure/application-gateway/overview) |
| Azure Load Balancer | [Load Balancer](https://learn.microsoft.com/azure/load-balancer/load-balancer-overview) |

---

## Skill 4 - Infrastructure Solutions (~30-35%)

### Compute decision

| Concept | Microsoft Learn |
|---|---|
| Compute decision tree | [Choose an Azure compute service](https://learn.microsoft.com/azure/architecture/guide/technology-choices/compute-decision-tree) |
| Azure Virtual Machines | [Linux VMs](https://learn.microsoft.com/azure/virtual-machines/linux/overview) - [Windows VMs](https://learn.microsoft.com/azure/virtual-machines/windows/overview) |
| Virtual Machine Scale Sets | [VMSS overview](https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview) |
| Azure App Service | [App Service overview](https://learn.microsoft.com/azure/app-service/overview) |
| App Service plans and tiers | [App Service plans](https://learn.microsoft.com/azure/app-service/overview-hosting-plans) |
| Deployment slots | [Deployment slots](https://learn.microsoft.com/azure/app-service/deploy-staging-slots) |
| Azure Functions | [Functions overview](https://learn.microsoft.com/azure/azure-functions/functions-overview) |
| Functions hosting plans (Consumption / Premium / Flex / Dedicated) | [Functions scale](https://learn.microsoft.com/azure/azure-functions/functions-scale) |
| Azure Container Apps | [Container Apps overview](https://learn.microsoft.com/azure/container-apps/overview) |
| Azure Kubernetes Service | [AKS overview](https://learn.microsoft.com/azure/aks/intro-kubernetes) |
| Azure Container Instances | [ACI overview](https://learn.microsoft.com/azure/container-instances/container-instances-overview) |
| Azure Container Registry | [ACR overview](https://learn.microsoft.com/azure/container-registry/container-registry-intro) |
| Azure Batch | [Batch overview](https://learn.microsoft.com/azure/batch/batch-technical-overview) |

### Networking

| Concept | Microsoft Learn |
|---|---|
| Virtual Network (VNet) | [VNet overview](https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview) |
| VNet peering | [VNet peering](https://learn.microsoft.com/azure/virtual-network/virtual-network-peering-overview) |
| Network Security Groups (NSG) | [NSG overview](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview) |
| Application Security Groups (ASG) | [Application Security Groups](https://learn.microsoft.com/azure/virtual-network/application-security-groups) |
| Azure Firewall | [Azure Firewall](https://learn.microsoft.com/azure/firewall/overview) |
| Hub-and-spoke topology | [Hub-spoke](https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke) |
| Azure Virtual WAN | [Virtual WAN](https://learn.microsoft.com/azure/virtual-wan/virtual-wan-about) |
| ExpressRoute | [ExpressRoute](https://learn.microsoft.com/azure/expressroute/expressroute-introduction) |
| VPN Gateway (site-to-site, point-to-site) | [VPN Gateway](https://learn.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) |
| Private Endpoint | [Private Endpoint](https://learn.microsoft.com/azure/private-link/private-endpoint-overview) |
| Service Endpoints | [Service Endpoints](https://learn.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) |
| Private DNS zones | [Private DNS](https://learn.microsoft.com/azure/dns/private-dns-overview) |
| Azure Bastion | [Azure Bastion](https://learn.microsoft.com/azure/bastion/bastion-overview) |
| DDoS Protection | [DDoS Protection](https://learn.microsoft.com/azure/ddos-protection/ddos-protection-overview) |

### Migration

| Concept | Microsoft Learn |
|---|---|
| Azure Migrate | [Azure Migrate overview](https://learn.microsoft.com/azure/migrate/migrate-services-overview) |
| Server migration (VMware, Hyper-V, physical) | [Server Migration](https://learn.microsoft.com/azure/migrate/concepts-migration-planning) |
| Database Migration Service (Azure DMS) | [DMS overview](https://learn.microsoft.com/azure/dms/dms-overview) |
| Storage migration (Storage Mover, AzCopy, Data Box) | [Data Box](https://learn.microsoft.com/azure/databox/data-box-overview) |
| App migration patterns (rehost, refactor, rearchitect, rebuild) | [The 6 Rs](https://learn.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-overview) |

### API and integration

| Concept | Microsoft Learn |
|---|---|
| Azure API Management | [API Management concepts](https://learn.microsoft.com/azure/api-management/api-management-key-concepts) |
| API Management policies | [API Management policies](https://learn.microsoft.com/azure/api-management/api-management-howto-policies) |
| Azure Logic Apps | [Logic Apps overview](https://learn.microsoft.com/azure/logic-apps/logic-apps-overview) |

---

## Cross-cutting frameworks

| Concept | Microsoft Learn |
|---|---|
| Cloud Adoption Framework (CAF) | [CAF overview](https://learn.microsoft.com/azure/cloud-adoption-framework/) |
| Azure Well-Architected Framework (WAF) | [WAF overview](https://learn.microsoft.com/azure/well-architected/) |
| Reliability pillar | [Reliability](https://learn.microsoft.com/azure/well-architected/reliability/) |
| Security pillar | [Security](https://learn.microsoft.com/azure/well-architected/security/) |
| Cost optimization pillar | [Cost optimization](https://learn.microsoft.com/azure/well-architected/cost-optimization/) |
| Operational excellence pillar | [Operational excellence](https://learn.microsoft.com/azure/well-architected/operational-excellence/) |
| Performance efficiency pillar | [Performance efficiency](https://learn.microsoft.com/azure/well-architected/performance-efficiency/) |
| Azure Architecture Center reference architectures | [Reference architectures](https://learn.microsoft.com/azure/architecture/browse/) |
| Azure pricing calculator | [Pricing calculator](https://azure.microsoft.com/pricing/calculator/) |
| Azure Total Cost of Ownership calculator | [TCO calculator](https://azure.microsoft.com/pricing/tco/calculator/) |

---

## Exam logistics

| Resource | Link |
|---|---|
| Exam AZ-305 page | [AZ-305 exam](https://learn.microsoft.com/credentials/certifications/exams/az-305/) |
| Skills measured (PDF, official) | [Skills measured](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305) |
| Microsoft Learn AZ-305 learning paths | [Browse learning paths](https://learn.microsoft.com/training/browse/?products=azure&roles=solution-architect) |
| Practice assessment | [AZ-305 certification page](https://learn.microsoft.com/credentials/certifications/azure-solutions-architect/) |
| Microsoft Certification renewal | [Renew your certification](https://learn.microsoft.com/credentials/certifications/renew-your-microsoft-certification) |

---

 **Back to:** [00-MASTER-INDEX.md](00-MASTER-INDEX.md) - **Decision reference:** [05-exam-cheatsheet.md](05-exam-cheatsheet.md) - **Extras:** [07-extra-az305-concepts.md](07-extra-az305-concepts.md)
