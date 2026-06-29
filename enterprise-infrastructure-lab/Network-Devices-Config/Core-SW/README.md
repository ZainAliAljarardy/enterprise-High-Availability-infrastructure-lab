# Cisco Core Switch (Core-SW) - Active Running Configuration Backup

This file contains the clean, production-ready infrastructure deployment commands for the Layer 3 **Core Switch (Core-SW)**. It encapsulates only the customized physical trunk/routed interfaces, active SVI interfaces (VLANs), OSPF routing area configurations, and static routes.

---

## 🛠️ CLI Direct Deployment Commands (Copy & Paste)

You can copy and paste the entire block below directly into the Cisco IOS **Global Configuration Mode (`config t`)**:

```text
configure terminal
!
hostname Core-SW
!
no ip domain-lookup
ip cef
!
spanning-tree mode pvst
spanning-tree extend system-id
!
! ==============================================================================
! PHYSICAL ROUTED INTERFACES (LAYER 3 PORTS)
! ==============================================================================
interface Ethernet0/0
 description Routed Link 1
 no switchport
 ip address 192.168.70.1 255.255.255.252
 duplex auto
 no shutdown
exit
!
interface Ethernet1/1
 description Routed Link 2
 no switchport
 ip address 192.168.80.1 255.255.255.252
 duplex auto
 no shutdown
exit
!
! ==============================================================================
! PHYSICAL TRUNK INTERFACES (LAYER 2 PORTS)
! ==============================================================================
interface Ethernet0/1
 description Core Trunk Link 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
exit
!
interface Ethernet0/2
 description Core Trunk Link 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
exit
!
interface Ethernet0/3
 description Core Trunk Link 3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
exit
!
! ==============================================================================
! SWITCHED VIRTUAL INTERFACES (SVIs / VLAN GATEWAYS)
! ==============================================================================
interface Vlan4
 description Management VLAN
 ip address 192.168.40.1 255.255.255.0
 no shutdown
exit
!
interface Vlan10
 description Data Subnet VLAN 10
 ip address 192.168.100.1 255.255.255.0
 ip helper-address 192.168.40.16
 no shutdown
exit
!
interface Vlan11
 description Data Subnet VLAN 11
 ip address 192.168.110.1 255.255.255.0
 ip helper-address 192.168.40.16
 no shutdown
exit
!
interface Vlan12
 description Data Subnet VLAN 12
 ip address 192.168.120.1 255.255.255.0
 ip helper-address 192.168.40.16
 no shutdown
exit
!
interface Vlan13
 description Isolated DMZ / Segment
 ip address 192.168.130.1 255.255.255.0
 no shutdown
exit
!
interface Vlan14
 description Transit / Perimeter Gateway
 ip address 192.168.140.254 255.255.255.0
 no shutdown
exit
!
! ==============================================================================
! DYNAMIC OSPF ROUTING CORE ENGINE
! ==============================================================================
router ospf 1
 network 192.168.40.0 0.0.0.255 area 0
 network 192.168.100.0 0.0.0.255 area 0
 network 192.168.110.0 0.0.0.255 area 0
 network 192.168.120.0 0.0.0.255 area 0
 network 192.168.130.0 0.0.0.255 area 0
 network 192.168.140.0 0.0.0.255 area 0
exit
!
! ==============================================================================
! SYSTEM SERVICES & TUNABLES / STATIC ROUTING
! ==============================================================================
no ip http server
no ip http secure-server
!
! Core Static Routing Paths
ip route 20.20.20.0 255.255.255.0 192.168.140.1
ip route 192.168.60.0 255.255.255.0 192.168.140.1
!
end
write memory
