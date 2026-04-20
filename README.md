Ceph Overview
## 🔹 Ceph Function
Ceph is a distributed storage system where data is stored across multiple disks (OSDs) intelligently.
👉 **Flow:**  
Split → Distribute → Replicate → Reassemble
### 🔹 Meaning (Quick Recall)
- **Split** → Break file into pieces  
- **Distribute** → Store pieces on different OSDs  
- **Replicate** → Create copies for safety  
- **Reassemble** → Combine pieces during read  
### 🔹 Example
A 1GB file is split into multiple pieces and stored across different OSDs. Copies are also created.  
When reading, Ceph combines all pieces into a single file.  
Even if one OSD fails, data is still available from other copies.
---
# 🏬 Key Concepts
## 🔹 PG (Placement Group)
PG is a logical grouping of objects to manage data efficiently.

### 🛒 Mall Example
| Real Life | Ceph |
| Mall      | Cluster |
| Shop      | PG |
| Items     | Objects |
| Storage room | OSD |
| Mall manager | CRUSH |
---
## 🔹 CRUSH Algorithm
CRUSH decides where to store data.

### 🧠 Example
You buy an item → Manager decides storage  
- Room1  
- Room2 (copy)  
- Room3 (copy)  

✔ Data is distributed across multiple locations  
❌ Not stored in a single place  
---
# ⚙️ Ceph Components

## 1. MON (Monitor)
- Brain of Ceph  
- Tracks cluster health  
- Knows which OSD is up/down  

### Example
If OSD2 fails:
- MON detects failure  
- Updates cluster map  
- Triggers recovery  

👉 `ceph -s` talks to MON  

### Quorum
- 3 MONs → need 2  
- 5 MONs → need 3  

❗ Without quorum → cluster unavailable  
---
## 2. OSD (Object Storage Daemon)
- Stores actual data  
- Handles read/write  
💡 Think: Disk / worker  
---
## 3. MGR (Manager)
Provides monitoring and visibility.
- Dashboard  
- Metrics (CPU, Disk, IOPS)  
- Alerts  
👉 Integrates with Prometheus & Grafana  
---
## 4. MDS (Metadata Server)
Handles metadata (CephFS only)
### Difference
- Data → stored in OSD  
- Metadata → handled by MDS  
### Example
File: `/home/suresh/report.txt`
- Where file exists  
- Folder structure  
- Permissions  
💡 MDS manages this information  
### Library Example
- Books → OSD  
- Catalog → MDS  
---
# 🔄 Full Ceph Flow (Mall Example)

## 🎯 Mapping with Mall
| RealLife | Ceph |
| Customer | Pod |
| Request  | PVC |
| Entry    | CSI |
| Backend  | RADOS |
| Items    | Objects |
| Sections | PG |
| Manager  | CRUSH |
| Rooms    | OSD |
| Security | MON |
---
## 🚀 Step-by-Step Flow
1. Pod sends request (PVC)  
2. CSI receives request  
3. RADOS processes data  
4. Data is split into objects  
5. Objects grouped into PGs  
6. CRUSH decides placement  
7. Stored in OSDs  
8. MON tracks everything  
---
## 🔁 Read Flow
1. Find PG  
2. Locate OSDs  
3. Collect data  
4. Reassemble  
5. Return file  
---
## 🧠 Cluster Health status
ceph health detail
ceph -s
