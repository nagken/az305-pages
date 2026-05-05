# Domain 2 - Data Storage Solutions

> Maps to AZ-305 measured skill **Design data storage solutions** (~20-25%). Reference: [Microsoft Learn AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305) - [Azure SQL options](https://learn.microsoft.com/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview) - [Azure Cosmos DB](https://learn.microsoft.com/azure/cosmos-db/introduction) - [Azure Storage](https://learn.microsoft.com/azure/storage/common/storage-introduction) - [Azure Synapse](https://learn.microsoft.com/azure/synapse-analytics/overview-what-is).

> Big idea: **Right database for the right data shape**, with right **consistency**, **scale**, and **cost**.

---

## Domain mind map

```mermaid
mindmap
  root((Data Storage))
    Relational SQL
      Azure SQL Database
        Single DB
        Elastic Pool share DTU
        Hyperscale up to 100TB
        Serverless auto-pause
      SQL Managed Instance
        Near 100% SQL Server compat
        VNet injected
        Cross-DB queries
      SQL on Azure VM
        Full control IaaS
        OS access SQL Agent
      Azure DB for PostgreSQL
        Flexible Server
        Hyperscale Citus
      Azure DB for MySQL
        Flexible Server
      Azure DB for MariaDB EOL
    NoSQL
      Cosmos DB
        APIs Core SQL Mongo Cassandra Gremlin Table
        Partition Key
        Consistency 5 levels
        Multi-region writes
        Autoscale RUs
        Serverless
      Azure Cache for Redis
        Tiers Basic Std Premium Enterprise
    Storage Account
      Blob
        Hot Cool Cold Archive
        Block Append Page
        Lifecycle policies
        Versioning Immutability
        Static website
      Files
        SMB NFS
        Premium ZRS
        Sync to on-prem
      Queues vs Service Bus
      Tables vs Cosmos Table API
      Data Lake Gen2 hierarchical
    Analytics
      Synapse Analytics
        Dedicated SQL pool
        Serverless SQL pool
        Spark pool
      Data Factory ETL
      Databricks
      Stream Analytics
      Event Hubs ingest
      Purview governance
    Security
      Encryption at rest CMK
      Always Encrypted
      TDE
      Private Endpoints
      Firewall + VNet rules
      SAS tokens
      Storage account keys vs Entra
```

---

## 1 Pick the relational database

## Scenario patterns to recognize

AZ-305 scenarios is very data-heavy. Many questions are service-selection traps where two answers are technically possible, but only one meets compatibility, downtime, cost, or admin-effort constraints.

| Scenario clue | What to think |
|---|---|
| Lift-and-shift SQL with SQL Agent, cross-database query, or instance-level features | **Azure SQL Managed Instance** |
| New cloud relational app, minimal admin | **Azure SQL Database** |
| Full OS or unsupported SQL Server feature required | **SQL Server on Azure VM** |
| Many small databases with variable usage | **Elastic pool** |
| Very large SQL database or fast restore | **Hyperscale** |
| Global document data and low-latency reads/writes | **Cosmos DB**, regions, consistency, partition key |
| On-prem apps need private access to storage | **Private Endpoint** + Private DNS + disable public access when required |
| Long-term blob retention at lowest cost | Lifecycle policy to Cool/Cold/Archive plus immutability if compliance requires it |
| Copy and transform data between systems | **Data Factory** or Synapse pipelines; Synapse Link for Cosmos analytical store |

Official weight: **20-25%** of AZ-305.

```mermaid
flowchart TD
    Q1[Need a SQL DB?] --> Q2{Need full SQL Server features?<br/>Cross-DB query, SQL Agent,<br/>CLR, linked servers, custom OS?}
    Q2 -->|Need OS-level control<br/>or unsupported features| VM[SQL Server on Azure VM<br/>Most work, most flexibility]
    Q2 -->|Need most SQL Server features<br/>but managed| MI[SQL Managed Instance<br/>VNet, near-100% compat]
    Q2 -->|App is cloud-native<br/>no special needs| SDB{Workload pattern?}
    SDB -->|Many small DBs<br/>variable load| EP[Elastic Pool]
    SDB -->|Big DB > 4TB<br/>fast scale| HS[Hyperscale]
    SDB -->|Intermittent / dev| SS[Serverless<br/>auto-pause]
    SDB -->|Standard prod| SQL[Azure SQL Database]
```

| Tier | When | Compat | Effort |
|---|---|---|---|
| **SQL on VM** | Custom needs, full control | 100% | High (you patch, you backup) |
| **SQL MI** | Lift & shift on-prem SQL with minimal change | ~100% | Medium |
| **SQL DB** | New cloud apps | ~95% | Low |

 **Exam triggers:**
- "Cross-database queries" -> **MI** (SQL DB needs Elastic Query workaround)
- "SQL Agent jobs" -> **MI** or **VM**
- "Lift-and-shift with minimal code change + VNet integration" -> **MI**
- "Variable workload, save cost" -> **Serverless** SQL DB
- "Up to 100 TB, fast restore" -> **Hyperscale**

---

## 2 Cosmos DB - the NoSQL champion

### Choose the API

```mermaid
flowchart TD
    Need[Need NoSQL?] --> A{Existing data model?}
    A -->|None / new| CORE[Core SQL API<br/>document, default]
    A -->|Existing MongoDB app| MONGO[Mongo API]
    A -->|Existing Cassandra| CASS[Cassandra API]
    A -->|Graph / relationships| GREM[Gremlin API]
    A -->|Storage Tables migration| TBL[Table API]
```

### The 5 consistency levels (memorize order!)

```mermaid
flowchart LR
    S[Strong<br/>linearizable] --> BS[Bounded Staleness<br/>by k versions / time]
    BS --> SES[Session<br/> default]
    SES --> CP[Consistent Prefix<br/>order preserved]
    CP --> EV[Eventual<br/>cheapest, fastest]

    style S fill:#fecaca
    style BS fill:#fed7aa
    style SES fill:#bfdbfe
    style CP fill:#bbf7d0
    style EV fill:#bbf7d0
```

| Level | Latency | Throughput | Use |
|---|---|---|---|
| Strong | High | Low | Banking, leaderboards (single region only multi-write) |
| Bounded Staleness | Med | Med | "Last 5 mins ok" |
| **Session** | Low | High | **Default** - read-your-writes per user |
| Consistent Prefix | Low | High | Social feeds |
| Eventual | Lowest | Highest | View counters, telemetry |

 **Exam:** "Read your own write but not others' immediately" -> **Session**.

### Partition key rules

```mermaid
flowchart TD
    G[Good partition key] --> R1[High cardinality]
    G --> R2[Even access pattern]
    G --> R3[Filter on it in queries]
    B[BAD examples] --> B1[Boolean / status]
    B --> B2[Timestamp only]
    B --> B3[Tenant ID with one big tenant]
```

### Capacity modes

```mermaid
flowchart LR
    P[Provisioned RU/s<br/>predictable load] --> AS[Autoscale RU/s<br/>scales 10%-100% of max]
    AS --> SRV[Serverless<br/>per-request billing<br/><= 1M RU/s, <= 1TB]
```

 "Sporadic dev workload, pay per request" -> **Serverless**. "Spiky prod" -> **Autoscale**.

---

## 3 Storage Account - the Swiss Army knife

```mermaid
flowchart TD
    SA[Storage Account] --> B[Blob]
    SA --> F[Files]
    SA --> Q[Queues]
    SA --> T[Tables]
    B --> BT{Tier}
    BT --> H[Hot<br/>frequent, $$ storage<br/>$ access]
    BT --> C[Cool<br/>30+ days<br/>$ storage, $$ access]
    BT --> CO[Cold<br/>90+ days, lower than Cool]
    BT --> AR[Archive<br/>180+ days<br/>rehydrate hours]
```

### Blob tier decision

```mermaid
flowchart TD
    Q[Access frequency?] --> H{Daily?}
    H -->|Yes| HOT[Hot]
    H -->|No| Q2{Once a month?}
    Q2 -->|Yes| COOL[Cool]
    Q2 -->|No| Q3{Quarterly?}
    Q3 -->|Yes| COLD[Cold]
    Q3 -->|Almost never<br/>but must keep| ARCH[Archive<br/> rehydrate before read]
```

 **Lifecycle Mgmt rule example:**
- Move to Cool after 30 days of no access
- Move to Archive after 180 days
- Delete after 7 years (compliance)

### Storage redundancy ladder

```mermaid
flowchart LR
    LRS[LRS<br/>3 copies<br/>1 datacenter] --> ZRS[ZRS<br/>3 zones<br/>1 region]
    ZRS --> GRS[GRS<br/>LRS + LRS<br/>paired region async]
    GRS --> RAGRS[RA-GRS<br/>GRS + read access<br/>to secondary]
    GRS --> GZRS[GZRS<br/>ZRS + LRS in paired region]
    GZRS --> RAGZRS[RA-GZRS<br/>read secondary]
```

| Need | Choose |
|---|---|
| Cheapest, in-region only | **LRS** |
| Survive zone outage | **ZRS** |
| Survive region outage | **GRS** / **GZRS** |
| Read from secondary during outage | **RA-GRS** / **RA-GZRS** |

 **Archive tier does NOT support ZRS/GZRS** - only LRS/GRS in many cases. **Premium block blob** = LRS or ZRS only.

---

### Azure Files - SMB vs NFS vs Sync

```mermaid
flowchart TD
    Q[Need a file share?] --> P{Protocol?}
    P -->|Windows clients| SMB[Azure Files SMB<br/>+ Identity: AD DS / Entra DS / Entra Kerberos]
    P -->|Linux apps| NFS[Azure Files NFSv4.1<br/>Premium tier only<br/>private endpoint required]
    Q --> S{On-prem cache?}
    S -->|Yes| FS[Azure File Sync<br/>tier cold files to cloud]
```

 "Lift-and-shift Windows file server" -> **Azure Files + Azure File Sync** (cloud tiering).

---

### Queues vs Service Bus vs Event Hubs vs Event Grid

```mermaid
flowchart TD
    Q[Messaging need?] --> T{Pattern}
    T -->|Simple 1-to-1 task queue<br/>< 64KB| SQ[Storage Queue<br/>cheapest, basic]
    T -->|Enterprise messaging<br/>FIFO, sessions, dead-letter,<br/>transactions, pub-sub topics| SB[Service Bus<br/>Queues + Topics]
    T -->|Millions of events/sec<br/>telemetry, IoT, Kafka| EH[Event Hubs<br/>stream ingest]
    T -->|Reactive event routing<br/>blob created -> Function| EG[Event Grid<br/>discrete events]
```

| Service | Model | Throughput | Order | Pattern |
|---|---|---|---|---|
| Storage Queue | Pull | Low | No | Simple |
| Service Bus | Pull | Med | FIFO/sessions | Enterprise |
| Event Hubs | Pull | Massive | Per partition | Streaming |
| Event Grid | Push | High | No | Reactive |

---

## 4 Big Data & Analytics

```mermaid
flowchart LR
    SRC[Sources<br/>SQL, Blob, IoT, Kafka] --> ING{Ingest}
    ING --> ADF[Azure Data Factory<br/>ETL pipelines]
    ING --> EH2[Event Hubs<br/>real-time]
    ADF --> ADL[ADLS Gen2<br/>hierarchical Blob]
    EH2 --> SA[Stream Analytics<br/>SQL on streams]
    ADL --> SYN[Synapse Analytics]
    SYN --> SQLP[Dedicated SQL Pool<br/>old SQL DW, MPP]
    SYN --> SRV[Serverless SQL<br/>query files in lake]
    SYN --> SPK[Spark Pool]
    SYN --> PBI[Power BI]
    ADL --> DBX[Databricks<br/>premium Spark]
```

 **Exam:**
- "Petabyte data warehouse, MPP" -> **Synapse Dedicated SQL Pool**
- "Query parquet/CSV in Data Lake without provisioning" -> **Synapse Serverless SQL**
- "Code-first data engineers, ML, Delta Lake" -> **Azure Databricks**
- "Drag-drop ETL with 100+ connectors" -> **Azure Data Factory** (or Synapse Pipelines)
- "Real-time SQL on event stream" -> **Stream Analytics**
- "Catalog & lineage across all data" -> **Microsoft Purview**

---

## 5 Securing data

```mermaid
flowchart TD
    D[Data Security] --> E[Encryption]
    D --> N[Network]
    D --> A[Access]

    E --> E1[At rest: TDE / SSE platform-managed]
    E --> E2[CMK in Key Vault for compliance]
    E --> E3[Always Encrypted: data encrypted in client SDK]
    E --> E4[In transit: TLS 1.2+]

    N --> N1[Service Endpoints<br/>VNet -> public IP via backbone]
    N --> N2[Private Endpoint<br/>private IP in your VNet preferred]
    N --> N3[Firewall rules + VNet rules]

    A --> A1[Entra auth ]
    A --> A2[SAS tokens for Blob]
    A --> A3[RBAC data plane: Storage Blob Data Reader/Contributor]
    A --> A4[Disable Storage Account keys + Shared Key]
```

 **Always Encrypted** vs **TDE**:
- **TDE** = encrypts at rest on disk (DBA can still see data via SQL)
- **Always Encrypted** = encrypts in client driver, **DBA cannot read sensitive columns**

 **Private Endpoint** vs **Service Endpoint**:
- **Service Endpoint** = traffic stays on Azure backbone but goes to public IP
- **Private Endpoint** = resource gets a **private IP in your VNet**; can disable public access entirely -> **stronger isolation**

---

## Domain 2 cheat-sheet

| Scenario | Answer |
|---|---|
| Lift-and-shift on-prem SQL with cross-DB queries | **SQL Managed Instance** |
| New cloud app, simple SQL | **Azure SQL Database** |
| 80 TB SQL with fast restore | **Hyperscale** |
| Variable / dev SQL workload | **SQL DB Serverless** |
| Multi-region active writes globally | **Cosmos DB** multi-region writes |
| Read your own writes, NoSQL | **Cosmos Session** consistency |
| Migrate MongoDB app | **Cosmos for MongoDB** |
| Cheap object storage rarely accessed | **Cool / Cold / Archive** Blob |
| Survive region outage, read secondary | **RA-GZRS** |
| Replace on-prem file server | **Azure Files + File Sync** |
| Message queue with sessions/FIFO | **Service Bus** |
| 1M events/sec ingest | **Event Hubs** |
| Blob created -> trigger Function | **Event Grid** |
| Petabyte BI warehouse | **Synapse Dedicated SQL Pool** |
| Query data lake files ad-hoc | **Synapse Serverless SQL** |
| Disable public access to storage | **Private Endpoint** + firewall = Deny |
| DBA must NOT see PII columns | **Always Encrypted** |
| Auto-archive blobs after 180 days | **Lifecycle Management** policy |

---

## References (Microsoft Learn)

- [AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305)
- [Choose an Azure SQL option](https://learn.microsoft.com/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview)
- [Azure Cosmos DB consistency levels](https://learn.microsoft.com/azure/cosmos-db/consistency-levels)
- [Azure Storage redundancy](https://learn.microsoft.com/azure/storage/common/storage-redundancy)
- [Blob access tiers](https://learn.microsoft.com/azure/storage/blobs/access-tiers-overview) - [Lifecycle management](https://learn.microsoft.com/azure/storage/blobs/lifecycle-management-overview)
- [Azure Synapse Analytics](https://learn.microsoft.com/azure/synapse-analytics/overview-what-is)
- [Event Hubs](https://learn.microsoft.com/azure/event-hubs/event-hubs-about) - [Service Bus](https://learn.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) - [Event Grid](https://learn.microsoft.com/azure/event-grid/overview)
- [Always Encrypted](https://learn.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine)

 **Next:** [03-business-continuity.md](03-business-continuity.md)
