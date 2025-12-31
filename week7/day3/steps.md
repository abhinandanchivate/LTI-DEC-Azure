# Azure Storage Accounts - Portal-Based Guided Labs

## Lab 1: Create Storage Account with Different Redundancy Models

### **Scenario**: Create storage accounts for a production application with different redundancy needs

---

## **Task 1.1: Create LRS Storage Account (Portal)**

### **Step 1**: Navigate to Storage Accounts

```
1. Open browser and go to https://portal.azure.com
2. Sign in with your Azure credentials
3. In the search bar at the top, type "Storage accounts"
4. Click on "Storage accounts" from the results
5. Click the "+ Create" button at the top
```

### **Step 2**: Configure Basics Tab

```
PROJECT DETAILS:
-----------------
Subscription: Select your subscription from dropdown
Resource group: 
  - Click "Create new"
  - Enter name: rg-storage-lab
  - Click "OK"

INSTANCE DETAILS:
-----------------
Storage account name: stlabprodlrs01
  (Note: Must be lowercase, 3-24 characters, globally unique)
  (If red X appears, name is taken - try: stlabprodlrs[YOURNAME])

Region: (US) East US

Performance: 
  ○ Standard (selected)
  ○ Premium

Redundancy: 
  Click dropdown → Select "Locally-redundant storage (LRS)"
  
  (You'll see: "Locally-redundant storage (LRS) - 3 copies in 1 datacenter")
```

**What the page looks like:**
```
┌─────────────────────────────────────────────────────┐
│ Create a storage account                            │
├─────────────────────────────────────────────────────┤
│ Basics | Advanced | Networking | Data protection |  │
│ ─────                                               │
│                                                     │
│ Subscription: Pay-As-You-Go                        │
│ Resource group: rg-storage-lab [Create new]        │
│                                                     │
│ Storage account name: stlabprodlrs01 ✓             │
│ Region: (US) East US                               │
│ Performance: ● Standard  ○ Premium                 │
│ Redundancy: [Locally-redundant storage (LRS)  ▼]   │
│                                                     │
│         [Review + create]    [Next: Advanced >]     │
└─────────────────────────────────────────────────────┘
```

### **Step 3**: Configure Advanced Tab

```
Click "Next: Advanced >" at the bottom

SECURITY:
---------
☑ Require secure transfer for REST API operations (checked)
☑ Enable infrastructure encryption (unchecked - optional)

Minimum TLS version: [Version 1.2  ▼]

BLOB STORAGE:
-------------
☑ Allow enabling public access on containers (UNCHECK this box)
  (Best practice for security)

☑ Enable storage account key access (checked)
☑ Default to Azure Active Directory authorization in the Azure portal (checked)

Access tier (default):
  ● Hot (selected for frequently accessed data)
  ○ Cool (for infrequently accessed data)

AZURE FILES:
------------
☐ Enable large file shares (unchecked)

DATA LAKE STORAGE GEN2:
-----------------------
☐ Enable hierarchical namespace (unchecked - we'll use this later)
```

### **Step 4**: Configure Networking Tab

```
Click "Next: Networking >"

NETWORK CONNECTIVITY:
---------------------
● Enable public access from all networks (selected for lab)
○ Enable public access from selected virtual networks and IP addresses
○ Disable public access and use private access

  (In production, choose "selected networks" or "private access")

NETWORK ROUTING:
----------------
Routing preference: ● Microsoft network routing (recommended)
```

### **Step 5**: Configure Data Protection Tab

```
Click "Next: Data protection >"

RECOVERY:
---------
☐ Enable point-in-time restore for containers (unchecked)
☐ Enable soft delete for blobs (unchecked for lab)
☐ Enable soft delete for containers (unchecked for lab)
☐ Enable soft delete for file shares (unchecked for lab)

TRACKING:
---------
☐ Enable versioning for blobs (unchecked)
☐ Enable blob change feed (unchecked)

ACCESS CONTROL:
---------------
☐ Enable version-level immutability support (unchecked)

  (Note: In production, enable soft delete for data protection)
```

### **Step 6**: Configure Encryption Tab

```
Click "Next: Encryption >"

ENCRYPTION TYPE:
----------------
● Microsoft-managed keys (MMK) (selected)
○ Customer-managed keys (CMK)

☐ Enable support for customer-managed keys (unchecked)

ENABLE INFRASTRUCTURE ENCRYPTION:
---------------------------------
☐ (unchecked - optional double encryption)
```

### **Step 7**: Add Tags (Optional)

```
Click "Next: Tags >"

Name                    Value
--------------------------
Environment             Production
Department              IT
CostCenter              12345

Click "+ Add" to add more tags
```

### **Step 8**: Review and Create

```
Click "Next: Review + create >"

Review all settings displayed:
  - Subscription
  - Resource group: rg-storage-lab
  - Storage account name: stlabprodlrs01
  - Location: East US
  - Performance: Standard
  - Replication: Locally-redundant storage (LRS)
  - Account kind: StorageV2 (general purpose v2)

Validation status: ✓ Validation passed

Click "Create" button at the bottom

Wait for deployment (usually 30-60 seconds)
```

### **Step 9**: Verify Deployment

```
You'll see deployment progress screen:

┌─────────────────────────────────────────┐
│ Deployment is in progress              │
├─────────────────────────────────────────┤
│ Deployment name: Microsoft.StorageA... │
│ Resource group: rg-storage-lab         │
│                                        │
│ ✓ Deployment details                  │
│   └─ ✓ Microsoft.Storage/storageAcco..│
│                                        │
│ Your deployment is complete            │
│                                        │
│ [Go to resource]  [Download template]  │
└─────────────────────────────────────────┘

Click "Go to resource" button
```

### **Step 10**: Explore Storage Account Overview

```
You're now in the storage account blade:

Left Navigation:
  - Overview (currently selected)
  - Activity log
  - Access Control (IAM)
  - Tags
  - Diagnose and solve problems
  ──────────────
  Data storage
    - Containers
    - File shares
    - Queues
    - Tables
  ──────────────
  Data management
    - Redundancy
    - Lifecycle management
    - Object replication
  ──────────────
  Security + networking
    - Networking
    - Access keys
    - Shared access signature

Overview Tab shows:
  - Status: Available ✓
  - Location: East US
  - Subscription: [Your subscription]
  - Resource group: rg-storage-lab
  - Replication: Locally-redundant storage (LRS)
  - Performance: Standard
  - Primary endpoints:
    • Blob: https://stlabprodlrs01.blob.core.windows.net
    • Table: https://stlabprodlrs01.table.core.windows.net
    • Queue: https://stlabprodlrs01.queue.core.windows.net
    • File: https://stlabprodlrs01.file.core.windows.net
```

---

## **Task 1.2: Create ZRS Storage Account**

### **Follow same steps as Task 1.1, but change these values:**

```
STEP 2 - Basics Tab:
  Storage account name: stlabprodzrs01
  Redundancy: [Zone-redundant storage (ZRS)  ▼]
    
  (You'll see description: "Zone-redundant storage (ZRS) - 3 copies across 3 availability zones")

STEP 8 - Review:
  Replication: Zone-redundant storage (ZRS)
  
Click "Create"
```

---

## **Task 1.3: Create GRS Storage Account**

### **Follow same steps, but change these values:**

```
STEP 2 - Basics Tab:
  Storage account name: stlabprodgrs01
  Redundancy: [Geo-redundant storage (GRS)  ▼]
    
  (Description: "Geo-redundant storage (GRS) - 6 copies across 2 regions")

Click "Create"
```

---

## **Task 1.4: Verify and Compare Redundancy Settings**

### **Step 1**: Navigate to LRS Storage Account

```
1. In Portal search bar, type "Storage accounts"
2. Click "Storage accounts"
3. Click on "stlabprodlrs01"
4. In left menu, under "Data management" section
5. Click "Redundancy"

You'll see:
┌─────────────────────────────────────────────────┐
│ Redundancy                                      │
├─────────────────────────────────────────────────┤
│ Redundancy: Locally-redundant storage (LRS)    │
│                                                 │
│ Primary region: East US                        │
│ [Change]                                       │
│                                                 │
│ ℹ Your data is replicated 3 times within a    │
│   single datacenter in the primary region     │
│                                                 │
│ Properties:                                     │
│   ○ Protects against rack-level failures      │
│   ○ 99.999999999% (11 nines) durability      │
│   ○ No secondary region                        │
└─────────────────────────────────────────────────┘
```

### **Step 2**: View GRS Storage Account

```
1. Go back to Storage accounts list
2. Click on "stlabprodgrs01"
3. Click "Redundancy" in left menu

You'll see:
┌─────────────────────────────────────────────────┐
│ Redundancy                                      │
├─────────────────────────────────────────────────┤
│ Redundancy: Geo-redundant storage (GRS)       │
│                                                 │
│ Primary region: East US                        │
│ Secondary region: West US                      │
│ [Change] [Prepare for failover]               │
│                                                 │
│ Last sync time: 12/30/2024, 2:45:30 PM        │
│                                                 │
│ Properties:                                     │
│   ○ Data replicated to secondary region       │
│   ○ 99.99999999999999% (16 nines) durability │
│   ○ Manual failover available                 │
│   ○ Secondary region read-only               │
└─────────────────────────────────────────────────┘
```

---

## Lab 2: Working with Blob Storage - Containers and Different Blob Types

## **Task 2.1: Create Containers**

### **Step 1**: Navigate to Containers

```
1. In Portal, go to Storage accounts
2. Click on "stlabprodlrs01"
3. In left menu under "Data storage" section
4. Click "Containers"
5. Click "+ Container" at the top
```

### **Step 2**: Create Documents Container

```
A panel slides in from the right:

┌─────────────────────────────────────────┐
│ New container                           │
├─────────────────────────────────────────┤
│ Name *                                  │
│ [documents                         ]    │
│                                         │
│ Public access level                     │
│ [Private (no anonymous access)    ▼]    │
│                                         │
│ ℹ Choose Private to prevent public    │
│   read access to blobs               │
│                                         │
│          [Create]      [Cancel]         │
└─────────────────────────────────────────┘

Name: documents
Public access level: Private (no anonymous access)

Click "Create"
```

### **Step 3**: Create Additional Containers

```
Create these containers following same steps:

Container Name          Public Access Level
─────────────────────   ──────────────────────
logs                    Private
vhds                    Private
media                   Private
backups                 Private
```

### **Step 4**: View All Containers

```
You should now see:

┌──────────────────────────────────────────────────────┐
│ stlabprodlrs01 | Containers                          │
├──────────────────────────────────────────────────────┤
│ [+ Container] [Refresh] [Change access level]        │
│                                                      │
│ □  Name        Last modified         Access level   │
│ ─────────────────────────────────────────────────── │
│ □  backups     12/30/2024 2:15 PM   Private        │
│ □  documents   12/30/2024 2:10 PM   Private        │
│ □  logs        12/30/2024 2:11 PM   Private        │
│ □  media       12/30/2024 2:14 PM   Private        │
│ □  vhds        12/30/2024 2:12 PM   Private        │
└──────────────────────────────────────────────────────┘
```

---

## **Task 2.2: Upload Block Blobs**

### **Step 1**: Create a Test File on Your Computer

```
On Windows:
  - Open Notepad
  - Type: "This is a sample document for Azure Blob Storage testing"
  - Save as: sample-document.txt on your Desktop

On Mac/Linux:
  - Open TextEdit/any editor
  - Type same content
  - Save as: sample-document.txt
```

### **Step 2**: Upload to Documents Container

```
1. In Portal, click on "documents" container
2. Click "Upload" button at the top
3. The Upload blob panel opens on the right:

┌─────────────────────────────────────────┐
│ Upload blob                             │
├─────────────────────────────────────────┤
│ Files *                                 │
│ [📁 Browse for files...]               │
│                                         │
│ Advanced ▼                              │
│   Blob type: [Block blob           ▼]  │
│   Block size: [Default              ▼]  │
│   Access tier: [Hot (inferred)     ▼]  │
│                                         │
│   Upload to folder                      │
│   [                                 ]   │
│                                         │
│   Authentication method                 │
│   ● Account key (selected)             │
│   ○ Azure AD User Account              │
│                                         │
│   Encryption scope                      │
│   [Use account default             ▼]  │
│                                         │
│          [Upload]      [Cancel]         │
└─────────────────────────────────────────┘

Click "📁 Browse for files..."
Navigate to Desktop
Select "sample-document.txt"
Click "Open"

In Upload blob panel:
  - Expand "Advanced" section
  - Blob type: Block blob (should be default)
  - Access tier: Hot (inferred)
  
Click "Upload" button
```

### **Step 3**: Upload Multiple Files at Once

```
1. Create 3 more text files on your desktop:
   - report-q1.txt
   - report-q2.txt
   - presentation.txt

2. Click "Upload" button again
3. Click "Browse for files"
4. Hold Ctrl (Windows) or Cmd (Mac)
5. Select all 3 files
6. Click "Open"
7. You'll see all 3 files listed:
   
   Selected files:
     report-q1.txt (1.2 KB)
     report-q2.txt (1.3 KB)
     presentation.txt (0.9 KB)

8. Click "Upload"
```

### **Step 4**: Upload to a Folder (Virtual Directory)

```
1. Click "Upload" button
2. Browse and select "sample-document.txt" again
3. Expand "Advanced" section
4. In "Upload to folder" field, type: 2025/january
5. Click "Upload"

This creates a virtual folder structure:
  documents/
    └── 2025/
        └── january/
            └── sample-document.txt
```

### **Step 5**: View and Verify Uploaded Blobs

```
Container view now shows:

┌──────────────────────────────────────────────────────────────┐
│ documents                                           □ ▼ ⟳ ... │
├──────────────────────────────────────────────────────────────┤
│ [Upload] [Delete] [Change tier] [Download] [⋮ More]           │
│                                                              │
│ Path: documents >                                           │
│                                                              │
│ □  Name                  Modified         Size      Access  │
│ ──────────────────────────────────────────────────────────  │
│ 📁 2025                 12/30/2024       -         -        │
│ 📄 presentation.txt     12/30/2024       0.9 KB    Hot      │
│ 📄 report-q1.txt        12/30/2024       1.2 KB    Hot      │
│ 📄 report-q2.txt        12/30/2024       1.3 KB    Hot      │
│ 📄 sample-document.txt  12/30/2024       1.1 KB    Hot      │
└──────────────────────────────────────────────────────────────┘
```

### **Step 6**: View Blob Properties

```
1. Click on "sample-document.txt" (click the name, not checkbox)
2. Overview blade opens:

┌─────────────────────────────────────────────────┐
│ sample-document.txt                             │
├─────────────────────────────────────────────────┤
│ [Download] [Delete] [Copy URL] [⋮ More]         │
│                                                 │
│ PROPERTIES                                      │
│ URL: https://stlabprodlrs01.blob.core...      │
│ Size: 1.1 KB                                   │
│ Content type: text/plain                       │
│ Last modified: 12/30/2024, 2:30:45 PM         │
│ ETag: "0x8DBE9C8D4F2A1B0"                     │
│ Blob type: Block blob                          │
│ Access tier: Hot (inferred)                    │
│ Lease state: Available                         │
│ Server encrypted: true                         │
│                                                 │
│ METADATA                                        │
│ No metadata found                              │
│ [+ Add metadata]                               │
└─────────────────────────────────────────────────┘
```

---

## **Task 2.3: Add Metadata to Blobs**

### **Step 1**: Add Metadata to a Blob

```
1. Click on "report-q1.txt" to open its properties
2. Scroll down to "METADATA" section
3. Click "+ Add metadata"
4. Enter:

   Key                Value
   ────────────────   ──────────────
   department         finance
   year               2025
   quarter            Q1
   author             John Smith

5. Click "Save" at the top
6. You'll see confirmation: "Successfully updated blob metadata"
```

### **Step 2**: Verify Metadata

```
Refresh the blade, you'll now see:

METADATA
────────
department: finance
year: 2025
quarter: Q1
author: John Smith

[+ Add metadata] [Save]
```

---

## **Task 2.4: Change Blob Access Tier**

### **Step 1**: Move Blob from Hot to Cool Tier

```
1. Go back to documents container (click "documents" in breadcrumb)
2. Check the box next to "report-q2.txt"
3. Click "Change tier" button at the top
4. Panel opens on right:

┌─────────────────────────────────────────┐
│ Change tier                             │
├─────────────────────────────────────────┤
│ Access tier *                           │
│ ● Hot                                   │
│ ○ Cool                                  │
│ ○ Archive                               │
│                                         │
│ ℹ Moving to Cool tier:                 │
│   • Lower storage cost                 │
│   • Higher access cost                 │
│   • 30-day minimum storage            │
│                                         │
│ Estimated monthly cost:                │
│   Storage: $0.01 (was $0.02)          │
│   Access: $0.10 per GB read           │
│                                         │
│          [Save]      [Cancel]           │
└─────────────────────────────────────────┘

Select: ○ Cool
Click "Save"
```

### **Step 2**: Move Blob to Archive Tier

```
1. Create a new text file: old-backup.txt
2. Upload it to documents container
3. Select old-backup.txt (checkbox)
4. Click "Change tier"
5. Select: ○ Archive
6. Read the warning:

   ⚠ Archiving a blob makes it offline
   
   To read archived blobs:
   • Must rehydrate to Hot or Cool tier first
   • Rehydration takes up to 15 hours
   • Cannot read during rehydration
   
   Minimum storage: 180 days
   
7. Click "Save"
8. Notice blob now shows "Archive" in Access tier column
   and status shows "Archived" with ❄ icon
```

### **Step 3**: Rehydrate from Archive

```
1. Select "old-backup.txt" (the archived blob)
2. Click "Change tier"
3. Select: ○ Hot
4. Panel shows rehydration options:

┌─────────────────────────────────────────┐
│ Change tier                             │
├─────────────────────────────────────────┤
│ Access tier: ● Hot                      │
│                                         │
│ Rehydrate priority *                    │
│ ○ Standard (up to 15 hours)           │
│ ● High (under 1 hour)                  │
│                                         │
│ ℹ Rehydration is required to access   │
│   archived data. High priority costs  │
│   more but completes faster.          │
│                                         │
│          [Save]      [Cancel]           │
└─────────────────────────────────────────┘

Select: ● High
Click "Save"

Blob will show "Rehydration pending" status
Wait 15-30 minutes (for High priority)
Refresh to see when complete
```

---

## Lab 3: Configure Lifecycle Management Policies

## **Task 3.1: Create Lifecycle Management Rule**

### **Step 1**: Navigate to Lifecycle Management

```
1. Go to Storage account "stlabprodlrs01"
2. In left menu, under "Data management"
3. Click "Lifecycle management"
4. You'll see:

┌──────────────────────────────────────────────────────┐
│ Lifecycle management                                 │
├──────────────────────────────────────────────────────┤
│ Automate data deletion and tiering with lifecycle   │
│ management policies.                                 │
│                                                      │
│ [+ Add a rule]                                      │
│                                                      │
│ No lifecycle management rules configured            │
│                                                      │
│ [Code view]  [List view] ●                         │
└──────────────────────────────────────────────────────┘

Click "+ Add a rule"
```

### **Step 2**: Configure Rule Details

```
Panel opens: "Add a rule"

┌─────────────────────────────────────────┐
│ Add a rule                              │
├─────────────────────────────────────────┤
│ Details  Base blobs  Snapshots          │
│ ───────                                 │
│                                         │
│ Rule name *                             │
│ [move-old-data-to-cool             ]    │
│                                         │
│ Rule scope                              │
│ ● Apply rule to all blobs in storage   │
│   account                               │
│ ○ Limit blobs with filters             │
│                                         │
│ Blob type                               │
│ ☑ Block blobs                          │
│ ☐ Append blobs                         │
│                                         │
│ Blob subtype (optional)                 │
│ ☐ Base blobs                           │
│ ☐ Snapshots                            │
│ ☐ Versions                             │
│                                         │
│          [Next]      [Cancel]           │
└─────────────────────────────────────────┘

Fill in:
  Rule name: move-old-data-to-cool
  Rule scope: ● Apply rule to all blobs
  Blob type: ☑ Block blobs (checked)
  
Click "Next"
```

### **Step 3**: Configure Base Blobs Actions

```
Now on "Base blobs" tab:

┌─────────────────────────────────────────────────────┐
│ Add a rule                                          │
├─────────────────────────────────────────────────────┤
│ Details  Base blobs  Snapshots                      │
│          ──────────                                 │
│                                                     │
│ IF                                                  │
│ Base blobs were                                     │
│ ● Last modified (selected)                         │
│ ○ Last accessed (requires access tracking)        │
│ ○ Created                                          │
│                                                     │
│ More than (days ago) *                             │
│ [30                                            ]    │
│                                                     │
│ THEN                                               │
│ ☑ Move to cool storage                            │
│ ☑ Move to archive storage                         │
│   More than (days ago): [90              ]         │
│ ☑ Delete the blob                                 │
│   More than (days ago): [365             ]         │
│                                                     │
│          [Next]      [Back]      [Cancel]          │
└─────────────────────────────────────────────────────┘

Configure:
  IF: ● Last modified
  More than: 30 days ago
  
  THEN:
  ☑ Move to cool storage (checked)
  ☑ Move to archive storage (checked)
    More than: 90 days
  ☑ Delete the blob (checked)
    More than: 365 days

Click "Next" (or "Add" if Snapshots tab is not needed)
```

