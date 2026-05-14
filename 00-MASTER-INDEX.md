# AZ-305 Visual Study Guide - Master Index

> **Designing Microsoft Azure Infrastructure Solutions**
> Built from the Microsoft Learn skills measured as of **April 17, 2026**. This is a concept study layer: diagrams, decision trees, and original summaries only.

---

## How to use this guide

```mermaid
flowchart LR
    A[Start Here<br/>Master Index] --> B[Read Mind Map<br/>below]
    B --> C[Pick a Domain<br/>1->4]
    C --> D[Study Diagrams]
    D --> E[Memorize<br/>Decision Trees]
    E --> F[Decision Reference<br/>final review]
    F --> G[Exam Ready]
```

---

## The 4 Exam Domains - Mind Map

```mermaid
mindmap
  root((AZ-305))
    Identity Governance Monitoring
      Microsoft Entra ID
        Tenants
        B2B B2C External ID
        Conditional Access
        PIM
        Hybrid Identity
      Governance
        Management Groups
        Subscriptions
        Azure Policy
        RBAC vs Entra Roles
        Blueprints Landing Zones
      Monitoring
        Log Analytics
        Azure Monitor
        Application Insights
        Alerts Action Groups
    Data Storage
      Relational
        SQL DB
        SQL MI
        SQL on VM
        PostgreSQL MySQL
      NoSQL
        Cosmos DB APIs
        Consistency Levels
        Partition Keys
      Storage Accounts
        Blob Hot Cool Archive
        Files SMB NFS
        Queues Tables
        Lifecycle Mgmt
      Big Data
        Synapse
        Data Lake Gen2
        Data Factory
        Databricks
    Business Continuity
      Backup
        Recovery Services Vault
        Backup Center
        Soft delete
      Site Recovery ASR
        VM replication
        RTO RPO
      HA Patterns
        Availability Zones
        Availability Sets
        Paired Regions
      Data Replication
        LRS ZRS GRS RA-GRS
        SQL Auto-failover
        Cosmos multi-region
    Infrastructure
      Compute
        VMs VMSS
        App Service
        Functions
        AKS ACI
        Container Apps
      Networking
        VNet Subnets
        Peering vs VPN
        ExpressRoute
        Load Balancer
        App Gateway WAF
        Front Door CDN
        Private Endpoint
        Firewall NSG
      Migration
        Azure Migrate
        Database Migration
      Messaging
        Service Bus
        Event Hubs
        Event Grid
        Storage Queues
```

---

## Official Skills Weighting

```mermaid
pie title AZ-305 Skills Measured Midpoints
  "Identity, Governance, Monitoring" : 27.5
  "Data Storage" : 22.5
  "Business Continuity" : 17.5
  "Infrastructure" : 32.5
```

| Slice | Weight | Jump to chapter |
| --- | --- | --- |
| Identity, Governance, Monitoring | **25-30%** | [01 Identity + Governance](01-identity-governance-monitoring.md) |
| Data Storage | **20-25%** | [02 Data Storage](02-data-storage.md) |
| Business Continuity | **15-20%** | [03 Business Continuity](03-business-continuity.md) |
| Infrastructure | **30-35%** | [04 Infrastructure](04-infrastructure.md) |

Coverage note: this guide follows the official Microsoft Learn AZ-305 [skills measured](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305) weights - Identity, Governance, and Monitoring **25-30%**, Data Storage **20-25%**, Business Continuity **15-20%**, and Infrastructure **30-35%**.

---

## Service emphasis map

```mermaid
flowchart LR
    CASES[Common AZ-305 scenarios] --> SQL[Azure SQL choices]
    CASES --> STORE[Storage accounts and Blob tiers]
    CASES --> NET[Hybrid and private networking]
    CASES --> APP[App hosting and containers]
    CASES --> GOV[Policy, RBAC, managed identity]
    CASES --> BCDR[Backup, ASR, failover]

    SQL --> SQLA[SQL DB vs SQL MI vs VM]
    STORE --> STOREA[LRS/ZRS/GRS, private endpoint, lifecycle]
    NET --> NETA[ExpressRoute, VPN, Front Door, App Gateway]
    APP --> APPA[App Service, Functions, AKS, Container Apps]
    GOV --> GOVA[DeployIfNotExists, custom RBAC, Key Vault]
    BCDR --> BCDRA[RTO/RPO, vaults, geo-replication]
```

Core services to know across the four measured skills: Azure SQL Database, Blob/Storage accounts, ExpressRoute, virtual networks, AKS, App Service, Azure Policy, Data Factory, Cosmos DB, Load Balancer, Application Gateway, Traffic Manager, Log Analytics, Key Vault, Synapse, Databricks, and managed identity.

