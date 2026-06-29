# Cisco Router R6 - Active Running Configuration Backup

This file contains the clean, production-ready infrastructure deployment commands for **Router 6 (R6)**. It includes only the active customized interfaces and functional operational parameters.

---

## 🛠️ CLI Direct Deployment Commands (Copy & Paste)

You can copy and paste the entire block below directly into the Cisco IOS **Global Configuration Mode (`config t`)**:

```text
configure terminal
!
hostname R6
! ==============================================================================
! ACTIVE INTERFACE CONFIGURATIONS
! ==============================================================================
interface FastEthernet0/0
 description WAN Interface Connection
 ip address 192.168.200.30 255.255.255.0
 duplex full
 no shutdown
exit
!
interface GigabitEthernet2/0
 description Local Subnet Gateway (LAN)
 ip address 192.168.70.254 255.255.255.0
 negotiation auto
 no shutdown
exit
!
end
write memory
```
