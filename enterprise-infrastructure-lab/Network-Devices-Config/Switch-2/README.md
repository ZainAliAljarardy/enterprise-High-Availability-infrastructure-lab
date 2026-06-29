# Cisco Access Switch (SW2) - Active Running Configuration Backup

This file contains the clean, production-ready infrastructure deployment commands for the Access Switch **SW2**. It includes only the active access interfaces with their respective VLAN assignments and essential management line parameters.

---

## 🛠️ CLI Direct Deployment Commands (Copy & Paste)

You can copy and paste the entire block below directly into the Cisco IOS **Global Configuration Mode (`config t`)**:

```text
configure terminal
!
hostname SW2
!
no ip domain-lookup
ip cef
!
spanning-tree mode pvst
spanning-tree extend system-id
!
! ==============================================================================
! ACTIVE PHYSICAL ACCESS INTERFACES (VLAN ASSIGNMENTS)
! ==============================================================================
! VLAN 10 Ports (Data / Workstations Block 1)
interface Ethernet1/1
 description Access Port - VLAN 10
 switchport access vlan 10
 switchport mode access
 no shutdown
exit
!
interface Ethernet1/2
 description Access Port - VLAN 10
 switchport access vlan 10
 switchport mode access
 no shutdown
exit
!
! VLAN 11 Ports (Data / Workstations Block 2)
interface Ethernet2/1
 description Access Port - VLAN 11
 switchport access vlan 11
 switchport mode access
 no shutdown
exit
!
interface Ethernet2/2
 description Access Port - VLAN 11
 switchport access vlan 11
 switchport mode access
 no shutdown
exit
!
! VLAN 12 Ports (Data / Workstations Block 3)
interface Ethernet3/1
 description Access Port - VLAN 12
 switchport access vlan 12
 switchport mode access
 no shutdown
exit
!
interface Ethernet3/2
 description Access Port - VLAN 12
 switchport access vlan 12
 switchport mode access
 no shutdown
exit
!
end
write memory
```
