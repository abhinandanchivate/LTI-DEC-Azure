
---

# **Case Study**

## **Enterprise Data Platform Modernization on Azure**

**(Azure SQL Database, Serverless SQL, Managed Instance & Synapse Integration)**

**Function / Name:** Data & Analytics
**Sub-Function Name:** Cloud Data Platform Engineering
**Process Name:** Enterprise OLTP → Analytics Modernization
**Version:** **2.2 (Updated – Serverless SQL Added)**
**Date:** 02-Jan-2023
**Classification:** Privileged & Confidential

---

## **1. Business Context**

A **global financial services enterprise** is modernizing its **on-premise SQL Server estate** to Azure to achieve:

* Secure **OLTP workloads** for core banking & transaction systems
* **High availability & disaster recovery (HA/DR)** across regions
* **On-demand, cost-efficient SQL compute** for variable workloads
* **Analytics & reporting modernization** using Azure Synapse
* **Cost optimization** using **serverless, elastic, and reserved models**
* **Zero-Trust network architecture** with Private Endpoints
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
              │  Firewall | Bastion | Log Mgmt  │
              └───────────────┬────────────────┘
                              │ Peering
        ┌─────────────────────┼────────────────────────┐
        │                     │                        │
┌───────▼───────┐   ┌─────────▼─────────┐   ┌──────────▼──────────┐
│ SPOKE VNET 1  │   │ SPOKE VNET 2       │   │ SPOKE VNET 3        │
│ App Tier      │   │ Data Tier          │   │ Analytics Tier     │
│ App Service   │   │ Azure SQL MI       │   │ Synapse             │
│ Functions     │   │ Azure SQL DB       │   │ Serverless SQL Pool │
│ VM-APP-01     │   │ SQL Serverless DB  │   │ ADLS Gen2           │
└───────────────┘   └────────────────────┘   └─────────────────────┘
```

---

## **3. Network Design (VNET, Subnet, 2 VMs)**

### **Virtual Networks**

### **Hub VNET**

* Azure Firewall
* Azure Bastion
* Log Analytics Workspace

### **Spoke VNET – App**

* Subnet: `app-subnet`
* **VM-APP-01** (legacy app workload)
* App Service & Azure Functions

### **Spoke VNET – Data**

* Subnet: `data-subnet`
* **VM-SQL-01** (SQL Server on VM)
* Azure SQL Managed Instance
* **Azure SQL Database (Serverless + Provisioned)**

### **Spoke VNET – Analytics**

* Azure Synapse Analytics
* Serverless SQL Pool
* ADLS Gen2

### **Connectivity**

* Hub ↔ Spoke VNET peering
* Private Endpoints for all PaaS services
* No public IP exposure (**Zero Trust**)

---

## **4. Azure SQL Deployment Models (WHY & WHERE)**

| Deployment Model                     | Enterprise Use Case                   |
| ------------------------------------ | ------------------------------------- |
| **Azure SQL Database – Provisioned** | Steady OLTP workloads                 |
| **Azure SQL Database – Serverless**  | Intermittent, unpredictable workloads |
| **Azure SQL Managed Instance**       | Lift-and-shift from on-prem SQL       |
| **SQL Server on VM**                 | Legacy apps requiring OS access       |

### **In This Case Study**

* **Azure SQL Database (Provisioned)**
  → Core customer profile services (steady traffic)

* **Azure SQL Database (Serverless)**
  → Reporting, ad-hoc analytics, UAT, Dev/Test workloads

* **Azure SQL Managed Instance**
  → Core banking transaction systems

* **SQL Server on VM**
  → Legacy reporting & third-party vendor integrations

---

## **5. DTU vs vCore Purchasing Models**

| DTU Model        | vCore Model                 |
| ---------------- | --------------------------- |
| Bundled CPU + IO | Independent CPU, memory, IO |
| Simple           | Enterprise control          |
| Entry-level      | Required for Serverless     |

### **Selected Approach**

* **vCore model** for:

  * Managed Instance
  * Business Critical tiers
* **vCore Serverless** for:

  * Dev/Test
  * Reporting
  * Intermittent workloads
* **DTU** limited to small PoC databases only

---

## **6. Service Tiers & Compute Models (Updated)**

| Tier / Compute                   | Usage                            |
| -------------------------------- | -------------------------------- |
| General Purpose (Provisioned)    | Dev/Test steady workloads        |
| **General Purpose (Serverless)** | Bursty & unpredictable workloads |
| Business Critical                | Mission-critical OLTP            |
| Hyperscale                       | Very large transactional history |

### **Serverless SQL Characteristics**

* Auto-scale compute (vCores)
* Auto-pause after inactivity
* Billed per second
* Storage billed separately
* Ideal for **non-24×7 workloads**

---

## **7. Serverless SQL – Enterprise Usage Pattern (NEW)**

### **Where Serverless SQL Is Used**

| Scenario               | Reason                   |
| ---------------------- | ------------------------ |
| Dev/Test environments  | Auto-pause saves cost    |
| UAT                    | Irregular usage          |
| Reporting DBs          | Read-heavy, non-constant |
| Ad-hoc analyst queries | Pay-per-use              |
| Month-end processing   | Short compute bursts     |

### **Why NOT Serverless for Core OLTP**

* Cold start latency
* Auto-pause not suitable for 24×7 systems
* Predictable workloads are cheaper on provisioned compute

---

## **8. Elastic Pools**

* Multiple **Azure SQL Databases (Provisioned + Serverless)** share compute
* Prevents over-provisioning
* Used for **multi-tenant SaaS databases**

---

## **9. Storage Scaling & IOPS**

* Independent scaling:

  * Compute
  * Storage
  * IOPS
* Hyperscale → instant storage growth
* Business Critical → local SSD replicas
* Serverless → compute pauses, storage remains available

---

## **10. Backup & Restore Strategy**

| Capability          | Implementation  |
| ------------------- | --------------- |
| PITR                | 7–35 days       |
| Long Term Retention | 7 years         |
| Geo-restore         | Cross-region    |
| Serverless support  | Fully supported |

---

## **11. High Availability & Disaster Recovery**

### **HA**

* Built-in replicas
* Automatic failover
* Zone redundancy (Business Critical)

### **DR**

* Active geo-replication
* Auto-failover groups
* Read-only secondary for reporting

---

## **12. Firewall Rules & Private Endpoints**

* All SQL services via **Private Endpoint**
* Public access disabled
* Azure Firewall controls outbound traffic
* SQL firewall restricted to application subnets

---

## **13. Authentication & Authorization**

### **Authentication**

* Azure AD authentication
* Managed Identity (Functions, ADF)
* SQL Auth retained only for legacy

### **Authorization**

* Azure RBAC (platform)
* SQL roles (data layer)

---

## **14. Data Security & Compliance**

| Feature              | Purpose            |
| -------------------- | ------------------ |
| TDE                  | Encryption at rest |
| Always Encrypted     | PII protection     |
| Dynamic Data Masking | Non-prod masking   |
| Auditing             | Compliance         |
| Defender for SQL     | Threat detection   |

---

## **15. Query Performance Optimization**

* Index strategies
* Query Store
* Intelligent Insights
* Automatic tuning
* Read replicas for reporting

---

## **16. Partitioning & Sharding**

* Date-based partitioning
* Horizontal sharding for global scale
* Hyperscale supports massive partition counts

---

## **17. Monitoring & Observability**

| Tool                      | Purpose               |
| ------------------------- | --------------------- |
| Azure Monitor             | Metrics               |
| Log Analytics             | Logs                  |
| Query Performance Insight | SQL tuning            |
| Alerts                    | Proactive remediation |

---

## **18. Cost Optimization Strategy (Updated)**

| Technique                 | Impact                   |
| ------------------------- | ------------------------ |
| **Serverless auto-pause** | Up to 60–70% savings     |
| Reserved vCore capacity   | Predictable savings      |
| Elastic pools             | Avoid over-provisioning  |
| Read replicas             | Reporting cost isolation |

---

## **19. Connectivity with Other Azure Services**

* App Service / Functions → OLTP
* Azure Data Factory → ETL
* Power BI → Analytics
* Logic Apps → Workflow orchestration

---

## **20. Synapse & Data Lake Integration**

### **Data Flow**

```
Azure SQL (Provisioned + Serverless)
        ↓
     ADLS Gen2
        ↓
Azure Synapse (Serverless SQL Pool)
```

### **Techniques**

* PolyBase
* External Tables
* COPY command

---

## **21. Automation & Infrastructure as Code**

### **Tools**

* Azure CLI
* ARM / Bicep
* PowerShell
* Terraform (optional)

### **Automated Items**

* SQL provisioning (Serverless & Provisioned)
* Failover groups
* Firewall rules
* Backup retention
* Monitoring & alerts

---

## **22. Business Outcomes**

* **99.99% availability**
* **40–60% cost reduction** using Serverless + Elastic Pools
* **Zero data breaches**
* **Faster analytics with Synapse**
* **Audit-ready compliance**

---



