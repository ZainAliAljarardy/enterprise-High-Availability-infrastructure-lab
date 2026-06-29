# Cisco Access Switch (SW1) - Active Running Configuration Backup

This file contains the clean, production-ready infrastructure deployment commands for the Access Switch **SW1**. It includes only the customized trunking interfaces, active access VLAN memberships, and basic operational management configurations.

---

## 🛠️ CLI Direct Deployment Commands (Copy & Paste)

You can copy and paste the entire block below directly into the Cisco IOS **Global Configuration Mode (`config t`)**:

```text
configure terminal
!
hostname SW1
!
no ip domain-lookup
ip cef
!
spanning-tree mode pvst
spanning-tree extend system-id
!
! ==============================================================================
! ACTIVE PHYSICAL TRUNK INTERFACES (LAYER 2 UPLINK)
! ==============================================================================
interface Ethernet0/0
 description Uplink Trunk to Core Switch
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
exit
!
! ==============================================================================
! ACTIVE PHYSICAL ACCESS INTERFACES (VLAN ASSIGNMENTS)
! ==============================================================================
interface Ethernet1/1
 description Access Port - VLAN 4 Network
 switchport access vlan 4
 switchport mode access
 no shutdown
exit
!
interface Ethernet3/1
 description Access Port - VLAN 13 Network
 switchport access vlan 13
 switchport mode access
 no shutdown
exit
!
end
write memory
```