### **Step 4**: Review and Add Rule

```
Review screen shows:

Rule: move-old-data-to-cool
─────────────────────────────
Scope: All block blobs
Conditions:
  • Last modified > 30 days → Move to cool
  • Last modified > 90 days → Move to archive
  • Last modified > 365 days → Delete

Click "Add"
```

### **Step 5**: Create Additional Rule for Logs (Filter-based)

```
1. Click "+ Add a rule" again
2. Details tab:
   
   Rule name: delete-old-logs
   Rule scope: ○ Limit blobs with filters (select this)
   
   When you select filters, additional fields appear:
   
   ┌────────────────────────────────────────┐
   │ Prefix match (optional)                │
   │ [logs/                            ]    │
   │                                        │
   │ Blob index match (optional)           │
   │ Tag name      Operator    Value       │
   │ [          ]  [==      ▼] [        ]  │
   │                                        │
   └────────────────────────────────────────┘
   
   Prefix match: logs/
   Blob type: ☑ Block blobs and ☑ Append blobs
   
   Click "Next"

3. Base blobs tab:
   
   IF: ● Last modified
   More than: 7 days ago
   
   THEN:
   ☑ Delete the blob only (check this)
   
   Click "Add"
```

### **Step 6**: View All Lifecycle Rules

```
Lifecycle management page now shows:

┌──────────────────────────────────────────────────────────┐
│ Lifecycle management                   [+ Add a rule]    │
├──────────────────────────────────────────────────────────┤
│ [List view] ●  [Code view]                              │
│                                                          │
│ □ Name                    Enabled  Last modified        │
│ ────────────────────────────────────────────────────── │
│ ☑ move-old-data-to-cool   Yes ✓   12/30/2024 3:15 PM │
│ ☑ delete-old-logs         Yes ✓   12/30/2024 3:18 PM │
│                                                          │
│ Actions available:                                      │
│ [Disable] [Enable] [Delete] [Edit]                     │
└──────────────────────────────────────────────────────────┘
```

### **Step 7**: View Code (JSON Policy)

```
Click "Code view" tab to see JSON:

{
  "rules": [
    {
      "enabled": true,
      "name": "move-old-data-to-cool",
      "type": "Lifecycle",
      "definition": {
        "actions": {
          "baseBlob": {
            "tierToCool": {
              "daysAfterModificationGreaterThan": 30
            },
            "tierToArchive": {
              "daysAfterModificationGreaterThan": 90
            },
            "delete": {
              "daysAfterModificationGreaterThan": 365
            }
          }
        },
        "filters": {
          "blobTypes": ["blockBlob"]
        }
      }
    }
  ]
}
```

---

## Lab 4: Configure ADLS Gen2 with Hierarchical Namespace

## **Task 4.1: Create ADLS Gen2 Storage Account**

### **Step 1**: Create New Storage Account

```
1. Portal → Storage accounts → + Create
2. Basics tab:
   
   Resource group: rg-storage-lab
   Storage account name: stlabdatalake01
   Region: East US
   Performance: ● Standard
   Redundancy: Locally-redundant storage (LRS)
   
3. Click "Next: Advanced >"
```

### **Step 2**: Enable Hierarchical Namespace

```
Advanced tab:

Scroll to DATA LAKE STORAGE GEN2 section:

┌─────────────────────────────────────────────────────┐
│ DATA LAKE STORAGE GEN2                              │
├─────────────────────────────────────────────────────┤
│ ☑ Enable hierarchical namespace                    │
│                                                     │
│ ℹ Enables Data Lake Storage Gen2 capabilities     │
│   including directory operations and ACLs.         │
│   This cannot be changed after creation.           │
│                                                     │
│   Benefits:                                        │
│   • Atomic directory operations                   │
│   • Efficient directory management                │
│   • POSIX-compliant ACLs                         │
│   • Optimized for big data analytics             │
└─────────────────────────────────────────────────────┘

☑ Check "Enable hierarchical namespace"

Click "Next: Networking >"
Click "Next: Data protection >"
Click "Next: Encryption >"
Click "Review + create"
Click "Create"

Wait for deployment (1-2 minutes)
Click "Go to resource"
```

### **Step 3**: Verify ADLS Gen2 is Enabled

```
In storage account overview, you'll see:

Properties section shows:
  Account kind: StorageV2 (general purpose v2)
  Hierarchical namespace: Enabled ✓
  
Left menu now shows:
  Data storage
    - Containers (now called "File systems")
    - File shares
    - Queues
    - Tables
```

---

## **Task 4.2: Create File System and Directory Structure**

### **Step 1**: Create File System (Container)

```
1. In left menu, click "Containers"
   (Note: With ADLS Gen2, these are called "File systems")
2. Click "+ File system" (or "+ Container")
3. Panel opens:

┌─────────────────────────────────────────┐
│ New file system                         │
├─────────────────────────────────────────┤
│ Name *                                  │
│ [rawdata                           ]    │
│                                         │
│ Public access level                     │
│ [Private (no anonymous access)     ▼]   │
│                                         │
│          [Create]      [Cancel]         │
└─────────────────────────────────────────┘

Name: rawdata
Public access level: Private

Click "Create"
```

### **Step 2**: Create Directory Structure

```
1. Click on "rawdata" file system to open it
2. You'll see Storage Browser interface with directory capabilities
3. Click "+ Add Directory" at the top
4. Panel opens:

┌─────────────────────────────────────────┐
│ Create directory                        │
├─────────────────────────────────────────┤
│ Directory name *                        │
│ [departments                       ]    │
│                                         │
│          [Create]      [Cancel]         │
└─────────────────────────────────────────┘

Directory name: departments
Click "Create"

5. You'll see 📁 departments appear in the list
```

### **Step 3**: Create Nested Directories

```
1. Double-click on "departments" folder to open it
2. Click "+ Add Directory"
3. Create: sales
4. Click "Create"

5. Double-click "sales" to open it
6. Click "+ Add Directory"
7. Create: 2025
8. Click "Create"

9. Double-click "2025"
10. Click "+ Add Directory"
11. Create: Q1

Final structure:
📁 departments
   └── 📁 sales
       └── 📁 2025
           └── 📁 Q1
```

### **Step 4**: Create Additional Department Folders

```
Navigate back to "departments" (click in breadcrumb):
  rawdata > departments

Create these directories:
1. finance
2. hr
3. engineering
4. marketing

Now you have:
📁 departments
   ├── 📁 sales
   │   └── 📁 2025
   │       └── 📁 Q1
   ├── 📁 finance
   ├── 📁 hr
   ├── 📁 engineering
   └── 📁 marketing
```

---

## **Task 4.3: Upload Files to Directories**

### **Step 1**: Create Sample Data File

```
On your computer:
1. Create new text file: sales-data.csv
2. Add content:
   
   date,product,amount,region
   2025-01-15,Product A,1000,East
   2025-01-16,Product B,1500,West
   2025-01-17,Product A,1200,North
   2025-01-18,Product C,800,South

3. Save file
```

### **Step 2**: Upload to Specific Directory

```
1. In Portal, navigate to: rawdata > departments > sales > 2025 > Q1
2. Click "Upload" button
3. Upload panel opens:

┌─────────────────────────────────────────┐
│ Upload files                            │
├─────────────────────────────────────────┤
│ Files *                                 │
│ [📁 Select files...]                   │
│                                         │
│ Current path:                          │
│ /departments/sales/2025/Q1             │
│                                         │
│ Overwrite if files already exist       │
│ ☐                                      │
│                                         │
│          [Upload]      [Cancel]         │
└─────────────────────────────────────────┘

Click "Select files"
Choose sales-data.csv
Click "Upload"

File appears: 📄 sales-data.csv
```

---

## **Task 4.4: Configure Access Control Lists (ACLs)**

### **Step 1**: Set ACLs on Directory

```
1. Navigate to: rawdata > departments > sales
2. Right-click on "sales" directory (or click ⋮ menu)
3. Select "Manage access"
4. "Access control" panel opens:

┌─────────────────────────────────────────────────────┐
│ Access control - sales                              │
├─────────────────────────────────────────────────────┤
│ [+ Add principal]  [Save]  [Discard]               │
│                                                     │
│ CURRENT ACCESS (Access ACL)                        │
│ ─────────────────────────────────────────────────  │
│ Name              Type    Access        Inherited  │
│ Owning user       User    Read,Write,Exec   No    │
│ Owning group      Group   Read,Exec         No    │
│ Other            Other   ---              No    │
│                                                     │
│ DEFAULT ACCESS (Default ACL)                       │
│ ─────────────────────────────────────────────────  │
│ No default ACLs configured                         │
│                                                     │
│ ℹ Access ACLs control access to this directory   │
│   Default ACLs are inherited by new children      │
└─────────────────────────────────────────────────────┘
```

### **Step 2**: Add User/Group Access

```
Click "+ Add principal"
Panel expands:

┌─────────────────────────────────────────┐
│ Add principal                           │
├─────────────────────────────────────────┤
│ Search for user, group, or service      │
│ principal                               │
│ [Search...                         🔍]  │
│                                         │
│ Results:                               │
│ (Type name or email to search)         │
│                                         │
└─────────────────────────────────────────┘

Search for a user (e.g., your email or another user)
Results appear:
  ● John Doe (john.doe@company.com)

Select the user
Panel updates:

┌─────────────────────────────────────────┐
│ Principal: John Doe                     │
├─────────────────────────────────────────┤
│ Access ACL                              │
│ ☑ Read                                 │
│ ☑ Write                                │
│ ☑ Execute                              │
│                                         │
│ ☑ Apply to this directory              │
│ ☐ Apply to children (recursive)       │
│                                         │
│ Default ACL                             │
│ ☑ Read                                 │
│ ☐ Write                                │
│ ☑ Execute                              │
│                                         │
│ ℹ Default ACLs are inherited by       │
│   new files and directories            │
│                                         │
│          [Save]      [Cancel]           │
└─────────────────────────────────────────┘

Configure permissions:
  Access ACL:
    ☑ Read
    ☑ Write  
    ☑ Execute
    ☑ Apply to this directory
  
  Default ACL (for new files):
    ☑ Read
    ☐ Write (unchecked - read-only for new files)
    ☑ Execute

Click "Save"
```

### **Step 3**: Set Group Permissions

```
Click "+ Add principal" again
Search for a security group
Example: "Sales-Team"

Configure:
  Access ACL:
    ☑ Read
    ☐ Write (unchecked)
    ☑ Execute
    ☑ Apply to children (recursive) - check this
    
  Default ACL:
    ☑ Read
    ☐ Write
    ☑ Execute

Click "Save"

Now the Access control panel shows:

CURRENT ACCESS (Access ACL)
─────────────────────────────────────────
Name          Type    Access           Inherited
Owning user   User    Read,Write,Exec  No
John Doe      User    Read,Write,Exec  No
Sales-Team    Group   Read,Exec        No (recursive)
Owning group  Group   Read,Exec        No
Other         Other   ---              No
```

---

## **Task 4.5: Configure RBAC (Role-Based Access Control)**

### **Step 1**: Navigate to IAM

```
1. Go back to storage account "stlabdatalake01" main page
2. In left menu, click "Access Control (IAM)"
3. You'll see:

┌──────────────────────────────────────────────────────┐
│ Access control (IAM)                                 │
├──────────────────────────────────────────────────────┤
│ [+ Add] ▼  [Check access]  [View my access]        │
│                                                      │
│ Tabs: [Role assignments] [Deny assignments] [Roles] │
│       ─────────────────                             │
│                                                      │
│ Current role assignments:                           │
│ (Shows current assignments)                         │
└──────────────────────────────────────────────────────┘

Click "+ Add" dropdown
Select "Add role assignment"
```

### **Step 2**: Select Role

```
"Add role assignment" page opens with 3 tabs:

┌─────────────────────────────────────────────────────┐
│ Add role assignment                                 │
├─────────────────────────────────────────────────────┤
│ [Role] [Members] [Review + assign]                 │
│  ────                                              │
│                                                     │
│ Search: [Storage Blob                         🔍]  │
│                                                     │
│ Job function roles  Privileged administrator roles │
│                                                     │
│ Results:                                           │
│ ○ Storage Blob Data Owner                         │
│   Full access including ACLs                      │
│                                                     │
│ ○ Storage Blob Data Contributor                   │
│   Read, write, delete blob data                   │
│                                                     │
│ ○ Storage Blob Data Reader                        │
│   Read blob data only                             │
│                                                     │
│ ○ Storage Blob Delegator                          │
│   Generate user delegation SAS tokens             │
│                                                     │
│          [Next]                                    │
└─────────────────────────────────────────────────────┘

Search for: Storage Blob Data Contributor
Select: ○ Storage Blob Data Contributor
Click "Next"
```

### **Step 3**: Assign to Members

```
Members tab:

┌─────────────────────────────────────────────────────┐
│ Add role assignment                                 │
├─────────────────────────────────────────────────────┤
│ [Role] [Members] [Review + assign]                 │
│         ───────                                    │
│                                                     │
│ Assign access to *                                 │
│ ● User, group, or service principal (selected)     │
│ ○ Managed identity                                │
│                                                     │
│ Members                                            │
│ [+ Select members]                                │
│                                                     │
│ Selected members:                                  │
│ (none selected)                                   │
│                                                     │
│          [Next]      [Previous]                    │
└─────────────────────────────────────────────────────┘

Click "+ Select members"
Right panel opens:

Search: [john.doe                              🔍]
Results show:
  ☐ John Doe (john.doe@company.com)
  
Check the box ☑
Click "Select" button at bottom

Now shows:
  Selected members:
  • John Doe (john.doe@company.com)

Click "Next"
```

### **Step 4**: Review and Assign

```
Review + assign tab shows summary:

Role: Storage Blob Data Contributor
Scope: stlabdatalake01 (Storage account)
Members: John Doe

Click "Review + assign" button

Wait for notification:
  ✓ Successfully added role assignment
```

### **Step 5**: Verify Role Assignment

```
1. Stay on "Access Control (IAM)" page
2. Click "Role assignments" tab
3. Filter by searching: "John Doe"
4. You'll see:

Name       Role                           Scope
─────────  ────────────────────────────  ──────────────
John Doe   Storage Blob Data Contributor Storage account
```

---

# Azure Virtual Machines - Portal-Based Guided Labs

## Lab 5: Create and Configure Azure Virtual Machines

## **Task 5.1: Create Basic Web Server VM**

### **Step 1**: Start VM Creation

```
1. Portal home → Click "Create a resource"
2. In "Popular Azure services", click "Virtual machine"
   OR search "Virtual machine" and click it
3. Click "Create" dropdown
4. Select "Azure virtual machine"
```

### **Step 2**: Configure Basics Tab - Project Details

```
┌──────────────────────────────────────────────────────────┐
│ Create a virtual machine                                 │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Disks] [Networking] [Management] [Monitoring]  │
│  ──────                        [Advanced] [Tags] [Review]│
│                                                          │
│ PROJECT DETAILS                                          │
│ ──────────────────                                      │
│ Subscription *                                          │
│ [Your Subscription                                  ▼]  │
│                                                          │
│ Resource group *                                        │
│ [rg-vm-lab                                         ▼]  │
│ [Create new]                                           │
└──────────────────────────────────────────────────────────┘

If resource group doesn't exist:
  Click "Create new"
  Enter: rg-vm-lab
  Click "OK"
```

### **Step 3**: Instance Details

```
INSTANCE DETAILS
────────────────
Virtual machine name *
[vm-webserver-01                                      ]

Region *
[(US) East US                                         ▼]

Availability options
[No infrastructure redundancy required                ▼]
  (Other options: Availability zone, Availability set, 
   Virtual machine scale set)

Availability zone
[Zones 1, 2, 3                                       ▼]
  (Only if "Availability zone" selected above)

Security type
[Standard                                            ▼]
  (Options: Standard, Trusted launch, Confidential)

Image *
[Ubuntu Server 22.04 LTS - x64 Gen2                  ▼]
  (Click "See all images" to browse marketplace)

VM architecture
● x64  ○ Arm64

Size *
[Standard_B2s - 2 vcpus, 4 GiB memory               ▼]
  (Click "See all sizes" to explore options)
```

### **Step 4**: Click "See all sizes" to Choose VM Size

```
When you click "See all sizes", panel opens:

┌──────────────────────────────────────────────────────────┐
│ Select a VM size                                  [Close] │
├──────────────────────────────────────────────────────────┤
│ Filter: [B-series                              ] 🔍      │
│                                                          │
│ Recommended sizes:                                      │
│                                                          │
│ VM Size          vCPUs  RAM    Temp storage  Price/month│
│ ──────────────────────────────────────────────────────  │
│ ○ Standard_B1s      1    1 GiB    4 GiB      ~$7.59    │
│ ● Standard_B2s      2    4 GiB    8 GiB      ~$30.37   │
│ ○ Standard_B2ms     2    8 GiB    16 GiB     ~$60.74   │
│ ○ Standard_B4ms     4    16 GiB   32 GiB     ~$121.47  │
│                                                          │
│ [Show all sizes] - Click to see D-series, E-series, etc│
│                                                          │
│              [Select]              [Cancel]             │
└──────────────────────────────────────────────────────────┘

Select: ● Standard_B2s (2 vCPU, 4 GB RAM)
Click "Select"
```

### **Step 5**: Administrator Account

```
ADMINISTRATOR ACCOUNT
─────────────────────
Authentication type
● SSH public key (recommended for Linux)
○ Password

Username *
[azureuser                                            ]

SSH public key source
● Generate new key pair (selected)
○ Use existing key stored in Azure
○ Use existing public key

Key pair name
[vm-webserver-01_key                                  ]
```

### **Step 6**: Inbound Port Rules

```
INBOUND PORT RULES
──────────────────
Public inbound ports *
● Allow selected ports
○ None

Select inbound ports *
☑ HTTP (80)
☑ HTTPS (443)
☑ SSH (22)

⚠ Allowing SSH from all IPs is a security risk.
  Use Network Security Groups for better control.
```

### **Step 7**: Configure Disks Tab

```
Click "Next: Disks >" at bottom

┌──────────────────────────────────────────────────────────┐
│ Create a virtual machine                                 │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Disks] [Networking] [Management] [Monitoring]  │
│          ──────                                          │
│                                                          │
│ DISK OPTIONS                                            │
│ ────────────                                            │
│ OS disk type *                                          │
│ [Premium SSD (locally-redundant storage)            ▼]  │
│   (Options: Standard HDD, Standard SSD, Premium SSD,    │
│    Premium SSD v2, Ultra Disk)                         │
│                                                          │
│ OS disk size                                            │
│ [Default size (30 GiB)                              ▼]  │
│                                                          │
│ ☑ Delete with VM                                       │
│ ☐ Enable Ultra Disk compatibility                     │
│                                                          │
│ ENCRYPTION                                              │
│ ───────────                                            │
│ Encryption at host                                      │
│ ☐ (Requires subscription feature)                     │
│                                                          │
│ Key management                                          │
│ ● Platform-managed key (selected)                     │
│ ○ Customer-managed key                                │
└──────────────────────────────────────────────────────────┘

Leave defaults:
  OS disk type: Premium SSD
  OS disk size: Default (30 GiB)
  ☑ Delete with VM
```

### **Step 8**: Add Data Disk

```
Scroll down to DATA DISKS section:

┌──────────────────────────────────────────────────────────┐
│ DATA DISKS                                              │
│ ──────────                                              │
│ [Create and attach a new disk]  [Attach an existing disk]│
│                                                          │
│ LUN  Name          Size    Host caching  Encryption     │
│ ───────────────────────────────────────────────────────  │
│ (No data disks attached)                                │
│                                                          │
└──────────────────────────────────────────────────────────┘

Click "Create and attach a new disk"
Panel opens on right:

┌─────────────────────────────────────────┐
│ Create a new disk                       │
├─────────────────────────────────────────┤
│ Disk name                               │
│ [vm-webserver-01_DataDisk_0        ]    │
│                                         │
│ Source type                             │
│ [None (empty disk)                 ▼]   │
│                                         │
│ Size                                    │
│ [Premium SSD                       ▼]   │
│ [128 GiB                           ▼]   │
│                                         │
│ ⓘ Premium SSD: $19.71/month            │
│                                         │
│ Encryption                              │
│ ● Platform-managed key                 │
│ ○ Customer-managed key                 │
│                                         │
│ ☑ Delete disk with VM                  │
│                                         │
│          [OK]           [Cancel]        │
└─────────────────────────────────────────┘

Configure:
  Disk name: vm-webserver-01_DataDisk_0
  Source type: None (empty disk)
  Size: Premium SSD, 128 GiB
  ☑ Delete disk with VM

Click "OK"

Data disk now appears in list:
LUN  Name                        Size     Encryption
0    vm-webserver-01_DataDisk_0  128 GiB  SSE with PMK
```

