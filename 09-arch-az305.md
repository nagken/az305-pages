# AZ-305 - Reference Architectures

> Real reference architectures from the [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/browse/), [Well-Architected Framework](https://learn.microsoft.com/azure/well-architected/), and [Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/) that map directly to AZ-305 exam topics. Each entry calls out which **skills-measured area** it reinforces.

## Domain 1 - Design identity, governance, and monitoring solutions (25-30%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Azure landing zone conceptual architecture](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/) | Management groups, policy, RBAC, platform vs application LZ |
| [Hybrid identity design](https://learn.microsoft.com/entra/identity/hybrid/connect/plan-hybrid-identity-design-considerations-overview) | Entra Connect topologies, PHS / PTA / federation trade-offs |
| [Multi-tenant identity for SaaS](https://learn.microsoft.com/azure/architecture/multitenant-identity/) | Workforce vs External ID, app registration patterns |
| [Privileged Identity Management at scale](https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure) | Just-in-time, access reviews, break-glass |
| [Monitoring at enterprise scale](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/management) | Log Analytics workspace strategy, Sentinel, Defender for Cloud |

## Domain 2 - Design data storage solutions (25-30%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Choose a data store](https://learn.microsoft.com/azure/architecture/guide/technology-choices/data-store-decision-tree) | Relational vs document vs key-value vs graph vs analytics |
| [Modern data warehouse](https://learn.microsoft.com/azure/architecture/solution-ideas/articles/enterprise-data-warehouse) | Synapse / Fabric, Data Lake Gen2, ADF / Pipelines |
| [Transactional data with Cosmos DB](https://learn.microsoft.com/azure/cosmos-db/distribute-data-globally) | Multi-region writes, consistency levels, partitioning |
| [SQL Database HA / DR options](https://learn.microsoft.com/azure/azure-sql/database/business-continuity-high-availability-disaster-recover-hadr-overview) | Active geo-replication, failover groups, zone redundancy |
| [Big data analytics architecture](https://learn.microsoft.com/azure/architecture/solution-ideas/articles/big-data-azure-data-explorer) | ADX / Fabric Real-Time Intelligence for telemetry |
| [Storage account security and CMK](https://learn.microsoft.com/azure/storage/common/security-baseline) | Encryption at rest, key rotation, private endpoints |

## Domain 3 - Design business continuity solutions (10-15%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Reliable web app pattern](https://learn.microsoft.com/azure/architecture/web-apps/guides/reliable-web-app/overview) | Active-passive multi-region, retry / circuit breaker |
| [Multi-region App Service](https://learn.microsoft.com/azure/architecture/web-apps/app-service/architectures/multi-region) | Front Door / Traffic Manager, paired regions |
| [Backup and disaster recovery for VMs](https://learn.microsoft.com/azure/architecture/framework/resiliency/backup-and-recovery) | RTO / RPO, Recovery Services vault, immutability |
| [Azure Site Recovery zone-to-zone](https://learn.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery) | Availability zone DR for IaaS |
| [Highly available SQL Server on VMs](https://learn.microsoft.com/azure/azure-sql/virtual-machines/windows/business-continuity-high-availability-disaster-recovery-hadr-overview) | Always On AGs, distributed AGs across regions |

## Domain 4 - Design infrastructure solutions (25-30%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Hub-and-spoke network topology](https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke) | Shared services hub, transitive routing, segmentation |
| [Virtual WAN baseline](https://learn.microsoft.com/azure/architecture/networking/architecture/virtual-wan-network-topology) | Global transit, secured hubs, branch-to-branch |
| [AKS baseline architecture](https://learn.microsoft.com/azure/architecture/reference-architectures/containers/aks/baseline-aks) | Production Kubernetes, ingress, identity, policy |
| [Container Apps microservices](https://learn.microsoft.com/azure/architecture/example-scenario/serverless/microservices-with-container-apps) | When to choose ACA over AKS |
| [App Service web app baseline](https://learn.microsoft.com/azure/architecture/web-apps/app-service/architectures/baseline-zone-redundant) | Zone-redundant, slots, scale rules |
| [Event-driven architecture with Functions](https://learn.microsoft.com/azure/architecture/reference-architectures/serverless/event-processing) | Functions + Event Grid + Service Bus |
| [API Management landing zone accelerator](https://learn.microsoft.com/azure/architecture/example-scenario/integration/app-gateway-internal-api-management-function) | API gateway, internal vs external, WAF in front |
| [Hybrid networking with ExpressRoute](https://learn.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/expressroute) | Private connectivity, FastPath, redundant circuits |

## How to study with these

1. Read the matching domain page in this guide first.
2. Open the architecture; trace **identity -> governance -> networking -> workload -> data -> BCDR -> monitoring**.
3. Re-answer the [exam decision reference](05-exam-cheatsheet.md) with that architecture as the worked example.
