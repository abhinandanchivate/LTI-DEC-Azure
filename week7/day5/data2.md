# **Case Study**

## **Enterprise Data Platform Modernization on Azure**

**(Azure SQL Database – Provisioned & Serverless, SQL Managed Instance, SQL Server on VM & Synapse Integration)**

**Function / Name:** Data & Analytics
**Sub-Function Name:** Cloud Data Platform Engineering
**Process Name:** Enterprise OLTP → Analytics Modernization
**Version:** **2.4 (Final – Networking, Private Endpoints & MI Design Fully Explained)**
**Date:** 02-Jan-2023
**Classification:** Privileged & Confidential

---

## **1. Business Context**

A **global BFSI enterprise** is modernizing its **on-premise SQL Server estate** to Azure to achieve:

* Secure **OLTP workloads** for core banking systems
* **High availability & disaster recovery (HA/DR)** across regions
* **On-demand SQL compute** for non-steady workloads
* **Analytics modernization** using Azure Synapse
* **Cost optimization** using serverless, elastic pools & reserved capacity
* **Zero-Trust networking** using Private Endpoints and VNET injection
* **Centralized monitoring, auditing, and automation**

---

## **2. Target Architecture Overview**

### **High-Level Architecture**

```
                ┌───────────────────────────┐
                │        Azure AD            │
                │  (RBAC + Authentication)   │
                └─────────────┬─────────────┘
                              │
              ┌───────────────▼────────────────┐
              │           HUB VNET              │
              │  Firewall | Bastion | Logs      │
              └───────────────┬────────────────┘
                              │ VNET Peering
        ┌─────────────────────┼─────────────────────────────┐
        │                     │                             │
┌───────▼───────┐   ┌─────────▼─────────┐       ┌──────────▼──────────┐
│ SPOKE – APP   │   │ SPOKE – DATA       │       │ SPOKE – ANALYTICS   │
│ App Service   │   │ SQL Managed Inst.  │       │ Synapse Workspace   │
│ Functions     │   │ Azure SQL DB       │       │ Serverless SQL Pool │
│ VM-APP-01     │   │ SQL Server (VM)    │       │ ADLS Gen2           │
└───────────────┘   └────────────────────┘       └─────────────────────┘
```

---

## **3. Network Design (VNETs, Subnets, 2 VMs)**

### **Hub VNET**

* Azure Firewall (centralized egress control)
* Azure Bastion (secure VM access)
* Log Analytics Workspace

---

### **Spoke VNET – Application Tier**

* Subnet: `app-subnet`
* **VM-APP-01** (legacy application)
* App Service & Azure Functions with VNET Integration

---

### **Spoke VNET – Data Tier**

* **Dedicated subnet: `mi-subnet`**

  * Azure SQL Managed Instance (VNET injected)
* Subnet: `data-subnet`

  * **VM-SQL-01** (SQL Server on VM)
  * Private Endpoints for Azure SQL Database
* Azure SQL Database:

  * Provisioned
  * Serverless

---

### **Spoke VNET – Analytics Tier**

* Azure Synapse Analytics
* Serverless SQL Pool
* ADLS Gen2 with **Blob + DFS Private Endpoints**

---

### **Connectivity Principles**

* Hub-and-Spoke VNET peering
* **No public IP exposure**
* All PaaS accessed privately (**Zero Trust**)

---

## **4. Azure SQL Deployment Models – WHY MULTIPLE MODELS EXIST**

| Deployment Model                    | Why It Exists                                 |
| ----------------------------------- | --------------------------------------------- |
| **Azure SQL Database**              | Fully managed SaaS for cloud-native apps      |
| **Azure SQL Database – Serverless** | Cost-efficient compute for irregular usage    |
| **SQL Managed Instance**            | PaaS with near-100% SQL Server compatibility  |
| **SQL Server on VM**                | Full OS control for legacy & vendor workloads |

> **Enterprise reality:**
> A single model cannot satisfy all regulatory, legacy, and modernization needs.

---

## **5. Workload Mapping in This Architecture**

* **Azure SQL DB (Provisioned)**
  → Customer profile & steady OLTP services

* **Azure SQL DB (Serverless)**
  → Dev/Test, UAT, reporting, ad-hoc analytics

* **SQL Managed Instance**
  → Core banking transactions (lift-and-shift)

* **SQL Server on VM**
  → Legacy schemas, SQL Agent-heavy jobs, vendor tools, CLR assemblies

---

## **6. DTU vs vCore – Explicit Rationale**

| DTU               | vCore                    |
| ----------------- | ------------------------ |
| Bundled resources | Independent scaling      |
| Limited control   | Enterprise flexibility   |
| Entry-level       | Mandatory for Serverless |

**Decision**

* vCore for MI, Serverless, Business Critical
* DTU only for PoC

---

## **7. Service Tiers & Compute Strategy**

| Tier                | Use Case                      |
| ------------------- | ----------------------------- |
| GP – Provisioned    | Steady non-prod               |
| **GP – Serverless** | Bursty workloads              |
| Business Critical   | Mission-critical OLTP         |
| Hyperscale          | Massive transactional history |

### **Why Serverless Is Used**