### **Step 9**: Configure Networking Tab

```
Click "Next: Networking >"

┌──────────────────────────────────────────────────────────┐
│ NETWORK INTERFACE                                        │
│ ─────────────────────                                   │
│ Virtual network *                                       │
│ [(new) vnet-webapp                                  ▼]  │
│ [Create new]                                            │
│                                                          │
│ Subnet *                                                │
│ [(new) subnet-web (10.0.1.0/24)                    ▼]  │
│ [Manage subnet configuration]                           │
│                                                          │
│ Public IP *                                             │
│ [(new) vm-webserver-01-ip                          ▼]  │
│ [Create new]                                            │
│                                                          │
│ NIC network security group                              │
│ ○ None                                                 │
│ ● Basic (selected)                                     │
│ ○ Advanced                                             │
│                                                          │
│ Public inbound ports *                                  │
│ ● Allow selected ports                                 │
│                                                          │
│ Select inbound ports                                    │
│ [22 (SSH), 80 (HTTP), 443 (HTTPS)                 ▼]  │
│                                                          │
│ ☑ Delete NIC when VM is deleted                        │
│ ☑ Delete public IP when VM is deleted                  │
└──────────────────────────────────────────────────────────┘

Accept defaults or customize VNet:
  Virtual network: (new) vnet-webapp
  Subnet: (new) subnet-web
  Public IP: (new) vm-webserver-01-ip
  NSG: Basic
  Ports: 22, 80, 443
```

### **Step 10**: Optional - Create Custom VNet

```
If you want custom address space:

Click "Create new" under Virtual network
Panel opens:

┌─────────────────────────────────────────┐
│ Create virtual network                  │
├─────────────────────────────────────────┤
│ Name *                                  │
│ [vnet-webapp                       ]    │
│                                         │
│ ADDRESS SPACE                           │
│ IPv4 address space *                    │
│ [10.0.0.0/16                       ]    │
│ (Provides 65,536 addresses)            │
│                                         │
│ SUBNETS                                 │
│ Subnet name      Address range         │
│ ─────────────────────────────────────  │
│ default          10.0.0.0/24           │
│                  [✏ Edit] [🗑 Delete]   │
│                                         │
│ [+ Add subnet]                         │
│                                         │
│          [OK]           [Cancel]        │
└─────────────────────────────────────────┘

Modify:
  Name: vnet-webapp
  Address space: 10.0.0.0/16
  
  Click ✏ Edit on default subnet:
    Subnet name: subnet-web
    Address range: 10.0.1.0/24
    
Click "OK"
```

### **Step 11**: Configure Management Tab

```
Click "Next: Management >"

┌──────────────────────────────────────────────────────────┐
│ AZURE SECURITY CENTER                                   │
│ ─────────────────────────                              │
│ Microsoft Defender for Cloud                            │
│ ☐ Enable basic plan (free)                            │
│                                                          │
│ IDENTITY                                                │
│ ────────                                                │
│ System assigned managed identity                        │
│ ☐ Enable system assigned managed identity             │
│   (Allows VM to authenticate to Azure services)        │
│                                                          │
│ Azure AD                                                │
│ ☐ Login with Azure AD (Preview)                       │
│                                                          │
│ AUTO-SHUTDOWN                                           │
│ ─────────────                                          │
│ ☑ Enable auto-shutdown                                │
│                                                          │
│ Shutdown time *                                         │
│ [7:00 PM                                           ]    │
│                                                          │
│ Time zone                                               │
│ [(UTC-08:00) Pacific Time (US & Canada)           ▼]  │
│                                                          │
│ Notification settings                                   │
│ Send notification before auto-shutdown                  │
│ ☑ Enabled                                              │
│                                                          │
│ Email address                                           │
│ [your-email@company.com                             ]   │
│                                                          │
│ Webhook URL (optional)                                  │
│ [                                                   ]   │
└──────────────────────────────────────────────────────────┘

Configure:
  ☑ Enable auto-shutdown (for cost savings in lab)
  Shutdown time: 7:00 PM
  Time zone: Your timezone
  ☑ Send notification
  Email: your-email@company.com
```

### **Step 12**: Configure Monitoring Tab

```
Click "Next: Monitoring >"

┌──────────────────────────────────────────────────────────┐
│ BOOT DIAGNOSTICS                                        │
│ ─────────────────────                                  │
│ ☑ Enable boot diagnostics                             │
│   (Recommended for troubleshooting)                     │
│                                                          │
│ Diagnostics storage account                             │
│ ● Managed storage account (recommended)                │
│ ○ Use custom storage account                           │
│                                                          │
│ OS GUEST DIAGNOSTICS                                    │
│ ─────────────────────────                              │
│ ☐ Enable OS guest diagnostics                         │
│   (Collects performance metrics and logs)               │
└──────────────────────────────────────────────────────────┘

Leave defaults:
  ☑ Enable boot diagnostics (checked)
  ● Managed storage account
```

### **Step 13**: Review Advanced and Tags

```
Click "Next: Advanced >" (Optional)
  - Extensions: Can install software after deployment
  - Cloud init: Custom scripts for Linux
  - Host: Dedicated host options
  
Click "Next: Tags >" (Optional)

Add tags:
  Name          Value
  ────────────  ─────────────
  Environment   Development
  Owner         YourName
  Purpose       WebServer
```

### **Step 14**: Review + Create

```
Click "Next: Review + create >"

Summary page shows all configuration:
┌──────────────────────────────────────────────────────────┐
│ Validation passed ✓                                     │
├──────────────────────────────────────────────────────────┤
│ BASICS                                                   │
│ Subscription: Pay-As-You-Go                             │
│ Resource group: rg-vm-lab                               │
│ VM name: vm-webserver-01                                │
│ Region: East US                                          │
│ Image: Ubuntu Server 22.04 LTS                          │
│ Size: Standard_B2s (2 vCPUs, 4 GiB RAM)                │
│                                                          │
│ DISKS                                                    │
│ OS disk: Premium SSD 30 GiB                             │
│ Data disks: 1 x 128 GiB Premium SSD                     │
│                                                          │
│ NETWORKING                                               │
│ Virtual network: vnet-webapp                            │
│ Public IP: vm-webserver-01-ip                           │
│ Ports: 22, 80, 443                                      │
│                                                          │
│ Estimated cost: $32.00/month                            │
│ [Terms of use]                                          │
│                                                          │
│  [< Previous]  [Download template]  [Create]           │
└──────────────────────────────────────────────────────────┘

Review all settings carefully
Click "Create"
```

### **Step 15**: Download SSH Key

```
Popup appears:
┌─────────────────────────────────────────┐
│ Generate new key pair                   │
├─────────────────────────────────────────┤
│ Key pair name                           │
│ vm-webserver-01_key                     │
│                                         │
│ ⚠ Private key will be downloaded to   │
│   your computer. Store it securely!    │
│                                         │
│   File: vm-webserver-01_key.pem        │
│                                         │
│ [Download private key and create resource]│
│                     [Cancel]            │
└─────────────────────────────────────────┘

Click "Download private key and create resource"
Save vm-webserver-01_key.pem to safe location
```

### **Step 16**: Monitor Deployment

```
Deployment screen appears:
┌──────────────────────────────────────────────────────────┐
│ Deployment is in progress                                │
├──────────────────────────────────────────────────────────┤
│ Deployment name: Microsoft.Template-202...               │
│ Resource group: rg-vm-lab                                │
│ Start time: 12/30/2024, 3:45:32 PM                      │
│                                                          │
│ Deployment details:                                      │
│ ✓ Microsoft.Network/networkSecurityGroups               │
│ ✓ Microsoft.Network/publicIPAddresses                   │
│ ✓ Microsoft.Network/virtualNetworks                     │
│ ⟳ Microsoft.Compute/disks                               │
│ ⟳ Microsoft.Network/networkInterfaces                   │
│ ⏳ Microsoft.Compute/virtualMachines                    │
│                                                          │
│ Estimated time remaining: 2-3 minutes                   │
└──────────────────────────────────────────────────────────┘

Wait 3-5 minutes for deployment to complete

When done:
┌──────────────────────────────────────────────────────────┐
│ ✓ Your deployment is complete                           │
├──────────────────────────────────────────────────────────┤
│ Deployment name: Microsoft.Template-202...               │
│ Completion time: 12/30/2024, 3:49:15 PM                │
│ Duration: 3 minutes 43 seconds                          │
│                                                          │
│ Deployed resources: 7                                    │
│                                                          │
│ [Go to resource]  [Outputs]  [Download template]       │
└──────────────────────────────────────────────────────────┘

Click "Go to resource"
```

---

## **Task 5.2: Connect to VM and Install Web Server**

### **Step 1**: Get VM Public IP

```
In VM overview page you'll see:

┌──────────────────────────────────────────────────────────┐
│ vm-webserver-01                                          │
├──────────────────────────────────────────────────────────┤
│ [Start] [Restart] [Stop] [Connect ▼] [⋮ More]          │
│                                                          │
│ Status: ● Running                                       │
│ Location: East US                                        │
│ Subscription: Pay-As-You-Go                             │
│ Resource group: rg-vm-lab                               │
│ Public IP address: 52.168.45.20                         │
│ Private IP address: 10.0.1.4                            │
│ DNS name: Not configured                                │
│ Computer name: vm-webserver-01                          │
│ Operating system: Linux (Ubuntu 22.04)                  │
│ Size: Standard_B2s (2 vcpus, 4 GiB memory)             │
└──────────────────────────────────────────────────────────┘

Note the Public IP: 52.168.45.20
```

### **Step 2**: Connect Using Azure Cloud Shell (Easiest Method)

```
1. In VM page, click "Connect" dropdown at top
2. Select "Connect"
3. Panel opens on right:

┌─────────────────────────────────────────┐
│ Connect                                 │
├─────────────────────────────────────────┤
│ ○ Native SSH                           │
│ ● SSH using Azure CLI (selected)      │
│ ○ Bastion                              │
│ ○ RDP                                  │
│                                         │
│ [SSH using Azure CLI]                  │
│ ────────────────────                   │
│ Prerequisites:                         │
│ ✓ Azure CLI installed                 │
│ ✓ SSH key generated                   │
│                                         │
│ Command:                               │
│ az ssh vm --resource-group \          │
│   rg-vm-lab --name vm-webserver-01    │
│                                         │
│ [Copy] [Open Cloud Shell]              │
└─────────────────────────────────────────┘

Click "Open Cloud Shell" at bottom of portal
Cloud Shell opens as panel at bottom

In Cloud Shell, type:
```

```bash
az ssh vm --resource-group rg-vm-lab --name vm-webserver-01
```

```
You'll see:
The authenticity of host can't be established.
Are you sure you want to continue connecting (yes/no)? 

Type: yes

Connected to vm-webserver-01!

azureuser@vm-webserver-01:~$ 
```

### **Step 3**: Install NGINX Web Server

```
In the SSH session:

# Update package list
azureuser@vm-webserver-01:~$ sudo apt update

Output shows:
Hit:1 http://azure.archive.ubuntu.com/ubuntu jammy InRelease
Get:2 http://azure.archive.ubuntu.com/ubuntu jammy-updates InRelease
...
Reading package lists... Done

# Install NGINX
azureuser@vm-webserver-01:~$ sudo apt install nginx -y

Output shows:
Reading package lists... Done
Building dependency tree... Done
...
Setting up nginx (1.18.0-6ubuntu14.3) ...
Created symlink ...

# Start and enable NGINX
azureuser@vm-webserver-01:~$ sudo systemctl start nginx
azureuser@vm-webserver-01:~$ sudo systemctl enable nginx

# Check status
azureuser@vm-webserver-01:~$ sudo systemctl status nginx

Output shows:
● nginx.service - A high performance web server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled)
     Active: ● active (running) since Mon 2024-12-30 20:50:12 UTC
       Docs: man:nginx(8)
   Main PID: 1234 (nginx)
      Tasks: 3 (limit: 4653)
     Memory: 6.2M
        CPU: 45ms
     CGroup: /system.slice/nginx.service
             ├─1234 "nginx: master process"
             ├─1235 "nginx: worker process"
             └─1236 "nginx: worker process"

Press 'q' to quit
```

### **Step 4**: Create Custom Web Page

```
azureuser@vm-webserver-01:~$ sudo nano /var/www/html/index.html

Delete existing content and paste:

<!DOCTYPE html>
<html>
<head>
    <title>Azure VM Lab</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 50px;
            background-color: #f0f0f0;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 { color: #0078D4; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎉 Welcome to Azure VM!</h1>
        <h2>VM: vm-webserver-01</h2>
        <p><strong>Status:</strong> Running Successfully</p>
        <p><strong>OS:</strong> Ubuntu 22.04 LTS</p>
        <p><strong>Web Server:</strong> NGINX</p>
        <p><strong>Lab:</strong> Azure Portal Guided Lab</p>
    </div>
</body>
</html>

Save: Ctrl+O, Enter
Exit: Ctrl+X

# Test locally
azureuser@vm-webserver-01:~$ curl http://localhost
(Should show your HTML)
```

### **Step 5**: Test from Browser

```
1. Open your web browser
2. Navigate to: http://52.168.45.20
   (Replace with your actual public IP)
3. You should see your custom web page!

If it doesn't work, check NSG rules in next step.
```

---

## **Task 5.3: View and Modify Network Security Group Rules**

### **Step 1**: Navigate to NSG

```
1. Go to VM "vm-webserver-01"
2. In left menu, under "Settings"
3. Click "Networking"
4. You'll see:

┌──────────────────────────────────────────────────────────┐
│ Networking                                               │
├──────────────────────────────────────────────────────────┤
│ Network interface: vm-webserver-01VMNic                 │
│ Network security group: vm-webserver-01-nsg             │
│                                                          │
│ INBOUND PORT RULES                                       │
│ ──────────────────                                      │
│ Port   Protocol Source        Destination   Action      │
│ ───────────────────────────────────────────────────────  │
│ 22     TCP      Any           Any            Allow       │
│ 80     TCP      Any           Any            Allow       │
│ 443    TCP      Any           Any            Allow       │
│ *      *        *             *              Deny        │
│                                                          │
│ [+ Add inbound port rule]                               │
│                                                          │
│ OUTBOUND PORT RULES                                      │
│ ───────────────────                                     │
│ Port   Protocol Source        Destination   Action      │
│ *      *        *             *              Allow       │
└──────────────────────────────────────────────────────────┘

Click on NSG name: "vm-webserver-01-nsg"
```

### **Step 2**: View All NSG Rules

```
NSG overview shows:

┌──────────────────────────────────────────────────────────┐
│ vm-webserver-01-nsg                                      │
├──────────────────────────────────────────────────────────┤
│ [+ Add] [Refresh] [Delete]                              │
│                                                          │
│ Left menu:                                              │
│   - Overview                                            │
│   - Inbound security rules ← Click this                │
│   - Outbound security rules                             │
│   - Network interfaces                                  │
│   - Subnets                                             │
└──────────────────────────────────────────────────────────┘

Click "Inbound security rules"
```

### **Step 3**: View Detailed Rules

```
┌──────────────────────────────────────────────────────────────────────────┐
│ Inbound security rules                          [+ Add]  [Refresh]      │
├──────────────────────────────────────────────────────────────────────────┤
│ Priority  Name       Port  Protocol  Source       Destination  Action   │
│ ───────────────────────────────────────────────────────────────────────  │
│ 300       SSH        22    TCP       Any          Any           Allow   │
│ 310       HTTPS      443   TCP       Any          Any           Allow   │
│ 320       HTTP       80    TCP       Any          Any           Allow   │
│ 65000     AllowVnet  Any   Any       VirtualNet   VirtualNet    Allow   │
│ 65001     AllowLB    Any   Any       AzureLB      Any           Allow   │
│ 65500     DenyAll    Any   Any       Any          Any           Deny    │
└──────────────────────────────────────────────────────────────────────────┘
```

### **Step 4**: Add Custom Inbound Rule

```
Click "+ Add" at top

Panel opens on right:

┌─────────────────────────────────────────────────────────┐
│ Add inbound security rule                               │
├─────────────────────────────────────────────────────────┤
│ Source *                                                │
│ [Any                                                ▼]  │
│   (Options: Any, IP Addresses, Service Tag, ASG)       │
│                                                         │
│ Source port ranges *                                    │
│ [*                                                  ]   │
│                                                         │
│ Destination *                                           │
│ [Any                                                ▼]  │
│                                                         │
│ Service                                                 │
│ [Custom                                             ▼]  │
│   (Select common services or choose Custom)            │
│                                                         │
│ Destination port ranges *                               │
│ [8080                                               ]   │
│                                                         │
│ Protocol *                                              │
│ ● Any  ○ TCP  ○ UDP  ○ ICMP                          │
│                                                         │
│ Action *                                                │
│ ● Allow  ○ Deny                                        │
│                                                         │
│ Priority *                                              │
│ [330                                                ]   │
│   (100-4096, lower = higher priority)                  │
│                                                         │
│ Name *                                                  │
│ [Port_8080                                          ]   │
│                                                         │
│ Description (optional)                                  │
│ [Allow application on port 8080                     ]   │
│                                                         │
│          [Add]                    [Cancel]              │
└─────────────────────────────────────────────────────────┘

Configure:
  Source: Any
  Source port ranges: *
  Destination: Any
  Service: Custom
  Destination port ranges: 8080
  Protocol: ● TCP
  Action: ● Allow
  Priority: 330
  Name: Port_8080
  Description: Allow application on port 8080

Click "Add"

Rule appears in list within 10-15 seconds
```

---


## Lab 6: VM Lifecycle Operations

## **Task 6.1: Stop, Deallocate, and Start VM**

### **Step 1**: Stop (Deallocate) VM

```
1. Go to Virtual machines
2. Click on "vm-webserver-01"
3. At the top menu bar:

┌──────────────────────────────────────────────────────────┐
│ vm-webserver-01                                          │
├──────────────────────────────────────────────────────────┤
│ [Connect ▼] [Start] [Restart] [Stop] [Delete] [⋮ More]  │
│                                                          │
│ Current Status: ● Running                               │
└──────────────────────────────────────────────────────────┘

Click "Stop" button

Confirmation dialog appears:
┌─────────────────────────────────────────┐
│ Stop virtual machine                    │
├─────────────────────────────────────────┤
│ Are you sure you want to stop the      │
│ following virtual machines?             │
│                                         │
│ • vm-webserver-01                       │
│                                         │
│ ℹ Stopping will deallocate the VM and │
│   release the compute resources. You   │
│   won't be charged for compute but     │
│   will still be charged for storage.   │
│                                         │
│          [Yes]         [No]             │
└─────────────────────────────────────────┘

Click "Yes"

Notification appears (bell icon top-right):
  ⟳ Stopping virtual machine 'vm-webserver-01'...
  
Wait 1-2 minutes

  ✓ Successfully stopped virtual machine 'vm-webserver-01'

VM Overview now shows:
  Status: ● Stopped (deallocated)
```

### **Step 2**: Start VM

```
In VM overview, the buttons change:

┌──────────────────────────────────────────────────────────┐
│ vm-webserver-01                                          │
├──────────────────────────────────────────────────────────┤
│ [Connect ▼] [Start] [Delete] [⋮ More]                   │
│ (Restart and Stop are grayed out)                       │
│                                                          │
│ Current Status: ● Stopped (deallocated)                 │
└──────────────────────────────────────────────────────────┘

Click "Start" button

Notification:
  ⟳ Starting virtual machine 'vm-webserver-01'...
  
Wait 2-3 minutes

  ✓ Successfully started virtual machine 'vm-webserver-01'

Status changes to: ● Running
```

---

## **Task 6.2: Resize VM**

### **Step 1**: Navigate to Size Settings

```
1. Ensure VM is Stopped (deallocated)
   (If running, click Stop and wait for deallocation)

2. In VM left menu, under "Settings"
3. Click "Size"

You'll see current configuration:

┌──────────────────────────────────────────────────────────┐
│ Size                                                     │
├──────────────────────────────────────────────────────────┤
│ CURRENT SIZE                                            │
│ ─────────────                                           │
│ Current: Standard_B2s                                   │
│   vCPUs: 2                                              │
│   RAM: 4 GiB                                            │
│   Data disks: 4                                         │
│   Max IOPS: 1,920                                       │
│   Temp storage: 8 GiB                                   │
│   Cost: ~$30.37/month                                   │
│                                                          │
│ [Change size]                                           │
│                                                          │
│ ℹ VM must be stopped to resize                         │
└──────────────────────────────────────────────────────────┘

Click "Change size" button
```

