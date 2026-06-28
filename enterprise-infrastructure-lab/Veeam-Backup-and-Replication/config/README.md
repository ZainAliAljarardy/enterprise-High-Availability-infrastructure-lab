
# Veeam Backup & Replication: Enterprise Backup Infrastructure & Disaster Recovery

This section documents the end-to-end deployment of **Veeam Backup & Replication** to orchestrate centralized backups, immutable storage retention, and granular object recovery across the enterprise environment.

---

## 🛠️ Phase 1: Veeam Core Installation & Component Setup
* Deployed the **Veeam Backup & Replication server** on a dedicated management instance.
* Initialized the SQL/PostgreSQL database backend and verified the operational readiness of the Veeam transport services and core console.

---

## 💾 Phase 2: Configuring Backup Repositories (Storage Layer)

### 1. Attaching the SAN Local Storage Pool
* Integrated the **Oracle ZFS SAN Storage Volume** (`LUN-backup`) which was mounted locally to the Veeam server via iSCSI.
* Assigned this block volume as a standard local backup repository to serve as the landing zone for immediate operational restores.

![SAN Storage Added as Local Repository](./success-added-disk_SAN_Storage_that_mount_by_veeam-server.png)

### 2. Provisioning the External Linux Hardened Repository (Immutability Protection)
To defend the architecture against ransomware attacks, a hardened repository was established on an isolated Linux instance.
* Registered an external Linux Server into the backup infrastructure layer.
* Designated a secure, isolated directory partition (`/mnt/hardened_repo`) explicitly to act as the primary storage repository.
* Enabled **Immutability policies** along with data encryption constraints to prevent unauthorized modifications or deletions of active backup streams.

![Linux Server Added to Infrastructure](./success-added-linux-server-as-backup%20server.jpg)
![Configuring Hardened Directory Path](./success-determine-folder-on-linux%20server-to-be-repo.png)

---

## 🖥️ Phase 3: Population of the Inventory Ecosystem

To safeguard compute instances, both the single identity server and the highly available clustering nodes were mapped into the Veeam Inventory view.

### 1. Active Directory Domain Controller Mapping
* Added the standalone Domain Controller (`DC.zfood.local`) containing corporate user objects, schemas, and organizational units.

### 2. Failover Cluster Mapping
* Registered the core production cluster environment (`Cluster-Name.zfood.local`) hosting the virtualized file servers and operational DHCP roles.

---

## 📸 Verification Layout: Infrastructure Status Views

### 1. Centralized Backup Repositories Catalog
The view below confirms all provisioned storage points, highlighting the coexistence of the local storage volumes alongside the immutable Linux Hardened repository:

![Veeam Infrastructure Repositories Inventory](./added-devices-as-backup-repo.png)

### 2. Inventory Nodes Overview
The unified inventory workspace catalogs the live verification paths of the integrated infrastructure cluster endpoints:

![Veeam Managed Virtual Infrastructure Catalog](./added-cluster-to-Inventory.png)

---

## 🚀 Phase 4: Job Execution & Granular Recovery Proofs

### 1. Full Domain Controller Backup Execution
* Configured a full image-level backup task targeting the primary Domain Controller. 
* The job terminated with absolute **Success status**, mapping the VM data blocks securely into the storage targets.

![Active Directory DC Backup Success](./success-backup-DC.png)

### 2. Item-Level Active Directory Granular Object Restore
* Launched the **Veeam Explorer for Microsoft Active Directory** to verify recovery capabilities without taking the DC offline.
* Successfully restored specific tombstoned user accounts and OUs, validating database schema comparison points.

![Granular Object Restore Proof](./success-restore-object-from-DC.jpg)

### 3. High Availability Cluster Share Backup Task
* Engineered a secondary backup routine targeting the highly available cluster volumes (`LUN_Users_Storage`) originating from the Oracle SAN ZFS array.
* The task successfully cloned active user production volumes with transactional consistency.

![Failover Cluster Storage Backup Success](./success-backup-File-Server-Cluster.jpg)

### 4. Immutable Encrypted Transfer Validation
* Verified the secure, encrypted transmission of data blocks to the remote **Linux Hardened Repo**.
* The tracking layout logs state compliance parameters, confirming complete data immutability locks.

![Encrypted Immutability Job Run Status](./success-transfer-data-backup-to-external-linux-hardened-repo-encrypted.jpg)
