# Glossary and Acronym Reference

> Authoritative definitions for the architecture terms, acronyms, and product names that appear in AZ-305 scenarios.

## Identity and Governance

| Term | Definition |
| --- | --- |
| **Entra ID** | Microsoft's identity platform (formerly Azure AD); issues tokens for users, apps, and managed identities. |
| **Tenant** | An instance of Entra ID; the top-level boundary for identity and many governance features. |
| **Management group** | Container above subscriptions used to apply policy and RBAC at scale. |
| **RBAC** | Role-Based Access Control; built-in or custom roles assigned at scopes (MG, sub, RG, resource). |
| **PIM** | Privileged Identity Management; just-in-time elevation with approval and audit. |
| **Conditional Access** | Policy engine that grants/blocks access based on signals (user, device, location, risk). |
| **Azure Policy** | Declarative policy engine; supports audit, deny, append, modify, deployIfNotExists, modify effects. |
| **Initiative** | Group of related policies deployed together (e.g., MCSB baseline). |
| **Blueprints** *(legacy)* | Bundle of templates, policies, and role assignments. Being retired in favor of CAF + Bicep + Policy. |
| **Landing zone** | Pre-built environment template with networking, identity, governance baselines applied. |
| **MCSB** | Microsoft Cloud Security Benchmark; default control framework for Defender for Cloud. |

## Networking

| Term | Definition |
| --- | --- |
| **VNet** | Virtual Network; private IP space in Azure. |
| **Hub-and-spoke** | Networking pattern with shared services in a hub VNet and workload VNets as spokes. |
| **Virtual WAN** | Microsoft-managed global hub fabric for branch and SD-WAN connectivity. |
| **VNet peering** | Private high-throughput connection between two VNets, in same or different regions. |
| **Private endpoint** | NIC in your VNet that exposes an Azure service over a private IP. |
| **Private Link** | Umbrella service powering private endpoints. |
| **Service endpoint** | Older mechanism that grants a subnet access to Azure services via the public endpoint. |
| **NSG** | Network Security Group; stateful 5-tuple rules. |
| **ASG** | Application Security Group; named groups of NICs used in NSG rules. |
| **Azure Firewall** | Stateful firewall-as-a-service with TLS inspection (Premium) and threat intel. |
| **Application Gateway** | Layer-7 load balancer with WAF; HTTP/S only. |
| **Front Door** | Global Layer-7 load balancer + CDN + WAF; multi-region failover. |
| **Traffic Manager** | DNS-based global router (no proxying). |
| **ExpressRoute** | Private connectivity from on-prem to Azure over a partner. |
| **VPN Gateway** | Site-to-site or point-to-site IPsec VPN. |
| **Bastion** | Managed jump-host service for RDP/SSH over HTTPS. |

## Compute

| Term | Definition |
| --- | --- |
| **VMSS** | Virtual Machine Scale Set; identical VMs with autoscale. |
| **Availability Zone** | Physically separate datacenter inside a region. |
| **Availability Set** | Legacy fault/update domain grouping within one datacenter. |
| **AKS** | Azure Kubernetes Service. |
| **Container Apps** | Serverless container runtime built on KEDA + Dapr. |
| **App Service** | PaaS web hosting with deployment slots. |
| **Azure Functions** | Serverless event-driven compute (Consumption, Premium, Dedicated). |

## Data and Storage

| Term | Definition |
| --- | --- |
| **LRS** | Locally Redundant Storage; 3 copies in one zone. |
| **ZRS** | Zone-Redundant Storage; 3 copies across 3 zones in one region. |
| **GRS** | Geo-Redundant Storage; LRS + async copy to paired region. |
| **GZRS** | Geo-Zone-Redundant Storage; ZRS + async copy to paired region. |
| **RA-GRS / RA-GZRS** | Read-Access variants exposing the secondary read endpoint. |
| **Azure SQL Database** | PaaS single database or elastic pool. |
| **SQL Managed Instance** | Near-100% SQL Server compatible PaaS. |
| **Cosmos DB** | Globally distributed, multi-model NoSQL with five consistency levels. |
| **Synapse / Microsoft Fabric** | Analytics platform: SQL pools, Spark, pipelines, Power BI integration. |
| **Data Lake Storage Gen2** | Hierarchical namespace on Blob Storage; primary analytics data lake. |

## Business Continuity

| Term | Definition |
| --- | --- |
| **RTO** | Recovery Time Objective - how long until restored. |
| **RPO** | Recovery Point Objective - acceptable data loss. |
| **Active-active** | Both regions serve traffic simultaneously. |
| **Active-passive** | Secondary region is warm/cold standby. |
| **Azure Backup** | Centralized backup vault for VMs, files, SQL, SAP HANA. |
| **Azure Site Recovery (ASR)** | Replication-based DR for VMs and physical servers. |

## Architecture and Design Frameworks

| Term | Definition |
| --- | --- |
| **WAF** | Well-Architected Framework - five pillars (reliability, security, cost, OpEx, performance). |
| **CAF** | Cloud Adoption Framework - strategy/plan/ready/govern/manage. |
| **Cost pillar** | Right-size, right-tier, eliminate waste, govern via tags + budgets. |
| **Reliability pillar** | Redundancy, recovery design, observability, capacity. |
| **Security pillar** | Identity, network, data, app, operational security. |
| **Operational excellence** | DevOps, IaC, monitoring, automation. |
| **Performance efficiency** | Scaling, caching, content distribution, data partitioning. |

## Monitoring

| Term | Definition |
| --- | --- |
| **Azure Monitor** | Umbrella service for metrics, logs, alerts, autoscale. |
| **Log Analytics workspace** | Log storage + KQL query endpoint for Monitor. |
| **Application Insights** | APM connected to a Log Analytics workspace (workspace-based). |
| **Diagnostic settings** | Resource-side configuration that routes platform/diagnostic logs to a destination. |
| **Action group** | Reusable set of alert recipients/automations. |
| **Microsoft Sentinel** | Cloud-native SIEM/SOAR built on Log Analytics. |
| **Azure Monitor Workbooks** | Customizable dashboards over metrics and logs. |