### **Step 2**: Select New Size

```
"Select a VM size" page loads:

┌──────────────────────────────────────────────────────────────────────────┐
│ Select a VM size                                    [Refresh] [Columns] │
├──────────────────────────────────────────────────────────────────────────┤
│ Filter by VM family: [All] [B-series ▼] [D-series ▼] [E-series ▼]     │
│ Search: [                                                          ] 🔍  │
│                                                                          │
│ ○ VM Size          vCPUs  RAM     Temp    Data    Max     Price/month │
│                                   storage disks  IOPS                   │
│ ──────────────────────────────────────────────────────────────────────  │
│ ○ Standard_B1s       1    1 GiB   4 GiB     2    320     ~$7.59      │
│ ● Standard_B2s       2    4 GiB   8 GiB     4    1,920   ~$30.37     │
│   (Current size)                                                       │
│ ○ Standard_B2ms      2    8 GiB   16 GiB    4    1,920   ~$60.74     │
│ ○ Standard_B4ms      4    16 GiB  32 GiB    8    2,880   ~$121.47    │
│ ○ Standard_B8ms      8    32 GiB  64 GiB    16   4,320   ~$242.95    │
│                                                                          │
│ [Show all sizes] to see D-series, E-series, etc.                       │
│                                                                          │
│ Selected: Standard_B4ms                                                │
│   vCPUs: 4, RAM: 16 GiB, Temp: 32 GiB                                │
│   Cost increase: +$91.10/month                                         │
│                                                                          │
│          [Resize]                    [Cancel]                          │
└──────────────────────────────────────────────────────────────────────────┘

Select: ○ Standard_B4ms (4 vCPU, 16 GB RAM)

Click "Resize" button at bottom
```

### **Step 3**: Monitor Resize Operation

```
Notification appears:
  ⟳ Resizing virtual machine 'vm-webserver-01'...

Page shows progress:
┌──────────────────────────────────────────────────────────┐
│ Size                                                     │
├──────────────────────────────────────────────────────────┤
│ ⟳ Resize in progress...                                 │
│                                                          │
│ From: Standard_B2s (2 vCPUs, 4 GiB)                    │
│ To:   Standard_B4ms (4 vCPUs, 16 GiB)                  │
│                                                          │
│ Estimated time: 2-3 minutes                            │
└──────────────────────────────────────────────────────────┘

Wait for completion (2-3 minutes)

  ✓ Successfully resized virtual machine 'vm-webserver-01'

New size displayed:
┌──────────────────────────────────────────────────────────┐
│ CURRENT SIZE                                            │
│ Current: Standard_B4ms ✓                               │
│   vCPUs: 4                                              │
│   RAM: 16 GiB                                           │
│   Data disks: 8                                         │
│   Max IOPS: 2,880                                       │
│   Cost: ~$121.47/month                                  │
└──────────────────────────────────────────────────────────┘
```

### **Step 4**: Start Resized VM

```
1. Click "Overview" in left menu
2. Status shows: ● Stopped (deallocated)
3. Click "Start" button
4. Wait 2-3 minutes for VM to start
5. Verify new size in Overview:
   Size: Standard_B4ms (4 vcpus, 16 GiB memory)
```

---

## **Task 6.3: Create VM Snapshot (Backup)**

### **Step 1**: Navigate to Disks

```
1. Go to VM "vm-webserver-01"
2. In left menu under "Settings"
3. Click "Disks"

You'll see:
┌──────────────────────────────────────────────────────────┐
│ Disks                                                    │
├──────────────────────────────────────────────────────────┤
│ OS DISK                                                 │
│ ───────                                                 │
│ Name: vm-webserver-01_OsDisk_1_abc123                  │
│ Type: Premium SSD                                       │
│ Size: 30 GiB                                           │
│ IOPS: 120                                              │
│ Throughput: 25 MB/s                                    │
│ Host caching: Read/write                               │
│ Encryption: SSE with PMK                               │
│                                                          │
│ [Create snapshot]  [Change]                            │
│                                                          │
│ DATA DISKS                                              │
│ ──────────                                             │
│ LUN  Name                        Size    Host caching  │
│ 0    vm-webserver-01_DataDisk_0  128 GiB  None        │
│                                                          │
│ [+ Create and attach a new disk]                       │
└──────────────────────────────────────────────────────────┘

Click "Create snapshot" under OS DISK
```

### **Step 2**: Configure Snapshot

```
"Create snapshot" page opens:

┌──────────────────────────────────────────────────────────┐
│ Create a snapshot                                        │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Encryption] [Tags] [Review + create]          │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ ────────────────                                        │
│ Subscription *                                          │
│ [Pay-As-You-Go                                      ▼]  │
│                                                          │
│ Resource group *                                        │
│ [rg-vm-lab                                          ▼]  │
│                                                          │
│ INSTANCE DETAILS                                        │
│ ─────────────────                                       │
│ Name *                                                  │
│ [vm-webserver-01-snapshot-20241230                  ]   │
│                                                          │
│ Region                                                  │
│ [East US                                            ▼]  │
│   (Same as source disk)                                │
│                                                          │
│ Source subscription                                     │
│ Pay-As-You-Go (read-only)                              │
│                                                          │
│ Source disk                                             │
│ vm-webserver-01_OsDisk_1_abc123 (read-only)            │
│                                                          │
│ Snapshot type                                           │
│ ● Full (selected)                                       │
│ ○ Incremental                                          │
│                                                          │
│ Storage type                                            │
│ [Standard HDD (locally-redundant storage)           ▼]  │
│   Options: Standard HDD, Standard SSD, Premium SSD     │
│                                                          │
│ ℹ Cost: ~$1.20/month for 30 GiB Standard HDD           │
└──────────────────────────────────────────────────────────┘

Configure:
  Name: vm-webserver-01-snapshot-20241230
  Snapshot type: ● Full
  Storage type: Standard HDD (LRS) - cheapest for backups
  
Click "Next: Encryption >"
```

### **Step 3**: Encryption Settings

```
Encryption tab:

┌──────────────────────────────────────────────────────────┐
│ Encryption                                              │
├──────────────────────────────────────────────────────────┤
│ Encryption type                                         │
│ ● Encryption at-rest with a platform-managed key       │
│ ○ Encryption at-rest with a customer-managed key       │
│                                                          │
│ (Leave as platform-managed for simplicity)             │
└──────────────────────────────────────────────────────────┘

Click "Next: Tags >"
```

### **Step 4**: Add Tags

```
Tags tab:

Name                Value
──────────────────  ────────────────────────
Purpose             Backup
Date                2024-12-30
Environment         Production

Click "Next: Review + create >"
```

### **Step 5**: Review and Create

```
Review page shows:

┌──────────────────────────────────────────────────────────┐
│ Validation passed ✓                                     │
├──────────────────────────────────────────────────────────┤
│ Snapshot details:                                       │
│   Name: vm-webserver-01-snapshot-20241230              │
│   Resource group: rg-vm-lab                            │
│   Region: East US                                       │
│   Source disk: vm-webserver-01_OsDisk_1_abc123         │
│   Snapshot type: Full                                   │
│   Size: 30 GiB                                         │
│   Storage type: Standard HDD (LRS)                     │
│                                                          │
│ Estimated cost: $1.20/month                            │
│                                                          │
│          [< Previous]              [Create]             │
└──────────────────────────────────────────────────────────┘

Click "Create"

Deployment takes 1-2 minutes

  ✓ Your deployment is complete

Click "Go to resource"
```

### **Step 6**: View Snapshot Details

```
Snapshot overview:

┌──────────────────────────────────────────────────────────┐
│ vm-webserver-01-snapshot-20241230                       │
├──────────────────────────────────────────────────────────┤
│ [Delete] [Create disk] [Export]                        │
│                                                          │
│ ESSENTIALS                                              │
│ Resource group: rg-vm-lab                               │
│ Location: East US                                       │
│ Subscription: Pay-As-You-Go                            │
│ Status: Succeeded ✓                                    │
│ Snapshot ID: /subscriptions/.../snapshots/...          │
│                                                          │
│ PROPERTIES                                              │
│ Source disk: vm-webserver-01_OsDisk_1_abc123           │
│ Time created: 12/30/2024, 4:15:32 PM                   │
│ Size: 30 GiB                                           │
│ Storage type: Standard HDD (LRS)                       │
│ Snapshot type: Full                                     │
│ Disk state: Unattached                                 │
│ Source disk size: 30 GiB                               │
│ Incremental: No                                         │
│ Encryption: Platform-managed                            │
└──────────────────────────────────────────────────────────┘
```

---

## **Task 6.4: Restore VM from Snapshot**

### **Step 1**: Create Disk from Snapshot

```
1. In snapshot overview page
2. Click "Create disk" button at top

"Create a managed disk" page opens:

┌──────────────────────────────────────────────────────────┐
│ Create a managed disk                                    │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Encryption] [Networking] [Advanced] [Tags]    │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: rg-vm-lab                               │
│                                                          │
│ DISK DETAILS                                            │
│ Disk name *                                             │
│ [vm-webserver-01-restored-disk                      ]   │
│                                                          │
│ Region                                                  │
│ [East US                                            ▼]  │
│                                                          │
│ Availability zone                                       │
│ [None                                               ▼]  │
│                                                          │
│ Source type                                             │
│ [Snapshot                                           ▼]  │
│   (Read-only, pre-filled)                              │
│                                                          │
│ Source snapshot                                         │
│ [vm-webserver-01-snapshot-20241230              ▼]     │
│   (Pre-selected)                                        │
│                                                          │
│ Size                                                    │
│ Change size: [Premium SSD                          ▼]  │
│              [30 GiB                               ▼]  │
│                                                          │
│ [Review + create]                                       │
└──────────────────────────────────────────────────────────┘

Configure:
  Disk name: vm-webserver-01-restored-disk
  Source type: Snapshot (pre-filled)
  Source snapshot: vm-webserver-01-snapshot-20241230
  Size: Premium SSD, 30 GiB

Click "Review + create"
Click "Create"

Wait 1-2 minutes for disk creation
```

### **Step 2**: Swap OS Disk (Optional - to restore VM)

```
If you want to restore the VM to snapshot state:

1. Go to VM "vm-webserver-01"
2. Stop and deallocate the VM
3. In left menu, click "Disks"
4. Under OS DISK section
5. Click "Swap OS disk" button

Panel opens:

┌─────────────────────────────────────────┐
│ Swap OS disk                            │
├─────────────────────────────────────────┤
│ Current OS disk                         │
│ vm-webserver-01_OsDisk_1_abc123        │
│                                         │
│ New OS disk *                           │
│ [Select disk                       ▼]  │
│                                         │
│ Available disks:                        │
│ ○ vm-webserver-01-restored-disk        │
│                                         │
│ ⚠ This operation will:                │
│   • Replace current OS disk            │
│   • Reboot the VM                      │
│   • Current disk will remain in RG     │
│                                         │
│          [Swap]        [Cancel]         │
└─────────────────────────────────────────┘

Select: vm-webserver-01-restored-disk
Click "Swap"

This restores VM to snapshot state!
```

---

## **Task 6.5: Create Custom VM Image**

### **Step 1**: Prepare VM for Imaging

```
Important: Before creating image, generalize the VM

Method 1: Using Azure Portal Run Command

1. Go to VM "vm-webserver-01"
2. Ensure VM is Running
3. In left menu, under "Operations"
4. Click "Run command"
5. Click "RunShellScript"

Panel opens:

┌─────────────────────────────────────────────────────────┐
│ Run Command Script                                      │
├─────────────────────────────────────────────────────────┤
│ Script *                                                │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ #!/bin/bash                                         │ │
│ │ # Generalize VM for imaging                         │ │
│ │ sudo waagent -deprovision+user -force               │ │
│ │                                                      │ │
│ └─────────────────────────────────────────────────────┘ │
│                                                          │
│          [Run]                    [Cancel]              │
└─────────────────────────────────────────────────────────┘

Click "Run"

Output shows:
WARNING! The waagent service will be stopped.
WARNING! All SSH host key pairs will be deleted.
WARNING! Cached DHCP leases will be deleted.
WARNING! root password will be disabled. You will not be able to login as root.
WARNING! azureuser account and entire home directory will be deleted.

Script completed successfully
```

### **Step 2**: Deallocate and Generalize VM

```
1. Go back to VM Overview
2. Click "Stop" to deallocate VM
3. Wait for "Stopped (deallocated)" status

4. In Overview page, at the very top, click on JSON View
   (Or use the URL path to get to Properties)

Actually, easier way:

1. Click Cloud Shell icon (>_) at top of portal
2. In Cloud Shell, run:

az vm deallocate \
  --resource-group rg-vm-lab \
  --name vm-webserver-01

az vm generalize \
  --resource-group rg-vm-lab \
  --name vm-webserver-01

Output:
Deallocating VM...
VM deallocated successfully
Generalizing VM...
VM generalized successfully
```

### **Step 3**: Create Image from Generalized VM

```
1. Go to VM "vm-webserver-01"
2. At the top menu
3. Click "Capture" button

┌──────────────────────────────────────────────────────────┐
│ vm-webserver-01                                          │
├──────────────────────────────────────────────────────────┤
│ [Connect ▼] [Start] [Capture] [Delete] [⋮ More]         │
│                                                          │
│ Status: ● Stopped (generalized)                         │
└──────────────────────────────────────────────────────────┘

Click "Capture"

"Create an image" page opens:

┌──────────────────────────────────────────────────────────┐
│ Create an image                                          │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Tags] [Review + create]                       │
│  ──────                                                 │
│                                                          │
│ ⓘ Creating an image from this VM                       │
│                                                          │
│ TARGET                                                   │
│ ───────                                                 │
│ ● Azure Compute Gallery (recommended)                  │
│ ○ Managed image (legacy)                               │
│                                                          │
│ --- Azure Compute Gallery selected ---                 │
│                                                          │
│ Gallery details                                         │
│ ● Create new gallery                                    │
│ ○ Use existing gallery                                 │
│                                                          │
│ Gallery name *                                          │
│ [gallery_webserver_images                           ]   │
│                                                          │
│ Operating system state                                  │
│ ● Generalized (selected)                               │
│ ○ Specialized                                          │
│                                                          │
│ VM IMAGE DEFINITION                                     │
│ ──────────────────────                                 │
│ Target VM image definition *                            │
│ ● Create new                                           │
│ ○ Use existing                                         │
│                                                          │
│ VM image definition name *                              │
│ [ubuntu-webserver-nginx                             ]   │
│                                                          │
│ Publisher *                                             │
│ [MyCompany                                          ]   │
│                                                          │
│ Offer *                                                 │
│ [WebServer                                          ]   │
│                                                          │
│ SKU *                                                   │
│ [Ubuntu2204-NGINX                                   ]   │
│                                                          │
│ VERSION                                                 │
│ ────────                                                │
│ Version number *                                        │
│ [1.0.0                                              ]   │
│                                                          │
│ Default storage SKU                                     │
│ [Premium SSD LRS                                    ▼]  │
│                                                          │
│ SOURCE VM                                               │
│ ───────────                                            │
│ ☐ Automatically delete this virtual machine after      │
│   creating the image                                    │
│                                                          │
│          [Review + create]                              │
└──────────────────────────────────────────────────────────┘

Configure:
  Target: ● Azure Compute Gallery
  Gallery: ● Create new
    Name: gallery_webserver_images
  Operating system state: ● Generalized
  VM image definition: ● Create new
    Name: ubuntu-webserver-nginx
    Publisher: MyCompany
    Offer: WebServer
    SKU: Ubuntu2204-NGINX
  Version: 1.0.0
  ☐ Do NOT check "delete VM" (keep it for now)

Click "Review + create"
Click "Create"

Deployment takes 3-5 minutes

  ✓ Your deployment is complete
  
Click "Go to resource"
```

### **Step 4**: View Created Image

```
You'll see the Compute Gallery:

┌──────────────────────────────────────────────────────────┐
│ gallery_webserver_images                                 │
├──────────────────────────────────────────────────────────┤
│ ESSENTIALS                                              │
│ Resource group: rg-vm-lab                               │
│ Location: East US                                       │
│ Subscription: Pay-As-You-Go                            │
│                                                          │
│ VM IMAGE DEFINITIONS                                    │
│ ────────────────────────                               │
│ Name                      Publisher  Offer      SKU     │
│ ───────────────────────────────────────────────────────  │
│ ubuntu-webserver-nginx    MyCompany  WebServer  Ubuntu.. │
│   Versions: 1                                           │
│   Latest: 1.0.0                                        │
│                                                          │
│ [+ Create VM image definition]                         │
└──────────────────────────────────────────────────────────┘

Click on "ubuntu-webserver-nginx" to see details
```

---

## **Task 6.6: Deploy New VM from Custom Image**

### **Step 1**: Create VM from Image

```
1. In the image definition page
2. Click "+ Create VM" at the top

OR

1. Go to Virtual machines
2. Click "+ Create" → "Azure virtual machine"
3. In Basics tab, under Image
4. Click "See all images"
5. Click "My Images" tab at top
6. Select your image

The Create VM page with image pre-selected:

┌──────────────────────────────────────────────────────────┐
│ Create a virtual machine                                 │
├──────────────────────────────────────────────────────────┤
│ INSTANCE DETAILS                                        │
│                                                          │
│ Virtual machine name *                                  │
│ [vm-webserver-02                                    ]   │
│                                                          │
│ Region                                                  │
│ [East US                                            ▼]  │
│                                                          │
│ Image *                                                 │
│ [ubuntu-webserver-nginx - 1.0.0 (Custom)           ▼]  │
│ ✓ This is a custom image from your gallery            │
│                                                          │
│ Size                                                    │
│ [Standard_B2s - 2 vcpus, 4 GiB memory              ▼]  │
│                                                          │
│ ADMINISTRATOR ACCOUNT                                   │
│ Username: azureuser                                     │
│ SSH: (generate new or use existing)                    │
└──────────────────────────────────────────────────────────┘

Complete the rest of the wizard:
  Name: vm-webserver-02
  Image: ubuntu-webserver-nginx 1.0.0 (pre-selected)
  Size: Standard_B2s
  VNet: vnet-webapp (same as VM 1)
  Public IP: Create new

Click "Review + create"
Click "Create"

Wait 3-5 minutes

New VM deploys with NGINX pre-installed!
Access it via browser - web server is already configured!
```

---

## Lab 7: VM Availability Sets and Availability Zones

## **Task 7.1: Create Availability Set**

### **Step 1**: Navigate to Availability Sets

```
1. In Portal search bar, type "Availability sets"
2. Click "Availability sets" from results
3. Click "+ Create"
```

### **Step 2**: Configure Availability Set

```
┌──────────────────────────────────────────────────────────┐
│ Create availability set                                  │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Tags] [Review + create]                       │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: rg-vm-lab                               │
│                                                          │
│ INSTANCE DETAILS                                        │
│ Name *                                                  │
│ [avset-webapp                                       ]   │
│                                                          │
│ Region *                                                │
│ [East US                                            ▼]  │
│                                                          │
│ AVAILABILITY SET SETTINGS                               │
│ Fault domains *                                         │
│ [2                                                  ▼]  │
│   (Range: 1-3)                                         │
│   ⓘ Physical separation - different racks             │
│                                                          │
│ Update domains *                                        │
│ [5                                                  ▼]  │
│   (Range: 1-20)                                        │
│   ⓘ Logical grouping for updates                      │
│                                                          │
│ Use managed disks                                       │
│ ● Yes (aligned) - Recommended                          │
│ ○ No (classic)                                         │
│                                                          │
│ ℹ Fault domains: VMs distributed across different      │
│   physical hardware (racks with separate power/network)│
│                                                          │
│ ℹ Update domains: VMs grouped so not all update at    │
│   the same time during planned maintenance             │
│                                                          │
│ SLA: 99.95% availability                               │
│                                                          │
│          [Review + create]                              │
└──────────────────────────────────────────────────────────┘

Configure:
  Name: avset-webapp
  Region: East US
  Fault domains: 2
  Update domains: 5
  Managed disks: ● Yes (aligned)

Click "Review + create"
Click "Create"

Deployment completes in seconds

Click "Go to resource"
```

### **Step 3**: View Availability Set Details

```
┌──────────────────────────────────────────────────────────┐
│ avset-webapp                                             │
├──────────────────────────────────────────────────────────┤
│ ESSENTIALS                                              │
│ Resource group: rg-vm-lab                               │
│ Location: East US                                       │
│ Subscription: Pay-As-You-Go                            │
│ Fault domains: 2                                        │
│ Update domains: 5                                       │
│ Managed: Yes                                            │
│                                                          │
│ VIRTUAL MACHINES                                        │
│ ───────────────                                        │
│ Count: 0                                                │
│                                                          │
│ No virtual machines in this availability set            │
│                                                          │
│ ℹ Add VMs by creating new VMs or moving existing VMs  │
└──────────────────────────────────────────────────────────┘
```

