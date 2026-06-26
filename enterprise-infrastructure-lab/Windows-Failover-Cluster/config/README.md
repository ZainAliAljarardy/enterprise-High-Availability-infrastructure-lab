# Windows Server 2025 Failover Clustering & High Availability Deployment

This section documents the end-to-end engineering implementation of a Highly Available (HA) Microsoft Failover Cluster architecture across two infrastructure compute nodes (`Node1` and `Node2`), backed by enterprise shared iSCSI storage blocks managed through the Oracle ZFS Storage Appliance.

---

## 🏛️ Topology Cluster Specifications
* **Domain Environment:** `zfood.local`
* **Node 1 IP Address:** `192.168.40.105` 
* **Node 2 IP Address:** `192.168.40.106` 
* **Cluster Management VIP:** `192.168.40.110`
* **Cluster File Server VIP:** `192.168.40.115`
* **Cluster DHCP VIP:** `192.168.40.16`
* **Shared Storage Target Portal:** `192.168.40.200` (Oracle ZFS Interface IP)

---

## ⚙️ Step-by-Step Cluster Implementation Framework

### Step 1: Mapping Shared LUNs via iSCSI Initiator
To initialize cluster communication, shared blocks from the Oracle ZFS SAN storage pool must be mapped locally to both server targets.
1. Launched the **iSCSI Initiator Properties** dashboard on both compute instances.
2. Specified the dedicated Oracle ZFS SAN IP address inside the **Target Portal** field.
3. Discovered and established a persistent connection status to the allocated shared logical volumes.

![iSCSI Initiator Connection Target](../screenshots/iSCSI-initiator.jpg)

---

### Step 2: Provisioning the Failover Clustering Role
The platform dependencies must be installed across all participating endpoints before spinning up the infrastructure cluster.
1. Executed the Windows Server **Add Roles and Features Wizard** on `Node1` and `Node2`.
2. Selected and deployed the **Failover Clustering** feature framework along with the **Multipath I/O (MPIO)** feature block to enable path redundancy.
3. *Alternative Automated Deployment via PowerShell:*
   ```powershell
   Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools -ComputerName Node1, Node2
   
---

### Step 3: Executing the Cluster Validation Configuration Wizard
Prior to building the live framework, the environment must pass structural support checks to identify misconfigured networks or shared disk arbitration locks.

* Opened the **Failover Cluster Manager** and launched the **Validate a Configuration Wizard**.
* Added the authenticated target nodes: `Node1.zfood.local` and `Node2.zfood.local`.
* Executed comprehensive evaluation layers verifying BIOS information compliance, network metrics, and disk path readiness.

![Cluster Validation Summary](./validate-config-wizard.png)

---

### Step 4: Provisioning the Highly Available Cluster
Once validation logs passed with strict compliance parameters, the structural cluster management perimeter was spawned.

* Launched the **Create Cluster Wizard** from the centralized management interface.
* Defined the management endpoint identifier as **`Cluster-Name`**.
* Assigned the dedicated cluster management Virtual IP: **`192.168.40.110`**.
* Configured the operational voting quorum under the **Node and Disk Majority (Cluster Disk 3)** witness allocation.

![Cluster Generation Verification](./Create-Cluster.png)

---

### Step 5: Provisioning the Highly Available File Server Role
To build a continuous file distribution point resilient to node failovers, the File Server cluster role was deployed.

* Selected **Configure Role** from the cluster action layout to instantiate the **High Availability Wizard**.
* Designated **File Server for general use** as the primary high-availability operational layout.
* Assigned the network interface role identifier as **`FS-Users`** bound to the static VIP Address **`192.168.40.115`**.
* Provisioned a continuous SMB share target directory mapped to the shared SAN storage volume (`E:\Shares\User_Storage`).

---

### Step 6: Deploying the Clustered DHCP Server Role
To provide non-disruptive dynamic IP address allocation across operational corporate subnets, the DHCP service was brought into the high-availability layer.

* Added the **DHCP Server** role to the active cluster configuration array.
* Defined the localized access point naming profile as **`Cluster-DHCP`**.
* Bound the service tracking interface to the network VIP Address **`192.168.40.16`**.

> 💡 **High Availability Note:** This configuration ensures that even if one node crashes, the secondary node takes control of the active leases database seamlessly without dropping client connectivity.

![Clustered DHCP Server Role Deployment](./add_dhcp_role.png)
