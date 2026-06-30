# Enterprise High-Availability Infrastructure, Secure Hybrid Networking & Disaster Recovery Architecture

## 📌 Project Overview
This project represents a full-scale deployment of a highly available, secure, and resilient enterprise-grade infrastructure lab. The architecture integrates multi-tier switching, specialized firewall routing, virtualized shared storage networks (SAN), high-availability application clustering, and robust offsite disaster recovery strategies. 

The lab simulates a modern corporate enterprise workflow, focusing on continuous service availability, strict security access lines, automated group policies, and immutable backup orchestration.

---

## 🗺️ Network & Infrastructure Architecture

![Network Topology](./Infrastructure.png)

### 🏷️ Network Design Specifications
* **Core Switching:** Layer 2/3 architecture utilizing **VLANs**, **802.1Q Trunking**, and **Switched Virtual Interfaces (SVIs)** for optimal inter-VLAN routing and security boundary control via Core11.
* **Perimeter Defense:** **OPNsense Firewall** deployed as the primary edge device, managing NAT rules, internal stateful inspections, and core cryptographic VPN endpoints.

---

## 🛡️ Technical Components & Key Features

### 1. Perimeter Security & Hybrid Networking
* **OPNsense Firewall:** Deployed as an enterprise edge security gateway handling stateful packet inspection, NAT rules, and network isolation.
* **Site-to-Site IPsec VPN:** Secure, encrypted cryptographic tunnels engineered to bridge multi-site enterprise environments and branch offices.
* **Remote Access WireGuard VPN:** High-performance, low-latency VPN termination allowing secure, encrypted remote administration for external corporate users.

### 2. High Availability & Centralized Storage Layer
* **Centralized Oracle ZFS SAN Storage:** Configured as a centralized block-storage array presenting high-performance shared Logical Unit Numbers (LUNs) via optimized iSCSI network portals.
* **Windows Failover Clustering (HA Compute):** A 2-node high-availability cluster architecture guaranteeing seamless node failover for mission-critical enterprise roles:
  * **Highly Available File Server:** Continuous availability for shared corporate data volumes.
  * **Highly Available DHCP Server:** Fault-tolerant, dynamic IP addressing across the enterprise.

### 3. Identity & Centralized Management
* **Active Directory Domain Services (AD DS):** Centralized identity lifecycle management, domain name resolution (DNS), and global authentication policy control (`zfood.local`).
* **Windows Admin Center (WAC) Gateway:** A unified web-based management platform providing centralized dashboard oversight and secure remote operations management across all infrastructure nodes.

### 4. Client Policy, Automation & Governance
* **Automated Folder Redirection:** Centralized Active Directory Group Policies (GPOs) automating the redirection of user profiles (`Desktop` & `Documents`) directly into the HA File Server Cluster.
* **Dynamic Network Drive Mapping:** GPO-driven network drive deployment mapping shared department volumes automatically based on user security principles.
* **FSRM Storage Quotas:** File Server Resource Manager (FSRM) integration enforcing strict hard storage capacity limits to prevent SAN storage exhaustion.

### 5. Enterprise Backup & Ransomware Resilience
* **Veeam Backup & Replication Core:** Automated backup scheduling, replication cycles, and bare-metal disaster recovery (DR) orchestration.
* **Immutable Linux Hardened Repository:** An isolated offsite Ubuntu Server repository configured with immutable file flags to secure backup sets against ransomware, deletion, or unauthorized infrastructure modifications.

---

## 📋 Logical Addressing & Protocol Blueprint

| Network Segment / Service | IP Address / Subnet | Interface / Gateway Reference | Host Protocol Rules / Role |
| :--- | :--- | :--- | :--- |
| **Server Infrastructure LAN (VLAN 4)** | `192.168.40.0/24` | VLAN 4 SVI Gateway / Core11 | Core AD, Node1, Node2, Storage, Veeam, WAC |
| **Clustered File-Server Virtual IP** | `192.168.40.115` | Dynamic Virtual Pointer | High-Availability File Server IP Assignment |
| **Clustered DHCP Virtual IP** | `192.168.40.16` | Dynamic Virtual Pointer | High-Availability DHCP IP Assignment |
| **Marketing Network (VLAN 10)** | `192.168.100.0/24` | VLAN 10 SVI / Core11 | End-user Nodes & Workstations (Marketer) |
| **Finance Network (VLAN 11)** | `192.168.110.0/24` | VLAN 11 SVI / Core11 | End-user Nodes & Workstations (Finance) |
| **Accounting Network (VLAN 12)** | `192.168.120.0/24` | VLAN 12 SVI / Core11 | End-user Nodes & Workstations (Accountant) |
| **IT Administration (VLAN 13)** | `192.168.130.0/24` | VLAN 13 SVI / Core11 | Core Management & Admin Nodes (Admin) |
| **Inter-Switch Link (Core to OPNsense)**| `192.168.140.0/30` | Core11 <--> SW3 <--> OPNsense-1 | Core internal transit routing and packet flow |
| **OPNsense WAN / Transit Network** | `192.168.200.0/24` | OPNsense-1 (.10) <--> IOU1 (.254)| External edge transit for secure VPN tunnels |
| **Disaster Recovery DR Site Network** | `192.168.60.0/24` | Connected via R5 / IOU1 | Offsite DR Storage Location (Linux-Storage) |
| **Remote User Home Network** | `192.168.70.0/24` | Connected via R6 / IOU1 | Remote road warrior workers / Home nodes |

---

## 🏁 How to Verify & Inspect Feature Functionality

* **Domain Automation Verification:** Run `gpupdate /force` on any domain-joined workstation; corporate drive mappings will dynamically mount, and Folder Redirection will instantly synchronize data with the active cluster share.
* **Cluster HA Verification:** Simulate a hardware failure by manually draining roles or stopping the cluster service on an active node; the secondary node will assume compute control with sub-second failover execution and zero service downtime.
* **Backup Immutability Verification:** Log into the offsite Ubuntu Server repository and attempt a manual terminal deletion (`rm -rf`) inside the backup data path; the Linux kernel will explicitly block the operation, verifying active ransomware defense.