---

## **Task 7.2: Create VMs in Availability Set**

### **Step 1**: Create First VM in Availability Set

```
1. Go to Virtual machines
2. Click "+ Create" → "Azure virtual machine"
3. Basics tab:

PROJECT DETAILS:
  Resource group: rg-vm-lab

INSTANCE DETAILS:
  VM name: vm-web-avset-01
  Region: East US
  Availability options: [Availability set                ▼]
  
  Availability set: [avset-webapp                        ▼]
  
  ⓘ VM will be placed in availability set
  
  Image: Ubuntu Server 22.04 LTS
  Size: Standard_B2s
  
ADMINISTRATOR ACCOUNT:
  Username: azureuser
  SSH: Generate new key pair
  Key name: vm-web-avset-01_key

INBOUND PORT RULES:
  Select ports: SSH (22), HTTP (80), HTTPS (443)

Continue with Disks tab:
  OS disk: Premium SSD, 30 GiB

Networking tab:
  VNet: vnet-webapp
  Subnet: subnet-web
  Public IP: None (we'll use load balancer later)
  NIC NSG: Basic
  Ports: 22, 80, 443

Management tab:
  Auto-shutdown: Disabled for this demo

Click "Review + create"
Click "Create"
Download SSH key

Wait for deployment
```

### **Step 2**: Create Second and Third VMs

```
Repeat above steps but change:

VM 2:
  Name: vm-web-avset-02
  Availability set: avset-webapp (same)
  Key name: vm-web-avset-02_key
  Public IP: None

VM 3:
  Name: vm-web-avset-03
  Availability set: avset-webapp (same)
  Key name: vm-web-avset-03_key
  Public IP: None

Create both VMs

Total deployment time: 5-10 minutes for all
```

### **Step 3**: Verify Fault and Update Domain Distribution

```
1. Go back to "avset-webapp" availability set
2. In left menu, click "Virtual machines"

You'll see:

┌──────────────────────────────────────────────────────────────────┐
│ Virtual machines                                                 │
├──────────────────────────────────────────────────────────────────┤
│ VMs in availability set: 3                                      │
│                                                                  │
│ Name              Status   Fault   Update   Size                │
│                            Domain  Domain                        │
│ ─────────────────────────────────────────────────────────────── │
│ vm-web-avset-01   Running    0       0     Standard_B2s        │
│ vm-web-avset-02   Running    1       1     Standard_B2s        │
│ vm-web-avset-03   Running    0       2     Standard_B2s        │
│                                                                  │
│ ℹ Fault Domain Distribution:                                   │
│   FD 0: vm-web-avset-01, vm-web-avset-03                       │
│   FD 1: vm-web-avset-02                                        │
│                                                                  │
│ ℹ Update Domain Distribution:                                  │
│   UD 0: vm-web-avset-01                                        │
│   UD 1: vm-web-avset-02                                        │
│   UD 2: vm-web-avset-03                                        │
│                                                                  │
│ ✓ SLA: 99.95% availability                                     │
└──────────────────────────────────────────────────────────────────┘

This distribution means:
  - If FD 0 fails (power/network), only 2 VMs affected
  - During updates, only 1 UD updated at a time
  - At least 1 VM always available
```

---

## **Task 7.3: Create VMs in Availability Zones**

### **Step 1**: Create VM in Zone 1

```
1. Virtual machines → + Create
2. Basics tab:

INSTANCE DETAILS:
  VM name: vm-web-zone1
  Region: East US
  
  Availability options: [Availability zone              ▼]
  
  Availability zones: 
  ☑ Zone 1
  ☐ Zone 2
  ☐ Zone 3
  
  ⓘ Deploy across zones for 99.99% SLA
  
  Image: Ubuntu Server 22.04 LTS
  Size: Standard_B2s

Networking:
  VNet: vnet-webapp
  Subnet: subnet-web
  Public IP: Create new → vm-web-zone1-ip
    SKU: Standard (required for zones)
    Assignment: Static

Complete wizard and create VM
```

### **Step 2**: Create VM in Zone 2

```
Follow same steps:
  Name: vm-web-zone2
  Availability zone: ☑ Zone 2 only
  Public IP: vm-web-zone2-ip (Standard, Static)

Create VM
```

### **Step 3**: Create VM in Zone 3

```
Follow same steps:
  Name: vm-web-zone3
  Availability zone: ☑ Zone 3 only
  Public IP: vm-web-zone3-ip (Standard, Static)

Create VM
```

### **Step 4**: Verify Zone Distribution

```
1. Go to Virtual machines
2. Click "Columns" button
3. Check: ☑ Availability zone
4. View list:

┌──────────────────────────────────────────────────────────────┐
│ Virtual machines                                             │
├──────────────────────────────────────────────────────────────┤
│ Name           Status   Location  Availability  Size         │
│                                    Zone                      │
│ ───────────────────────────────────────────────────────────  │
│ vm-web-zone1   Running  East US   1            Standard_B2s │
│ vm-web-zone2   Running  East US   2            Standard_B2s │
│ vm-web-zone3   Running  East US   3            Standard_B2s │
│                                                              │
│ ✓ 99.99% SLA - protected against datacenter failure        │
└──────────────────────────────────────────────────────────────┘

Zones are physically separate datacenters:
  - Zone 1: Datacenter A (miles apart)
  - Zone 2: Datacenter B
  - Zone 3: Datacenter C
  
If entire datacenter fails, other zones unaffected!
```

---

## Lab 8: VM Monitoring with Azure Monitor

## **Task 8.1: Enable VM Insights**

### **Step 1**: Navigate to VM Monitoring

```
1. Go to VM "vm-webserver-01"
2. In left menu, under "Monitoring"
3. Click "Insights"

You'll see:

┌──────────────────────────────────────────────────────────┐
│ Insights                                                 │
├──────────────────────────────────────────────────────────┤
│ ℹ VM Insights is not enabled for this virtual machine  │
│                                                          │
│ VM Insights provides:                                   │
│   • Performance monitoring                              │
│   • Dependency mapping                                  │
│   • Log collection                                      │
│   • Pre-built workbooks                                │
│                                                          │
│ [Enable]                                                │
└──────────────────────────────────────────────────────────┘

Click "Enable" button
```

### **Step 2**: Configure Monitoring

```
"Enable VM Insights" panel opens:

┌─────────────────────────────────────────────────────────┐
│ Enable VM Insights                                      │
├─────────────────────────────────────────────────────────┤
│ DATA COLLECTION RULE                                    │
│ ● Create new data collection rule (recommended)        │
│ ○ Use existing data collection rule                    │
│                                                          │
│ Data collection rule name                               │
│ [MSVMI-rg-vm-lab                                   ]    │
│                                                          │
│ WORKSPACE CONFIGURATION                                 │
│ ● Create new Log Analytics workspace                   │
│ ○ Use existing Log Analytics workspace                 │
│                                                          │
│ Log Analytics workspace                                 │
│ Subscription: Pay-As-You-Go                            │
│ Workspace: [DefaultWorkspace-EastUS                ▼]  │
│ Location: East US                                       │
│                                                          │
│ GUEST PERFORMANCE COUNTERS                             │
│ ☑ Enable guest performance counters                   │
│   (Collects CPU, memory, disk, network metrics)        │
│                                                          │
│ PROCESSES AND DEPENDENCIES                             │
│ ☑ Enable processes and dependencies                   │
│   (Maps connections between VMs and services)          │
│                                                          │
│ Estimated cost: ~$2.30/day per VM                      │
│                                                          │
│          [Enable]                [Cancel]               │
└─────────────────────────────────────────────────────────┘

Configure:
  ● Create new data collection rule
  ● Create new workspace (or use existing)
  ☑ Enable guest performance counters
  ☑ Enable processes and dependencies

Click "Enable"

Notification appears:
  ⟳ Enabling VM Insights for vm-webserver-01...
  
Wait 5-10 minutes for agents to install and data to appear
```

### **Step 3**: View Performance Metrics

```
After waiting, refresh the Insights page:

┌──────────────────────────────────────────────────────────────────┐
│ Insights - Performance                                           │
├──────────────────────────────────────────────────────────────────┤
│ [Performance] [Map] [Health (preview)]                          │
│  ───────────                                                    │
│                                                                  │
│ Time range: [Last 1 hour ▼]    Auto refresh: [Off ▼]          │
│                                                                  │
│ CPU UTILIZATION                                                 │
│ ───────────────                                                 │
│ 100% ┤                                                          │
│  75% ┤     ╭─╮                                                 │
│  50% ┤   ╭─╯ ╰╮                                                │
│  25% ┤───╯    ╰─────────                                       │
│   0% └────────────────────────────────────────                 │
│      3:00  3:15  3:30  3:45  4:00  4:15  4:30                 │
│                                                                  │
│ Current: 45% | Average: 38% | Peak: 82%                        │
│                                                                  │
│ AVAILABLE MEMORY                                                │
│ ────────────────────                                           │
│ 4GB  ┤─────────╮                                               │
│ 3GB  ┤         ╰╮                                              │
│ 2GB  ┤          ╰──────────                                    │
│ 1GB  ┤                                                          │
│  0GB └────────────────────────────────────────                 │
│                                                                  │
│ Available: 2.1 GB | Used: 1.9 GB | Total: 4 GB                │
│                                                                  │
│ DISK IOPS                                                       │
│ ──────────                                                      │
│ Read: 45/sec | Write: 120/sec                                  │
│                                                                  │
│ NETWORK                                                         │
│ ───────                                                        │
│ Bytes received: 1.2 MB/s | Bytes sent: 450 KB/s               │
└──────────────────────────────────────────────────────────────────┘

Click different tabs to explore:
  - Performance: CPU, memory, disk, network
  - Map: Dependency visualization
  - Health: System health metrics
```

---

## **Task 8.2: Create Alert Rules**

### **Step 1**: Navigate to Alerts

```
1. In VM "vm-webserver-01"
2. Left menu → Monitoring → Alerts
3. Click "+ Create" → "Alert rule"
```

### **Step 2**: Configure Alert Scope

```
"Create an alert rule" page opens:

┌──────────────────────────────────────────────────────────┐
│ Create an alert rule                                     │
├──────────────────────────────────────────────────────────┤
│ [Scope] [Condition] [Actions] [Details] [Tags] [Review] │
│  ─────                                                  │
│                                                          │
│ SCOPE                                                   │
│ ─────                                                   │
│ Resource: vm-webserver-01 ✓                            │
│ Resource type: Virtual machine                          │
│ Subscription: Pay-As-You-Go                            │
│                                                          │
│ (Scope is pre-selected since you started from VM)      │
│                                                          │
│          [Next: Condition >]                            │
└──────────────────────────────────────────────────────────┘

Click "Next: Condition >"
```

### **Step 3**: Add Condition (High CPU)

```
Condition tab:

┌──────────────────────────────────────────────────────────┐
│ Select a signal                                          │
├──────────────────────────────────────────────────────────┤
│ Search: [cpu                                        ] 🔍 │
│                                                          │
│ Popular signals:                                        │
│ ───────────────                                        │
│ ○ Percentage CPU (Platform)                            │
│   Metric: Average CPU percentage                        │
│                                                          │
│ ○ CPU Credits Consumed (Platform)                      │
│ ○ CPU Credits Remaining (Platform)                     │
│ ○ Disk Read Bytes (Platform)                           │
│ ○ Disk Write Bytes (Platform)                          │
│ ○ Network In Total (Platform)                          │
│                                                          │
└──────────────────────────────────────────────────────────┘

Select: ○ Percentage CPU

Configure signal logic panel opens:

┌─────────────────────────────────────────────────────────┐
│ Configure signal logic - Percentage CPU                 │
├─────────────────────────────────────────────────────────┤
│ CHART PREVIEW (Last 6 hours)                           │
│ ───────────────────────────────────────                │
│ 100% ┤                                                  │
│  75% ┤                                                  │
│  50% ┤    ╭╮    ╭╮                                     │
│  25% ┤────╯╰────╯╰────────                            │
│   0% └──────────────────────────────                   │
│                                                          │
│ Threshold line will appear based on your setting       │
│                                                          │
│ ALERT LOGIC                                             │
│ ──────────                                             │
│ Threshold *                                             │
│ ● Static                                               │
│ ○ Dynamic                                              │
│                                                          │
│ Aggregation type                                        │
│ [Average                                            ▼]  │
│                                                          │
│ Operator                                                │
│ [Greater than                                       ▼]  │
│                                                          │
│ Threshold value *                                       │
│ [80                                                 ]   │
│   (Trigger when CPU exceeds 80%)                       │
│                                                          │
│ EVALUATION                                              │
│ ──────────                                             │
│ Check every                                             │
│ [1 minute                                           ▼]  │
│                                                          │
│ Lookback period                                         │
│ [5 minutes                                          ▼]  │
│   (Alert triggers if condition met for 5 minutes)      │
│                                                          │
│ Preview:                                                │
│ Alert triggers when average CPU > 80% for 5 minutes    │
│                                                          │
│          [Done]                   [Cancel]              │
└─────────────────────────────────────────────────────────┘

Configure:
  Threshold: ● Static
  Aggregation: Average
  Operator: Greater than
  Threshold value: 80
  Check every: 1 minute
  Lookback period: 5 minutes

Click "Done"
Click "Next: Actions >"
```

### **Step 4**: Configure Action Group

```
Actions tab:

┌──────────────────────────────────────────────────────────┐
│ Actions                                                  │
├──────────────────────────────────────────────────────────┤
│ Select or create an action group                        │
│                                                          │
│ ● Create action group                                   │
│ ○ Select existing action group                         │
│                                                          │
│ [+ Create action group]                                 │
└──────────────────────────────────────────────────────────┘

Click "+ Create action group"

New page opens:

┌──────────────────────────────────────────────────────────┐
│ Create an action group                                   │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Notifications] [Actions] [Tags] [Review]      │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: rg-vm-lab                               │
│                                                          │
│ INSTANCE DETAILS                                        │
│ Action group name *                                     │
│ [ag-vm-alerts                                       ]   │
│                                                          │
│ Display name *                                          │
│ [VM Alerts                                          ]   │
│   (Max 12 characters, appears in notifications)        │
│                                                          │
│ Region                                                  │
│ [Global                                             ▼]  │
│                                                          │
│          [Next: Notifications >]                        │
└──────────────────────────────────────────────────────────┘

Fill in:
  Action group name: ag-vm-alerts
  Display name: VM Alerts
  Region: Global

Click "Next: Notifications >"
```

### **Step 5**: Add Email Notification

```
Notifications tab:

┌──────────────────────────────────────────────────────────┐
│ Notifications                                            │
├──────────────────────────────────────────────────────────┤
│ Add or edit notifications to send when an alert triggers│
│                                                          │
│ [+ Add notification]                                    │
│                                                          │
│ No notifications configured                             │
└──────────────────────────────────────────────────────────┘

Click "+ Add notification"

Panel opens:

┌─────────────────────────────────────────┐
│ Select notification type                │
├─────────────────────────────────────────┤
│ Notification type *                     │
│ [Email/SMS message/Push/Voice       ▼] │
│                                         │
│ [Panel expands with selected type]     │
│                                         │
│ Email ☑                                │
│ [your-email@company.com             ]  │
│                                         │
│ SMS ☐                                  │
│ Country code: [+1                   ]  │
│ Phone: [                            ]  │
│                                         │
│ Azure app Push Notifications ☐         │
│                                         │
│ Voice ☐                                │
│                                         │
│ Name *                                  │
│ [EmailAdmin                         ]  │
│                                         │
│ ☑ Enable the common alert schema      │
│   (Standardizes alert format)          │
│                                         │
│          [OK]            [Cancel]       │
└─────────────────────────────────────────┘

Configure:
  Notification type: Email/SMS/Push/Voice
  Email ☑: your-email@company.com
  SMS ☐: (optional)
  Name: EmailAdmin
  ☑ Enable common alert schema

Click "OK"

Now shows:
┌──────────────────────────────────────────────────────────┐
│ Notifications                                            │
│ ───────────────────────────────────────────────────────  │
│ Type          Name         Enabled                      │
│ Email/SMS..   EmailAdmin   Yes ✓                        │
└──────────────────────────────────────────────────────────┘

Click "Next: Actions >" (optional - skip for now)
Click "Next: Tags >" (optional - skip)
Click "Review + create"
Click "Create"
```

### **Step 6**: Complete Alert Rule

```
Back on the alert rule creation page:

Actions tab now shows:
  Selected action group: ag-vm-alerts ✓

Click "Next: Details >"

Details tab:

┌──────────────────────────────────────────────────────────┐
│ Alert rule details                                       │
├──────────────────────────────────────────────────────────┤
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: rg-vm-lab                               │
│                                                          │
│ ALERT RULE DETAILS                                      │
│ Severity *                                              │
│ [2 - Warning                                        ▼]  │
│   (0-Critical, 1-Error, 2-Warning, 3-Info, 4-Verbose)  │
│                                                          │
│ Alert rule name *                                       │
│ [High CPU Alert - vm-webserver-01                   ]   │
│                                                          │
│ Alert rule description                                  │
│ [Triggers when CPU exceeds 80% for 5 minutes        ]   │
│                                                          │
│ Region                                                  │
│ East US (read-only)                                    │
│                                                          │
│ ☑ Enable upon creation                                 │
│ ☐ Automatically resolve alerts                         │
│                                                          │
│ ADVANCED OPTIONS                                        │
│ Check workspace data                                    │
│ [Yes                                                ▼]  │
│                                                          │
│          [Review + create]                              │
└──────────────────────────────────────────────────────────┘

Fill in:
  Severity: 2 - Warning
  Alert rule name: High CPU Alert - vm-webserver-01
  Description: Triggers when CPU exceeds 80% for 5 minutes
  ☑ Enable upon creation

Click "Review + create"
Click "Create"

Alert rule is now active!
```

---

## **Task 8.3: Test Alert by Generating CPU Load**

### **Step 1**: Connect to VM

```
1. Go to VM "vm-webserver-01"
2. Click "Connect" → "Connect"
3. Use Azure Cloud Shell or SSH
```

### **Step 2**: Generate CPU Load

```
In SSH session:

# Install stress tool
azureuser@vm-webserver-01:~$ sudo apt install stress -y

# Generate CPU load (80%+ utilization)
azureuser@vm-webserver-01:~$ stress --cpu 4 --timeout 600s

Output shows:
stress: info: [12345] dispatching hogs: 4 cpu, 0 io, 0 vm, 0 hdd

This will run for 10 minutes (600 seconds)
```

### **Step 3**: Monitor Alert Status

```
1. Go back to VM → Alerts page
2. You'll see:

┌──────────────────────────────────────────────────────────┐
│ Alerts                                                   │
├──────────────────────────────────────────────────────────┤
│ [Time range: Last 24 hours ▼]  [Refresh]               │
│                                                          │
│ ALERT RULES                                             │
│ Total: 1 | Enabled: 1                                   │
│                                                          │
│ FIRED ALERTS                                            │
│ ──────────────                                          │
│ Severity  Name                    State     Time        │
│ ───────────────────────────────────────────────────────  │
│ ⚠ Warning High CPU Alert        Fired     4:15 PM     │
│            vm-webserver-01                              │
│                                                          │
│ Click on alert to see details                          │
└──────────────────────────────────────────────────────────┘

After ~5 minutes of high CPU:
  - Alert state changes to "Fired"
  - Email notification sent to your address
  - Alert appears in portal
```

### **Step 4**: View Alert Details

```
Click on the fired alert:

┌──────────────────────────────────────────────────────────┐
│ Alert: High CPU Alert - vm-webserver-01                 │
├──────────────────────────────────────────────────────────┤
│ Status: ● Fired                                         │
│ Severity: ⚠ Warning (Sev 2)                            │
│ Fired at: 12/30/2024, 4:15:32 PM                       │
│                                                          │
│ CONDITION                                               │
│ Metric: Percentage CPU                                  │
│ Operator: Greater than                                  │
│ Threshold: 80%                                          │
│ Current value: 94.5%                                    │
│ Aggregation: Average over 5 minutes                     │
│                                                          │
│ ACTIONS TAKEN                                           │
│ • Email sent to: your-email@company.com ✓              │
│   Sent at: 4:15:35 PM                                  │
│                                                          │
│ [View in metrics] [Close alert]                        │
└──────────────────────────────────────────────────────────┘
```

### **Step 5**: Check Email Notification

```
In your email inbox, you'll receive:

From: Microsoft Azure <azure-noreply@microsoft.com>
Subject: ⚠ Fired: High CPU Alert - vm-webserver-01

───────────────────────────────────────────────
Azure Monitor Alert
───────────────────────────────────────────────
Alert: High CPU Alert - vm-webserver-01
Severity: Warning
Status: Fired
Fired at: 12/30/2024 4:15:32 PM UTC

Resource: vm-webserver-01
Resource group: rg-vm-lab
Subscription: Pay-As-You-Go

Condition:
Percentage CPU Greater than 80%
Current value: 94.5%

View alert in Azure Portal:
[View Alert] button

───────────────────────────────────────────────
```

