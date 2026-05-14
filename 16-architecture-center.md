# Azure Architecture Center

> The **[Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)** is the single most important external resource for AZ-305. It hosts every reference architecture, design pattern, and Well-Architected workload review you may be tested on. Browse the full catalog at [learn.microsoft.com/azure/architecture/browse](https://learn.microsoft.com/en-us/azure/architecture/browse/) and filter by product, category, or scenario.

## How to use it for AZ-305

1. Read the **WAF and CAF** entry pages first; understand the five pillars and the strategy/plan/ready/govern/manage flow.
2. Pick **two or three reference architectures per topic area** (network, data, identity, compute, BCDR) and walk them end-to-end.
3. Memorize the **decision guides** - the exam often boils down to "which service / which redundancy / which topology."
4. Use the **browse filters** (cost, security, reliability) to find the WAF-aligned variant of each scenario.

## Top entry points

| Resource | Why it matters for AZ-305 |
| --- | --- |
| [Architecture Center home](https://learn.microsoft.com/en-us/azure/architecture/) | Curated landing page; start here. |
| [Browse architectures](https://learn.microsoft.com/en-us/azure/architecture/browse/) | Filterable catalog of every reference architecture. |
| [Cloud Adoption Framework](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/) | Governance, landing zones, migration. |
| [Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/) | Five pillars used in AZ-305 design questions. |
| [Cloud design patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/) | Retry, CQRS, Saga, Event Sourcing, Sharding, Throttling. |
| [Best practices for cloud applications](https://learn.microsoft.com/en-us/azure/architecture/best-practices/) | API design, autoscaling, caching, data partitioning. |

## Reference architectures by AZ-305 topic

### Identity and governance

- [Azure landing zones](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)
- [Identity and access management for hybrid](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/identity/)
- [Hybrid identity options](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/identity/azure-ad)

### Networking

- [Hub-and-spoke topology](https://learn.microsoft.com/en-us/azure/architecture/networking/architecture/hub-spoke)
- [Virtual WAN topology](https://learn.microsoft.com/en-us/azure/architecture/networking/architecture/azure-virtual-wan)
- [Multi-region N-tier](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)
- [Private Link in landing zones](https://learn.microsoft.com/en-us/azure/architecture/networking/guide/private-link-virtual-wan)

### Compute and platform

- [AKS production baseline](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks/baseline-aks)
- [Mission-critical online](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks-mission-critical/mission-critical-intro)
- [Reliable web app pattern](https://learn.microsoft.com/en-us/azure/architecture/web-apps/guides/reliable-web-app/overview)
- [Modern web app pattern](https://learn.microsoft.com/en-us/azure/architecture/web-apps/guides/enterprise-app-patterns/modern-web-app/overview)

### Data

- [Choose a data store](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/data-store-decision-tree)
- [Cosmos DB design patterns](https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/modeling-data)
- [Azure SQL DR with auto-failover groups](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)
- [Modern data warehouse with Microsoft Fabric](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/analytics/modern-data-warehouse)

### Business continuity and DR

- [Disaster recovery for Azure applications](https://learn.microsoft.com/en-us/azure/architecture/reliability/disaster-recovery)
- [Backup and DR strategy](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/manage/protect)

### Monitoring and operations

- [Azure Monitor enterprise rollout](https://learn.microsoft.com/en-us/azure/azure-monitor/best-practices)
- [Sentinel deployment guide](https://learn.microsoft.com/en-us/azure/sentinel/deployment-overview)

## Decision guides worth memorizing

- [Choose a compute service](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree)
- [Choose an Azure data service](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/data-store-decision-tree)
- [Choose an Azure messaging service](https://learn.microsoft.com/en-us/azure/service-bus-messaging/compare-messaging-services)
- [Choose a load-balancing service](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview)
- [Choose a containerization option](https://learn.microsoft.com/en-us/azure/architecture/guide/choose-azure-container-service)

## Patterns you should know for the exam

| Pattern | When to apply |
| --- | --- |
| **Retry + Circuit Breaker** | Distributed dependencies; transient failures. |
| **CQRS + Event Sourcing** | Complex domains with high read/write asymmetry. |
| **Saga** | Long-running multi-service transactions with compensations. |
| **Throttling / Queue-Based Load Leveling** | Smooth bursty load on downstream services. |
| **Cache-Aside** | Hot reads for relational/NoSQL stores. |
| **Backends for Frontends (BFF)** | Per-channel APIs over shared microservices. |
| **Strangler Fig** | Gradual replacement of legacy systems. |
| **Bulkhead** | Isolate failure domains across tenants/services. |
