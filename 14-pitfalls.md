# Common Pitfalls and Distractor Patterns

> Mistakes that look right on the AZ-305 exam but lose points. Each entry pairs the wrong choice candidates pick with the correct one and the rule.

## Storage Redundancy

### Choosing GRS when ZRS is the right pillar answer

**Pitfall**: "Recommend GRS for high availability."

**Reality**: Read carefully. **ZRS** = zone redundancy in one region (HA). **GRS** = cross-region redundancy (DR). If the requirement is "tolerate datacenter failure" without cross-region cost/latency, ZRS wins. GRS is for regional disasters.

### RA-GRS for "high availability"

**Pitfall**: Adding read-access purely "for HA."

**Reality**: RA-GRS adds a *secondary read endpoint* in the paired region. The primary remains the same. Use it when readers can tolerate eventual consistency and you need read offload - not as a generic HA upgrade.

### LRS in a tier-1 production design

**Pitfall**: LRS "to save cost" on production-tier storage.

**Reality**: WAF reliability pillar fails. LRS is acceptable only for low-tier dev/test, easily reproduced data, or when locality is mandated.

## Compute Selection

### AKS when Container Apps is sufficient

**Pitfall**: Defaulting to AKS for any container workload.

**Reality**: WAF operational excellence + cost pillars fail when you don't need full Kubernetes. **Azure Container Apps** is the higher-abstraction default; AKS is for fine-grained Kubernetes control or existing K8s investments.

### Functions Consumption for low-latency tier-1

**Pitfall**: Consumption plan for a tier-1 API with strict latency SLAs.

**Reality**: Cold starts hurt P95. Use **Functions Premium** or **App Service plan** for predictable latency, or move to Container Apps with min-replicas > 0.

### Availability sets in zone-capable regions

**Pitfall**: Using availability sets when zones are available.

**Reality**: **Availability zones** beat availability sets for resilience (separate datacenters vs separate racks). Choose AS only when zones are unavailable in the region.

## Networking

### Service endpoints for "lockdown"

**Pitfall**: Selecting service endpoints over private endpoints.

**Reality**: Service endpoints route traffic to the *public endpoint* over the Microsoft backbone with a subnet-level firewall rule - the resource is still publicly reachable. **Private endpoints** assign a private IP in your VNet and remove the public surface (when public access is disabled). Default to private endpoints.

### Application Gateway in front of non-HTTP workloads

**Pitfall**: Using Application Gateway for a TCP/UDP service.

**Reality**: App Gateway is HTTP/HTTPS only. Use **Azure Load Balancer** (regional L4) or **Front Door** (global L7 HTTP only).

### Front Door without WAF on internet-facing apps

**Pitfall**: Front Door Standard for a tier-1 internet workload.

**Reality**: Standard has no WAF. Use **Front Door Premium** for WAF, bot protection, and Private Link to origins.

### Hub-and-spoke when Virtual WAN fits

**Pitfall**: Building 12 paired hubs across global regions.

**Reality**: **Virtual WAN** provides Microsoft-managed any-to-any connectivity, integrated SD-WAN, and centralized routing. Hub-and-spoke is right for a small number of hubs.

## Identity and Governance

### Owner where Contributor (or less) suffices

**Pitfall**: Granting Owner "to be safe."

**Reality**: WAF security pillar fails. Owner includes role assignments - least-privilege is **Contributor** or, more often, a **specific resource-type role** (e.g., VM Contributor, Storage Blob Data Contributor).

### Permanent privileged role assignments

**Pitfall**: Assigning Owner to a person permanently for occasional admin.

**Reality**: **PIM** with JIT activation, approval, MFA, and audit is the WAF answer.

### Policy `audit` instead of `deny`

**Pitfall**: Using `audit` when the requirement is to *prevent* non-compliant resources.

**Reality**: `audit` only logs. `deny` blocks the deployment. `deployIfNotExists` remediates after creation.

### Subscriptions for governance grouping

**Pitfall**: Splitting subscriptions per team for "policy isolation."

**Reality**: **Management groups** are the policy/RBAC grouping primitive - they inherit. Subscriptions are billing/quota/network boundaries. Don't proliferate subs for governance alone.

## Data and Databases

### Cosmos DB Strong consistency across regions

**Pitfall**: Strong consistency on a multi-region write account.

**Reality**: Strong significantly increases write latency and limits configurations. **Bounded staleness** is the recommended default for multi-region writes when you need ordered, near-real-time consistency.

### SQL DB when SQL MI's features are required

**Pitfall**: Picking SQL Database when the workload uses SQL Agent, linked servers, cross-DB queries, or Service Broker.

**Reality**: **SQL Managed Instance** offers near-100% SQL Server compatibility. SQL Database is single-DB cloud-native and lacks instance-scoped features.

### Synapse Dedicated SQL for ad-hoc small analytics

**Pitfall**: Dedicated SQL pools for sub-TB workloads.

**Reality**: Dedicated SQL pools are for sustained large analytics. **Serverless SQL** in Synapse/Fabric or **Microsoft Fabric** is correct for ad-hoc and small workloads.

## Business Continuity

### "Backup is DR"

**Pitfall**: Treating Azure Backup as a DR solution.

**Reality**: **Azure Backup** is point-in-time recovery; restore can be slow. **Azure Site Recovery** replicates VMs continuously for low-RTO failover. Combine: ASR for DR, Backup for retention/compliance.

### Active-active when active-passive meets the SLA

**Pitfall**: Defaulting to active-active.

**Reality**: Active-active doubles cost and increases data-conflict complexity. Choose only when RTO/SLA truly requires it or when geo-load distribution is the goal.

## Monitoring

### Diagnostic logs to Storage when Sentinel is the goal

**Pitfall**: Routing diagnostics to a Storage account, then trying to query for security.

**Reality**: **Log Analytics** is the queryable destination for KQL and Sentinel. Storage is cheap archive only. Event Hubs is for streaming to third-party SIEMs.

### One Log Analytics workspace per resource

**Pitfall**: Workspace sprawl.

**Reality**: **Centralize** workspaces by sovereignty and access boundary, not by resource. Multiple workspaces are correct only when data residency or RBAC requires separation.
