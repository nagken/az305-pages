# Flashcards: Active Recall

> Click any card to reveal the answer. Use the **Domain pager bottom-right** to switch between exam areas. ~50 cards across 6 domains.

<section class="fc-section" data-fc-title="Identity, Governance & Monitoring">
<h2>1 - Identity, Governance & Monitoring</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Entra ID tenant vs subscription?</div><div class="fc-a">Tenant = identity boundary. Subscription = billing + RBAC scope, trusts one tenant.</div></div>

<div class="flashcard"><div class="fc-q">When use B2B vs B2C?</div><div class="fc-a">B2B = invite partners (guest users in your tenant). B2C = customer identity (separate tenant, social IdPs, custom flows).</div></div>

<div class="flashcard"><div class="fc-q">Privileged Identity Management (PIM) - purpose?</div><div class="fc-a">Just-in-time, time-bound, approval-required role activation. Reduce standing privilege.</div></div>

<div class="flashcard"><div class="fc-q">Management group hierarchy max depth?</div><div class="fc-a"><strong>6 levels</strong> below root. Apply policy/RBAC at scale.</div></div>

<div class="flashcard"><div class="fc-q">Azure Policy <code>deny</code> vs <code>audit</code> vs <code>deployIfNotExists</code>?</div><div class="fc-a">deny = block. audit = log non-compliance. DINE = remediate by deploying corrective resource.</div></div>

<div class="flashcard"><div class="fc-q">Workload identity vs managed identity?</div><div class="fc-a">Managed identity = Azure-resource identity. Workload identity federation = external (GitHub/AKS) -> Entra without secrets.</div></div>

<div class="flashcard"><div class="fc-q">Long-term, cheap log retention strategy?</div><div class="fc-a">Log Analytics interactive (<=730d) -> <strong>archive tier</strong> (<=12y) or export to storage cool/cold.</div></div>

<div class="flashcard"><div class="fc-q">Azure Lighthouse use case?</div><div class="fc-a">Cross-tenant management - MSP/SI manages customer subs from their tenant via delegated RBAC.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Data Storage">
<h2>2 - Data Storage</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Cosmos DB API choices?</div><div class="fc-a">NoSQL (core), MongoDB, Cassandra, Gremlin (graph), Table, PostgreSQL (Citus).</div></div>

<div class="flashcard"><div class="fc-q">Cosmos consistency levels?</div><div class="fc-a">Strong, Bounded staleness, Session (default), Consistent prefix, Eventual.</div></div>

<div class="flashcard"><div class="fc-q">Choose partition key - what matters?</div><div class="fc-a">High cardinality, even RU/storage distribution, included in most queries.</div></div>

<div class="flashcard"><div class="fc-q">Azure SQL DB Hyperscale vs Business Critical?</div><div class="fc-a">Hyperscale = up to 100TB, fast scale. BC = OLTP low latency, in-memory, AG read replicas, 99.995%.</div></div>

<div class="flashcard"><div class="fc-q">Synapse dedicated vs serverless SQL pool?</div><div class="fc-a">Dedicated = provisioned MPP DW. Serverless = pay-per-query over data lake (Parquet/Delta).</div></div>

<div class="flashcard"><div class="fc-q">Storage tiers Hot/Cool/Cold/Archive break-even?</div><div class="fc-a">Cool 30d+, Cold 90d+, Archive 180d+. Archive needs rehydrate (hours).</div></div>

<div class="flashcard"><div class="fc-q">When use ADLS Gen2 vs Blob?</div><div class="fc-a">ADLS Gen2 = blob + hierarchical namespace + POSIX ACL -> big-data analytics. Blob = generic object.</div></div>

<div class="flashcard"><div class="fc-q">Shared key vs SAS vs Entra auth on storage?</div><div class="fc-a">Prefer Entra (RBAC + identity). SAS = scoped time-limited URL. Shared key = avoid (broad access).</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Business Continuity (BCDR)">
<h2>3 - Business Continuity (BCDR)</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">RPO vs RTO?</div><div class="fc-a">RPO = data-loss tolerance (how fresh). RTO = downtime tolerance (how fast back).</div></div>

<div class="flashcard"><div class="fc-q">VM HA: AS vs AZ vs region pair?</div><div class="fc-a">AS 99.95% (1 DC, FD/UD). AZ 99.99% (zones in region). Region pair = paired region async replication / failover.</div></div>

<div class="flashcard"><div class="fc-q">ASR vs Azure Backup?</div><div class="fc-a">ASR = replication for DR (failover, low RPO). Backup = point-in-time restore from corruption/loss.</div></div>

<div class="flashcard"><div class="fc-q">Azure SQL DB geo-replication options?</div><div class="fc-a">Active geo-replication (per-DB), Auto-failover groups (server-level), Geo-restore from automated backup.</div></div>

<div class="flashcard"><div class="fc-q">Cosmos multi-region writes - when?</div><div class="fc-a">Active-active globally distributed writes; lower write latency, higher availability. Adds RU cost & conflict resolution.</div></div>

<div class="flashcard"><div class="fc-q">Storage RA-GRS vs GZRS?</div><div class="fc-a">RA-GRS = LRS + paired-region async + read access. GZRS = ZRS + paired-region async (read with RA-GZRS).</div></div>

<div class="flashcard"><div class="fc-q">App Service multi-region pattern?</div><div class="fc-a">Two regions + Front Door / Traffic Manager (priority/perf). Sync content via Run-From-Package + storage.</div></div>

