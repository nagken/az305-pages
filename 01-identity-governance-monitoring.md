# Domain 1 - Identity, Governance & Monitoring

> Big idea: **WHO** can do **WHAT**, **WHERE**, and how do we **SEE** what happened.
>
> Maps to AZ-305 measured skill **Design identity, governance, and monitoring solutions** (~25-30%). Reference: [Microsoft Learn AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305) - [Microsoft Entra documentation](https://learn.microsoft.com/entra/) - [Azure RBAC](https://learn.microsoft.com/azure/role-based-access-control/overview) - [Azure Policy](https://learn.microsoft.com/azure/governance/policy/overview) - [Azure Monitor](https://learn.microsoft.com/azure/azure-monitor/overview).

---

## Domain mind map

```mermaid
mindmap
  root((Identity Governance Monitoring))
    WHO Identity
      Microsoft Entra ID
        Tenant = identity boundary
        Editions Free P1 P2
      Hybrid Identity
        Entra Connect Sync
        Cloud Sync lightweight
        Pass-through Auth
        Password Hash Sync
        Federation ADFS
      External Users
        B2B guest invites
        B2C consumer apps
        External ID new
      Strong Auth
        MFA
        Conditional Access
        Passwordless FIDO2
        Identity Protection P2
      Just-in-Time
        PIM eligible vs active
        Access Reviews
    WHAT Authorization
      Entra roles tenant-wide
      Azure RBAC resource-scoped
      Custom RBAC roles
      Managed Identity
        System-assigned
        User-assigned
    WHERE Governance
      Mgmt Groups
      Subscriptions
      Resource Groups
      Azure Policy guardrails
      Azure Blueprints deprecated
      Landing Zones ALZ
      Resource Locks
      Tags + Cost Mgmt
    SEE Monitoring
      Azure Monitor umbrella
      Metrics platform numeric
      Logs Log Analytics KQL
      App Insights APM
      VM Insights
      Container Insights
      Alerts + Action Groups
      Workbooks dashboards
      Activity Log who did what
```

---

## Scenario patterns to recognize

AZ-305 case-study questions repeatedly test identity and governance as **architecture constraints**, not isolated vocabulary. Watch for these patterns:

| Scenario clue | What to think |
|---|---|
| App must access Key Vault or storage without credentials | **Managed identity** plus the right data-plane RBAC role |
| Users need least-privilege Azure access | Built-in RBAC first, custom RBAC only for missing `Actions`/`DataActions` |
| Policy must remediate SQL TDE, diagnostics, or tags | Azure Policy **DeployIfNotExists** or **Modify** with managed identity |
| Many subscriptions need shared controls | Management groups + initiatives + inherited RBAC/policy |
| Admins need temporary elevated access | **PIM**, MFA, approval, time-bound activation |
| Monitor web/API failures or log patterns | Application Insights or Log Analytics query alert + action group |

Official weight: **25-30%** of AZ-305.

### Hybrid identity decision tree

```mermaid
flowchart TD
    Start[On-prem AD users<br/>need cloud access?] --> Q1{Need SSO<br/>without password<br/>in cloud?}
    Q1 -->|Yes, modern| PHS[Password Hash Sync<br/> Simplest, recommended]
    Q1 -->|Yes, must validate<br/>on-prem in real time| PTA[Pass-Through Auth<br/> No password in cloud]
    Q1 -->|Need 3rd-party MFA<br/>or smart cards| FED[AD FS Federation<br/> Complex, legacy]

    Start --> Q2{Multi-forest<br/>or filtering needs?}
    Q2 -->|Simple, single forest<br/>small org| CS[Entra Cloud Sync<br/> lightweight agent]
    Q2 -->|Complex, large<br/>device sync, writeback| CONNECT[Entra Connect Sync<br/>full server]
```

**Exam trick:** "Minimize on-prem footprint" -> **Cloud Sync** or **PHS**. "Real-time disabled accounts" -> **Pass-Through Auth**.

---

### Entra editions - what unlocks what?

```mermaid
flowchart LR
    Free[Free<br/>basic users + groups] --> P1[P1<br/>+ Conditional Access<br/>+ MFA<br/>+ Self-service password reset writeback<br/>+ Dynamic groups]
    P1 --> P2[P2<br/>+ PIM<br/>+ Identity Protection<br/>+ Risk-based CA<br/>+ Access Reviews]
```

| Need | Edition |
|---|---|
| MFA enforcement via CA | **P1** |
| Just-in-time admin (PIM) | **P2** |
| Risky sign-in detection | **P2** |
| Access reviews | **P2** |

---

### Conditional Access - the IF-THEN engine

```mermaid
flowchart LR
    subgraph IFG[IF Signals]
      U[User/Group]
      A[App]
      L[Location]
      D[Device state]
      R[Risk P2]
    end
    subgraph THEN[THEN Controls]
      B[Block]
      M[Require MFA]
      C[Compliant device]
      P[Password change]
    end
    IFG --> THEN
```

**Pattern:** "Require MFA only when signing in from outside corporate network" -> CA policy with **named location** as condition.

---

### B2B vs B2C vs External ID

```mermaid
flowchart TD
    Q{Who is the user?} --> Q1[Partner / contractor<br/>collaborating on internal apps]
    Q --> Q2[Customer using<br/>your public-facing app]
    Q1 --> B2B[Entra B2B<br/>Guest in your tenant<br/>Uses their own creds]
    Q2 --> B2C[Entra B2C<br/>Separate tenant<br/>Custom branding<br/>Social logins]
    Q --> Q3[New unified model<br/>2024+]
    Q3 --> EXT[External ID<br/>Replacing B2C going forward]
```

---

### PIM - Privileged Identity Management

```mermaid
sequenceDiagram
    participant U as Admin User
    participant PIM
    participant Resource
    Note over U: Eligible (not active by default)
    U->>PIM: Request activation (with reason + MFA)
    PIM->>U: Approve (auto or manual)
    PIM->>Resource: Grant role TEMPORARILY
    Note over Resource: Time-bound (e.g., 4h)
    PIM->>Resource: Auto-revoke at expiry
```

**Use PIM when:** "minimize standing admin permissions", "just-in-time access", "approval workflow for admins".

---

## 2 Authorization - RBAC vs Entra Roles

```mermaid
flowchart TB
    subgraph Entra[Entra ID Roles - Identity plane]
      E1[Global Admin]
      E2[User Admin]
      E3[Application Admin]
      Note1[Scope: Tenant]
    end
    subgraph RBAC[Azure RBAC - Resource plane]
      R1[Owner]
      R2[Contributor]
      R3[Reader]
      R4[User Access Administrator]
      R5[Custom roles]
      Note2[Scope: MG / Sub / RG / Resource]
    end
```

**Memorize:**
- **Manage users/groups/apps** -> Entra role
- **Manage VMs/storage/networks** -> Azure RBAC role
- **Both?** -> User Access Administrator + assignments

### Custom RBAC role JSON pattern

```json
{
  "Name": "VM Operator",
  "Actions": ["Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action"],
  "NotActions": [],
  "AssignableScopes": ["/subscriptions/<id>"]
}
```

 **Exam:** Custom role needs `AssignableScopes`. Lowest-privilege built-in role: prefer **Reader** + targeted custom over **Contributor**.

---

### Managed Identity decision

```mermaid
flowchart TD
    Need[App needs to call<br/>Azure resource securely<br/>without storing secrets] --> Q{Multiple<br/>resources share<br/>identity?}
    Q -->|No, just one app| SYS[System-assigned MI<br/>Lifecycle = resource]
    Q -->|Yes, share across<br/>multiple apps/resources| USR[User-assigned MI<br/>Independent lifecycle]
```

 **Exam:** "Connect from VM to Key Vault without storing credentials" -> **Managed Identity** + Key Vault access policy/RBAC.

---

## 3 Governance - the WHERE

### Hierarchy you must memorize

```mermaid
flowchart TD
    R[Root Mgmt Group<br/>Tenant root] --> MG1[MG: Production]
    R --> MG2[MG: Dev/Test]
    MG1 --> S1[Subscription A]
    MG1 --> S2[Subscription B]
    S1 --> RG1[RG: web-prod]
    S1 --> RG2[RG: data-prod]
    RG1 --> RES[VM / SQL / App Service]

    style R fill:#fde68a
    style MG1 fill:#bfdbfe
    style MG2 fill:#bfdbfe
    style S1 fill:#bbf7d0
    style RG1 fill:#fecaca
```

**Inheritance:** Policy & RBAC flow **downward**. Apply at the highest level possible.

---

### Azure Policy vs RBAC

```mermaid
flowchart LR
    subgraph Policy[Azure Policy = WHAT can exist]
      P1[Allowed regions: US only]
      P2[Allowed SKUs]
      P3[Require tags]
      P4[Audit / Deny / Modify / DeployIfNotExists]
    end
    subgraph RBACx[Azure RBAC = WHO can do]
      RR1[Who can create]
      RR2[Who can read]
      RR3[Who can delete]
    end
```

 **Exam triggers:**
- "Prevent deployment outside East US" -> **Policy**: Allowed locations
- "Auto-add tag from RG" -> Policy effect **Modify**
- "Auto-deploy diagnostics" -> **DeployIfNotExists** (needs Managed Identity)

---

### Initiatives, Blueprints, Landing Zones

```mermaid
flowchart LR
    POL[Single Policy] --> INI[Initiative<br/>= bundle of policies]
    INI --> BP[Blueprint<br/>policies + RBAC + ARM<br/> DEPRECATED]
    BP -.->|Replaced by| TF[Azure Landing Zones ALZ<br/>= Bicep/Terraform templates]
```

 **2026+ exam:** Prefer **ALZ + Initiatives + Template Specs** over Blueprints.

---

### Resource Locks

```mermaid
flowchart LR
    L1[ReadOnly] --> E1[Cannot modify or delete]
    L2[CanNotDelete] --> E2[Can modify, cannot delete]
```

Apply at MG / Sub / RG / Resource. **Inherits down**. Owners + User Access Administrators can manage locks.

---

## 4 Monitoring - the SEE

### The Azure Monitor universe

```mermaid
flowchart TB
    subgraph Sources[Data Sources]
      A1[Azure Resources]
      A2[VMs Agent]
      A3[Apps SDK]
      A4[On-prem]
    end
    subgraph Store[Storage]
      M[Metrics<br/>numeric, real-time<br/>93 days]
      L[Logs<br/>Log Analytics Workspace<br/>KQL, retention configurable]
    end
    subgraph Consume[Consume]
      AL[Alerts]
      DASH[Workbooks/Dashboards]
      AI[App Insights]
      VMI[VM Insights]
      CI[Container Insights]
    end
    A1 --> M & L
    A2 -->|Azure Monitor Agent AMA| L
    A3 -->|App Insights SDK| L
    A4 -->|AMA| L
    M --> AL & DASH
    L --> AL & DASH & AI & VMI & CI
```

 **Critical fact:** **Azure Monitor Agent (AMA)** replaces the old MMA/OMS agent. Use **Data Collection Rules (DCR)** to configure what's collected.

---

### Alert decision tree

```mermaid
flowchart TD
    Q[I need an alert when...] --> A{What kind of signal?}
    A -->|CPU/memory/numeric metric| M[Metric Alert<br/>fast, cheap]
    A -->|Specific log pattern<br/>e.g., 5xx in App| L[Log Alert<br/>KQL query]
    A -->|Resource created/deleted<br/>config change| AC[Activity Log Alert]
    A -->|Service health incident| SH[Service Health Alert]
    M & L & AC & SH --> AG[Action Group<br/>Email / SMS / Webhook / Logic App / Function]
```

---

### KQL must-know patterns

```kusto
// Failed sign-ins last 24h
SigninLogs
| where TimeGenerated > ago(24h)
| where ResultType != 0
| summarize count() by UserPrincipalName

// VM CPU > 80%
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where CounterValue > 80
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5m)
```

---

### When to use which monitoring tool

```mermaid
flowchart LR
    APP[Web/API performance<br/>dependencies, exceptions] --> AI[Application Insights]
    VM[Linux/Windows VM health] --> VMI[VM Insights]
    K8S[AKS pod metrics, logs] --> CI[Container Insights]
    NET[VNet flow, NSG hits] --> NW[Network Watcher + NSG flow logs]
    SEC[Threats, posture] --> DEF[Microsoft Defender for Cloud]
    AUDIT[Who created/deleted what] --> ACT[Activity Log]
```

---

## Domain 1 cheat-sheet card

| Scenario | Answer |
|---|---|
| Sync on-prem users, minimal on-prem servers | **Entra Cloud Sync** |
| Real-time on-prem password validation | **Pass-Through Auth** |
| Just-in-time admin role | **PIM** (P2) |
| Block sign-in from risky IP | **Conditional Access** + Identity Protection |
| App accesses Key Vault without secrets | **Managed Identity** |
| Allow only specific VM SKUs | **Azure Policy** Allowed SKUs |
| Auto-tag resources | Policy effect **Modify** |
| Stop accidental deletion | **Resource Lock CanNotDelete** |
| Alert on VM CPU > 90% | **Metric Alert** |
| Alert on KQL log pattern | **Log Alert** |
| Centralize logs from 30 subscriptions | One **Log Analytics Workspace** + DCRs |
| Customer-facing identity | **Entra External ID / B2C** |
| Partner collaboration | **B2B guest** |

---

## References (Microsoft Learn)

- [AZ-305 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-305)
- [Microsoft Entra ID overview](https://learn.microsoft.com/entra/fundamentals/whatis)
- [Conditional Access](https://learn.microsoft.com/entra/identity/conditional-access/overview)
- [Privileged Identity Management (PIM)](https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure)
- [Azure RBAC](https://learn.microsoft.com/azure/role-based-access-control/overview)
- [Azure Policy](https://learn.microsoft.com/azure/governance/policy/overview)
- [Management groups](https://learn.microsoft.com/azure/governance/management-groups/overview)
- [Azure Monitor](https://learn.microsoft.com/azure/azure-monitor/overview) - [Log Analytics](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-overview) - [Application Insights](https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview)

 **Next:** [02-data-storage.md](02-data-storage.md)