---

## **Task 8.4: View Metrics in Portal**

### **Step 1**: Access Metrics

```
1. Go to VM "vm-webserver-01"
2. Left menu → Monitoring → Metrics

Metrics explorer opens:

┌──────────────────────────────────────────────────────────────────┐
│ Metrics                                                          │
├──────────────────────────────────────────────────────────────────┤
│ Scope: vm-webserver-01                         [Change scope]   │
│ Time range: [Last 24 hours ▼]  [Local time ▼]  [Auto refresh ▼] │
│                                                                  │
│ [+ New chart] [+ New alert rule] [Pin to dashboard] [Share]    │
│                                                                  │
│ CHART 1                                                         │
│ ───────                                                         │
│ Metric: [Select a metric...                                ▼]  │
└──────────────────────────────────────────────────────────────────┘

Click "Select a metric..." dropdown
```

### **Step 2**: Add CPU Metric

```
Dropdown shows available metrics:

┌─────────────────────────────────────────┐
│ Select a metric                         │
├─────────────────────────────────────────┤
│ Search: [                          ] 🔍 │
│                                         │
│ Virtual Machine Host (Preview)         │
│   Percentage CPU                        │
│   Available Memory Bytes                │
│   Disk Read Bytes                       │
│   Disk Write Bytes                      │
│   Network In Total                      │
│   Network Out Total                     │
│                                         │
│ Virtual Machine Guest (classic)         │
│   Percentage CPU                        │
│   Memory\Available Bytes                │
│   ...                                   │
└─────────────────────────────────────────┘

Select: "Percentage CPU" (under Virtual Machine Host)

Chart appears:

┌──────────────────────────────────────────────────────────────────┐
│ Percentage CPU                                                   │
│ ────────────────                                                │
│ Aggregation: [Average ▼]  [Line chart ▼]                       │
│                                                                  │
│ 100% ┤                   ╭────────╮                             │
│  80% ┤                ╭──╯        ╰──╮                         │
│  60% ┤              ╭─╯              ╰─╮                        │
│  40% ┤         ╭────╯                  ╰────╮                  │
│  20% ┤─────────╯                            ╰─────             │
│   0% └────────────────────────────────────────────────         │
│      12AM  3AM   6AM   9AM  12PM  3PM   6PM   9PM  12AM       │
│                                                                  │
│ Current: 94.5% | Average: 52.3% | Max: 98.1%                   │
└──────────────────────────────────────────────────────────────────┘

You can see the spike when you ran the stress test!
```

### **Step 3**: Add Multiple Metrics

```
Below the chart, click "+ Add metric"

Add these metrics:
1. Available Memory Bytes
2. Network In Total  
3. Disk Read Bytes

You'll see multiple lines on the chart:

┌──────────────────────────────────────────────────────────────────┐
│ vm-webserver-01 Metrics                                          │
│ ───────────────────────────────────────────────────────────────  │
│                                                                  │
│ Legend:                                                          │
│ ─── Percentage CPU (%)                                          │
│ ─── Available Memory (GB) [scaled]                              │
│ ─── Network In (MB)                                             │
│ ─── Disk Read (MB)                                              │
│                                                                  │
│ 100  ┤                   ╭────╮                                  │
│  80  ┤                ╭──╯    ╰──╮    ┌Memory                  │
│  60  ┤     ┌CPU    ╭─╯          ╰─╮  ╱                         │
│  40  ┤  ╱──╯   ╱──╯                ╰─╯                          │
│  20  ┤─╯     ╱                                                  │
│   0  └─────────────────────────────────────────                 │
│                                                                  │
│ [Split by] [Add filter] [⋮ More]                               │
└──────────────────────────────────────────────────────────────────┘
```

### **Step 4**: Pin Chart to Dashboard

```
Click "Pin to dashboard" button at top

Panel opens:

┌─────────────────────────────────────────┐
│ Pin to dashboard                        │
├─────────────────────────────────────────┤
│ Dashboard *                             │
│ ● Create new (selected)                │
│ ○ Existing                             │
│                                         │
│ Type                                    │
│ ● Private                              │
│ ○ Shared                               │
│                                         │
│ Dashboard name                          │
│ [VM Monitoring Dashboard           ]   │
│                                         │
│          [Pin]         [Cancel]         │
└─────────────────────────────────────────┘

Configure:
  ● Create new
  ● Private
  Dashboard name: VM Monitoring Dashboard

Click "Pin"

Notification: ✓ Pinned to dashboard

Now accessible from Portal home → Dashboards
```

---

# Azure Virtual Networks - Portal Labs

## Lab 9: Create Hub-Spoke VNet Architecture

## **Task 9.1: Create Hub Virtual Network**

### **Step 1**: Create Hub VNet

```
1. Portal search → "Virtual networks"
2. Click "+ Create"

┌──────────────────────────────────────────────────────────┐
│ Create virtual network                                   │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Security] [IP Addresses] [Tags] [Review]      │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: [rg-network-lab                     ▼]  │
│   [Create new]                                          │
│                                                          │
│ INSTANCE DETAILS                                        │
│ Virtual network name *                                  │
│ [vnet-hub                                           ]   │
│                                                          │
│ Region *                                                │
│ [(US) East US                                       ▼]  │
│                                                          │
│          [Next: Security >]                             │
└──────────────────────────────────────────────────────────┘

Create resource group if needed:
  Click "Create new"
  Enter: rg-network-lab
  Click "OK"

Fill in:
  Resource group: rg-network-lab
  VNet name: vnet-hub
  Region: East US

Click "Next: Security >"
```

### **Step 2**: Security Settings

```
Security tab:

┌──────────────────────────────────────────────────────────┐
│ Security                                                 │
├──────────────────────────────────────────────────────────┤
│ AZURE BASTION                                           │
│ ☐ Enable Azure Bastion                                 │
│   (We'll add this later to a specific subnet)          │
│                                                          │
│ AZURE FIREWALL                                          │
│ ☐ Enable Azure Firewall                                │
│   (Optional - for centralized firewall)                 │
│                                                          │
│ AZURE DDOS PROTECTION                                   │
│ ☐ Enable Azure DDoS Protection Standard                │
│   (Costs ~$2,944/month - skip for lab)                 │
│                                                          │
│          [Next: IP Addresses >]    [Previous]           │
└──────────────────────────────────────────────────────────┘

Leave all unchecked for now
Click "Next: IP Addresses >"
```

### **Step 3**: Configure IP Address Space

```
IP Addresses tab:

┌──────────────────────────────────────────────────────────────────┐
│ IP Addresses                                                     │
├──────────────────────────────────────────────────────────────────┤
│ IPv4 address space *                                            │
│ [10.10.0.0/16                                              ] ✓  │
│   Provides 65,536 addresses                                     │
│                                                                  │
│ [+ Add IPv6 address space] (optional)                          │
│                                                                  │
│ SUBNETS                                                         │
│ ───────                                                         │
│ Subnet name       Address range       Available IPs    Actions │
│ ─────────────────────────────────────────────────────────────   │
│ default           10.10.0.0/24        251               [✏][🗑] │
│                                                                  │
│ [+ Add subnet]                                                  │
└──────────────────────────────────────────────────────────────────┘

Change address space:
  Click in the address space field
  Change to: 10.10.0.0/16
  Press Enter

Delete default subnet:
  Click 🗑 (Delete) next to "default"
  Confirm deletion

Now add proper subnets - Click "+ Add subnet"
```

### **Step 4**: Add Gateway Subnet

```
Add subnet panel opens:

┌─────────────────────────────────────────────────────────┐
│ Add a subnet                                            │
├─────────────────────────────────────────────────────────┤
│ SUBNET DETAILS                                          │
│ ────────────────                                        │
│ Subnet purpose                                          │
│ [Gateway Subnet                                     ▼]  │
│                                                          │
│ Starting address *                                      │
│ [10.10.0.0                                          ]   │
│                                                          │
│ Subnet size *                                           │
│ [/24 (256 addresses)                                ▼]  │
│                                                          │
│ Address range: 10.10.0.0/24                            │
│   (Required name: GatewaySubnet)                        │
│   (For VPN Gateway or ExpressRoute)                     │
│                                                          │
│          [Add]                    [Cancel]              │
└─────────────────────────────────────────────────────────┘

Configure:
  Subnet purpose: Gateway Subnet
  Starting address: 10.10.0.0
  Size: /24

Click "Add"
```

### **Step 5**: Add Management Subnet

```
Click "+ Add subnet" again

┌─────────────────────────────────────────────────────────┐
│ Add a subnet                                            │
├─────────────────────────────────────────────────────────┤
│ Subnet purpose                                          │
│ [Default                                            ▼]  │
│                                                          │
│ Name *                                                  │
│ [subnet-management                                  ]   │
│                                                          │
│ Starting address *                                      │
│ [10.10.2.0                                          ]   │
│                                                          │
│ Subnet size                                             │
│ [/24 (256 addresses)                                ▼]  │
│                                                          │
│ NAT GATEWAY                                             │
│ NAT gateway: [None                                  ▼]  │
│                                                          │
│ NETWORK SECURITY GROUP                                  │
│ [None                                               ▼]  │
│   (Can add later)                                       │
│                                                          │
│ ROUTE TABLE                                             │
│ [None                                               ▼]  │
│                                                          │
│ SUBNET DELEGATION                                       │
│ [None                                               ▼]  │
│                                                          │
│          [Add]                    [Cancel]              │
└─────────────────────────────────────────────────────────┘

Configure:
  Name: subnet-management
  Starting address: 10.10.2.0
  Size: /24

Click "Add"
```

### **Step 6**: Add Azure Bastion Subnet

```
Click "+ Add subnet"

Select:
  Subnet purpose: [Azure Bastion Subnet                  ▼]

Panel auto-fills:
  Name: AzureBastionSubnet (fixed name, cannot change)
  Starting address: 10.10.3.0
  Subnet size: /26 (64 addresses minimum)

Click "Add"
```

### **Step 7**: Review All Subnets

```
IP Addresses tab now shows:

┌──────────────────────────────────────────────────────────────────┐
│ Address space: 10.10.0.0/16                                     │
│                                                                  │
│ SUBNETS                                                         │
│ Subnet name          Address range    Available   Actions      │
│ ──────────────────────────────────────────────────────────────  │
│ GatewaySubnet        10.10.0.0/24     251         [✏][🗑]      │
│ subnet-management    10.10.2.0/24     251         [✏][🗑]      │
│ AzureBastionSubnet   10.10.3.0/26     59          [✏][🗑]      │
│                                                                  │
│ Address space used: 832 of 65,536 addresses                    │
└──────────────────────────────────────────────────────────────────┘

Click "Next: Tags >" (optional - skip)
Click "Review + create"
Click "Create"

Deployment completes in 10-15 seconds
Click "Go to resource"
```

---

## **Task 9.2: Create Spoke Virtual Networks**

### **Step 1**: Create Production Spoke VNet

```
1. Virtual networks → + Create

Basics:
  Resource group: rg-network-lab
  Name: vnet-spoke-prod
  Region: East US

Security: (skip all)

IP Addresses:
  IPv4 address space: 10.20.0.0/16
  
  Delete default subnet
  
  Add subnets:
  
  Subnet 1:
    Name: subnet-web
    Starting address: 10.20.1.0
    Size: /24
    
  Subnet 2:
    Name: subnet-app
    Starting address: 10.20.2.0
    Size: /24
    
  Subnet 3:
    Name: subnet-data
    Starting address: 10.20.3.0
    Size: /24

Review + create → Create
```

### **Step 2**: Create Development Spoke VNet

```
Virtual networks → + Create

Basics:
  Resource group: rg-network-lab
  Name: vnet-spoke-dev
  Region: East US

IP Addresses:
  IPv4 address space: 10.30.0.0/16
  
  Add subnet:
    Name: subnet-dev
    Starting address: 10.30.1.0
    Size: /24

Review + create → Create
```

---

## **Task 9.3: Configure VNet Peering (Hub-Spoke)**

### **Step 1**: Peer Hub to Production Spoke

```
1. Go to "vnet-hub"
2. In left menu, under "Settings"
3. Click "Peerings"

You'll see:

┌──────────────────────────────────────────────────────────┐
│ Peerings                                                 │
├──────────────────────────────────────────────────────────┤
│ [+ Add]  [Refresh]                                      │
│                                                          │
│ No peerings configured                                  │
│                                                          │
│ ℹ VNet peering connects virtual networks and allows    │
│   resources to communicate                              │
└──────────────────────────────────────────────────────────┘

Click "+ Add"
```

### **Step 2**: Configure Peering Settings

```
Add peering panel opens:

┌─────────────────────────────────────────────────────────────┐
│ Add peering                                                 │
├─────────────────────────────────────────────────────────────┤
│ THIS VIRTUAL NETWORK (vnet-hub)                            │
│ ──────────────────────────────────────                    │
│ Peering link name *                                        │
│ [hub-to-prod                                            ]  │
│                                                             │
│ Traffic to remote virtual network                           │
│ ● Allow (default) - Recommended                           │
│ ○ Block traffic                                            │
│                                                             │
│ Traffic forwarded from remote virtual network              │
│ ● Allow (default)                                          │
│ ○ Block traffic                                            │
│   (Allow lets spoke VMs route through hub)                │
│                                                             │
│ Virtual network gateway or Route Server                    │
│ ● Use this virtual network's gateway or Route Server     │
│ ○ None (default)                                          │
│   (Select first option to share gateway with spoke)       │
│                                                             │
│ ─────────────────────────────────────────────────────────  │
│ REMOTE VIRTUAL NETWORK (vnet-spoke-prod)                  │
│ ──────────────────────────────────────                    │
│ Peering link name *                                        │
│ [prod-to-hub                                            ]  │
│                                                             │
│ Virtual network deployment model                           │
│ ● Resource manager (selected)                             │
│                                                             │
│ I know my resource ID                                      │
│ ☐ (leave unchecked)                                       │
│                                                             │
│ Subscription *                                             │
│ [Pay-As-You-Go                                         ▼] │
│                                                             │
│ Virtual network *                                          │
│ [vnet-spoke-prod                                       ▼] │
│                                                             │
│ Traffic to remote virtual network                           │
│ ● Allow (default)                                          │
│                                                             │
│ Traffic forwarded from remote virtual network              │
│ ● Allow (default)                                          │
│                                                             │
│ Virtual network gateway or Route Server                    │
│ ● Use the remote virtual network's gateway or Route Serv..│
│ ○ None (default)                                          │
│   (Spoke will use hub's gateway)                          │
│                                                             │
│          [Add]                          [Cancel]           │
└─────────────────────────────────────────────────────────────┘

Configure THIS VIRTUAL NETWORK (Hub):
  Peering link name: hub-to-prod
  Traffic to remote: ● Allow
  Forwarded traffic: ● Allow
  Gateway: ● Use this virtual network's gateway

Configure REMOTE VIRTUAL NETWORK (Spoke):
  Peering link name: prod-to-hub
  Virtual network: vnet-spoke-prod
  Traffic to remote: ● Allow
  Forwarded traffic: ● Allow
  Gateway: ● Use remote gateway

Click "Add"

Wait 30-60 seconds
```

### **Step 3**: Verify Peering Status

```
Peerings page now shows:

┌──────────────────────────────────────────────────────────────────────┐
│ Peerings                                            [+ Add] [Refresh]│
├──────────────────────────────────────────────────────────────────────┤
│ Name          Peering status  Remote VNet        Remote peering name│
│ ────────────────────────────────────────────────────────────────────  │
│ hub-to-prod   Connected ✓     vnet-spoke-prod    prod-to-hub        │
│                                                                      │
│ ℹ Status "Connected" means peering is active and working          │
└──────────────────────────────────────────────────────────────────────┘

Click on "hub-to-prod" to see details:

┌─────────────────────────────────────────────────────────┐
│ hub-to-prod                                             │
├─────────────────────────────────────────────────────────┤
│ Peering status: Connected ✓                            │
│ Remote virtual network: vnet-spoke-prod                │
│ Remote peering: prod-to-hub                            │
│                                                          │
│ Settings:                                               │
│ • Allow virtual network access: Yes                    │
│ • Allow forwarded traffic: Yes                         │
│ • Allow gateway transit: Yes                           │
│ • Use remote gateways: No                              │
│                                                          │
│ Provisioning state: Succeeded                          │
└─────────────────────────────────────────────────────────┘
```

### **Step 4**: Create Peering to Dev Spoke

```
Repeat steps for development spoke:

1. In vnet-hub → Peerings
2. Click "+ Add"
3. Configure:

  Hub side:
    Name: hub-to-dev
    ● Use gateway
    
  Remote side:
    VNet: vnet-spoke-dev
    Name: dev-to-hub
    ● Use remote gateway

4. Click "Add"

Final peerings list:

┌──────────────────────────────────────────────────────────────────────┐
│ Name          Peering status  Remote VNet        Remote peering name│
│ ────────────────────────────────────────────────────────────────────  │
│ hub-to-prod   Connected ✓     vnet-spoke-prod    prod-to-hub        │
│ hub-to-dev    Connected ✓     vnet-spoke-dev     dev-to-hub         │
└──────────────────────────────────────────────────────────────────────┘

Now all three VNets are connected!
Hub can route traffic between spokes
```

---


## Lab 10: Network Security Groups (NSG) and Application Security Groups (ASG)

## **Task 10.1: Create Application Security Groups**

### **Step 1**: Navigate to Application Security Groups

```
1. Portal search bar → type "Application security groups"
2. Click "Application security groups"
3. Click "+ Create"
```

### **Step 2**: Create Web Tier ASG

```
┌──────────────────────────────────────────────────────────┐
│ Create an application security group                     │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Tags] [Review + create]                       │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: rg-network-lab                          │
│                                                          │
│ INSTANCE DETAILS                                        │
│ Name *                                                  │
│ [asg-web-tier                                       ]   │
│                                                          │
│ Region *                                                │
│ [(US) East US                                       ▼]  │
│                                                          │
│          [Review + create]                              │
└──────────────────────────────────────────────────────────┘

Fill in:
  Resource group: rg-network-lab
  Name: asg-web-tier
  Region: East US

Click "Review + create"
Click "Create"

Deployment completes in seconds
```

### **Step 3**: Create Additional ASGs

```
Repeat for these ASGs:

ASG 2:
  Name: asg-app-tier
  Resource group: rg-network-lab
  Region: East US

ASG 3:
  Name: asg-database-tier
  Resource group: rg-network-lab
  Region: East US

ASG 4:
  Name: asg-management
  Resource group: rg-network-lab
  Region: East US

You should now have 4 ASGs created.
```

### **Step 4**: View All ASGs

```
Application security groups page shows:

┌──────────────────────────────────────────────────────────────────┐
│ Application security groups                    [+ Create] [⟳]   │
├──────────────────────────────────────────────────────────────────┤
│ Name                Resource group    Location                  │
│ ───────────────────────────────────────────────────────────────  │
│ asg-web-tier        rg-network-lab    East US                   │
│ asg-app-tier        rg-network-lab    East US                   │
│ asg-database-tier   rg-network-lab    East US                   │
│ asg-management      rg-network-lab    East US                   │
└──────────────────────────────────────────────────────────────────┘
```

---

## **Task 10.2: Create Network Security Group with ASG Rules**

### **Step 1**: Create NSG

```
1. Portal search → "Network security groups"
2. Click "+ Create"

┌──────────────────────────────────────────────────────────┐
│ Create network security group                           │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Tags] [Review + create]                       │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: rg-network-lab                          │
│                                                          │
│ INSTANCE DETAILS                                        │
│ Name *                                                  │
│ [nsg-prod-app                                       ]   │
│                                                          │
│ Region *                                                │
│ [(US) East US                                       ▼]  │
│                                                          │
│          [Review + create]                              │
└──────────────────────────────────────────────────────────┘

Fill in:
  Resource group: rg-network-lab
  Name: nsg-prod-app
  Region: East US

Click "Review + create"
Click "Create"
Click "Go to resource"
```

### **Step 2**: View Default NSG Rules

