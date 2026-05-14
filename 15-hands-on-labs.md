# Hands-On Labs and Sample Repositories

> Curated, executable references for AZ-305 architecture topics. All links point to official Microsoft Learn modules, Azure-Samples repositories, or Cloud Adoption Framework / Well-Architected Framework assessments.

## Microsoft Learn - Sandbox-Backed Modules

| Path / Module | What you build |
| --- | --- |
| [Design solutions in Azure (AZ-305 path)](https://learn.microsoft.com/training/courses/az-305t00) | Full AZ-305 learning path: identity, governance, data, infrastructure, monitoring. |
| [Design a governance solution](https://learn.microsoft.com/training/modules/design-governance/) | Management groups, policy, RBAC at scale. |
| [Design an identity and access management solution](https://learn.microsoft.com/training/modules/design-identity-access-management-solution/) | Entra ID, Conditional Access, PIM, B2B/B2C. |
| [Design a data storage solution for relational data](https://learn.microsoft.com/training/modules/design-data-storage-solution-relational-data/) | SQL DB, MI, Synapse SQL, redundancy choices. |
| [Design a data storage solution for non-relational data](https://learn.microsoft.com/training/modules/design-data-storage-solution-non-relational-data/) | Cosmos DB consistency, partitioning, multi-region. |
| [Design a data storage solution for semi-structured data](https://learn.microsoft.com/training/modules/design-data-storage-solution-semi-structured-data/) | Storage tiers, Data Lake Gen2, lifecycle. |
| [Design a solution for backup and disaster recovery](https://learn.microsoft.com/training/modules/design-solution-for-backup-disaster-recovery/) | Azure Backup, Site Recovery, RPO/RTO design. |
| [Design a solution to log and monitor Azure resources](https://learn.microsoft.com/training/modules/design-monitoring-solution-azure-resources/) | Azure Monitor, Log Analytics, Sentinel design. |
| [Design network infrastructure](https://learn.microsoft.com/training/modules/design-network-infrastructure/) | Hub-and-spoke, Virtual WAN, ExpressRoute. |
| [Design an application architecture](https://learn.microsoft.com/training/modules/design-application-architecture/) | Microservices, async messaging, caching, API gateway. |
| [Design migrations](https://learn.microsoft.com/training/modules/design-migrations/) | Azure Migrate, rehosting, replatforming, refactoring. |

## Cloud Adoption Framework and Well-Architected Framework

| Resource | Purpose |
| --- | --- |
| [Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/) | Strategy, plan, ready, govern, manage methodology. |
| [Azure landing zones](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/) | Reference architecture for enterprise-scale environments. |
| [Well-Architected Framework](https://learn.microsoft.com/azure/well-architected/) | Five pillars: reliability, security, cost, OpEx, performance. |
| [WAF Reliable Web App pattern](https://learn.microsoft.com/azure/architecture/web-apps/guides/reliable-web-app/overview) | Reference workload migration pattern. |
| [Mission-critical baseline](https://learn.microsoft.com/azure/architecture/reference-architectures/containers/aks-mission-critical/mission-critical-intro) | Reference for active-active multi-region tier-1 workloads. |
| [Azure Architecture Center patterns](https://learn.microsoft.com/azure/architecture/patterns/) | Catalog of cloud design patterns (CQRS, saga, retry, circuit breaker, etc.). |

## Self-Assessments

| Assessment | Outcome |
| --- | --- |
| [WAF review](https://learn.microsoft.com/assessments/azure-architecture-review/) | Score a workload against the five pillars; produces a remediation backlog. |
| [Strategic Migration Assessment (SMART)](https://learn.microsoft.com/assessments/cloud-adoption-framework-strategic-migration-assessment/) | Migration readiness scoring. |
| [Cloud Adoption Strategy Evaluator](https://learn.microsoft.com/assessments/cloud-adoption-strategy-evaluator/) | Strategy, financial, operating-model readiness. |

## Azure-Samples Reference Repositories

| Repository | Purpose |
| --- | --- |
| [Azure/Enterprise-Scale](https://github.com/Azure/Enterprise-Scale) | Canonical enterprise-scale landing zone Bicep + ARM. |
| [Azure/ALZ-Bicep](https://github.com/Azure/ALZ-Bicep) | Azure Landing Zones in Bicep. |
| [Azure/terraform-azurerm-caf-enterprise-scale](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale) | ALZ for Terraform. |
| [mspnp/iaas-landing-zone-baseline](https://github.com/mspnp/iaas-landing-zone-baseline) | IaaS workload landing zone reference. |
| [mspnp/aks-baseline](https://github.com/mspnp/aks-baseline) | AKS production baseline. |
| [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) | Hundreds of ARM/Bicep reference templates. |
| [Azure-Samples/azure-monitor-baseline-alerts](https://github.com/Azure/azure-monitor-baseline-alerts) | Recommended alert rules per service. |
| [Azure/Mission-Critical-Online](https://github.com/Azure/Mission-Critical-Online) | Reference active-active mission-critical workload. |
| [Azure/Mission-Critical-Connected](https://github.com/Azure/Mission-Critical-Connected) | Mission-critical with private connectivity. |

## Service Quickstarts Worth Running

| Quickstart | Outcome |
| --- | --- |
| [Hub-and-spoke with Bicep](https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke) | Network reference deployment. |
| [Azure Front Door + WAF](https://learn.microsoft.com/azure/frontdoor/quickstart-create-front-door) | Global L7 with WAF. |
| [Cosmos DB multi-region](https://learn.microsoft.com/azure/cosmos-db/nosql/quickstart-portal) | Multi-region writes, consistency tuning. |
| [Azure SQL auto-failover groups](https://learn.microsoft.com/azure/azure-sql/database/auto-failover-group-overview) | Cross-region DR for SQL DB / MI. |
| [Azure Site Recovery for Azure VMs](https://learn.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication) | Region-to-region VM DR. |

## Recommended Practice Path

1. Deploy an **Azure Landing Zone** (Bicep or Terraform) into a sandbox tenant.
2. Run the **WAF self-assessment** against a sample workload; pick three findings to remediate.
3. Add a **hub-and-spoke** baseline with Azure Firewall + private endpoints.
4. Replicate a SQL DB with **auto-failover groups**; test failover.
5. Stand up **mission-critical-online** in a dev subscription; observe active-active patterns.
6. Configure **Sentinel** on a centralized Log Analytics workspace; ingest activity + diagnostics.
7. Apply a **policy initiative** (MCSB) and remediate non-compliant resources.