* Auto-scale vCores
* Auto-pause during inactivity
* Pay-per-second billing

### **Why Serverless Is NOT Used for Core OLTP**

* Cold-start latency
* 24×7 systems cheaper on provisioned compute

---

## **8. Elastic Pools**

* Shared compute for multi-tenant SaaS databases
* Prevents over-provisioning
* Cost-effective scaling

---

## **9. Storage & IOPS Scaling**

* Compute, storage, and IOPS scale independently
* Hyperscale → instant growth
* Business Critical → local SSD replicas
* Serverless → compute pauses, storage remains online

---

## **10. Backup & Restore**

| Capability  | Strategy        |
| ----------- | --------------- |
| PITR        | 7–35 days       |
| LTR         | 7 years         |
| Geo-restore | Enabled         |
| Serverless  | Fully supported |

---

## **11. HA & DR Strategy**

### **HA**

* Built-in replicas
* Zone redundancy (Business Critical)

### **DR**

* Active geo-replication
* Auto-failover groups
* Read-only secondaries for reporting

---

## **12. Private Endpoint Strategy – FULL EXPLANATION**

### **Why Private Endpoints Are Mandatory (BFSI)**

* Eliminate public exposure
* Prevent data exfiltration
* Meet RBI / PCI-DSS / ISO-27001
* Keep traffic on Microsoft backbone

> **Rule:**
> If a service supports Private Endpoint, public access is disabled.

---

### **Private Endpoint Usage Matrix**

| Service                  | Private Endpoint |
| ------------------------ | ---------------- |
| Azure SQL Database       | ✅ Yes            |
| Azure SQL Serverless     | ✅ Yes            |
| Azure Synapse            | ✅ Yes            |
| ADLS Gen2                | ✅ Yes            |
| Azure Data Factory       | ✅ Managed PE     |
| App Service / Functions  | ✅ Outbound       |
| **SQL Managed Instance** | ❌ Not applicable |

---

## **13. WHY SQL Managed Instance DOES NOT USE Private Endpoints**

Azure SQL Managed Instance:

* Is **VNET-injected**
* Deployed **inside customer subnet**
* Uses **native private IPs**

| Aspect           | SQL DB      | SQL MI         |
| ---------------- | ----------- | -------------- |
| Network model    | Shared PaaS | VNET injected  |
| Private Endpoint | Required    | Not supported  |
| IP address       | PE NIC      | Native VNET IP |

> **Conclusion:**
> Private Endpoints are neither required nor supported for MI.

---

## **14. SQL Managed Instance – Route Tables (UDR) – FULL EXPLANATION**

### **Why MI Needs Internet Route**

MI requires outbound connectivity for:

* Control plane operations
* HA replication
* Patching
* Backups to Azure Storage
* Telemetry & monitoring

> **“Internet” does NOT mean public exposure**
> It means **Azure-managed service endpoints**

---

### **Why Forced Tunneling Breaks MI**

❌ `0.0.0.0/0 → Firewall / NVA`

This blocks:

* Control plane traffic
* Backup uploads
* HA synchronization

Result:

* Provisioning failures
* Silent backup failures
* Unstable MI behavior

---

### **Correct UDR Design**

| Address Prefix | Next Hop         |
| -------------- | ---------------- |
| 0.0.0.0/0      | Internet         |
| AzureStorage   | Service endpoint |
| AzureMonitor   | Service endpoint |
| VirtualNetwork | Peering          |

---

## **15. NSG Rules – MI Subnet**

* NSGs allowed **only with Microsoft-approved rules**
* Platform ports must not be blocked
* Keep MI subnet minimal

---

## **16. Why Firewall Is Used for App Tier but NOT MI Subnet**

| Tier             | Firewall        |
| ---------------- | --------------- |
| App Subnet       | ✅ Required      |
| Analytics Subnet | ✅ Conditional   |
| **MI Subnet**    | ❌ Not supported |

Reason:

* MI is platform-managed
* Firewall breaks required service traffic

---

## **17. Authentication & Authorization**

* Azure AD Authentication
* Managed Identity
* SQL Auth (legacy only)
* Azure RBAC + SQL roles

---

## **18. Data Security & Compliance**

* TDE
* Always Encrypted
* Dynamic Data Masking
* Auditing
* Defender for SQL

---

## **19. Performance Optimization**

* Indexing
* Query Store
* Automatic tuning
* Read replicas

---

## **20. Partitioning & Sharding**

* Date-based partitioning
* Horizontal sharding
* Hyperscale support

---

## **21. Monitoring & Observability**

* Azure Monitor
* Log Analytics
* Query Performance Insight
* Alerts

---

## **22. Cost Optimization**

* Serverless auto-pause (60–70%)
* Reserved vCores
* Elastic pools
* Reporting replicas

---

## **23. Synapse & Data Lake Integration**

```
Azure SQL (Prov + Serverless)
        ↓
     ADLS Gen2
        ↓
Synapse Serverless SQL Pool
```

* PolyBase
* External Tables
* COPY

---

## **24. Automation & IaC**

* Azure CLI
* ARM / Bicep
* PowerShell
* Terraform

---