```
NSG overview shows default rules:

┌──────────────────────────────────────────────────────────────────┐
│ nsg-prod-app                                                     │
├──────────────────────────────────────────────────────────────────┤
│ [+ Associate] [Refresh] [Delete]                                │
│                                                                  │
│ Left menu:                                                      │
│   Overview                                                      │
│   Inbound security rules ← Click here                          │
│   Outbound security rules                                       │
│   Network interfaces (0)                                        │
│   Subnets (0)                                                   │
└──────────────────────────────────────────────────────────────────┘

Click "Inbound security rules"

Default rules shown:

┌──────────────────────────────────────────────────────────────────────────┐
│ Inbound security rules                            [+ Add] [⟳] [⋮]       │
├──────────────────────────────────────────────────────────────────────────┤
│ Priority  Name            Port  Protocol Source    Dest      Action     │
│ ───────────────────────────────────────────────────────────────────────  │
│ 65000     AllowVnetIn     Any   Any      VirtNet   VirtNet   Allow      │
│ 65001     AllowAzureLB    Any   Any      AzureLB   Any       Allow      │
│ 65500     DenyAllInbound  Any   Any      Any       Any       Deny       │
│                                                                          │
│ ℹ These are default rules (cannot be deleted)                          │
│   Add custom rules with priority 100-4096                              │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## **Task 10.3: Create Inbound Rules Using ASGs**

### **Step 1**: Allow Internet to Web Tier (HTTP/HTTPS)

```
Click "+ Add" button

Add inbound security rule panel opens:

┌─────────────────────────────────────────────────────────┐
│ Add inbound security rule                               │
├─────────────────────────────────────────────────────────┤
│ Source *                                                │
│ [IP Addresses                                       ▼]  │
│   Options: Any, IP Addresses, Service Tag, ASG         │
│                                                          │
│ Select: [Service Tag                                ▼]  │
│                                                          │
│ Source service tag *                                    │
│ [Internet                                           ▼]  │
│                                                          │
│ Source port ranges *                                    │
│ [*                                                  ]   │
│                                                          │
│ Destination *                                           │
│ [Application security group                         ▼]  │
│                                                          │
│ Destination application security groups *               │
│ [+ Add]                                                │
│                                                          │
│ Click [+ Add] to select ASG                            │
└─────────────────────────────────────────────────────────┘

Configure:
  Source: Service Tag
  Source service tag: Internet
  Source port ranges: *
  Destination: Application security group
  
Click "+ Add" under Destination ASGs
Selection panel opens:

┌─────────────────────────────────────────┐
│ Select application security groups      │
├─────────────────────────────────────────┤
│ Search: [                          ] 🔍 │
│                                         │
│ ☐ asg-web-tier                         │
│ ☐ asg-app-tier                         │
│ ☐ asg-database-tier                    │
│ ☐ asg-management                       │
│                                         │
│          [OK]           [Cancel]        │
└─────────────────────────────────────────┘

Check: ☑ asg-web-tier
Click "OK"

Back on main panel:

┌─────────────────────────────────────────────────────────┐
│ Destination application security groups                 │
│ • asg-web-tier                                  [Remove]│
│                                                          │
│ Service                                                 │
│ [HTTP                                               ▼]  │
│   (This auto-fills port 80)                            │
│                                                          │
│ Destination port ranges *                               │
│ [80                                                 ]   │
│   (Read-only when service selected)                    │
│                                                          │
│ Protocol *                                              │
│ [TCP                                                ▼]  │
│   (Auto-selected based on service)                     │
│                                                          │
│ Action *                                                │
│ ● Allow                                                │
│ ○ Deny                                                 │
│                                                          │
│ Priority *                                              │
│ [100                                                ]   │
│   (Lower number = higher priority)                     │
│   (Range: 100-4096)                                    │
│                                                          │
│ Name *                                                  │
│ [AllowInternetToWebHTTP                             ]   │
│                                                          │
│ Description (optional)                                  │
│ [Allow HTTP traffic from Internet to web tier       ]   │
│                                                          │
│          [Add]                    [Cancel]              │
└─────────────────────────────────────────────────────────┘

Fill in:
  Source: Service Tag - Internet
  Source port ranges: *
  Destination: ASG - asg-web-tier
  Service: HTTP
  Destination port ranges: 80 (auto-filled)
  Protocol: TCP
  Action: ● Allow
  Priority: 100
  Name: AllowInternetToWebHTTP
  Description: Allow HTTP from Internet to web tier

Click "Add"

Rule is created (takes 10-15 seconds)
```

### **Step 2**: Add HTTPS Rule

```
Click "+ Add" again

Configure similarly:
  Source: Service Tag - Internet
  Source port ranges: *
  Destination: ASG - asg-web-tier
  Service: HTTPS
  Destination port: 443 (auto-filled)
  Protocol: TCP
  Action: Allow
  Priority: 110
  Name: AllowInternetToWebHTTPS
  Description: Allow HTTPS from Internet to web tier

Click "Add"
```

### **Step 3**: Allow Web Tier to App Tier

```
Click "+ Add"

┌─────────────────────────────────────────────────────────┐
│ Add inbound security rule                               │
├─────────────────────────────────────────────────────────┤
│ Source *                                                │
│ [Application security group                         ▼]  │
│                                                          │
│ Source application security groups *                    │
│ [+ Add]                                                │
│                                                          │
└─────────────────────────────────────────────────────────┘

Click "+ Add", select: ☑ asg-web-tier

Continue configuring:

Source: ASG - asg-web-tier
Source port ranges: *
Destination: ASG - asg-app-tier
Service: Custom
Destination port ranges: 8080
Protocol: TCP
Action: Allow
Priority: 200
Name: AllowWebToApp
Description: Allow web tier to communicate with app tier on port 8080

Click "Add"
```

### **Step 4**: Allow App Tier to Database Tier

```
Click "+ Add"

Configure:
  Source: ASG - asg-app-tier
  Source port ranges: *
  Destination: ASG - asg-database-tier
  Service: Custom
  Destination port ranges: 1433,5432
    (SQL Server and PostgreSQL)
  Protocol: TCP
  Action: Allow
  Priority: 210
  Name: AllowAppToDatabase
  Description: Allow app tier to database tier (SQL and PostgreSQL)

Click "Add"
```

### **Step 5**: Allow Management SSH to All Tiers

```
Click "+ Add"

Configure:
  Source: ASG - asg-management
  Source port ranges: *
  Destination: ASG - Multiple ASGs
    Click "+ Add"
    Select: ☑ asg-web-tier
    Select: ☑ asg-app-tier
    Select: ☑ asg-database-tier
    Click "OK"
  Service: SSH
  Destination port ranges: 22 (auto-filled)
  Protocol: TCP
  Action: Allow
  Priority: 300
  Name: AllowManagementSSH
  Description: Allow SSH from management to all tiers

Click "Add"
```

### **Step 6**: Deny Direct Internet to Database

```
Click "+ Add"

Configure:
  Source: Service Tag - Internet
  Source port ranges: *
  Destination: ASG - asg-database-tier
  Service: Custom
  Destination port ranges: *
  Protocol: Any
  Action: ● Deny (Important!)
  Priority: 400
  Name: DenyInternetToDatabase
  Description: Block all Internet access to database tier

Click "Add"
```

### **Step 7**: View All Inbound Rules

```
Inbound security rules page now shows:

┌──────────────────────────────────────────────────────────────────────────────┐
│ Inbound security rules                                      [+ Add] [⟳]     │
├──────────────────────────────────────────────────────────────────────────────┤
│ Priority Name                  Port    Protocol Source       Dest    Action │
│ ───────────────────────────────────────────────────────────────────────────  │
│ 100      AllowInternetToWebHTTP  80      TCP      Internet      Web     Allow│
│ 110      AllowInternetToWebHTTPS 443     TCP      Internet      Web     Allow│
│ 200      AllowWebToApp           8080    TCP      Web           App     Allow│
│ 210      AllowAppToDatabase      1433... TCP      App           DB      Allow│
│ 300      AllowManagementSSH      22      TCP      Mgmt          All     Allow│
│ 400      DenyInternetToDatabase  *       Any      Internet      DB      Deny │
│ 65000    AllowVnetInBound        Any     Any      VirtualNet    VNet    Allow│
│ 65001    AllowAzureLBInbound     Any     Any      AzureLB       Any     Allow│
│ 65500    DenyAllInbound          Any     Any      Any           Any     Deny │
│                                                                              │
│ Rules evaluated from top to bottom (lowest priority number first)          │
└──────────────────────────────────────────────────────────────────────────────┘

Note the order:
- Custom rules (100-400) evaluated first
- Default rules (65000+) evaluated last
- First matching rule wins
```

---

## **Task 10.4: Associate NSG with Subnet**

### **Step 1**: Navigate to Subnets

```
1. In NSG "nsg-prod-app"
2. Left menu → click "Subnets"
3. You'll see:

┌──────────────────────────────────────────────────────────┐
│ Subnets                                                  │
├──────────────────────────────────────────────────────────┤
│ [+ Associate]  [Dissociate]                             │
│                                                          │
│ No subnets associated                                   │
│                                                          │
│ ℹ Associate NSG with subnets to apply rules            │
└──────────────────────────────────────────────────────────┘

Click "+ Associate"
```

### **Step 2**: Select Subnet

```
Associate subnet panel:

┌─────────────────────────────────────────┐
│ Associate subnet                        │
├─────────────────────────────────────────┤
│ Virtual network *                       │
│ [Select a virtual network           ▼] │
│                                         │
│ Dropdown shows:                        │
│ ○ vnet-hub                             │
│ ○ vnet-spoke-prod                      │
│ ○ vnet-spoke-dev                       │
└─────────────────────────────────────────┘

Select: vnet-spoke-prod

Subnet dropdown appears:

┌─────────────────────────────────────────┐
│ Subnet *                                │
│ [Select a subnet                    ▼] │
│                                         │
│ ○ subnet-web                           │
│ ○ subnet-app                           │
│ ○ subnet-data                          │
└─────────────────────────────────────────┘

Select: subnet-web

┌─────────────────────────────────────────┐
│ Associate subnet                        │
├─────────────────────────────────────────┤
│ Virtual network: vnet-spoke-prod ✓     │
│ Subnet: subnet-web ✓                   │
│                                         │
│          [OK]           [Cancel]        │
└─────────────────────────────────────────┘

Click "OK"

Association completes in 5-10 seconds
```

### **Step 3**: Associate Additional Subnets

```
Repeat to associate:
  - subnet-app
  - subnet-data

Final subnet associations:

┌──────────────────────────────────────────────────────────────────┐
│ Subnets                                       [+ Associate]      │
├──────────────────────────────────────────────────────────────────┤
│ Virtual network     Subnet          Address range               │
│ ─────────────────────────────────────────────────────────────── │
│ vnet-spoke-prod     subnet-web      10.20.1.0/24                │
│ vnet-spoke-prod     subnet-app      10.20.2.0/24                │
│ vnet-spoke-prod     subnet-data     10.20.3.0/24                │
│                                                                  │
│ ✓ NSG rules now apply to all VMs in these subnets              │
└──────────────────────────────────────────────────────────────────┘
```

---

## **Task 10.5: Create VMs and Assign to ASGs**

### **Step 1**: Create Web Server VM

```
1. Virtual machines → + Create
2. Basics tab:

Resource group: rg-network-lab
VM name: vm-web-01
Region: East US
Availability options: No infrastructure redundancy
Image: Ubuntu Server 22.04 LTS
Size: Standard_B2s
Username: azureuser
SSH: Generate new key pair

3. Networking tab:

Virtual network: vnet-spoke-prod
Subnet: subnet-web
Public IP: (new) vm-web-01-ip
NIC NSG: None (we're using subnet NSG)
Select inbound ports: None (controlled by NSG)

4. Create VM

Wait for deployment
```

### **Step 2**: Assign VM to Web ASG

```
1. Go to VM "vm-web-01"
2. Left menu → Networking → Network settings
3. Click on the NIC name (e.g., "vm-web-01VMNic")

NIC page opens:

┌──────────────────────────────────────────────────────────┐
│ vm-web-01VMNic                                          │
├──────────────────────────────────────────────────────────┤
│ Left menu:                                              │
│   Overview                                              │
│   IP configurations                                     │
│   DNS servers                                           │
│   Application security groups ← Click here             │
│   Network security group                                │
└──────────────────────────────────────────────────────────┘

Click "Application security groups"

┌──────────────────────────────────────────────────────────┐
│ Application security groups                              │
├──────────────────────────────────────────────────────────┤
│ [Configure application security groups]                 │
│                                                          │
│ Associated application security groups:                 │
│ None                                                    │
└──────────────────────────────────────────────────────────┘

Click "Configure application security groups"

Panel opens:

┌─────────────────────────────────────────┐
│ Configure application security groups   │
├─────────────────────────────────────────┤
│ Select application security groups      │
│                                         │
│ ☐ asg-web-tier                         │
│ ☐ asg-app-tier                         │
│ ☐ asg-database-tier                    │
│ ☐ asg-management                       │
│                                         │
│          [Save]         [Cancel]        │
└─────────────────────────────────────────┘

Check: ☑ asg-web-tier
Click "Save"

Notification: ✓ Successfully updated application security groups
```

### **Step 3**: Create App Server and Assign to ASG

```
Create VM:
  Name: vm-app-01
  VNet: vnet-spoke-prod
  Subnet: subnet-app
  Public IP: None
  NIC NSG: None

After creation:
  Go to NIC → Application security groups
  Assign: ☑ asg-app-tier
  Save
```

### **Step 4**: Create Database Server and Assign to ASG

```
Create VM:
  Name: vm-db-01
  VNet: vnet-spoke-prod
  Subnet: subnet-data
  Public IP: None
  NIC NSG: None

After creation:
  Go to NIC → Application security groups
  Assign: ☑ asg-database-tier
  Save
```

### **Step 5**: Create Jump Box and Assign to ASG

```
Create VM:
  Name: vm-jumpbox
  VNet: vnet-hub
  Subnet: subnet-management
  Public IP: (new) vm-jumpbox-ip
  NIC NSG: None

After creation:
  Go to NIC → Application security groups
  Assign: ☑ asg-management
  Save
```

---

## **Task 10.6: Test Network Security Rules**

### **Step 1**: Test Internet to Web Server (Should Work)

```
1. Get public IP of vm-web-01:
   Go to VM → Overview
   Note: Public IP address: 52.168.45.30

2. SSH to vm-web-01 from your computer:

   (Make sure port 22 is allowed temporarily or use Azure Bastion)
   
   Install NGINX on vm-web-01:
   ssh -i vm-web-01_key.pem azureuser@52.168.45.30
   sudo apt update
   sudo apt install nginx -y
   sudo systemctl start nginx

3. Test HTTP access from browser:
   http://52.168.45.30
   
   ✓ Should work - Rule 100 allows Internet → Web tier on port 80
```

### **Step 2**: Test Jump Box SSH to Database (Should Work)

```
1. Get private IP of vm-db-01:
   Go to VM → Overview
   Private IP: 10.20.3.4

2. SSH to jump box:
   ssh -i vm-jumpbox_key.pem azureuser@[jumpbox-public-ip]

3. From jump box, SSH to database:
   ssh azureuser@10.20.3.4
   
   ✓ Should work - Rule 300 allows Management ASG → All tiers on port 22
```

### **Step 3**: Test Internet Direct to Database (Should Fail)

```
From your computer, try to connect to database private IP:
(This would only work if you had VPN, but conceptually...)

The NSG rule 400 (DenyInternetToDatabase) blocks this at priority 400
Even before the default DenyAllInbound at 65500

✓ Database is protected from Internet
```

### **Step 4**: View NSG Flow Logs (Optional - Advanced)

```
1. Go to NSG "nsg-prod-app"
2. Left menu → Monitoring → NSG flow logs
3. Click "+ Create"

┌──────────────────────────────────────────────────────────┐
│ Create a flow log                                        │
├──────────────────────────────────────────────────────────┤
│ Select NSG: nsg-prod-app (already selected)             │
│                                                          │
│ Storage account:                                         │
│ [+ Create new storage account]                          │
│                                                          │
│ Retention (days): 7                                     │
│                                                          │
│ Version: Version 2                                       │
│                                                          │
│ Traffic Analytics (optional): Enabled                   │
│ Processing interval: 10 minutes                         │
│                                                          │
│          [Review + create]                              │
└──────────────────────────────────────────────────────────┘

This logs all traffic allowed/denied by NSG
Useful for troubleshooting and security auditing
```

---

# Azure VPN Gateway - Portal Lab

## Lab 11: Create Site-to-Site VPN Gateway

## **Task 11.1: Create VPN Gateway**

### **Step 1**: Navigate to Virtual Network Gateways

```
1. Portal search → "Virtual network gateways"
2. Click "+ Create"
```

### **Step 2**: Configure Gateway Basics

```
┌──────────────────────────────────────────────────────────┐
│ Create virtual network gateway                           │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Tags] [Review + create]                       │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: rg-network-lab                          │
│                                                          │
│ INSTANCE DETAILS                                        │
│ Name *                                                  │
│ [vpngw-hub                                          ]   │
│                                                          │
│ Region *                                                │
│ [(US) East US                                       ▼]  │
│   (Must match VNet region)                             │
│                                                          │
│ Gateway type *                                          │
│ ● VPN                                                  │
│ ○ ExpressRoute                                         │
│                                                          │
│ SKU *                                                   │
│ [VpnGw1                                             ▼]  │
│                                                          │
│   SKU Options:                                          │
│   ┌──────────────────────────────────────────────────┐ │
│   │ SKU      Throughput  Tunnels  Cost/month        │ │
│   │ VpnGw1   650 Mbps    30       ~$140             │ │
│   │ VpnGw2   1 Gbps      30       ~$360             │ │
│   │ VpnGw3   1.25 Gbps   30       ~$1,105           │ │
│   │ Basic    100 Mbps    10       ~$27 (legacy)     │ │
│   └──────────────────────────────────────────────────┘ │
│                                                          │
│ Generation *                                            │
│ [Generation 1                                       ▼]  │
│   (Gen2 supports higher performance)                   │
│                                                          │
│ Virtual network *                                       │
│ [vnet-hub                                           ▼]  │
│   (Must have GatewaySubnet)                            │
│                                                          │
│ Gateway subnet address range                            │
│ 10.10.0.0/24 (read-only, from GatewaySubnet)          │
│                                                          │
└──────────────────────────────────────────────────────────┘

Fill in:
  Resource group: rg-network-lab
  Name: vpngw-hub
  Region: East US
  Gateway type: ● VPN
  SKU: VpnGw1
  Generation: Generation 1
  Virtual network: vnet-hub

Click "Next: IP address" (or scroll down)
```

### **Step 3**: Configure Public IP Address

```
┌──────────────────────────────────────────────────────────┐
│ PUBLIC IP ADDRESS                                        │
│ ─────────────────                                       │
│ Public IP address type *                                │
│ ● Standard (Zone-redundant recommended)                │
│ ○ Basic                                                 │
│                                                          │
│ Public IP address *                                     │
│ ● Create new                                           │
│ ○ Use existing                                         │
│                                                          │
│ Public IP address name *                                │
│ [vpngw-hub-pip                                      ]   │
│                                                          │
│ Public IP address SKU                                   │
│ Standard (read-only)                                    │
│                                                          │
│ Assignment                                              │
│ Static (read-only)                                      │
│                                                          │
│ Enable active-active mode                               │
│ ☐ Enabled                                              │
│   (Requires 2 public IPs for redundancy)               │
│   (Adds cost)                                          │
│                                                          │
│ Configure BGP                                           │
│ ☐ Enabled                                              │
│   (Border Gateway Protocol for dynamic routing)        │
│                                                          │
│ ⚠ WARNING: Gateway creation takes 30-45 minutes!      │
└──────────────────────────────────────────────────────────┘

Configure:
  Public IP address type: ● Standard
  Public IP address: ● Create new
  Public IP address name: vpngw-hub-pip
  Enable active-active: ☐ (unchecked for single gateway)
  Configure BGP: ☐ (unchecked unless needed)

Click "Review + create"
```

### **Step 4**: Review and Create

```
Review page shows:

┌──────────────────────────────────────────────────────────┐
│ Validation passed ✓                                     │
├──────────────────────────────────────────────────────────┤
│ Gateway type: VPN                                       │
│ VPN type: Route-based                                   │
│ SKU: VpnGw1                                            │
│ Generation: Generation1                                 │
│ Virtual network: vnet-hub                               │
│ Gateway subnet: 10.10.0.0/24                           │
│ Public IP: vpngw-hub-pip (new)                         │
│                                                          │
│ Estimated monthly cost: ~$140                           │
│                                                          │
│ ⚠ Deployment time: 30-45 minutes                      │
│                                                          │
│          [< Previous]              [Create]             │
└──────────────────────────────────────────────────────────┘

Click "Create"

Deployment begins - this is a good time for a coffee break! ☕
```

### **Step 5**: Monitor Deployment Progress

```
Deployment screen:

