# ISP Switch - Active Running Configuration Backup

This file contains the clean, production-ready infrastructure deployment commands for the **ISP Switched Node**. It includes only the active interface memberships, custom VLAN interfaces, and configured static routing entries.

---

## 🛠️ CLI Direct Deployment Commands (Copy & Paste)

You can copy and paste the entire block below directly into the Cisco IOS **Global Configuration Mode (`config t`)**:

```text
configure terminal
!
hostname ISP
!
no ip domain-lookup
ip cef
!
spanning-tree mode pvst
spanning-tree extend system-id
!
! ==============================================================================
! LAYER 2 SWITCHPORT & VLAN ASSIGNMENTS
! ==============================================================================
interface Ethernet0/1
 description Access Port - Link 1
 switchport access vlan 100
 switchport mode access
 no shutdown
exit
!
interface Ethernet0/2
 description Access Port - Link 2
 switchport access vlan 100
 switchport mode access
 no shutdown
exit
!
interface Ethernet0/3
 description Access Port - Link 3
 switchport access vlan 100
 switchport mode access
 no shutdown
exit
!
! ==============================================================================
! LAYER 3 SWITCHED VIRTUAL INTERFACE (SVI)
! ==============================================================================
interface Vlan100
 description ISP Transit Gateway
 ip address 192.168.200.254 255.255.255.0
 no shutdown
exit
!
! Static Route pointing back to the Remote LAN via R6 WAN IP
ip route 192.168.70.0 255.255.255.0 192.168.200.30
!
end
write memory
```
