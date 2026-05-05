# Domain 3 - Business Continuity (BCDR)

> Maps to AZ-305 measured skill **Design business continuity solutions** (~15-20%). Reference: [Microsoft Learn AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305) - [Azure Backup](https://learn.microsoft.com/azure/backup/backup-overview) - [Azure Site Recovery](https://learn.microsoft.com/azure/site-recovery/site-recovery-overview) - [Azure regions and availability zones](https://learn.microsoft.com/azure/reliability/availability-zones-overview) - [Storage redundancy](https://learn.microsoft.com/azure/storage/common/storage-redundancy).

> Big idea: **How fast** can we recover (RTO), and **how much data** can we lose (RPO)?

---

## Domain mind map

```mermaid
mindmap
  root((Business Continuity))
    The Two Numbers
      RTO time to recover
      RPO data loss tolerance
    High Availability
      Availability Sets fault/update domains
      Availability Zones 99.99 SLA
      Paired Regions
      VMSS auto-recovery
      Load balancing
    Backup
      Recovery Services Vault
      Azure Backup
        VMs Files Disks
        SQL in VM
        SAP HANA
      Soft delete on by default
      Backup Center single pane
      Immutable vault ransomware
    Disaster Recovery
      Azure Site Recovery ASR
        VM replication cross-region
        Recovery Plans
        Test failover
        Failover and failback
      DB-specific DR
        SQL Auto-failover groups
        Cosmos multi-region
        Storage GRS / RA-GRS
      App-level DR
        Front Door multi-region
        Traffic Manager
    Patterns
      Active-Passive cold
      Active-Passive warm
      Active-Active
      Pilot Light
      Hot Standby
```

---

## 1 The two sacred numbers

## Scenario patterns to recognize

AZ-305 case-study questions keep coming back to recovery objective matching. Read every BCDR scenario in this order: **what fails, how much downtime is allowed, how much data loss is allowed, and which service owns recovery**.

| Scenario clue | What to think |
|---|---|
| VM or whole workload must recover in another region with minutes of RPO | **Azure Site Recovery** |
| Need point-in-time restore or long-term retention | **Azure Backup**, SQL backups, or database-native retention |
| Need to prove DR without touching production | **ASR test failover** in an isolated network |
| SQL needs app connection string stability after failover | **Auto-failover group** listener |
| Cosmos needs near-zero global write downtime | Multi-region writes; avoid Strong consistency with multi-write |
| Storage must survive regional outage and allow secondary reads | **RA-GRS** or **RA-GZRS** |
| Ransomware or destructive admin risk | Soft delete, immutability, multi-user authorization, alerts |

Official weight: **15-20%** of AZ-305.

```mermaid
flowchart LR
    INC[Incident] -.lost data.-> RPO[RPO<br/>Recovery Point Objective<br/>= max acceptable data loss]
    INC --> DOWN[Downtime begins]
    DOWN --> REC[Recovery complete]
    DOWN -.elapsed.-> RTO[RTO<br/>Recovery Time Objective<br/>= max acceptable downtime]
```

 **Memorize:**
- **RPO is in the past** (how far back is your last good copy)
- **RTO is in the future** (how long to get back online)

### Mapping requirements -> solution

```mermaid
flowchart TD
    Q[Required RTO/RPO?] --> R1[RPO: hours<br/>RTO: hours] --> S1[Azure Backup<br/>nightly snapshots]
    Q --> R2[RPO: minutes<br/>RTO: < 1h] --> S2[Azure Site Recovery<br/>continuous replication]
    Q --> R3[RPO: ~0<br/>RTO: ~0] --> S3[Active-Active multi-region<br/>Cosmos / Front Door / SQL AG]
    Q --> R4[RPO: 0 inside region<br/>RTO: seconds] --> S4[Availability Zones<br/>+ ZRS storage]
```

---

## 2 High Availability inside one region

```mermaid
flowchart TB
    subgraph Region[Azure Region]
      subgraph Z1[Zone 1]
        VM1[VM]
      end
      subgraph Z2[Zone 2]
        VM2[VM]
      end
      subgraph Z3[Zone 3]
        VM3[VM]
      end
      LB[Standard Load Balancer<br/>Zone-redundant]
      LB --> VM1
      LB --> VM2
      LB --> VM3
    end
    style Z1 fill:#bfdbfe
    style Z2 fill:#bbf7d0
    style Z3 fill:#fed7aa
```

| Feature | SLA | Protects against |
|---|---|---|
| Single VM Premium SSD | 99.9% | Nothing structural |
| **Availability Set** (FD/UD) | 99.95% | Rack/host failure, planned maint |
| **Availability Zone** | **99.99%** | **Datacenter failure** |
| Multi-region | 99.99%+ | Region failure |

 **Exam:** "Highest SLA in a single region" -> **Availability Zones** (not sets!).
 You **cannot mix** Availability Set and Availability Zone for the same VM.

### Availability Set internals

```mermaid
flowchart LR
    AS[Availability Set] --> FD[Fault Domains<br/>physical rack, power, network<br/>up to 3]
    AS --> UD[Update Domains<br/>reboot groups<br/>up to 20]
```

---

## 3 Azure Backup

```mermaid
flowchart TB
    subgraph Sources
      VM[Azure VM]
      DSK[Managed Disks]
      FS[Azure Files]
      SQL[SQL in VM]
      SAP[SAP HANA in VM]
      WL[On-prem via MARS / MABS]
    end
    subgraph Vault[Recovery Services Vault]
      POL[Backup Policy<br/>frequency + retention]
      RP[Recovery Points]
      SD[Soft Delete 14 days]
      IMM[Immutable Vault]
    end
    Sources --> Vault
    Vault --> Restore[Restore: full / file / disk]
```

### Vault redundancy decision

```mermaid
flowchart LR
    LRS2[LRS - cheap, in-region only] --> ZRS2[ZRS - zone redundant]
    ZRS2 --> GRS2[GRS - cross-region restore default]
    GRS2 --> CR[Enable Cross-Region Restore CRR]
```

 **Default for prod:** **GRS + CRR enabled** so you can restore in paired region.

### Anti-ransomware features (memorize!)

```mermaid
flowchart LR
    A[Soft delete<br/>14 days, on by default] --> B[Immutable Vault<br/>recovery points cannot be deleted before retention]
    B --> C[MUA - Multi-User Authorization<br/>second admin must approve destructive ops]
    C --> D[Alerts on suspicious activity]
```

---

## 4 Azure Site Recovery (ASR)

```mermaid
flowchart LR
    subgraph Primary
      P[VM running]
    end
    subgraph ASR
      M[Mobility Agent]
      C[Cache Storage]
      RV[Recovery Services Vault]
    end
    subgraph Secondary[Paired/Chosen Region]
      S[Replica disks<br/>spun up on failover]
    end
    P --> M --> C --> RV --> S
    RV -.test failover.-> T[Isolated test VM<br/>no impact to prod]
```

| Mode | Use |
|---|---|
| **Test failover** | Validate DR plan with no impact (creates isolated copy) |
| **Planned failover** | Zero data loss; shuts down primary first |
| **Unplanned failover** | Real disaster; possible small data loss |
| **Failback** | Return to primary after recovery |

 **Recovery Plan** = ordered sequence of VM groups, scripts, manual actions for orchestrated failover (e.g., DB tier first, then app, then web).

 ASR scenarios on the exam:
- **Azure VM -> Azure VM** (cross-region) 
- VMware/physical -> Azure 
- Hyper-V -> Azure 
- Azure -> on-prem (failback) 

---

## 5 Database-specific DR

### SQL Database - Auto-failover groups

```mermaid
flowchart LR
    APP[App] --> LR[Listener<br/>read-write endpoint]
    LR --> P[Primary - East US]
    P -.async geo-replication.-> S[Secondary - West US]
    APP --> RO[Read-only endpoint] --> S

    P -.outage.-> F[Auto-failover<br/>< 1 hour grace]
    F --> S
```

**Key facts:**
- Works for **SQL DB** and **SQL MI**
- Provides **listener endpoints** (no app connection-string change)
- **RPO** typically seconds, **RTO** ~1 minute
- For **MI**: failover groups across regions; instances must be in **paired** regions

### Cosmos DB

```mermaid
flowchart LR
    W[Writer Region] -.replicate.-> R1[Reader 1]
    W -.replicate.-> R2[Reader 2]
    note1[Single-write account<br/>RTO: minutes after manual failover<br/>or auto-failover priority list]

    W2[Multi-region writes<br/>= active-active<br/>RPO ~0, RTO ~0]
```

### Storage replication recap

| Type | Purpose |
|---|---|
| LRS / ZRS | HA, no DR |
| GRS / GZRS | DR async |
| RA-GRS / RA-GZRS | DR + read secondary anytime |

---

## 6 Application-level DR & global routing

```mermaid
flowchart TD
    USER[Users] --> Q{Routing layer}
    Q --> AFD[Azure Front Door<br/>L7 HTTP/HTTPS<br/>WAF + global LB + CDN<br/>fastest failover]
    Q --> ATM[Traffic Manager<br/>DNS-based<br/>any protocol<br/>slower TTL failover]
    AFD --> R1[East US backend]
    AFD --> R2[West EU backend]
    ATM --> R1
    ATM --> R2
```

### Front Door vs Traffic Manager

| Feature | Front Door | Traffic Manager |
|---|---|---|
| Layer | **L7 HTTP/HTTPS** | **DNS** (any TCP/UDP) |
| Failover | Connection-level, fast | TTL-dependent (~minutes) |
| WAF | Built-in | |
| TLS terminate | |  |
| Use | Global web apps | Generic, on-prem endpoints |

 "Active-active global website with WAF" -> **Azure Front Door Premium**.
 "Route to on-prem datacenter as DR" -> **Traffic Manager**.

---

## 7 DR architecture patterns

```mermaid
flowchart TB
    subgraph Cold[Active-Passive Cold]
      C1[Primary running] -.->|Backups only| C2[Secondary OFF<br/>Restore from backup]
    end
    subgraph Pilot[Pilot Light]
      P1[Primary] -.->|Replicate data| P2[Minimal infra<br/>scale up on failover]
    end
    subgraph Warm[Warm Standby]
      W1[Primary 100%] -.->|Replicate| W2[Secondary running smaller]
    end
    subgraph Hot[Active-Active]
      H1[Primary 50%] <-.bidir.-> H2[Secondary 50%<br/>both serve traffic]
    end
```

| Pattern | RTO | RPO | Cost |
|---|---|---|---|
| Cold (backup only) | Hours-days | Hours | $ |
| Pilot light (ASR) | < 1h | Minutes | $$ |
| Warm standby | Minutes | Seconds | $$$ |
| Active-active | ~0 | ~0 | $$$$ |

---

## Domain 3 cheat-sheet

| Scenario | Answer |
|---|---|
| Survive datacenter (zone) failure, single region | **Availability Zones** + ZRS |
| Survive region failure, VMs | **Azure Site Recovery** |
| Backup VM/files/SQL nightly | **Azure Backup + Recovery Services Vault** |
| Ransomware-resistant backups | **Immutable Vault + Soft Delete + MUA** |
| RPO = seconds, RTO = minutes for SQL | **Auto-failover groups** |
| Multi-region active writes for NoSQL | **Cosmos DB multi-region writes** |
| Global HTTP app failover with WAF | **Azure Front Door** |
| DNS-based failover to on-prem | **Traffic Manager** |
| Test DR without impacting prod | **ASR Test Failover** |
| Restore VM in a different region | **GRS vault + CRR** |
| Coordinate failover order across tiers | **ASR Recovery Plan** |
| Cheapest DR (long RTO acceptable) | **Backup-only / Cold** |
| Highest SLA, lowest RTO/RPO | **Active-Active multi-region** |
| 99.99% VM SLA in one region | **Availability Zones** |

---

## References (Microsoft Learn)

- [AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305)
- [Azure Backup overview](https://learn.microsoft.com/azure/backup/backup-overview)
- [Azure Site Recovery overview](https://learn.microsoft.com/azure/site-recovery/site-recovery-overview)
- [Reliability - availability zones](https://learn.microsoft.com/azure/reliability/availability-zones-overview) - [Region pairs](https://learn.microsoft.com/azure/reliability/cross-region-replication-azure)
- [Azure SQL high availability](https://learn.microsoft.com/azure/azure-sql/database/high-availability-sla) - [Failover groups](https://learn.microsoft.com/azure/azure-sql/database/auto-failover-group-overview)
- [Cosmos DB business continuity](https://learn.microsoft.com/azure/cosmos-db/high-availability)
- [Azure Front Door](https://learn.microsoft.com/azure/frontdoor/front-door-overview) - [Traffic Manager](https://learn.microsoft.com/azure/traffic-manager/traffic-manager-overview)

 **Next:** [04-infrastructure.md](04-infrastructure.md)
