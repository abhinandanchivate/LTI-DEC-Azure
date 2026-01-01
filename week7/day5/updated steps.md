# Azure SQL Database Enterprise Modernization
## Complete Step-by-Step UI Guide

**Version:** 2.4
**Date:** January 2026
**Classification:** Implementation Guide

---

## Table of Contents
1. [Network Foundation Setup](#network-foundation-setup)
2. [Hub VNET Configuration](#hub-vnet-configuration)
3. [Spoke VNETs Setup](#spoke-vnets-setup)
4. [Azure SQL Database - Provisioned](#azure-sql-database-provisioned)
5. [Azure SQL Database - Serverless](#azure-sql-database-serverless)
6. [SQL Managed Instance](#sql-managed-instance)
7. [SQL Server on Azure VM](#sql-server-on-azure-vm)
8. [Private Endpoints Configuration](#private-endpoints-configuration)
9. [High Availability & Disaster Recovery](#high-availability-disaster-recovery)
10. [Azure Synapse Analytics Setup](#azure-synapse-analytics-setup)
11. [Security & Compliance](#security-compliance)
12. [Monitoring & Automation](#monitoring-automation)

---

## PHASE 1: NETWORK FOUNDATION SETUP

### Step 1: Create Resource Group

**Portal Navigation:**
```
Azure Portal → Resource Groups → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create a resource group                                  │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Select your subscription ▼]              │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Region: (US) East US 2                        ▼          │
│                                                          │
│ Tags:                                                    │
│   Environment: Production                                │
│   Project: SQL-Modernization                             │
│   CostCenter: BFSI-DataPlatform                          │
│                                                          │
│         [Review + create]  [< Previous]  [Next >]        │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

## PHASE 2: HUB VNET CONFIGURATION

### Step 2: Create Hub Virtual Network

**Portal Navigation:**
```
Azure Portal → Virtual Networks → + Create
```

**UI Configuration - Basics Tab:**
```
┌─────────────────────────────────────────────────────────┐
│ Create virtual network                                   │
├─────────────────────────────────────────────────────────┤
│ Basics  IP Addresses  Security  Tags  Review + create    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Virtual network name: vnet-hub-prod-eastus2              │
│                                                          │
│ Region: (US) East US 2                        ▼          │
│                                                          │
│                              [Next: IP Addresses >]      │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - IP Addresses Tab:**
```
┌─────────────────────────────────────────────────────────┐
│ IP Addresses                                             │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ IPv4 address space: 10.0.0.0/16                          │
│                                                          │
│ ✓ Add IPv6 address space                                │
│                                                          │
│ Subnets:                                                 │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Name             Address range        Actions        │ │
│ ├─────────────────────────────────────────────────────┤ │
│ │ AzureFirewallSubnet  10.0.1.0/26    [Edit] [Delete] │ │
│ │ AzureBastionSubnet   10.0.2.0/26    [Edit] [Delete] │ │
│ │ GatewaySubnet        10.0.3.0/27    [Edit] [Delete] │ │
│ └─────────────────────────────────────────────────────┘ │
│                                                          │
│ [+ Add subnet]                                           │
│                                                          │
│                         [< Previous]  [Next: Security >] │
└─────────────────────────────────────────────────────────┘
```

**Important Notes:**
- **AzureFirewallSubnet** - Must be named exactly this
- **AzureBastionSubnet** - Must be named exactly this
- **GatewaySubnet** - For VPN Gateway if needed

**Action:** Click **Review + create** → **Create**

---

### Step 3: Deploy Azure Firewall

**Portal Navigation:**
```
Azure Portal → Firewalls → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create a firewall                                        │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Name: afw-hub-prod                                       │
│                                                          │
│ Region: East US 2                                        │
│                                                          │
│ Firewall tier:                                           │
│   ○ Standard                                             │
│   ● Premium  (For TLS inspection & IDPS)                 │
│                                                          │
│ Firewall policy: [Create new]                            │
│   Name: afwp-hub-prod                                    │
│                                                          │
│ Choose a virtual network:                                │
│   ● Use existing                                         │
│   Virtual network: vnet-hub-prod-eastus2      ▼          │
│                                                          │
│ Public IP address:                                       │
│   [Create new]                                           │
│   Name: pip-afw-hub-prod                                 │
│   SKU: Standard                                          │
│                                                          │
│ Forced tunneling: Disabled  (Default)                    │
│                                                          │
│         [Review + create]  [< Previous]  [Next >]        │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create** (Takes 5-10 minutes)

---

### Step 4: Deploy Azure Bastion

**Portal Navigation:**
```
Azure Portal → Bastions → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create a Bastion                                         │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Name: bastion-hub-prod                                   │
│                                                          │
│ Region: East US 2                                        │
│                                                          │
│ Tier: Standard  (For file copy support)                  │
│                                                          │
│ Virtual network: vnet-hub-prod-eastus2        ▼          │
│                                                          │
│ Subnet: AzureBastionSubnet (10.0.2.0/26)                 │
│                                                          │
│ Public IP address:                                       │
│   ● Create new                                           │
│   Name: pip-bastion-hub-prod                             │
│                                                          │
│         [Review + create]  [< Previous]  [Next >]        │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

## PHASE 3: SPOKE VNETS SETUP

### Step 5: Create Application Tier Spoke VNET

**Portal Navigation:**
```
Azure Portal → Virtual Networks → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create virtual network - vnet-spoke-app-prod             │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Virtual network name: vnet-spoke-app-prod-eastus2        │
│                                                          │
│ Region: East US 2                                        │
│                                                          │
│ IP Addresses:                                            │
│   IPv4 address space: 10.1.0.0/16                        │
│                                                          │
│ Subnets:                                                 │
│   Name: snet-app-01                                      │
│   Address range: 10.1.1.0/24                             │
│                                                          │
│   Name: snet-functions-01                                │
│   Address range: 10.1.2.0/24                             │
│                                                          │
│         [Review + create]                                │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

### Step 6: Create Data Tier Spoke VNET

**Portal Navigation:**
```
Azure Portal → Virtual Networks → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create virtual network - vnet-spoke-data-prod            │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Virtual network name: vnet-spoke-data-prod-eastus2       │
│                                                          │
│ Region: East US 2                                        │
│                                                          │
│ IP Addresses:                                            │
│   IPv4 address space: 10.2.0.0/16                        │
│                                                          │
│ Subnets:                                                 │
│   Name: snet-sqlmi-01                                    │
│   Address range: 10.2.1.0/24                             │
│   ⚠️  Delegate to: Microsoft.Sql/managedInstances        │
│                                                          │
│   Name: snet-data-pe-01                                  │
│   Address range: 10.2.2.0/24                             │
│   (For Private Endpoints)                                │
│                                                          │
│   Name: snet-sqlvm-01                                    │
│   Address range: 10.2.3.0/24                             │
│   (For SQL Server VMs)                                   │
│                                                          │
│         [Review + create]                                │
└─────────────────────────────────────────────────────────┘
```

**⚠️ CRITICAL: SQL MI Subnet Delegation**
```
When configuring snet-sqlmi-01:
1. Click subnet → Subnet delegation
2. Select: Microsoft.Sql/managedInstances
3. This is MANDATORY for MI deployment
```

**Action:** Click **Review + create** → **Create**

---

### Step 7: Create Analytics Tier Spoke VNET

**Portal Navigation:**
```
Azure Portal → Virtual Networks → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create virtual network - vnet-spoke-analytics-prod       │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Virtual network name: vnet-spoke-analytics-prod-eastus2  │
│                                                          │
│ Region: East US 2                                        │
│                                                          │
│ IP Addresses:                                            │
│   IPv4 address space: 10.3.0.0/16                        │
│                                                          │
│ Subnets:                                                 │
│   Name: snet-synapse-01                                  │
│   Address range: 10.3.1.0/24                             │
│                                                          │
│   Name: snet-analytics-pe-01                             │
│   Address range: 10.3.2.0/24                             │
│   (For Private Endpoints - Synapse, ADLS)                │
│                                                          │
│         [Review + create]                                │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

### Step 8: Configure VNET Peering (Hub-Spoke Model)

**Portal Navigation:**
```
Azure Portal → Virtual Networks → vnet-hub-prod-eastus2 → Peerings → + Add
```

**Peering 1: Hub → App Spoke**
```
┌─────────────────────────────────────────────────────────┐
│ Add peering                                              │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ This virtual network:                                    │
│   Peering link name: peer-hub-to-app                     │
│   ☑ Allow traffic to remote virtual network             │
│   ☑ Allow traffic forwarded from remote virtual network │
│   ☑ Allow gateway transit                                │
│                                                          │
│ Remote virtual network:                                  │
│   Peering link name: peer-app-to-hub                     │
│   Virtual network: vnet-spoke-app-prod-eastus2  ▼        │
│   ☑ Allow traffic to remote virtual network             │
│   ☑ Allow traffic forwarded from remote virtual network │
│   ☑ Use this virtual network's gateway                   │
│                                                          │
│                                       [Add]  [Cancel]    │
└─────────────────────────────────────────────────────────┘
```

**Peering 2: Hub → Data Spoke**
```
Repeat above with:
  - Peering link name: peer-hub-to-data / peer-data-to-hub
  - Virtual network: vnet-spoke-data-prod-eastus2
```

**Peering 3: Hub → Analytics Spoke**
```
Repeat above with:
  - Peering link name: peer-hub-to-analytics / peer-analytics-to-hub
  - Virtual network: vnet-spoke-analytics-prod-eastus2
```

**Action:** Click **Add** for each peering

---

## PHASE 4: AZURE SQL DATABASE - PROVISIONED

### Step 9: Create Azure SQL Database Server (Logical Server)

**Portal Navigation:**
```
Azure Portal → SQL servers → + Create
```

**UI Configuration - Basics:**
```
┌─────────────────────────────────────────────────────────┐
│ Create SQL Database server                               │
├─────────────────────────────────────────────────────────┤
│ Basics  Networking  Security  Additional settings        │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Server name: sql-server-prod-eastus2-001                 │
│   (Must be globally unique)                              │
│   ✓ Available                                            │
│                                                          │
│ Location: East US 2                           ▼          │
│                                                          │
│ Authentication:                                          │
│   ● Use both SQL and Azure AD authentication             │
│                                                          │
│   Server admin login: sqladmin                           │
│   Password: [Strong password - 16+ chars]                │
│   Confirm password: [Same password]                      │
│                                                          │
│   Set Azure AD admin: [Select]                           │
│     ☑ admin@yourdomain.com                               │
│                                                          │
│                              [Next: Networking >]        │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Networking:**
```
┌─────────────────────────────────────────────────────────┐
│ Networking                                               │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Network connectivity:                                    │
│   ○ Public endpoint                                      │
│   ● Private endpoint                                     │
│   ○ No access                                            │
│                                                          │
│ Private endpoints:                                       │
│   [+ Add private endpoint]                               │
│                                                          │
│   ⚠️ We'll configure this after server creation          │
│                                                          │
│ Connection policy:                                       │
│   ● Redirect (better performance)                        │
│   ○ Proxy                                                │
│   ○ Default                                              │
│                                                          │
│ Minimum TLS version:                                     │
│   1.2                                           ▼        │
│                                                          │
│                         [< Previous] [Next: Security >]  │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Security:**
```
┌─────────────────────────────────────────────────────────┐
│ Security                                                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Microsoft Defender for SQL:                              │
│   ● Enable  (Recommended for production)                 │
│                                                          │
│ Ledger:                                                  │
│   ☐ Enable ledger (for tamper-proof audit)              │
│                                                          │
│                    [< Previous] [Review + create]        │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

### Step 10: Create Azure SQL Database (Provisioned - vCore)

**Portal Navigation:**
```
Azure Portal → SQL databases → + Create
```

**UI Configuration - Basics:**
```
┌─────────────────────────────────────────────────────────┐
│ Create SQL Database                                      │
├─────────────────────────────────────────────────────────┤
│ Basics  Networking  Security  Additional settings        │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Database name: sqldb-customer-prod                       │
│                                                          │
│ Server: sql-server-prod-eastus2-001           ▼          │
│                                                          │
│ Want to use SQL elastic pool?                            │
│   ○ Yes                                                  │
│   ● No                                                   │
│                                                          │
│ Workload environment:                                    │
│   ● Production                                           │
│   ○ Development                                          │
│                                                          │
│ Compute + storage: [Configure database]                  │
│   ┌───────────────────────────────────────────────────┐ │
│   │ Service tier: General Purpose (GP)          ▼     │ │
│   │ Compute tier: Provisioned                   ▼     │ │
│   │ Hardware: Standard-series (Gen5)            ▼     │ │
│   │                                                   │ │
│   │ vCores: 4                                         │ │
│   │ [━━━━━━━━━━━━━━━━━━━━━━━━━━] 4 of 80 vCores      │ │
│   │                                                   │ │
│   │ Data max size (GB): 100                           │ │
│   │ [━━━━━━━━━━━━━━━━━━━━━━━━━━] 100 of 4096 GB      │ │
│   │                                                   │ │
│   │ Zone redundant: ☐ (Optional - adds cost)         │ │
│   │                                                   │ │
│   │ Estimated cost: ~$350/month                       │ │
│   │                                                   │ │
│   │                         [Apply]  [Cancel]         │ │
│   └───────────────────────────────────────────────────┘ │
│                                                          │
│ Backup storage redundancy:                               │
│   ● Geo-redundant backup storage (RA-GRS)                │
│   ○ Zone-redundant backup storage                        │
│   ○ Locally redundant backup storage                     │
│                                                          │
│                              [Next: Networking >]        │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Networking:**
```
┌─────────────────────────────────────────────────────────┐
│ Networking                                               │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Network connectivity:                                    │
│   ● Private access (via Private Endpoint)                │
│                                                          │
│   [+ Add private endpoint]                               │
│   (Will configure separately in Phase 8)                 │
│                                                          │
│                    [< Previous] [Next: Security >]       │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Security:**
```
┌─────────────────────────────────────────────────────────┐
│ Security                                                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Enable Microsoft Defender for SQL:                       │
│   ☑ (Inherited from server)                              │
│                                                          │
│                   [< Previous] [Next: Additional >]      │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Additional Settings:**
```
┌─────────────────────────────────────────────────────────┐
│ Additional settings                                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Data source:                                             │
│   ● None (blank database)                                │
│   ○ Backup                                               │
│   ○ Sample (AdventureWorksLT)                            │
│                                                          │
│ Database collation:                                      │
│   SQL_Latin1_General_CP1_CI_AS              ▼            │
│                                                          │
│ Enable auto-tuning:                                      │
│   ☑ Create index                                         │
│   ☑ Drop index                                           │
│   ☑ Force last good plan                                 │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create** (Takes 2-5 minutes)

---

## PHASE 5: AZURE SQL DATABASE - SERVERLESS

### Step 11: Create Azure SQL Database (Serverless - vCore)

**Portal Navigation:**
```
Azure Portal → SQL databases → + Create
```

**UI Configuration - Basics:**
```
┌─────────────────────────────────────────────────────────┐
│ Create SQL Database                                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Database name: sqldb-reporting-serverless                │
│                                                          │
│ Server: sql-server-prod-eastus2-001           ▼          │
│                                                          │
│ Want to use SQL elastic pool?: No                        │
│                                                          │
│ Workload environment: Development  (saves cost)          │
│                                                          │
│ Compute + storage: [Configure database]                  │
│   ┌───────────────────────────────────────────────────┐ │
│   │ Service tier: General Purpose (GP)          ▼     │ │
│   │ Compute tier: ● Serverless ◄── SELECT THIS       │ │
│   │ Hardware: Standard-series (Gen5)            ▼     │ │
│   │                                                   │ │
│   │ Min vCores: 0.5  [━━░░░░░░░░░░░░░░░░░░░░░░]      │ │
│   │ Max vCores: 2.0  [━━━━━░░░░░░░░░░░░░░░░░░░]      │ │
│   │                                                   │ │
│   │ Auto-pause delay (minutes):                       │ │
│   │   60  [━━━━━━━━░░░░░░░░░░░░░░░░░]                │ │
│   │   ☑ Enable auto-pause                             │ │
│   │   ⓘ Database pauses after 60 min of inactivity   │ │
│   │                                                   │ │
│   │ Data max size (GB): 50                            │ │
│   │ [━━━━━━━░░░░░░░░░░░░░░░░░░] 50 of 4096 GB        │ │
│   │                                                   │ │
│   │ ⚠️ Cold-start latency: ~30 seconds on resume      │ │
│   │                                                   │ │
│   │ Estimated cost: $15-60/month (usage-based)        │ │
│   │                                                   │ │
│   │                         [Apply]  [Cancel]         │ │
│   └───────────────────────────────────────────────────┘ │
│                                                          │
│ Backup storage redundancy:                               │
│   ● Locally redundant  (LRS - Cost savings for dev/test) │
│                                                          │
│                              [Next: Networking >]        │
└─────────────────────────────────────────────────────────┘
```

**Key Serverless Settings Explained:**

```
┌─────────────────────────────────────────────────────────┐
│ SERVERLESS CONFIGURATION GUIDE                           │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Min vCores: 0.5                                          │
│   • Minimum compute when active                          │
│   • Can set to 0.5 or 0.75 (0 not allowed)               │
│                                                          │
│ Max vCores: 2.0                                          │
│   • Maximum scale-up limit                               │
│   • Auto-scales based on workload                        │
│                                                          │
│ Auto-pause delay: 60 minutes                             │
│   • Time of inactivity before auto-pause                 │
│   • Range: 60 minutes to 7 days                          │
│   • Billing stops when paused                            │
│                                                          │
│ Billing Model:                                           │
│   • Per-second vCore usage when active                   │
│   • Storage billed separately (always)                   │
│   • No compute charges when paused                       │
│                                                          │
│ ⚠️ Not Suitable For:                                     │
│   • Mission-critical 24x7 apps                           │
│   • Apps needing instant response                        │
│   • Apps with constant traffic                           │
│                                                          │
│ ✅ Perfect For:                                          │
│   • Dev/Test environments                                │
│   • Reporting dashboards                                 │
│   • Ad-hoc analytics                                     │
│   • Bursty workloads                                     │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

## PHASE 6: SQL MANAGED INSTANCE

### Step 12: Create SQL Managed Instance

**Portal Navigation:**
```
Azure Portal → Azure SQL → + Create → SQL Managed Instance
```

**⚠️ IMPORTANT PRE-REQUISITES:**
```
Before creating MI, ensure:
1. ✓ Subnet delegated to Microsoft.Sql/managedInstances
2. ✓ Subnet has /24 or larger (minimum 32 IPs)
3. ✓ No NSG with restrictive rules on MI subnet
4. ✓ Route table prepared (we'll configure in Step 13)
```

**UI Configuration - Basics:**
```
┌─────────────────────────────────────────────────────────┐
│ Create Azure SQL Managed Instance                        │
├─────────────────────────────────────────────────────────┤
│ Basics  Networking  Security  Additional  Tags           │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Managed instance name: sqlmi-banking-prod                │
│   (3-63 chars, lowercase, no special chars)              │
│                                                          │
│ Region: East US 2                             ▼          │
│                                                          │
│ Managed instance admin login: miadmin                    │
│                                                          │
│ Password: [Strong password - 16+ chars]                  │
│ Confirm password: [Same password]                        │
│                                                          │
│ Compute + storage: [Configure managed instance]          │
│   ┌───────────────────────────────────────────────────┐ │
│   │ Service tier:                                     │ │
│   │   ● General Purpose (SSD storage, 8-100 vCores)   │ │
│   │   ○ Business Critical (local SSD, HA replicas)    │ │
│   │                                                   │ │
│   │ Hardware: Gen5                              ▼     │ │
│   │                                                   │ │
│   │ vCores: 8                                         │ │
│   │ [━━━━━━━━━━━━░░░░░░░░░░░░░░] 8 of 80            │ │
│   │                                                   │ │
│   │ Storage (GB): 256                                 │ │
│   │ [━━━━░░░░░░░░░░░░░░░░░░░░░░] 256 of 16384       │ │
│   │                                                   │ │
│   │ License type:                                     │ │
│   │   ● Azure Hybrid Benefit (Save up to 40%)        │ │
│   │   ○ License included                              │ │
│   │                                                   │ │
│   │ ⓘ Need existing SQL Server licenses               │ │
│   │                                                   │ │
│   │ Estimated cost: ~$600/month (with AHB)            │ │
│   │                                                   │ │
│   │                         [Apply]  [Cancel]         │ │
│   └───────────────────────────────────────────────────┘ │
│                                                          │
│                              [Next: Networking >]        │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Networking:**
```
┌─────────────────────────────────────────────────────────┐
│ Networking                                               │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Virtual network: vnet-spoke-data-prod-eastus2   ▼        │
│                                                          │
│ Subnet: snet-sqlmi-01 (10.2.1.0/24)             ▼        │
│   ✓ Delegated to Microsoft.Sql/managedInstances         │
│   ✓ 251 available IP addresses                          │
│                                                          │
│ Connection type:                                         │
│   ● Redirect  (Better performance, direct connection)    │
│   ○ Proxy  (For compatibility with some drivers)         │
│                                                          │
│ Public endpoint:                                         │
│   ○ Enable                                               │
│   ● Disable  ◄── RECOMMENDED for security               │
│                                                          │
│ ⚠️ CRITICAL: Route Table Configuration Required          │
│ └─────────────────────────────────────────────────────┐ │
│   After MI creation, you MUST configure:                │
│   • 0.0.0.0/0 → Internet (for control plane)            │
│   • Service tags for Storage, Monitor                   │
│   See Step 13 for detailed configuration                │
│ └─────────────────────────────────────────────────────┘ │
│                                                          │
│                      [< Previous] [Next: Security >]     │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Security:**
```
┌─────────────────────────────────────────────────────────┐
│ Security                                                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Enable Microsoft Defender for SQL:                       │
│   ☑ Enable  (Strongly recommended)                       │
│                                                          │
│ Azure AD authentication:                                 │
│   ☑ Set Azure AD admin                                   │
│   Admin: admin@yourdomain.com                            │
│                                                          │
│ Minimal TLS version:                                     │
│   1.2                                           ▼        │
│                                                          │
│ Transparent Data Encryption (TDE):                       │
│   ● Enabled (Default - cannot be disabled)               │
│                                                          │
│                         [< Previous] [Review + create]   │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

**⏱️ Deployment Time: 4-6 hours** (This is normal for MI)

---

### Step 13: Configure Route Table for SQL MI (CRITICAL)

**⚠️ WARNING: This step is MANDATORY after MI creation**

**Portal Navigation:**
```
Azure Portal → Route tables → + Create
```

**Create Route Table:**
```
┌─────────────────────────────────────────────────────────┐
│ Create route table                                       │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Region: East US 2                                        │
│                                                          │
│ Name: rt-sqlmi-subnet                                    │
│                                                          │
│ Propagate gateway routes:                                │
│   ● Yes  (Required for on-prem connectivity)             │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Add Routes:**
```
Azure Portal → Route tables → rt-sqlmi-subnet → Routes → + Add
```

**Route 1: Internet Route (CRITICAL)**
```
┌─────────────────────────────────────────────────────────┐
│ Add route                                                │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Route name: route-default-internet                       │
│                                                          │
│ Address prefix destination:                              │
│   ● IP Addresses                                         │
│                                                          │
│ Destination IP addresses/CIDR ranges:                    │
│   0.0.0.0/0                                              │
│                                                          │
│ Next hop type:                                           │
│   Internet  ▼  ◄── DO NOT CHANGE TO FIREWALL            │
│                                                          │
│ ⚠️ CRITICAL: This route is REQUIRED for MI operations    │
│    • Control plane management                            │
│    • Backup to Azure Storage                             │
│    • High availability traffic                           │
│    • Telemetry and monitoring                            │
│                                                          │
│    "Internet" does NOT mean public exposure              │
│    It means Azure backbone services                      │
│                                                          │
│                                       [OK]  [Cancel]     │
└─────────────────────────────────────────────────────────┘
```

**Route 2-4: Service Tags (Optional but Recommended)**
```
Route 2:
  Name: route-storage
  Address prefix: Service Tag
  Service tag: Storage
  Next hop: Internet

Route 3:
  Name: route-azuremonitor
  Address prefix: Service Tag
  Service tag: AzureMonitor
  Next hop: Internet

Route 4:
  Name: route-azuread
  Address prefix: Service Tag
  Service tag: AzureActiveDirectory
  Next hop: Internet
```

**Associate Route Table with MI Subnet:**
```
Azure Portal → Virtual networks → vnet-spoke-data-prod-eastus2
→ Subnets → snet-sqlmi-01 → Route table
```

```
┌─────────────────────────────────────────────────────────┐
│ snet-sqlmi-01 - Subnet configuration                     │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Route table:                                             │
│   rt-sqlmi-subnet                               ▼        │
│                                                          │
│                                       [Save]  [Discard]  │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Save**

---

### Step 14: Configure NSG for SQL MI (Optional)

**⚠️ CAUTION: NSGs are allowed but must follow Microsoft rules**

**Portal Navigation:**
```
Azure Portal → Network security groups → + Create
```

```
┌─────────────────────────────────────────────────────────┐
│ SQL MI NSG - Required Rules                              │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ ⚠️ DO NOT block these inbound ports:                     │
│   • 9000, 9003, 1438, 1440, 1452                         │
│   • Source: VirtualNetwork                               │
│   • Priority: < 4096                                     │
│                                                          │
│ ⚠️ DO NOT block these outbound ports:                    │
│   • 443, 12000                                           │
│   • Destination: Internet                                │
│                                                          │
│ Recommendation: Use default NSG or none                  │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

## PHASE 7: SQL SERVER ON AZURE VM

### Step 15: Deploy SQL Server on Azure VM

**Portal Navigation:**
```
Azure Portal → Virtual machines → + Create
```

**UI Configuration - Basics:**
```
┌─────────────────────────────────────────────────────────┐
│ Create a virtual machine                                 │
├─────────────────────────────────────────────────────────┤
│ Basics  Disks  Networking  Management  Advanced          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Virtual machine name: vm-sql-01                          │
│                                                          │
│ Region: East US 2                             ▼          │
│                                                          │
│ Availability options:                                    │
│   Availability set                            ▼          │
│   [Create new]: avset-sql-vms                            │
│                                                          │
│ Security type: Standard                                  │
│                                                          │
│ Image: [See all images]                                  │
│   Search: "SQL Server 2022 Enterprise"                   │
│   → SQL Server 2022 Enterprise on Windows Server 2022    │
│                                                          │
│ Size: Standard_E8s_v5                         ▼          │
│   (8 vCPUs, 64 GB RAM - Minimum for production SQL)      │
│                                                          │
│ Administrator account:                                   │
│   Username: sqladmin                                     │
│   Password: [Strong password]                            │
│   Confirm password: [Same password]                      │
│                                                          │
│ Inbound port rules:                                      │
│   Public inbound ports: None  ◄── No public access      │
│                                                          │
│                              [Next: Disks >]             │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Disks:**
```
┌─────────────────────────────────────────────────────────┐
│ Disks                                                    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ OS disk:                                                 │
│   OS disk type: Premium SSD (LRS)             ▼          │
│   Size: P30 (1024 GB)                         ▼          │
│                                                          │
│ Data disks:                                              │
│   [+ Create and attach a new disk]                       │
│                                                          │
│   Disk 1 - SQL Data:                                     │
│     Name: vm-sql-01_DataDisk_0                           │
│     Size: P40 (2048 GB)                       ▼          │
│     Storage type: Premium SSD (LRS)           ▼          │
│     Host caching: Read-only                   ▼          │
│                                                          │
│   Disk 2 - SQL Logs:                                     │
│     Name: vm-sql-01_LogDisk_0                            │
│     Size: P30 (1024 GB)                       ▼          │
│     Storage type: Premium SSD (LRS)           ▼          │
│     Host caching: None                        ▼          │
│                                                          │
│   Disk 3 - TempDB:                                       │
│     Name: vm-sql-01_TempDisk_0                           │
│     Size: P20 (512 GB)                        ▼          │
│     Storage type: Premium SSD (LRS)           ▼          │
│     Host caching: Read-only                   ▼          │
│                                                          │
│                              [Next: Networking >]        │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Networking:**
```
┌─────────────────────────────────────────────────────────┐
│ Networking                                               │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Virtual network: vnet-spoke-data-prod-eastus2   ▼        │
│                                                          │
│ Subnet: snet-sqlvm-01 (10.2.3.0/24)             ▼        │
│                                                          │
│ Public IP: None                               ▼          │
│                                                          │
│ NIC network security group: Advanced          ▼          │
│                                                          │
│ Configure network security group:                        │
│   [Create new]                                           │
│   Allow inbound: 1433 from VNET only                     │
│                                                          │
│ Accelerated networking:                                  │
│   ☑ Enable  (Recommended for SQL Server)                 │
│                                                          │
│                          [Next: Management >]            │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - SQL Server Settings:**
```
After VM deployment, configure SQL Server:

Azure Portal → Virtual machines → vm-sql-01 → SQL Server configuration
```

```
┌─────────────────────────────────────────────────────────┐
│ SQL Server configuration                                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ SQL connectivity:                                        │
│   ● Private (within Virtual Network)                     │
│   Port: 1433                                             │
│                                                          │
│ SQL Authentication:                                      │
│   ☑ Enable                                               │
│   SQL login name: sa                                     │
│   Password: [Strong password]                            │
│                                                          │
│ Azure Key Vault integration:                             │
│   ☑ Enable  (For TDE key management)                     │
│   Key vault: kv-sql-prod-eastus2              ▼          │
│                                                          │
│ SQL Server Machine Learning Services:                    │
│   ☐ Enable  (If needed)                                  │
│                                                          │
│ Storage configuration:                                   │
│   Data files: E:\ (vm-sql-01_DataDisk_0)                 │
│   Log files: F:\ (vm-sql-01_LogDisk_0)                   │
│   TempDB: D:\ (vm-sql-01_TempDisk_0)                     │
│                                                          │
│ Automated backup:                                        │
│   ☑ Enable                                               │
│   Retention: 30 days                                     │
│   Storage account: stprodbackup001            ▼          │
│                                                          │
│ Automated patching:                                      │
│   ☑ Enable                                               │
│   Day: Sunday                                 ▼          │
│   Start time: 02:00                                      │
│   Duration: 60 minutes                                   │
│                                                          │
│                                       [Apply]  [Cancel]  │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Apply**

---

## PHASE 8: PRIVATE ENDPOINTS CONFIGURATION

### Step 16: Create Private Endpoint for Azure SQL Database

**Portal Navigation:**
```
Azure Portal → Private Link Center → Private endpoints → + Create
```

**UI Configuration - Basics:**
```
┌─────────────────────────────────────────────────────────┐
│ Create a private endpoint                                │
├─────────────────────────────────────────────────────────┤
│ Basics  Resource  Virtual Network  DNS  Tags             │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Name: pe-sqldb-customer-prod                             │
│                                                          │
│ Network Interface Name:                                  │
│   pe-sqldb-customer-prod-nic  (auto-generated)           │
│                                                          │
│ Region: East US 2                             ▼          │
│                                                          │
│                              [Next: Resource >]          │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Resource:**
```
┌─────────────────────────────────────────────────────────┐
│ Resource                                                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Connection method:                                       │
│   ● Connect to an Azure resource in my directory         │
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource type: Microsoft.Sql/servers          ▼          │
│                                                          │
│ Resource: sql-server-prod-eastus2-001         ▼          │
│                                                          │
│ Target sub-resource: sqlServer                ▼          │
│                                                          │
│                      [Next: Virtual Network >]           │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Virtual Network:**
```
┌─────────────────────────────────────────────────────────┐
│ Virtual Network                                          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Virtual network: vnet-spoke-data-prod-eastus2   ▼        │
│                                                          │
│ Subnet: snet-data-pe-01 (10.2.2.0/24)           ▼        │
│                                                          │
│ Network policy for private endpoints:                    │
│   ○ Disabled                                             │
│   ● Enabled  (Recommended)                               │
│                                                          │
│ Private IP configuration:                                │
│   ● Dynamically allocate IP address                      │
│   ○ Statically allocate IP address                       │
│                                                          │
│ Application security group:                              │
│   [None selected]                                        │
│                                                          │
│                              [Next: DNS >]               │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - DNS:**
```
┌─────────────────────────────────────────────────────────┐
│ DNS                                                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Integrate with private DNS zone:                         │
│   ● Yes  ◄── CRITICAL for name resolution               │
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Private DNS Zone:                                        │
│   [+ Create new]                                         │
│   Name: privatelink.database.windows.net                 │
│   (Auto-populated, do not change)                        │
│                                                          │
│ ⓘ This creates A record for PE in private DNS zone       │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

### Step 17: Create Private Endpoint for ADLS Gen2 (Blob)

**Portal Navigation:**
```
Azure Portal → Storage accounts → Create storage account
```

**First, create ADLS Gen2 storage:**
```
┌─────────────────────────────────────────────────────────┐
│ Create storage account                                   │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Storage account name: stadlsprodeastus2                  │
│                                                          │
│ Region: East US 2                                        │
│                                                          │
│ Performance: ● Standard                                  │
│                                                          │
│ Redundancy: Geo-redundant storage (GRS)       ▼          │
│                                                          │
│ Advanced tab:                                            │
│   ☑ Enable hierarchical namespace  ◄── ADLS Gen2        │
│                                                          │
│ Networking tab:                                          │
│   Network connectivity:                                  │
│   ● Disable public access and use private access         │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Then create TWO Private Endpoints for ADLS:**

**Private Endpoint 1: Blob**
```
Portal → Private Link Center → Private endpoints → + Create

Resource:
  Resource type: Microsoft.Storage/storageAccounts
  Resource: stadlsprodeastus2
  Target sub-resource: blob  ◄── For Blob API

Virtual Network:
  Virtual network: vnet-spoke-analytics-prod-eastus2
  Subnet: snet-analytics-pe-01

DNS:
  Private DNS Zone: privatelink.blob.core.windows.net
```

**Private Endpoint 2: DFS (Data Lake)**
```
Repeat above but:
  Name: pe-adls-dfs-prod
  Target sub-resource: dfs  ◄── For Data Lake API
  Private DNS Zone: privatelink.dfs.core.windows.net
```

---

### Step 18: Disable Public Access

**For Azure SQL Database Server:**
```
Azure Portal → SQL servers → sql-server-prod-eastus2-001
→ Networking → Public access
```

```
┌─────────────────────────────────────────────────────────┐
│ Public network access                                    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Public network access:                                   │
│   ● Disabled  ◄── Zero Trust principle                   │
│   ○ Selected networks                                    │
│   ○ All networks                                         │
│                                                          │
│ ⚠️ After disabling, access only via Private Endpoint     │
│                                                          │
│                                       [Save]  [Discard]  │
└─────────────────────────────────────────────────────────┘
```

**For Storage Account:**
```
Azure Portal → Storage accounts → stadlsprodeastus2
→ Networking → Firewalls and virtual networks
```

```
┌─────────────────────────────────────────────────────────┐
│ Firewalls and virtual networks                           │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Public network access:                                   │
│   ● Disabled  ◄── Zero Trust principle                   │
│   ○ Enabled from selected virtual networks and IP addresses │
│   ○ Enabled from all networks                            │
│                                                          │
│                                       [Save]  [Discard]  │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Save**

---

## PHASE 9: HIGH AVAILABILITY & DISASTER RECOVERY

### Step 19: Configure Geo-Replication for Azure SQL Database

**Portal Navigation:**
```
Azure Portal → SQL databases → sqldb-customer-prod → Replicas
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create geo-replica                                       │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Replica database name: sqldb-customer-prod-dr            │
│                                                          │
│ Target server:                                           │
│   [Create new server]                                    │
│   Server name: sql-server-dr-westus2-001                 │
│   Location: West US 2  ◄── Different region             │
│   Admin login: sqladmin                                  │
│   Password: [Strong password]                            │
│                                                          │
│ Compute + storage:                                       │
│   Service tier: General Purpose                          │
│   Compute tier: Provisioned                              │
│   vCores: 4  (Same as primary)                           │
│   Storage: 100 GB  (Same as primary)                     │
│                                                          │
│ Replica type:                                            │
│   ● Geo-replica  (Different region, async replication)   │
│                                                          │
│ Make replica readable:                                   │
│   ☑ Yes  (Use for read-only reporting)                   │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

### Step 20: Create Failover Group

**Portal Navigation:**
```
Azure Portal → SQL servers → sql-server-prod-eastus2-001
→ Failover groups → + Add group
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Failover group                                           │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Failover group name: fog-sql-prod-dr                     │
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Secondary server:                                        │
│   sql-server-dr-westus2-001               ▼              │
│   (West US 2)                                            │
│                                                          │
│ Databases within the group:                              │
│   ☑ sqldb-customer-prod                                  │
│   ☐ sqldb-reporting-serverless  (Optional)               │
│                                                          │
│ Read/write failover policy:                              │
│   ● Automatic                                            │
│   Grace period: 1 hour                                   │
│   ⓘ Auto-failover after 1 hour of outage                 │
│                                                          │
│ Read-only failover policy:                               │
│   ● Enabled                                              │
│   ⓘ Read workloads fail over to secondary                │
│                                                          │
│ Failover group connection strings:                       │
│   Read-write listener:                                   │
│     fog-sql-prod-dr.database.windows.net                 │
│                                                          │
│   Read-only listener:                                    │
│     fog-sql-prod-dr.secondary.database.windows.net       │
│                                                          │
│                              [Create]  [Cancel]          │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Create**

---

### Step 21: Enable Zone Redundancy (Business Critical Tier)

**For mission-critical databases, upgrade to Business Critical:**

```
Azure Portal → SQL databases → sqldb-customer-prod
→ Compute + storage → Configure
```

```
┌─────────────────────────────────────────────────────────┐
│ Configure                                                │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Service tier:                                            │
│   ○ General Purpose                                      │
│   ● Business Critical  ◄── For 99.995% SLA              │
│                                                          │
│ Compute tier: Provisioned                                │
│                                                          │
│ Hardware: Gen5                                           │
│                                                          │
│ vCores: 4                                                │
│                                                          │
│ Data max size: 100 GB                                    │
│                                                          │
│ Zone redundant:                                          │
│   ☑ Enable  ◄── Spread replicas across zones            │
│   ⓘ Protects against datacenter failures                 │
│                                                          │
│ Cost impact: +30% for zone redundancy                    │
│                                                          │
│                              [Apply]  [Cancel]           │
└─────────────────────────────────────────────────────────┘
```

---

### Step 22: Configure SQL MI High Availability

**SQL MI automatically includes:**
```
┌─────────────────────────────────────────────────────────┐
│ SQL MI Built-in HA (Always Active)                       │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ General Purpose:                                         │
│   ✓ 3 replicas (1 primary + 2 secondary)                 │
│   ✓ Remote storage (Azure Premium Storage)              │
│   ✓ 99.99% SLA                                           │
│   ✓ Auto-healing on failure                              │
│                                                          │
│ Business Critical:                                       │
│   ✓ 4 replicas (1 primary + 3 secondary)                 │
│   ✓ Local SSD storage (all replicas)                     │
│   ✓ 99.99% SLA (99.995% with zone redundancy)           │
│   ✓ 1 readable secondary replica (free)                  │
│   ✓ Synchronous commit to one secondary                  │
│                                                          │
│ No configuration needed - always enabled                 │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**To enable Zone Redundancy for MI:**
```
Azure Portal → SQL Managed Instance → sqlmi-banking-prod
→ Compute + storage → Zone redundant
```

```
┌─────────────────────────────────────────────────────────┐
│ Zone redundancy                                          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ ☑ Enable zone redundancy                                │
│                                                          │
│ ⓘ Available only in Business Critical tier               │
│ ⓘ Spreads replicas across availability zones             │
│ ⓘ +30% cost premium                                      │
│                                                          │
│                              [Apply]  [Cancel]           │
└─────────────────────────────────────────────────────────┘
```

---

## PHASE 10: AZURE SYNAPSE ANALYTICS SETUP

### Step 23: Create Azure Synapse Workspace

**Portal Navigation:**
```
Azure Portal → Azure Synapse Analytics → + Create
```

**UI Configuration - Basics:**
```
┌─────────────────────────────────────────────────────────┐
│ Create Synapse workspace                                 │
├─────────────────────────────────────────────────────────┤
│ Basics  Security  Networking  Tags  Review + create      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Workspace name: synapse-analytics-prod                   │
│                                                          │
│ Region: East US 2                             ▼          │
│                                                          │
│ Select Data Lake Storage Gen2:                           │
│   Account name: stadlsprodeastus2             ▼          │
│   File system name: synapse-data                         │
│   [Create new]  (If doesn't exist)                       │
│                                                          │
│ Assign myself the Storage Blob Data Contributor role:    │
│   ☑ Yes                                                  │
│                                                          │
│                              [Next: Security >]          │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Security:**
```
┌─────────────────────────────────────────────────────────┐
│ Security                                                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Authentication method:                                   │
│   ● Use both local and Azure Active Directory auth       │
│                                                          │
│ SQL Server admin login: synapseadmin                     │
│ Password: [Strong password]                              │
│                                                          │
│ Azure Active Directory admin:                            │
│   [Set admin]                                            │
│   admin@yourdomain.com                                   │
│                                                          │
│ Managed Virtual Network:                                 │
│   ☑ Enable  ◄── For private workspace                    │
│   ⓘ Creates dedicated VNET for Synapse                   │
│                                                          │
│ Allow connections from all IP addresses:                 │
│   ☐ (Uncheck for security)                               │
│                                                          │
│                              [Next: Networking >]        │
└─────────────────────────────────────────────────────────┘
```

**UI Configuration - Networking:**
```
┌─────────────────────────────────────────────────────────┐
│ Networking                                               │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ ⚠️ Managed VNET is already enabled from Security tab     │
│                                                          │
│ Connectivity method:                                     │
│   ● Private endpoint only                                │
│   ○ Public endpoint (not recommended)                    │
│                                                          │
│ Private endpoints will be configured post-creation       │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

### Step 24: Create Synapse Private Endpoints

**Create 3 Private Endpoints for Synapse:**

**PE 1: SQL (Dedicated SQL Pool)**
```
Portal → Private endpoints → + Create

Resource:
  Resource type: Microsoft.Synapse/workspaces
  Resource: synapse-analytics-prod
  Target sub-resource: Sql  ◄── For SQL pools

Virtual Network:
  Virtual network: vnet-spoke-analytics-prod-eastus2
  Subnet: snet-analytics-pe-01

DNS:
  Private DNS Zone: privatelink.sql.azuresynapse.net
```

**PE 2: SqlOnDemand (Serverless SQL)**
```
Repeat above but:
  Name: pe-synapse-sqlondemand-prod
  Target sub-resource: SqlOnDemand
  Private DNS Zone: privatelink.sql.azuresynapse.net
```

**PE 3: Dev (Synapse Studio)**
```
Repeat above but:
  Name: pe-synapse-dev-prod
  Target sub-resource: Dev
  Private DNS Zone: privatelink.dev.azuresynapse.net
```

---

### Step 25: Create Serverless SQL Pool

**⚠️ Built-in by default - no deployment needed**

```
┌─────────────────────────────────────────────────────────┐
│ Synapse Serverless SQL Pool - Built-in                   │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Name: Built-in  (Cannot be changed)                      │
│                                                          │
│ Features:                                                │
│   ✓ Query data in ADLS Gen2 (Parquet, CSV, JSON)        │
│   ✓ Create external tables                               │
│   ✓ Pay-per-query (no provisioning)                      │
│   ✓ T-SQL interface                                      │
│                                                          │
│ Connection string:                                       │
│   synapse-analytics-prod-ondemand.sql.azuresynapse.net   │
│                                                          │
│ ⓘ Ready to use immediately after workspace creation      │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**To query data:**
```
Azure Portal → Synapse workspaces → synapse-analytics-prod
→ Open Synapse Studio
```

```
In Synapse Studio:
  → Data → Linked → Azure Data Lake Storage Gen2
  → stadlsprodeastus2 → synapse-data
  → Right-click file → New SQL script → Select TOP 100
```

---

### Step 26: Create External Table in Serverless SQL

**Open Synapse Studio → Develop → + → SQL script**

```sql
-- Step 1: Create database
CREATE DATABASE SynapseLake;
GO

USE SynapseLake;
GO

-- Step 2: Create master key (for encryption)
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword123!';
GO

-- Step 3: Create database-scoped credential
CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH IDENTITY = 'Managed Identity';
GO

-- Step 4: Create external data source
CREATE EXTERNAL DATA SOURCE ADLSSource
WITH (
    LOCATION = 'https://stadlsprodeastus2.dfs.core.windows.net/synapse-data/',
    CREDENTIAL = ADLSCredential
);
GO

-- Step 5: Create external file format
CREATE EXTERNAL FILE FORMAT ParquetFormat
WITH (
    FORMAT_TYPE = PARQUET,
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
);
GO

-- Step 6: Create external table
CREATE EXTERNAL TABLE dbo.FactSales
(
    SaleID INT,
    ProductID INT,
    CustomerID INT,
    SaleDate DATE,
    Amount DECIMAL(18,2)
)
WITH (
    LOCATION = 'sales/*.parquet',
    DATA_SOURCE = ADLSSource,
    FILE_FORMAT = ParquetFormat
);
GO

-- Step 7: Query the external table
SELECT 
    YEAR(SaleDate) AS SaleYear,
    COUNT(*) AS TotalSales,
    SUM(Amount) AS TotalRevenue
FROM dbo.FactSales
GROUP BY YEAR(SaleDate)
ORDER BY SaleYear;
GO
```

---

## PHASE 11: SECURITY & COMPLIANCE

### Step 27: Enable Transparent Data Encryption (TDE)

**For Azure SQL Database (Already enabled by default):**
```
Azure Portal → SQL databases → sqldb-customer-prod
→ Transparent data encryption
```

```
┌─────────────────────────────────────────────────────────┐
│ Transparent data encryption                              │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Data encryption:                                         │
│   ● On  (Default - cannot be disabled)                   │
│                                                          │
│ Encryption key management:                               │
│   ● Service-managed key  (Default)                       │
│   ○ Customer-managed key  (Azure Key Vault)              │
│                                                          │
│ If using customer-managed key:                           │
│   Key vault: kv-sql-prod-eastus2              ▼          │
│   Key: tde-key-2025                           ▼          │
│   Managed identity: Enabled                              │
│                                                          │
│                                       [Save]  [Discard]  │
└─────────────────────────────────────────────────────────┘
```

---

### Step 28: Enable Microsoft Defender for SQL

**Portal Navigation:**
```
Azure Portal → SQL servers → sql-server-prod-eastus2-001
→ Microsoft Defender for Cloud
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Microsoft Defender for SQL                               │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Status: ● Enabled                                        │
│                                                          │
│ Features:                                                │
│   ✓ Vulnerability assessment                             │
│   ✓ Advanced threat detection                            │
│   ✓ Data discovery & classification                      │
│   ✓ Security alerts & recommendations                    │
│                                                          │
│ Vulnerability assessment:                                │
│   Storage account: stprodvulnscan001          ▼          │
│   ☑ Periodic recurring scans (weekly)                    │
│   ☑ Send scan reports to:                                │
│       security-team@yourdomain.com                       │
│                                                          │
│ Advanced threat detection settings:                      │
│   Alert types: All  ✓                                    │
│   Send alerts to:                                        │
│     ☑ Email service and co-administrators                │
│     ☑ security-team@yourdomain.com                       │
│                                                          │
│ Cost: ~$15/server/month + $0.02/GB scanned               │
│                                                          │
│                                       [Save]  [Discard]  │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Save**

---

### Step 29: Configure Auditing

**Portal Navigation:**
```
Azure Portal → SQL servers → sql-server-prod-eastus2-001
→ Auditing
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Auditing                                                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Enable Azure SQL Auditing:                               │
│   ● On                                                   │
│                                                          │
│ Audit log destination:                                   │
│   ☑ Storage                                              │
│     Storage account: stprodauditlogs001       ▼          │
│     Retention (days): 90                                 │
│                                                          │
│   ☑ Log Analytics                                        │
│     Log Analytics workspace: law-prod-eastus2 ▼          │
│                                                          │
│   ☑ Event Hub                                            │
│     Event Hub namespace: eh-prod-eastus2      ▼          │
│     Event Hub name: sql-audit-events          ▼          │
│                                                          │
│ Audit Actions and Groups:                                │
│   ☑ SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP             │
│   ☑ FAILED_DATABASE_AUTHENTICATION_GROUP                 │
│   ☑ BATCH_COMPLETED_GROUP                                │
│   ☑ DATABASE_OBJECT_CHANGE_GROUP                         │
│   ☑ DATABASE_PRINCIPAL_CHANGE_GROUP                      │
│   ☑ DATABASE_ROLE_MEMBER_CHANGE_GROUP                    │
│   ☑ SCHEMA_OBJECT_CHANGE_GROUP                           │
│   ☑ SCHEMA_OBJECT_ACCESS_GROUP                           │
│                                                          │
│                                       [Save]  [Discard]  │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Save**

---

### Step 30: Configure Dynamic Data Masking

**Portal Navigation:**
```
Azure Portal → SQL databases → sqldb-customer-prod
→ Dynamic Data Masking
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Dynamic Data Masking                                     │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Recommended fields to mask:                              │
│   ☑ dbo.Customers.Email                                  │
│   ☑ dbo.Customers.PhoneNumber                            │
│   ☑ dbo.Accounts.CreditCardNumber                        │
│   ☑ dbo.Accounts.SSN                                     │
│                                                          │
│ [+ Add mask]  (For custom rules)                         │
│                                                          │
│ Custom masking rules:                                    │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Rule 1:                                              │ │
│ │   Schema: dbo                                        │ │
│ │   Table: Customers                                   │ │
│ │   Column: Email                             ▼        │ │
│ │   Masking function: Email  (xxx@xxxx.com)   ▼        │ │
│ │   [Save]  [Delete]                                   │ │
│ │                                                      │ │
│ │ Rule 2:                                              │ │
│ │   Schema: dbo                                        │ │
│ │   Table: Accounts                                    │ │
│ │   Column: CreditCardNumber                  ▼        │ │
│ │   Masking function: Credit card (XXXX-XXXX-XXXX-1234)│ │
│ │   [Save]  [Delete]                                   │ │
│ └─────────────────────────────────────────────────────┘ │
│                                                          │
│ Excluded SQL users:                                      │
│   [+ Add user]                                           │
│   • sqladmin  (Full access)                              │
│   • reporting_service  (Masked data)                     │
│                                                          │
│                                       [Save]  [Discard]  │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Save**

---

### Step 31: Enable Always Encrypted

**Prerequisites:**
1. Azure Key Vault with keys
2. Column Master Key (CMK)
3. Column Encryption Key (CEK)

**Using SQL Server Management Studio (SSMS):**
```
1. Connect to Azure SQL Database
2. Right-click table → Encrypt Columns
```

```
┌─────────────────────────────────────────────────────────┐
│ Always Encrypted Wizard - SSMS                           │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Step 1: Column Selection                                 │
│   Table: dbo.Customers                                   │
│   Columns to encrypt:                                    │
│     ☑ SSN  → Deterministic  (For searches)               │
│     ☑ CreditCardNumber  → Randomized  (Highest security) │
│                                                          │
│ Step 2: Master Key Configuration                         │
│   Location: Azure Key Vault                   ▼          │
│   Key Vault: kv-sql-prod-eastus2              ▼          │
│   Generate new CMK: ☑                                    │
│   CMK name: CMK_Auto_2025                                │
│                                                          │
│ Step 3: Run Settings                                     │
│   ● Proceed to finish now                                │
│   ○ Generate PowerShell script                           │
│                                                          │
│                              [Finish]  [Cancel]          │
└─────────────────────────────────────────────────────────┘
```

---

## PHASE 12: MONITORING & AUTOMATION

### Step 32: Create Log Analytics Workspace

**Portal Navigation:**
```
Azure Portal → Log Analytics workspaces → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create Log Analytics workspace                           │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Name: law-sql-monitoring-prod                            │
│                                                          │
│ Region: East US 2                             ▼          │
│                                                          │
│ Pricing tier: Pay-as-you-go  (Default)                   │
│                                                          │
│ Daily cap: [Optional - Set limit]                        │
│   1 GB per day                                           │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

### Step 33: Configure Diagnostic Settings

**For Azure SQL Database:**
```
Azure Portal → SQL databases → sqldb-customer-prod
→ Diagnostic settings → + Add diagnostic setting
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Diagnostic setting                                       │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Diagnostic setting name: diag-sqldb-customer             │
│                                                          │
│ Logs:                                                    │
│   ☑ SQLInsights                                          │
│   ☑ AutomaticTuning                                      │
│   ☑ QueryStoreRuntimeStatistics                          │
│   ☑ QueryStoreWaitStatistics                             │
│   ☑ Errors                                               │
│   ☑ DatabaseWaitStatistics                               │
│   ☑ Timeouts                                             │
│   ☑ Blocks                                               │
│   ☑ Deadlocks                                            │
│   ☑ DevOpsOperationsAudit                                │
│   ☑ SQLSecurityAuditEvents                               │
│                                                          │
│ Metrics:                                                 │
│   ☑ AllMetrics                                           │
│                                                          │
│ Destination details:                                     │
│   ☑ Send to Log Analytics workspace                      │
│     Subscription: [Your subscription]                    │
│     Log Analytics workspace: law-sql-monitoring-prod ▼   │
│                                                          │
│   ☑ Archive to a storage account                         │
│     Storage account: stproddiagnostics001     ▼          │
│     Retention (days): 90                                 │
│                                                          │
│                                       [Save]  [Cancel]   │
└─────────────────────────────────────────────────────────┘
```

**Repeat for:**
- SQL Managed Instance
- SQL Server (VM)
- Synapse Workspace
- ADLS Gen2

**Action:** Click **Save**

---

### Step 34: Create Alert Rules

**Portal Navigation:**
```
Azure Portal → Monitor → Alerts → + Create → Alert rule
```

**Alert 1: High DTU/CPU Usage**
```
┌─────────────────────────────────────────────────────────┐
│ Create alert rule                                        │
├─────────────────────────────────────────────────────────┤
│ Scope  Condition  Actions  Details                       │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Scope:                                                   │
│   Resource: sqldb-customer-prod                          │
│   Resource type: SQL Database                            │
│                                                          │
│ Condition:                                               │
│   Signal name: CPU percentage                 ▼          │
│   Operator: Greater than                      ▼          │
│   Threshold value: 80                                    │
│   Aggregation type: Average                   ▼          │
│   Evaluation period: 5 minutes                           │
│   Check every: 1 minute                                  │
│                                                          │
│ Actions:                                                 │
│   Action group: ag-sql-alerts-prod            ▼          │
│   [Create action group]                                  │
│     Email: dbateam@yourdomain.com                        │
│     SMS: +1-555-0100                                     │
│     Webhook: https://monitoring.company.com/alerts       │
│                                                          │
│ Alert rule name: Alert-SQL-High-CPU                      │
│                                                          │
│ Severity: 2 - Warning                         ▼          │
│                                                          │
│                              [Create alert rule]         │
└─────────────────────────────────────────────────────────┘
```

**Alert 2: Deadlock Detection**
```
Condition: Deadlocks > 0
Signal: Deadlocks (from diagnostic logs)
Severity: 3 - Informational
Action: Email + Log to SIEM
```

**Alert 3: Failed Connections**
```
Condition: Failed connections > 10 per minute
Signal: Failed database authentication
Severity: 2 - Warning
Action: Email Security Team
```

**Alert 4: Storage Space**
```
Condition: Storage used percentage > 85%
Severity: 2 - Warning
Action: Email + Create ticket
```

**Action:** Click **Create alert rule**

---

### Step 35: Query Performance Insight

**Portal Navigation:**
```
Azure Portal → SQL databases → sqldb-customer-prod
→ Intelligent Performance → Query Performance Insight
```

**UI View:**
```
┌─────────────────────────────────────────────────────────┐
│ Query Performance Insight                                │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Top resource consuming queries (Last 24 hours)           │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Resource metric: ● CPU  ○ Duration  ○ Execution count││
│ ├─────────────────────────────────────────────────────┤ │
│ │ Query ID  │ CPU (%)  │ Duration  │ Executions       │ │
│ │──────────────────────────────────────────────────── │ │
│ │ 12345     │ 42.3%    │ 1.2s avg  │ 15,234           │ │
│ │ 67890     │ 28.7%    │ 0.8s avg  │ 23,445           │ │
│ │ 34567     │ 18.2%    │ 2.1s avg  │ 5,678            │ │
│ └─────────────────────────────────────────────────────┘ │
│                                                          │
│ [View query text]  [Download report]                     │
│                                                          │
│ Query Store status: ✓ Active                             │
│ Data retention: 30 days                                  │
│ Capture mode: Auto                                       │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

### Step 36: Enable Automatic Tuning

**Portal Navigation:**
```
Azure Portal → SQL databases → sqldb-customer-prod
→ Intelligent Performance → Automatic tuning
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Automatic tuning                                         │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Automatic tuning options:                                │
│                                                          │
│ ☑ CREATE INDEX                                           │
│   ⓘ Automatically creates indexes for slow queries       │
│   Status: ● ON                                           │
│                                                          │
│ ☑ DROP INDEX                                             │
│   ⓘ Removes unused indexes after 90 days                 │
│   Status: ● ON                                           │
│                                                          │
│ ☑ FORCE LAST GOOD PLAN                                   │
│   ⓘ Reverts to last good execution plan on regression    │
│   Status: ● ON                                           │
│                                                          │
│ ☑ Email notifications                                    │
│   Send to: dbateam@yourdomain.com                        │
│                                                          │
│ Tuning history:                                          │
│   View recommendations applied in last 30 days           │
│   [View history]                                         │
│                                                          │
│                                       [Save]  [Discard]  │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Save**

---

### Step 37: Create Azure Automation Account

**Portal Navigation:**
```
Azure Portal → Automation Accounts → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create an Automation Account                             │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Subscription: [Your subscription]                        │
│                                                          │
│ Resource group: rg-enterprise-sql-prod                   │
│                                                          │
│ Name: automation-sql-prod                                │
│                                                          │
│ Region: East US 2                             ▼          │
│                                                          │
│ Create Azure Run As account:                             │
│   ● Yes  (For authentication)                            │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Review + create** → **Create**

---

### Step 38: Create Maintenance Runbook

**Portal Navigation:**
```
Azure Portal → Automation Accounts → automation-sql-prod
→ Runbooks → + Create a runbook
```

**Sample Runbook: Index Maintenance**
```powershell
<#
.SYNOPSIS
    Daily SQL Database Index Maintenance
.DESCRIPTION
    Rebuilds fragmented indexes and updates statistics
#>

param(
    [Parameter(Mandatory=$true)]
    [string]$ServerName,
    
    [Parameter(Mandatory=$true)]
    [string]$DatabaseName
)

# Authenticate using Managed Identity
Connect-AzAccount -Identity

# Get SQL connection
$SqlConnection = New-Object System.Data.SqlClient.SqlConnection
$SqlConnection.ConnectionString = "Server=$ServerName.database.windows.net;Database=$DatabaseName;Authentication=Active Directory Managed Identity;"
$SqlConnection.Open()

# Rebuild fragmented indexes
$RebuildQuery = @"
DECLARE @TableName NVARCHAR(256)
DECLARE @IndexName NVARCHAR(256)
DECLARE @SQL NVARCHAR(MAX)

DECLARE index_cursor CURSOR FOR
SELECT 
    t.name AS TableName,
    i.name AS IndexName
FROM sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'SAMPLED') ps
INNER JOIN sys.tables t ON t.object_id = ps.object_id
INNER JOIN sys.indexes i ON i.object_id = ps.object_id AND i.index_id = ps.index_id
WHERE ps.avg_fragmentation_in_percent > 30
AND ps.page_count > 1000

OPEN index_cursor
FETCH NEXT FROM index_cursor INTO @TableName, @IndexName

WHILE @@FETCH_STATUS = 0
BEGIN
    SET @SQL = 'ALTER INDEX ' + @IndexName + ' ON ' + @TableName + ' REBUILD WITH (ONLINE = ON)'
    EXEC sp_executesql @SQL
    
    FETCH NEXT FROM index_cursor INTO @TableName, @IndexName
END

CLOSE index_cursor
DEALLOCATE index_cursor
"@

$RebuildCommand = $SqlConnection.CreateCommand()
$RebuildCommand.CommandText = $RebuildQuery
$RebuildCommand.ExecuteNonQuery()

# Update statistics
$StatsQuery = "EXEC sp_updatestats"
$StatsCommand = $SqlConnection.CreateCommand()
$StatsCommand.CommandText = $StatsQuery
$StatsCommand.ExecuteNonQuery()

$SqlConnection.Close()

Write-Output "Index maintenance completed successfully"
```

**Create Schedule:**
```
Portal → Runbooks → rb-index-maintenance → Schedules → + Add a schedule

Schedule:
  Name: Daily-Index-Maintenance
  Starts: Today at 02:00 AM
  Recurrence: Daily
  Time zone: (UTC-05:00) Eastern Time
  Expires: Never

Parameters:
  ServerName: sql-server-prod-eastus2-001
  DatabaseName: sqldb-customer-prod
```

---

## PHASE 13: COST OPTIMIZATION

### Step 39: Configure Reserved Capacity

**Portal Navigation:**
```
Azure Portal → Reservations → + Purchase reservation → Azure SQL Database
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Purchase reserved capacity                               │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Scope:                                                   │
│   ● Shared  (Apply to all subscriptions)                 │
│   ○ Single subscription                                  │
│   ○ Single resource group                                │
│                                                          │
│ Region: East US 2                             ▼          │
│                                                          │
│ Deployment option:                                       │
│   ● vCore                                                │
│   ○ DTU                                                  │
│                                                          │
│ Product:                                                 │
│   ● SQL Database Single/Elastic Pool                     │
│   ○ SQL Managed Instance                                 │
│                                                          │
│ Hardware: Gen5                                ▼          │
│                                                          │
│ Service tier: General Purpose                ▼          │
│                                                          │
│ vCores: 8                                                │
│                                                          │
│ Term:                                                    │
│   ● 3 years  (72% discount)                              │
│   ○ 1 year   (40% discount)                              │
│                                                          │
│ Billing frequency:                                       │
│   ● Upfront                                              │
│   ○ Monthly                                              │
│                                                          │
│ Cost comparison:                                         │
│   Pay-as-you-go: $4,200/month × 36 = $151,200           │
│   3-year reserved: $42,336 (72% savings)                 │
│                                                          │
│                              [Purchase]  [Cancel]        │
└─────────────────────────────────────────────────────────┘
```

**Action:** Click **Purchase**

---

### Step 40: Configure Elastic Pool

**Portal Navigation:**
```
Azure Portal → SQL elastic pools → + Create
```

**UI Configuration:**
```
┌─────────────────────────────────────────────────────────┐
│ Create SQL elastic pool                                  │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Elastic pool name: sqlpool-multitenant-prod              │
│                                                          │
│ Server: sql-server-prod-eastus2-001           ▼          │
│                                                          │
│ Compute + storage: [Configure elastic pool]              │
│   ┌───────────────────────────────────────────────────┐ │
│   │ Service tier: General Purpose               ▼     │ │
│   │ Compute tier: Provisioned                   ▼     │ │
│   │                                                   │ │
│   │ Pool vCores: 16                                   │ │
│   │ Pool storage: 500 GB                              │ │
│   │                                                   │ │
│   │ Per database settings:                            │ │
│   │   Min vCores: 0                                   │ │
│   │   Max vCores: 4                                   │ │
│   │                                                   │ │
│   │ ⓘ Databases share pool resources                  │ │
│   │ ⓘ Good for multi-tenant SaaS apps                 │ │
│   │                                                   │ │
│   │ Estimated databases: 20-30                        │ │
│   │ Cost: $1,200/month for entire pool                │ │
│   │ vs $2,400/month for individual databases          │ │
│   │                                                   │ │
│   │                         [Apply]  [Cancel]         │ │
│   └───────────────────────────────────────────────────┘ │
│                                                          │
│                              [Review + create]           │
└─────────────────────────────────────────────────────────┘
```

**Add Databases to Pool:**
```
Portal → SQL elastic pools → sqlpool-multitenant-prod
→ Settings → Configure → Add databases

Select databases:
  ☑ sqldb-tenant-001
  ☑ sqldb-tenant-002
  ☑ sqldb-tenant-003
  ...
  ☑ sqldb-tenant-025
```

**Action:** Click **Review + create** → **Create**

---

## PHASE 14: TESTING & VALIDATION

### Step 41: Test Private Endpoint Connectivity

**From Application VM (VM-APP-01):**

```bash
# Connect via Azure Bastion
Azure Portal → Virtual machines → vm-app-01 → Connect → Bastion

# Test DNS resolution
nslookup sql-server-prod-eastus2-001.database.windows.net

# Expected output:
Name:    sql-server-prod-eastus2-001.privatelink.database.windows.net
Address: 10.2.2.4  ◄── Private IP from PE subnet

# Test SQL connectivity using sqlcmd
sqlcmd -S sql-server-prod-eastus2-001.database.windows.net -d sqldb-customer-prod -U sqladmin -P [password]

# If successful:
1> SELECT @@VERSION;
2> GO

# Output shows SQL Server version
```

---

### Step 42: Test Failover Group

**Portal Navigation:**
```
Azure Portal → SQL servers → sql-server-prod-eastus2-001
→ Failover groups → fog-sql-prod-dr
```

**UI - Initiate Failover:**
```
┌─────────────────────────────────────────────────────────┐
│ Failover group: fog-sql-prod-dr                          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Current Primary Server:                                  │
│   sql-server-prod-eastus2-001 (East US 2)                │
│                                                          │
│ Secondary Server:                                        │
│   sql-server-dr-westus2-001 (West US 2)                  │
│                                                          │
│ Replication State: ✓ Healthy                             │
│ Replication Lag: < 5 seconds                             │
│                                                          │
│ [Failover]  [Force Failover]  [Test Failover]           │
│                                                          │
│ ⚠️ Test Failover (Recommended):                          │
│   • Creates isolated test environment                    │
│   • No impact to production                              │
│   • Validates DR readiness                               │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**Test Failover Steps:**
1. Click **Test Failover**
2. Wait 5-10 minutes
3. Verify connectivity to fog-sql-prod-dr.database.windows.net
4. Application should connect automatically to new primary
5. Click **Complete Test** when done

---

### Step 43: Validate Serverless Auto-Pause

**Monitor Serverless Database:**
```
Azure Portal → SQL databases → sqldb-reporting-serverless
→ Metrics

View chart:
  Metric: CPU percentage + Connection count
  Time range: Last 4 hours
  
Expected behavior:
  ┌───────────────────────────────────────────┐
  │ CPU %                                     │
  │  100├─┐                                   │
  │   50│ │                                   │
  │    0│ └────────(pause)──────────────     │
  │     │   Active  │  Paused  │  Active     │
  │     └────────────┴──────────┴────────────│
  │        0-15min   15-75min   75-90min      │
  └───────────────────────────────────────────┘

After 60 minutes of inactivity:
  Status: Paused
  Compute cost: $0.00
  Storage cost: Still billed
  
On next query:
  Resumes in ~30 seconds
  Compute cost: Resumes
```

---

## DEPLOYMENT CHECKLIST

### ✅ Pre-Deployment Checklist

```
☐ Resource naming convention finalized
☐ IP address space documented (no overlaps)
☐ Azure AD groups created for RBAC
☐ Key Vault created with access policies
☐ Service principals for automation
☐ Budget alerts configured
☐ Stakeholder approvals received
```

### ✅ Network Deployment Checklist

```
☐ Hub VNET created (10.0.0.0/16)
☐ Spoke VNETs created (10.1-3.0.0/16)
☐ VNET peerings established
☐ Azure Firewall deployed and configured
☐ Azure Bastion deployed
☐ Route tables created (especially for MI)
☐ NSGs configured
☐ DNS zones created
```

### ✅ Data Platform Checklist

```
☐ Azure SQL Database (Provisioned) deployed
☐ Azure SQL Database (Serverless) deployed
☐ SQL Managed Instance deployed (allow 4-6 hours)
☐ SQL Server on VM deployed
☐ All private endpoints created
☐ Public access disabled on all PaaS services
☐ Azure Synapse workspace deployed
☐ ADLS Gen2 created
```

### ✅ Security Checklist

```
☐ Azure AD authentication enabled
☐ Managed identities assigned
☐ TDE enabled (default for PaaS)
☐ Always Encrypted configured for sensitive columns
☐ Dynamic Data Masking rules applied
☐ Microsoft Defender for SQL enabled
☐ Auditing configured (Storage + Log Analytics)
☐ All passwords stored in Key Vault
```

### ✅ HA/DR Checklist

```
☐ Geo-replication configured
☐ Failover groups created
☐ Zone redundancy enabled (Business Critical)
☐ Backup retention configured (7-35 days + LTR)
☐ DR testing scheduled
```

### ✅ Monitoring Checklist

```
☐ Log Analytics workspace created
☐ Diagnostic settings configured for all resources
☐ Alert rules created
☐ Query Performance Insight enabled
☐ Automatic tuning enabled
☐ Dashboards created in Azure Monitor
```

### ✅ Cost Optimization Checklist

```
☐ Reserved capacity purchased (if applicable)
☐ Elastic pools configured for multi-tenant workloads
☐ Serverless used for dev/test and bursty workloads
☐ Cost alerts configured
☐ Rightsizing recommendations reviewed monthly
```

### ✅ Post-Deployment Validation

```
☐ Private endpoint DNS resolution tested
☐ Application connectivity verified
☐ Failover group tested
☐ Backup and restore tested
☐ Security audit performed
☐ Performance baseline established
☐ Runbooks documented
☐ Team training completed
```

---

## COMMON TROUBLESHOOTING

### Issue 1: SQL MI Deployment Fails

**Symptoms:**
```
Error: "The subnet is not valid for SQL Managed Instance"
```

**Resolution:**
```
1. Verify subnet is /24 or larger
2. Check subnet delegation:
   Portal → VNET → Subnets → snet-sqlmi-01
   → Delegate subnet to: Microsoft.Sql/managedInstances

3. Verify no restrictive NSG rules
4. Check route table has 0.0.0.0/0 → Internet
5. Ensure no forced tunneling to firewall
```

---

### Issue 2: Private Endpoint Not Resolving

**Symptoms:**
```
nslookup returns public IP instead of private IP
```

**Resolution:**
```
1. Check private DNS zone is linked to VNET:
   Portal → Private DNS zones → privatelink.database.windows.net
   → Virtual network links
   → Add link to all relevant VNETs

2. Verify A record exists in DNS zone:
   Should show sql-server-prod-eastus2-001 → 10.2.2.4

3. Check VM is in VNET with DNS zone link

4. Test DNS from VM:
   nslookup sql-server-prod-eastus2-001.database.windows.net
   
5. If still fails, restart VM network interface
```

---

### Issue 3: Serverless Database Cold Start Too Slow

**Symptoms:**
```
Application times out waiting for serverless DB to resume
```

**Resolution:**
```
1. Increase application connection timeout:
   Connection timeout=60 (instead of default 15)

2. Implement retry logic in application

3. Consider keeping database always active during business hours:
   Portal → SQL database → Compute + storage
   → Disable auto-pause during 8 AM - 6 PM

4. Or upgrade to provisioned compute if cold starts unacceptable
```

---

### Issue 4: High Costs on Serverless

**Symptoms:**
```
Serverless database never pauses, costs same as provisioned
```

**Resolution:**
```
1. Check auto-pause is enabled:
   Portal → SQL database → Compute + storage
   → Auto-pause delay: 60 minutes (or adjust)

2. Review connection pooling in application:
   May be holding connections open preventing pause

3. Check for scheduled jobs keeping database active

4. Monitor actual pause time:
   Portal → SQL database → Metrics
   → CPU percentage over 24 hours
   
5. If database active 24x7, switch to provisioned compute
```

---

### Issue 5: MI Backup Failing

**Symptoms:**
```
Alert: "Backup to Azure Storage failed"
```

**Resolution:**
```
1. Check route table has correct routes:
   0.0.0.0/0 → Internet (NOT Firewall)
   Storage service tag → Internet

2. Verify NSG not blocking outbound 443

3. Check MI subnet has proper delegation

4. Ensure no forced tunneling to on-premises

5. Verify storage account not blocked by firewall

6. Check MI managed identity has Storage Blob Data Contributor role
```

---

## NEXT STEPS

### Immediate Post-Deployment (Week 1)

```
1. Performance baseline establishment
2. Security audit and compliance validation
3. Backup and restore testing
4. Failover testing
5. User acceptance testing (UAT)
6. Documentation updates
7. Knowledge transfer sessions
```

### Short-term (Month 1-3)

```
1. Optimize query performance using Query Store insights
2. Review and apply automatic tuning recommendations
3. Fine-tune alert thresholds based on actual patterns
4. Implement additional automation runbooks
5. Cost optimization review
6. Establish regular maintenance windows
```

### Long-term (Ongoing)

```
1. Monthly cost reviews and rightsizing
2. Quarterly DR drills
3. Annual security audits
4. Performance trending analysis
5. Capacity planning
6. Technology updates and upgrades
```

---

## ADDITIONAL RESOURCES

### Official Documentation

```
• Azure SQL Database: https://learn.microsoft.com/azure/azure-sql/database/
• SQL Managed Instance: https://learn.microsoft.com/azure/azure-sql/managed-instance/
• Azure Synapse: https://learn.microsoft.com/azure/synapse-analytics/
• Private Link: https://learn.microsoft.com/azure/private-link/
```

### Training Resources

```
• Microsoft Learn: Azure SQL Database Administrator path
• Microsoft Learn: Azure Solutions Architect path
• Azure SQL Database Workshop (GitHub)
• Private Endpoint Connectivity Architecture (whitepaper)
```

### Support Contacts

```
• Azure Support: https://portal.azure.com → Support + troubleshooting
• Premier Support: [Your account manager]
• Community Forums: Microsoft Q&A
• GitHub Issues: Azure SQL samples repo
```

---