┌──────────────────────────────────────────────────────────┐
│ Deployment in progress                                   │
├──────────────────────────────────────────────────────────┤
│ Deployment name: Microsoft.VirtualNetworkGateway...     │
│ Resource group: rg-network-lab                          │
│ Start time: 12/30/2024, 5:00:00 PM                     │
│                                                          │
│ Deployment details:                                      │
│ ✓ Microsoft.Network/publicIPAddresses                   │
│ ⟳ Microsoft.Network/virtualNetworkGateways             │
│                                                          │
│ Status: Running                                         │
│ Elapsed time: 15 minutes                               │
│ Estimated remaining: 15-30 minutes                      │
│                                                          │
│ You can navigate away - deployment continues           │
│ Check notifications (🔔) for completion                │
└──────────────────────────────────────────────────────────┘

Wait 30-45 minutes... ⏰

When complete:
┌──────────────────────────────────────────────────────────┐
│ ✓ Your deployment is complete                           │
│ Completion time: 12/30/2024, 5:37:25 PM                │
│ Duration: 37 minutes 25 seconds                         │
└──────────────────────────────────────────────────────────┘

Click "Go to resource"
```

---

## **Task 11.2: Create Local Network Gateway (Simulates On-Premises)**

### **Step 1**: Navigate to Local Network Gateways

```
1. Portal search → "Local network gateways"
2. Click "+ Create"
```

### **Step 2**: Configure Local Network Gateway

```
┌──────────────────────────────────────────────────────────┐
│ Create local network gateway                            │
├──────────────────────────────────────────────────────────┤
│ [Basics] [Tags] [Review + create]                       │
│  ──────                                                 │
│                                                          │
│ PROJECT DETAILS                                         │
│ Subscription: Pay-As-You-Go                            │
│ Resource group: rg-network-lab                          │
│                                                          │
│ INSTANCE DETAILS                                        │
│ Region *                                                │
│ [(US) East US                                       ▼]  │
│                                                          │
│ Name *                                                  │
│ [lng-onpremises                                     ]   │
│                                                          │
│ Endpoint *                                              │
│ ● IP address                                           │
│ ○ FQDN (Fully qualified domain name)                  │
│                                                          │
│ IP address *                                            │
│ [203.0.113.10                                       ]   │
│   (Simulated on-premises VPN device public IP)        │
│   (Use a real IP if you have on-prem device)          │
│                                                          │
│ Address space(s) *                                      │
│ [192.168.0.0/16                                     ]   │
│ [+ Add additional address range]                       │
│                                                          │
│   This represents on-premises network address space    │
│   Multiple ranges can be added                         │
│                                                          │
│ Configure BGP settings                                  │
│ ☐ (leave unchecked for static routing)                │
│                                                          │
│          [Review + create]                              │
└──────────────────────────────────────────────────────────┘

Fill in:
  Resource group: rg-network-lab
  Region: East US
  Name: lng-onpremises
  Endpoint: ● IP address
  IP address: 203.0.113.10 (example - use your real on-prem IP)
  Address space: 192.168.0.0/16 (your on-premises network)

Click "Review + create"
Click "Create"

Deployment completes in seconds
Click "Go to resource"
```

---

## **Task 11.3: Create VPN Connection**

### **Step 1**: Navigate to Connections

```
1. Go to VPN Gateway "vpngw-hub"
2. Left menu → Settings → Connections
3. You'll see:

┌──────────────────────────────────────────────────────────┐
│ Connections                                              │
├──────────────────────────────────────────────────────────┤
│ [+ Add]  [Refresh]  [Reset]                             │
│                                                          │
│ No connections configured                               │
│                                                          │
│ ℹ Create a connection to link VPN Gateway to          │
│   Local Network Gateway (site-to-site) or              │
│   Another VPN Gateway (vnet-to-vnet)                   │
└──────────────────────────────────────────────────────────┘

Click "+ Add"
```

### **Step 2**: Configure Connection

```
Add connection panel opens:

┌─────────────────────────────────────────────────────────┐
│ Add connection                                          │
├─────────────────────────────────────────────────────────┤
│ Name *                                                  │
│ [conn-hub-to-onpremises                             ]   │
│                                                          │
│ Connection type *                                       │
│ [Site-to-site (IPsec)                               ▼]  │
│   Options:                                              │
│   • Site-to-site (IPsec) - to on-premises             │
│   • VNet-to-VNet - to another Azure VPN Gateway       │
│   • ExpressRoute - to ExpressRoute circuit            │
│                                                          │
│ Virtual network gateway *                               │
│ vpngw-hub (read-only, pre-selected)                    │
│                                                          │
│ Local network gateway *                                 │
│ [lng-onpremises                                     ▼]  │
│                                                          │
│ Shared key (PSK) *                                      │
│ [●●●●●●●●●●●●●●●●●●●●                               ]   │
│   Min 8 chars                                          │
│   (Must match on-premises VPN device)                  │
│                                                          │
│ Enter: AzureVPNSharedKey2024!Secure                    │
│ (Use strong key in production!)                        │
│                                                          │
│ IKE Protocol *                                          │
│ [IKEv2                                              ▼]  │
│   (IKEv2 recommended over IKEv1)                       │
│                                                          │
│ Enable BGP                                              │
│ ☐ (unchecked for static routing)                      │
│                                                          │
│ Use policy based traffic selector                      │
│ ☐ (unchecked - route-based is default)                │
│                                                          │
│ CONNECTION MODE                                         │
│ Default (recommended)                                   │
│                                                          │
│          [OK]                    [Cancel]               │
└─────────────────────────────────────────────────────────┘

Fill in:
  Name: conn-hub-to-onpremises
  Connection type: Site-to-site (IPsec)
  Virtual network gateway: vpngw-hub (pre-selected)
  Local network gateway: lng-onpremises
  Shared key: AzureVPNSharedKey2024!Secure
  IKE Protocol: IKEv2
  Enable BGP: ☐ (unchecked)

Click "OK"

Connection is created in 1-2 minutes
```

### **Step 3**: Monitor Connection Status

```
Connections page now shows:

┌──────────────────────────────────────────────────────────────────────┐
│ Connections                                        [+ Add] [Refresh] │
├──────────────────────────────────────────────────────────────────────┤
│ Name                    Type          Status      Egress   Ingress  │
│ ───────────────────────────────────────────────────────────────────  │
│ conn-hub-to-onpremises  Site-to-site  Not connected  0 B    0 B     │
│                         (IPsec)       ⚠                              │
│                                                                      │
│ ⚠ Status "Not connected" is normal - on-premises side must be      │
│   configured with matching settings for tunnel to establish        │
└──────────────────────────────────────────────────────────────────────┘

Click on "conn-hub-to-onpremises" for details
```

### **Step 4**: View Connection Details

```
┌──────────────────────────────────────────────────────────┐
│ conn-hub-to-onpremises                                   │
├──────────────────────────────────────────────────────────┤
│ ESSENTIALS                                              │
│ Connection status: Not connected ⚠                     │
│ Connection type: Site-to-site (IPsec)                  │
│ Resource group: rg-network-lab                          │
│ Location: East US                                       │
│                                                          │
│ CONFIGURATION                                           │
│ Virtual network gateway: vpngw-hub                      │
│ Local network gateway: lng-onpremises                   │
│ Shared key: ●●●●●●●●●●●●●●●●                          │
│   [Show] to reveal                                     │
│ IKE Protocol: IKEv2                                    │
│                                                          │
│ DATA TRANSFER                                           │
│ Ingress: 0 B                                           │
│ Egress: 0 B                                            │
│                                                          │
│ ⓘ To establish connection:                            │
│   1. Configure on-premises VPN device with:           │
│      - Azure VPN Gateway public IP                    │
│      - Shared key (PSK)                               │
│      - IKEv2 protocol                                 │
│      - IPsec/IKE parameters                           │
│   2. Connection will show "Connected" when tunnel up  │
└──────────────────────────────────────────────────────────┘
```

---

## **Task 11.4: Download VPN Device Configuration**

### **Step 1**: Get Configuration Script

```
1. In connection "conn-hub-to-onpremises"
2. Click "Download configuration" at the top

Panel opens:

┌─────────────────────────────────────────────────────────┐
│ Download configuration                                  │
├─────────────────────────────────────────────────────────┤
│ Select your on-premises VPN device:                    │
│                                                          │
│ Device vendor *                                         │
│ [Cisco                                              ▼]  │
│                                                          │
│ Device family *                                         │
│ [ASA                                                ▼]  │
│                                                          │
│ Firmware version *                                      │
│ [8.4 and later                                      ▼]  │
│                                                          │
│ ℹ Azure will generate configuration script for your   │
│   specific device including:                           │
│   • Azure VPN Gateway public IP                       │
│   • Shared key                                        │
│   • IPsec parameters                                  │
│   • Sample configuration commands                     │
│                                                          │
│          [Download]              [Cancel]               │
└─────────────────────────────────────────────────────────┘

Select your device (example: Cisco ASA)
Click "Download"

A .txt file downloads with configuration:

Example content:
═══════════════════════════════════════════════════
Azure VPN Gateway Configuration
Device: Cisco ASA 8.4+
═══════════════════════════════════════════════════

Azure VPN Gateway Public IP: 52.170.123.45
Shared Key: AzureVPNSharedKey2024!Secure

Configuration commands:

crypto ikev2 policy 1
  encryption aes-256
  integrity sha256
  group 2
  prf sha256
  lifetime seconds 28800

crypto ikev2 keyring azure-keyring
  peer 52.170.123.45
    address 52.170.123.45
    pre-shared-key AzureVPNSharedKey2024!Secure

[... more configuration ...]
═══════════════════════════════════════════════════

Apply this configuration to your on-premises VPN device
```

---

## **Task 11.5: Configure Point-to-Site VPN (Remote Users)**

### **Step 1**: Generate Certificates (Self-signed for Testing)

```
Note: In production, use proper CA-signed certificates
For lab, we'll use Azure Cloud Shell to generate test certs

1. Open Cloud Shell (PowerShell mode)
2. Run these commands:

# Create root certificate
$rootCert = New-SelfSignedCertificate `
  -Type Custom `
  -Subject "CN=AzureVPNRootCert" `
  -KeySpec Signature `
  -KeyExportPolicy Exportable `
  -KeyAlgorithm RSA `
  -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -KeyUsageProperty Sign `
  -KeyUsage CertSign `
  -HashAlgorithm SHA256

# Export public key (base64)
$rootCertData = [Convert]::ToBase64String($rootCert.RawData)
$rootCertData | Out-File rootcert.txt

# Display for copying
Write-Host $rootCertData

Copy the output (long base64 string)
```

### **Step 2**: Configure Point-to-Site on VPN Gateway

```
1. Go to VPN Gateway "vpngw-hub"
2. Left menu → Settings → Point-to-site configuration
3. You'll see:

┌──────────────────────────────────────────────────────────┐
│ Point-to-site configuration                              │
├──────────────────────────────────────────────────────────┤
│ Point-to-site not configured                            │
│                                                          │
│ [Configure now]                                         │
└──────────────────────────────────────────────────────────┘

Click "Configure now"
```

### **Step 3**: Configure P2S Settings

```
Configuration page opens:

┌──────────────────────────────────────────────────────────┐
│ Point-to-site configuration                              │
├──────────────────────────────────────────────────────────┤
│ Address pool *                                           │
│ [172.16.0.0/24                                       ]   │
│ [+ Add address pool]                                    │
│                                                          │
│   ℹ This range is for VPN clients                      │
│     Must not overlap with Azure VNet or on-premises    │
│                                                          │
│ Tunnel type *                                            │
│ ☑ IKEv2                                                │
│ ☑ OpenVPN (SSL)                                        │
│                                                          │
│   ℹ IKEv2: Native Windows, macOS, iOS                 │
│     OpenVPN: Cross-platform, more firewall-friendly   │
│                                                          │
│ Authentication type *                                    │
│ ☑ Azure certificate                                    │
│ ☐ RADIUS                                               │
│ ☐ Azure Active Directory                              │
│                                                          │
│ ROOT CERTIFICATES                                        │
│ ─────────────────                                       │
│ Name *                                                  │
│ [VPNRootCert                                        ]   │
│                                                          │
│ Public certificate data *                               │
│ ┌────────────────────────────────────────────────────┐  │
│ │ Paste base64 certificate data here                │  │
│ │ (from rootcert.txt, without headers/footers)       │  │
│ │                                                     │  │
│ │ MIIDGzCCAgOgAwIBAgIQJWBwl2dZLq...                │  │
│ │ ...                                                 │  │
│ │ ...W3HKm5dXtjwQaVF9K6Hqw==                        │  │
│ └────────────────────────────────────────────────────┘  │
│                                                          │
│ [+ Add root certificate]                                │
│                                                          │
│ REVOKED CERTIFICATES (optional)                         │
│ None configured                                         │
│                                                          │
│          [Save]                   [Discard]             │
└──────────────────────────────────────────────────────────┘

Fill in:
  Address pool: 172.16.0.0/24
  Tunnel type: ☑ IKEv2 and ☑ OpenVPN (both)
  Authentication: ☑ Azure certificate
  Root certificate name: VPNRootCert
  Public certificate data: [Paste your base64 cert]

Click "Save"

Configuration takes 5-10 minutes to apply
```

### **Step 4**: Download VPN Client

```
After configuration completes:

1. Point-to-site configuration page shows:
   Status: Configured ✓

2. Click "Download VPN client" button at top

┌─────────────────────────────────────────┐
│ Download VPN client                     │
├─────────────────────────────────────────┤
│ Select platform:                        │
│                                         │
│ ○ Windows AMD64                        │
│ ○ Windows ARM64                        │
│ ○ Mac (OpenVPN)                        │
│ ○ Linux (OpenVPN)                      │
│                                         │
│          [Download]      [Cancel]       │
└─────────────────────────────────────────┘

Select your platform
Click "Download"

A ZIP file downloads containing:
  - VPN client installers
  - Configuration files
  - Connection profiles
```

### **Step 5**: Install and Connect VPN Client

```
On Windows:

1. Extract downloaded ZIP
2. Navigate to: WindowsAmd64 folder
3. Run: VpnClientSetup.exe
4. Follow installation wizard
5. After install:
   - Open Windows Settings → Network → VPN
   - You'll see: "vnet-hub-vpngw" (or similar name)
   - Click "Connect"

6. Connection establishes:
   ✓ Connected to Azure VNet
   Your IP on VPN: 172.16.0.10

7. Test connectivity:
   - Open Command Prompt
   - ping 10.10.2.4 (VM in Azure)
   - Should work! ✓

8. View connection details:
   - Duration: 00:15:32
   - Bytes sent: 45,234
   - Bytes received: 123,456
```

---

## **Task 11.6: Monitor and Troubleshoot VPN**

### **Step 1**: View VPN Metrics

```
1. Go to VPN Gateway "vpngw-hub"
2. Left menu → Monitoring → Metrics

┌──────────────────────────────────────────────────────────────────┐
│ Metrics                                                          │
├──────────────────────────────────────────────────────────────────┤
│ Scope: vpngw-hub                                                │
│ Time range: [Last 24 hours ▼]                                  │
│                                                                  │
│ Metric: [Select a metric...                                ▼]  │
│                                                                  │
│ Available metrics:                                              │
│ • P2S Connection Count                                          │
│ • Gateway S2S Bandwidth                                         │
│ • Tunnel Bandwidth                                              │
│ • Tunnel Ingress                                                │
│ • Tunnel Egress                                                 │
│ • Tunnel Ingress Packet Drop Count                             │
│ • Tunnel Egress Packet Drop Count                              │
└──────────────────────────────────────────────────────────────────┘

Select: P2S Connection Count

Chart shows:
┌──────────────────────────────────────────────────────────┐
│ P2S Connection Count                                     │
│                                                          │
│ 10  ┤              ╭─────╮                              │
│  8  ┤           ╭──╯     ╰──╮                          │
│  6  ┤       ╭───╯           ╰───╮                      │
│  4  ┤    ╭──╯                   ╰──╮                   │
│  2  ┤────╯                          ╰────              │
│  0  └────────────────────────────────────              │
│     9AM  10AM  11AM  12PM  1PM   2PM  3PM             │
│                                                          │
│ Current: 8 connections                                  │
│ Average: 6.2 connections                                │
│ Peak: 10 connections                                    │
└──────────────────────────────────────────────────────────┘
```

### **Step 2**: View Connection Status Details

```
1. VPN Gateway → Settings → Connections
2. Click on "conn-hub-to-onpremises"
3. Left menu → Monitoring → Connection monitor

If connected:

┌──────────────────────────────────────────────────────────┐
│ Connection monitor                                       │
├──────────────────────────────────────────────────────────┤
│ Status: ● Connected                                     │
│ Connection established: 12/30/2024, 6:15:32 PM         │
│ Uptime: 2 hours 15 minutes                             │
│                                                          │
│ TRAFFIC STATISTICS                                      │
│ Ingress: 156.3 MB                                       │
│ Egress: 89.7 MB                                         │
│ Total: 246.0 MB                                         │
│                                                          │
│ TUNNEL STATUS                                           │
│ IKE Phase 1: Established ✓                            │
│ IPsec Phase 2: Established ✓                          │
│ Last negotiation: 12/30/2024, 8:30:15 PM              │
└──────────────────────────────────────────────────────────┘
```

### **Step 3**: Troubleshoot Connection Issues

```
If status shows "Not connected":

1. Click "Reset" on the connection
2. Wait 2-3 minutes
3. Check these common issues:

   ☐ On-premises VPN device configured correctly?
   ☐ Shared key matches exactly?
   ☐ On-premises device can reach Azure public IP?
   ☐ Firewall allowing UDP ports 500, 4500?
   ☐ NAT traversal enabled if behind NAT?

4. View logs:
   Left menu → Monitoring → Logs
   
   Query example:
   AzureDiagnostics
   | where ResourceType == "VIRTUALNETWORKGATEWAYS"
   | where Category == "TunnelDiagnosticLog"
   | order by TimeGenerated desc
   | take 50

5. Common error messages and solutions:
   
   Error: "IKE authentication failed"
   → Check shared key matches exactly
   
   Error: "No proposal chosen"
   → Check IPsec/IKE policy compatibility
   
   Error: "Peer not responding"
   → Check on-premises device is online and reachable
```

---

## **Summary: What We've Built**

```
┌────────────────────────────────────────────────────────────────┐
│                    AZURE NETWORK ARCHITECTURE                  │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│                         INTERNET                               │
│                            ↕                                   │
│                    ┌──────────────┐                           │
│                    │  VPN Gateway  │                           │
│                    │  (vpngw-hub)  │                           │
│                    └──────┬───────┘                           │
│                           │                                    │
│              ┌────────────┼────────────┐                      │
│              │        HUB VNET          │                      │
│              │    (10.10.0.0/16)       │                      │
│              │  ┌──────────────────┐   │                      │
│              │  │ Jump Box         │   │                      │
│              │  │ (Management)     │   │                      │
│              │  └──────────────────┘   │                      │
│              └────────┬────────┬────────┘                     │
│                       │ Peering │                             │
│           ┌───────────┘        └────────────┐                │
│           │                                  │                │
│    ┌──────┴────────┐              ┌─────────┴────────┐      │
│    │  PROD SPOKE   │              │   DEV SPOKE      │      │
│    │ 10.20.0.0/16  │              │  10.30.0.0/16    │      │
│    │               │              │                  │      │
│    │ ┌───────────┐ │              │  ┌────────────┐  │      │
│    │ │Web Servers│ │              │  │Dev VMs     │  │      │
│    │ │(ASG: Web) │ │              │  └────────────┘  │      │
│    │ └───────────┘ │              │                  │      │
│    │       ↕ NSG   │              └──────────────────┘      │
│    │ ┌───────────┐ │                                        │
│    │ │App Servers│ │                                        │
│    │ │(ASG: App) │ │              ON-PREMISES               │
│    │ └───────────┘ │              ────────────              │
│    │       ↕ NSG   │              192.168.0.0/16            │
│    │ ┌───────────┐ │                   ↕                    │
│    │ │  Database │ │              (Site-to-Site VPN)        │
│    │ │(ASG: DB)  │ │                                        │
│    │ └───────────┘ │                                        │
│    └───────────────┘              REMOTE WORKERS            │
│                                    ──────────────            │
│    NSG Rules:                      172.16.0.0/24            │
│    • Internet → Web (80, 443)          ↕                    │
│    • Web → App (8080)             (Point-to-Site VPN)       │
│    • App → DB (1433, 5432)                                  │
│    • Mgmt → All (22)                                        │
│    • Internet ✗ DB (Denied)                                 │
│                                                              │
└────────────────────────────────────────────────────────────────┘
```

---

## **Final Verification Checklist**

```
✓ Hub-Spoke VNet topology created
✓ VNet peering configured (hub-to-spokes)
✓ Application Security Groups created (Web, App, DB, Mgmt)
✓ Network Security Group with ASG-based rules
✓ NSG associated with production subnets
✓ VMs deployed and assigned to appropriate ASGs
✓ VPN Gateway deployed in hub VNet
✓ Site-to-Site VPN connection configured
✓ Point-to-Site VPN configured for remote users
✓ Network security rules tested and verified
✓ VPN monitoring and metrics configured
```

