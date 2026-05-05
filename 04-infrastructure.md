# Domain 4 - Infrastructure Solutions

> Maps to AZ-305 measured skill **Design infrastructure solutions** (~30-35%). Reference: [Microsoft Learn AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305) - [Azure compute decision tree](https://learn.microsoft.com/azure/architecture/guide/technology-choices/compute-decision-tree) - [Networking architecture](https://learn.microsoft.com/azure/architecture/networking/) - [Azure Migrate](https://learn.microsoft.com/azure/migrate/migrate-services-overview) - [Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/).

> Big idea: **Compute + Network + Migration + Messaging** = the bones of any Azure solution.

---

## Domain mind map

```mermaid
mindmap
  root((Infrastructure))
    Compute
      VMs IaaS
        VMSS auto-scale
        Spot VMs cheap
        Dedicated Host
      App Service PaaS
        Plans Free B S P I
        Slots blue-green
        ASE isolated
      Functions serverless
        Consumption
        Premium
        Flex Consumption
        App Service plan
      Containers
        ACI tasks/dev
        Container Apps
        AKS production K8s
        ACR registry
      Logic Apps workflows
      Batch HPC
    Networking
      VNet + subnets
      Connectivity
        Peering
        VNet Gateway
        VPN P2S S2S
        ExpressRoute
        Virtual WAN
      Load balancing
        Load Balancer L4
        App Gateway L7 + WAF
        Front Door global L7
        Traffic Manager DNS
      Security
        NSG
        ASG
        Azure Firewall
        DDoS Protection
        Private Endpoint Private Link
      DNS
        Public DNS zone
        Private DNS zone
    Migration
      Azure Migrate hub
        Discovery + Assessment
        Server Migration ASR-based
        Database Migration Service
        Web App Migration Assistant
      Strategies 6 Rs
    Messaging
      Service Bus
      Event Hubs
      Event Grid
      Storage Queues
    APIs
      API Management
        Consumption / Std / Premium
        Self-hosted gateway
```

---

## 1 Compute decision tree (the most important diagram!)

## Scenario patterns to recognize

AZ-305 case-study questions lean hard into infrastructure choice questions: hosting model, hybrid connectivity, regional/global routing, and migration path. The right answer is usually the smallest managed service that satisfies a hard networking or operations requirement.

| Scenario clue | What to think |
|---|---|
| Python/.NET/Java web app, deployment slots, managed platform | **App Service** |
| Event-driven task or integration trigger | **Functions**; use Premium/Flex when VNet or cold-start constraints matter |
| Microservices with no Kubernetes management | **Container Apps** |
| Full Kubernetes, Windows node pools, ingress/service mesh control | **AKS** |
| Dedicated private bandwidth to on-prem | **ExpressRoute** |
| Global HTTPS app with WAF and acceleration | **Front Door Premium** |
| Regional HTTP routing or private backend WAF | **Application Gateway WAF_v2** |
| DNS failover or non-HTTP global endpoint | **Traffic Manager** |
| TCP/UDP inside a region | **Standard Load Balancer** |
| Migrate VMware/Hyper-V/physical servers | **Azure Migrate Server Migration** |

Official weight: **30-35%** of AZ-305.

```mermaid
flowchart TD
    Start[I need to run code/app] --> Q1{Containerized?}

    Q1 -->|No, traditional app| Q2{Need OS control?}
    Q2 -->|Yes - patches, agents,<br/>custom drivers| VM[Virtual Machine<br/>VMSS for scale]
    Q2 -->|No, just want web/API| Q3{Long-running web<br/>or short event?}
    Q3 -->|Long-running website/API| AS[App Service]
    Q3 -->|Event-driven, short tasks| FN[Functions]
    Q3 -->|Multi-step workflow,<br/>connectors, no-code| LA[Logic Apps]

    Q1 -->|Yes, containerized| Q4{Complexity?}
    Q4 -->|One-off task / dev / sidecar| ACI[Container Instances]
    Q4 -->|Microservices, scale to zero,<br/>event-driven, no K8s exposure| CA[Container Apps]
    Q4 -->|Full Kubernetes,<br/>complex orchestration,<br/>hybrid Arc| AKS[AKS]
```

### Compute service comparison

| Service | Model | Scale | Pricing | Best for |
|---|---|---|---|---|
| **VM / VMSS** | IaaS | Manual/auto | Per-second | Legacy, custom OS |
| **App Service** | PaaS | Up to 30 inst | Plan-based | Web/API |
| **Functions** | Serverless | Event-based | Per execution (Consumption) | Triggers, glue |
| **Logic Apps** | iPaaS | Auto | Per action | Workflows w/ 400+ connectors |
| **ACI** | Container | None | Per-second | Burst, single container |
| **Container Apps** | Serverless K8s | KEDA-based, scale to 0 | Per-second | Microservices w/o K8s mgmt |
| **AKS** | Managed K8s | Cluster-level | Node-based | Full K8s, multi-cluster |
| **Batch** | Job scheduler | 1000s of nodes | Per VM | HPC, render |

 **Key exam clues:**
- "Scale to **zero** when idle, microservices, no K8s skills" -> **Container Apps**
- "Full K8s with custom CRDs / Istio / advanced networking" -> **AKS**
- "Run a script when a file lands in blob" -> **Functions** (Blob trigger)
- "Connect SaaS apps with no code" -> **Logic Apps**
- "Web app needs deployment slots / staging" -> **App Service** Standard+

---

## 2 App Service plans - pick the right tier

```mermaid
flowchart LR
    F[Free / Shared<br/>dev only] --> B[Basic<br/>dev/test, 1-3 inst]
    B --> S[Standard<br/>5 slots, autoscale]
    S --> P[Premium v3<br/>30 slots, faster, VNet integration]
    P --> I[Isolated v2 ASE v3<br/>single tenant in your VNet]
```

 **Need VNet integration?** Standard or higher.
 **Need full network isolation?** Isolated (App Service Environment v3).
 **Deployment slots for blue/green?** Standard (5 slots) or Premium (20).

---

## 3 Functions hosting plans

```mermaid
flowchart TD
    Q[How will Functions run?] --> CON[Consumption<br/>Pay per execution<br/>Scale to 0<br/>5-min timeout default]
    Q --> FLEX[Flex Consumption<br/>NEW unified plan<br/>VNet + scale to 0]
    Q --> PREM[Premium<br/>Pre-warmed, VNet, no cold start]
    Q --> APP[App Service plan<br/>Pay for VM, share with web app]
    Q --> CN[Container Apps hosting<br/>Functions in container]
```

 "Avoid cold starts + VNet" historically -> **Premium**. Modern answer: **Flex Consumption**.

---

## 4 Networking - the foundation

### VNet anatomy

```mermaid
flowchart TB
    subgraph VNet[VNet 10.0.0.0/16]
      subgraph Web[Subnet web 10.0.1.0/24]
        VM1[VM]
        NSG1[NSG: allow 443]
      end
      subgraph App2[Subnet app 10.0.2.0/24]
        VM2[VM]
      end
      subgraph DB[Subnet db 10.0.3.0/24]
        SQL[SQL via Private Endpoint]
      end
      subgraph GW[GatewaySubnet]
        VPN[VPN/ER Gateway]
      end
      subgraph Bast[AzureBastionSubnet]
        BS[Bastion]
      end
      subgraph Fw[AzureFirewallSubnet]
        FW[Azure Firewall]
      end
    end
```

 **Reserved subnet names** must match exactly:
- `GatewaySubnet` (VPN/ExpressRoute) - **/27 or larger**
- `AzureBastionSubnet` - **/26 or larger**
- `AzureFirewallSubnet` - **/26 or larger**

---

### Hybrid connectivity decision

```mermaid
flowchart TD
    Q[Connect on-prem to Azure?] --> Q1{Few users / occasional?}
    Q1 -->|Yes, individual laptops| P2S[Point-to-Site VPN<br/>per-user, OpenVPN/IKEv2]
    Q1 -->|No, whole site| Q2{SLA / Bandwidth?}
    Q2 -->|< 1 Gbps, internet ok,<br/>cheap| S2S[Site-to-Site VPN<br/>IPSec over internet]
    Q2 -->|>= 1 Gbps, low latency,<br/>predictable, no internet| ER[ExpressRoute<br/>private circuit]
    Q --> Q3{Many sites + SD-WAN?}
    Q3 -->|Yes hub-spoke at scale| VWAN[Virtual WAN<br/>managed mega-hub]
```

| Option | Bandwidth | SLA | Over internet? | Cost |
|---|---|---|---|---|
| P2S VPN | Modest | None | Yes | $ |
| S2S VPN | Up to ~1.25 Gbps | 99.9-99.95% | Yes | $$ |
| **ExpressRoute** | 50 Mbps - 100 Gbps | **99.95%** | **No** (private) | $$$$ |
| ER + VPN failover | Mix | High | Mix | $$$$ |

 **Exam:** "Predictable bandwidth, no internet path, SLA" -> **ExpressRoute**.

---

### VNet peering

```mermaid
flowchart LR
    V1[VNet East US] <-.peering.-> V2[VNet East US 2]
    V1 <-.global peering.-> V3[VNet West EU]

    note1[Low latency<br/> Private Microsoft backbone<br/> Non-transitive by default]
```

 **A->B peered, B->C peered, but A<->C? NO** unless you use **Hub-spoke + NVA/Firewall + UDRs**, or **Virtual WAN**.

---

### Load balancing - pick the right device

```mermaid
flowchart TD
    Q{What traffic?} --> L4{Layer 4<br/>TCP/UDP, any protocol}
    Q --> L7{Layer 7<br/>HTTP/HTTPS only}

    L4 --> Q4{Scope}
    Q4 -->|Inside region| LB[Azure Load Balancer<br/>Public or Internal]
    Q4 -->|Across regions| CLB[Cross-region LB<br/>or Traffic Manager]

    L7 --> Q5{Scope}
    Q5 -->|Inside region<br/>WAF, URL routing| AG[Application Gateway<br/>+ WAF v2]
    Q5 -->|Global<br/>WAF + CDN + acceleration| AFD[Front Door<br/>Premium]
```

| Service | Layer | Scope | WAF | URL routing |
|---|---|---|---|---|
| **Load Balancer** | L4 | Regional / Global | |  |
| **App Gateway** | L7 | Regional | |  |
| **Front Door** | L7 | **Global** | |  |
| **Traffic Manager** | DNS | Global | |  |

 **Combo on the exam:** "Global users, WAF, fastest" -> **Front Door**.
 "Internal microservice over arbitrary TCP" -> **Internal Standard Load Balancer**.

---

### Network security stack

```mermaid
flowchart TD
    INET[Internet] --> DDOS[Azure DDoS Protection]
    DDOS --> AFD2[Front Door / WAF]
    AFD2 --> FW[Azure Firewall<br/>L3-L7, FQDN, threat intel]
    FW --> NSG[NSG on subnet/NIC<br/>5-tuple rules]
    NSG --> ASG[ASG: group VMs by role<br/>web / app / db]
    ASG --> VM[VM]

    PE[Private Endpoint<br/>private IP for PaaS] -.removes.-> INET
```

| Tool | Purpose |
|---|---|
| **NSG** | Stateful 5-tuple rules at subnet/NIC |
| **ASG** | Logical groups of VMs to use in NSG rules |
| **Azure Firewall** | Centralized stateful firewall, FQDN filtering, threat intel |
| **DDoS Protection Std/IP** | L3/L4 DDoS mitigation |
| **WAF** | L7 OWASP Top 10 (in App GW or Front Door) |
| **Private Endpoint** | Inject PaaS into your VNet via private IP |

 **NSG vs Azure Firewall:** NSG is free, basic; Firewall is paid, central, FQDN-aware, with logging.

---

### Azure DNS

```mermaid
flowchart LR
    PUB[Public DNS Zone<br/>e.g., contoso.com] --> WORLD[Resolvable from internet]
    PRV[Private DNS Zone<br/>e.g., privatelink.database.windows.net] --> VNETS[Resolvable inside linked VNets only]
    PE2[Private Endpoint<br/>auto-creates A record in Private DNS Zone] --> PRV
```

 **Private Endpoint setup always needs the matching Private DNS Zone** (e.g., `privatelink.blob.core.windows.net`) **linked to the VNet**.

---

## 5 Migration - Azure Migrate

```mermaid
flowchart LR
    subgraph Migrate[Azure Migrate Hub]
      D[Discovery + Assessment]
      SM[Server Migration]
      DMS[Database Migration Service]
      WMA[Web App Migration Assistant]
      DM[Data Box for huge data]
    end
    OnPrem[VMware / Hyper-V / Physical / AWS / GCP] --> D
    D --> Plan[Cost + Readiness report]
    Plan --> SM --> AzureVM[Azure VM]
    Plan --> DMS --> AzureSQL[Azure SQL DB / MI]
    Plan --> WMA --> AppSvc[App Service]
```

### The "6 Rs" of migration

```mermaid
flowchart LR
    R1[Rehost<br/>lift-and-shift to VM] --> R2[Refactor<br/>minor tweaks -> PaaS]
    R2 --> R3[Rearchitect<br/>break monolith -> microservices]
    R3 --> R4[Rebuild<br/>cloud-native rewrite]
    R5[Replace<br/>SaaS] --> R6[Retire / Retain]
```

 **Exam:**
- "Move on-prem SQL to Azure with **minimal downtime**" -> **DMS online migration**
- "Move VMware VMs to Azure" -> **Azure Migrate Server Migration** (uses ASR under the hood)
- "Move ASP.NET site quickly" -> **Web App Migration Assistant** -> App Service

---

## 6 API Management - the API gateway

```mermaid
flowchart LR
    DEV[Developers] --> APIM[API Management<br/>Gateway]
    APP[Apps] --> APIM
    APIM --> Backend1[Function]
    APIM --> Backend2[App Service]
    APIM --> Backend3[On-prem API via VNet]
    APIM --> Pol[Policies: auth, rate limit, transform, cache]
```

| Tier | When |
|---|---|
| **Consumption** | Pay-per-call, serverless, no VNet inject |
| **Basic / Standard v2** | Modern, VNet integration |
| **Premium** | Multi-region, full VNet, self-hosted gateways |

---

## 7 Messaging recap (also covered in Domain 2)

```mermaid
flowchart LR
    SQ[Storage Queue<br/>simple] --> SB[Service Bus<br/>enterprise FIFO sessions]
    SB --> EH[Event Hubs<br/>massive stream]
    EH --> EG[Event Grid<br/>reactive routing]
```

---

## Domain 4 cheat-sheet

| Scenario | Answer |
|---|---|
| Scale-to-zero microservices, no K8s | **Container Apps** |
| Need full K8s with CRDs | **AKS** |
| Run script when blob arrives | **Functions** Blob trigger |
| Multi-step SaaS workflow no-code | **Logic Apps** |
| Web app + 5 deployment slots | **App Service Standard** |
| Web app fully isolated in VNet | **App Service Environment v3** (Isolated) |
| Site-to-site, dedicated, SLA | **ExpressRoute** |
| Few remote workers connect to VNet | **P2S VPN** |
| Global HTTPS site + WAF + CDN | **Front Door Premium** |
| Regional HTTPS app + WAF | **App Gateway v2 + WAF** |
| Internal LB for arbitrary TCP | **Standard LB Internal** |
| Group VMs by role for NSG rules | **ASG** |
| Central FQDN filtering with threat intel | **Azure Firewall** |
| Inject PaaS into VNet (private IP) | **Private Endpoint** |
| Resolve `privatelink.*` for PE | **Private DNS Zone** linked to VNet |
| Migrate VMware to Azure | **Azure Migrate Server Migration** |
| Online migrate SQL to Azure | **Database Migration Service** |
| Hub-spoke at scale, SD-WAN | **Virtual WAN** |
| Single API front for many backends | **API Management** |

---

## References (Microsoft Learn)

- [AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305)
- [Compute decision tree](https://learn.microsoft.com/azure/architecture/guide/technology-choices/compute-decision-tree)
- [App Service plans](https://learn.microsoft.com/azure/app-service/overview-hosting-plans) - [Azure Functions hosting](https://learn.microsoft.com/azure/azure-functions/functions-scale)
- [Azure Kubernetes Service](https://learn.microsoft.com/azure/aks/intro-kubernetes) - [Container Apps](https://learn.microsoft.com/azure/container-apps/overview)
- [Hub-and-spoke topology](https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke) - [Virtual WAN](https://learn.microsoft.com/azure/virtual-wan/virtual-wan-about)
- [ExpressRoute](https://learn.microsoft.com/azure/expressroute/expressroute-introduction) - [VPN Gateway](https://learn.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)
- [Private Endpoint](https://learn.microsoft.com/azure/private-link/private-endpoint-overview) - [Azure Firewall](https://learn.microsoft.com/azure/firewall/overview)
- [Azure Migrate](https://learn.microsoft.com/azure/migrate/migrate-services-overview) - [Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/)
- [API Management](https://learn.microsoft.com/azure/api-management/api-management-key-concepts)

 **Final stop:** [05-exam-cheatsheet.md](05-exam-cheatsheet.md)