---

## Domain files in this guide

| # | Domain | File | Focus |
|---|--------|------|-------|
| 1 | Identity, Governance & Monitoring | [01-identity-governance-monitoring.md](01-identity-governance-monitoring.md) | Entra ID, RBAC, Policy, Monitor |
| 2 | Data Storage Solutions | [02-data-storage.md](02-data-storage.md) | SQL, Cosmos, Blob, Synapse |
| 3 | Business Continuity (BCDR) | [03-business-continuity.md](03-business-continuity.md) | Backup, ASR, HA, Replication |
| 4 | Infrastructure Solutions | [04-infrastructure.md](04-infrastructure.md) | Compute, Network, Migration |
| | **Exam Decision Reference** | [05-exam-cheatsheet.md](05-exam-cheatsheet.md) | Decision trees + scenario keyword map |
| | **Concept & Reference Index** | [06-references.md](06-references.md) | Every concept linked to Microsoft Learn |
| + | **Extra Concepts** | [07-extra-az305-concepts.md](07-extra-az305-concepts.md) | Architecture method + edge-case concepts |
| + | **Microsoft Learn Summaries** | [08-learn-summaries.md](08-learn-summaries.md) | Per-service overviews + recreated architecture diagrams |
| + | **Architectures - AZ-305** | [09-arch-az305.md](09-arch-az305.md) | Reference architectures mapped to each skill area |

---

## The 7 Core Question Patterns in AZ-305

```mermaid
flowchart TD
    Q[Any AZ-305 Question] --> P1
    Q --> P2
    Q --> P3
    Q --> P4
    Q --> P5
    Q --> P6
    Q --> P7

    P1["1 Choose cheapest service<br/>that meets requirements"]
    P2["2 Minimize admin effort"]
    P3["3 Achieve specific RTO/RPO"]
    P4["4 Least privilege access"]
    P5["5 Hybrid / on-prem connectivity"]
    P6["6 Auto-scale / elasticity"]
    P7["7 Geo-redundancy / global"]

    P1 --> R1[Pick PaaS over IaaS<br/>Functions/Logic Apps over VMs]
    P2 --> R2[Managed services<br/>SQL DB > SQL MI > SQL on VM]
    P3 --> R3[Backup=long RPO<br/>ASR=minutes<br/>Active-active=zero]
    P4 --> R4[Managed Identity<br/>Custom RBAC role<br/>PIM for admins]
    P5 --> R5[VPN cheap<br/>ExpressRoute SLA<br/>Private Endpoint = no public IP]
    P6 --> R6[VMSS for VMs<br/>App Service for web<br/>AKS for containers]
    P7 --> R7[Front Door=global HTTP<br/>Traffic Manager=DNS<br/>GRS for storage]
```

---

## The "Magic Words" Translator

When the exam uses these phrases, immediately think of these services:

```mermaid
flowchart LR
    subgraph Triggers
      T1["'minimize cost'"]
      T2["'minimize admin effort'"]
      T3["'serverless'"]
      T4["'no public internet exposure'"]
      T5["'on-prem AD users'"]
      T6["'global low-latency'"]
      T7["'compliance / audit'"]
      T8["'just-in-time admin'"]
      T9["'replicate VM to another region'"]
      T10["'lift and shift'"]
    end
    subgraph Answers
      A1["Consumption / PaaS / Cool tier"]
      A2["PaaS, Managed Identity"]
      A3["Functions / Logic Apps / Cosmos serverless"]
      A4["Private Endpoint + Private Link"]
      A5["Entra Connect (sync) or Cloud Sync"]
      A6["Front Door / Cosmos multi-region writes"]
      A7["Azure Policy + Microsoft Defender + Activity Log"]
      A8["Privileged Identity Management (PIM)"]
      A9["Azure Site Recovery (ASR)"]
      A10["IaaS VM + Azure Migrate"]
    end
    T1 --> A1
    T2 --> A2
    T3 --> A3
    T4 --> A4
    T5 --> A5
    T6 --> A6
    T7 --> A7
    T8 --> A8
    T9 --> A9
    T10 --> A10
```

---

## Recommended study order

```mermaid
gantt
    title Suggested 8-day plan
    dateFormat X
    axisFormat Day %d
    section Core
    Identity & Governance :a1, 0, 1d
    Monitoring add-on :a2, after a1, 1d
    Data Storage :a3, after a2, 2d
    section Resilience & Infra
    Business Continuity :b1, after a3, 1d
    Infrastructure :b2, after b1, 1d
    section Final
    Decision reference + practice :c1, after b2, 2d
```

 **Next:** open [01-identity-governance-monitoring.md](01-identity-governance-monitoring.md)