<div class="flashcard"><div class="fc-q">AKS DR pattern?</div><div class="fc-a">Stateless: 2 clusters paired regions + Front Door. Stateful: ASR for disks, or DB-native replication. GitOps for parity.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Infrastructure (Compute & Network)">
<h2>4 - Infrastructure (Compute & Network)</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">VMSS Flexible vs Uniform - pick?</div><div class="fc-a">Flexible = mixed SKUs, FD across zones, IaaS-style control. Uniform = identical, scale to thousands.</div></div>

<div class="flashcard"><div class="fc-q">App Gateway vs Front Door vs Load Balancer?</div><div class="fc-a">LB = L4 regional. App Gateway = L7 regional + WAF. Front Door = global L7 + WAF + CDN.</div></div>

<div class="flashcard"><div class="fc-q">Hub-and-spoke topology - why?</div><div class="fc-a">Centralize shared services (firewall, DNS, ER/VPN) in hub; spokes peer to hub. Cost + governance + segmentation.</div></div>

<div class="flashcard"><div class="fc-q">VPN vs ExpressRoute vs ER Direct?</div><div class="fc-a">VPN = internet IPsec. ER = private circuit via partner. ER Direct = 10/100Gbps direct port at peering location.</div></div>

<div class="flashcard"><div class="fc-q">Private endpoint vs service endpoint?</div><div class="fc-a">Private endpoint = NIC in your VNet, private IP. Service endpoint = VNet identity over public IP. Prefer private endpoint.</div></div>

<div class="flashcard"><div class="fc-q">Azure Firewall vs NVA?</div><div class="fc-a">Firewall = managed, scalable, FQDN/threat intel/DNAT. NVA = 3rd-party (Palo, Fortinet) for feature parity.</div></div>

<div class="flashcard"><div class="fc-q">When use AKS vs Container Apps vs App Service?</div><div class="fc-a">AKS = full k8s control. Container Apps = serverless containers + Dapr/KEDA. App Service = managed PaaS for web/API.</div></div>

<div class="flashcard"><div class="fc-q">Stretch a VNet across regions?</div><div class="fc-a">Not directly - use <strong>Global VNet peering</strong> or VNet-to-VNet VPN. No subnet spanning regions.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Migration & Modernization">
<h2>5 - Migration & Modernization</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">5 Rs of migration?</div><div class="fc-a">Rehost (lift-shift), Refactor (small change), Rearchitect, Rebuild, Replace (SaaS).</div></div>

<div class="flashcard"><div class="fc-q">Azure Migrate components?</div><div class="fc-a">Discovery & assessment, Server Migration (replication), DB Migration (DMS), Web App Migration assistant.</div></div>

<div class="flashcard"><div class="fc-q">SQL on-prem -> Azure SQL MI vs Azure SQL DB?</div><div class="fc-a">MI = highest compat (CLR, agent, cross-DB queries), VNet. DB = single/elastic pool, no SQL Agent.</div></div>

<div class="flashcard"><div class="fc-q">Storage migration: AzCopy vs Data Box vs Data Factory?</div><div class="fc-a">AzCopy = network-bound bulk. Data Box = ship physical, TB-PB offline. ADF = pipeline w/ transform.</div></div>

<div class="flashcard"><div class="fc-q">Refactor monolith to microservices on Azure?</div><div class="fc-a">Strangler fig: API gateway (APIM/Front Door) routes new endpoints to new microservices (AKS/ACA), legacy serves rest.</div></div>

<div class="flashcard"><div class="fc-q">Lift-and-shift Windows app w/ minimal change?</div><div class="fc-a">Azure VM (IaaS) or App Service if web app passes assessment. Migrate Server Migration for replication.</div></div>

<div class="flashcard"><div class="fc-q">Modernize data warehouse to cloud-scale?</div><div class="fc-a">Migrate to Synapse Dedicated SQL Pool or Microsoft Fabric (lakehouse + warehouse + OneLake).</div></div>

<div class="flashcard"><div class="fc-q">Cosmos DB migration tools?</div><div class="fc-a">Native Cosmos migration tool, ADF, Azure Databricks, Spark connector - depending on source API.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Application Architecture">
<h2>6 - Application Architecture</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Event-driven decoupling - Service Bus vs Event Grid vs Event Hubs?</div><div class="fc-a">Service Bus = enterprise messaging (FIFO, sessions, DLQ). Event Grid = reactive event routing. Event Hubs = high-throughput telemetry/streaming.</div></div>

<div class="flashcard"><div class="fc-q">Saga pattern - what does it solve?</div><div class="fc-a">Distributed transactions across microservices via compensating actions instead of 2PC.</div></div>

<div class="flashcard"><div class="fc-q">CQRS - when worth it?</div><div class="fc-a">Read-heavy + complex queries diverging from write model. Pair with materialized views / event sourcing.</div></div>

<div class="flashcard"><div class="fc-q">Cache-aside (lazy load) pattern?</div><div class="fc-a">App reads cache -> on miss reads DB and populates cache. Use Azure Cache for Redis.</div></div>

<div class="flashcard"><div class="fc-q">APIM use cases?</div><div class="fc-a">Facade, throttling, transformation, auth (JWT/OAuth), versioning, caching, AI Gateway.</div></div>

<div class="flashcard"><div class="fc-q">Functions vs Logic Apps vs Durable Functions?</div><div class="fc-a">Functions = code triggers. Logic Apps = visual connectors/integrations. Durable Functions = stateful workflows in code.</div></div>

<div class="flashcard"><div class="fc-q">Serverless cold start mitigation?</div><div class="fc-a">Premium plan (pre-warmed), Always Ready instances, smaller deps, container w/ App Service plan, Flex Consumption.</div></div>

<div class="flashcard"><div class="fc-q">Securely connect App Service to backend SQL?</div><div class="fc-a">Managed identity + Entra auth on SQL + VNet integration + private endpoint on SQL. No connection strings with passwords.</div></div>

</div>
</section>