# Cisco Router R5 - Active Running Configuration Backup

This file contains the clean, production-ready infrastructure deployment commands for **Router 5 (R5)**. It defines the interface metrics, static routing vectors, and cryptographic parameters required to align with the OPNsense edge firewall.

---

## 🛠️ CLI Direct Deployment Commands (Copy & Paste)

You can copy and paste the entire block below directly into the Cisco IOS **Global Configuration Mode (`config t`)**:

```text
configure terminal
!
hostname R5
!
no ip domain lookup
ip cef
!
! ==============================================================================
! PHASE 1: IKEv2 CRYPTOGRAPHIC PROPOSALS & POLICIES
! ==============================================================================
crypto ikev2 proposal IKE2_PROP
 encryption aes-cbc-256
 integrity sha256
 group 14
exit
!
crypto ikev2 proposal IPSEC_PROP
 encryption aes-cbc-256 aes-cbc-128
 integrity sha256 sha1
 group 14
exit
!
crypto ikev2 policy IPSEC_POL
 proposal IPSEC_PROP
exit
!
! ==============================================================================
! PHASE 1: KEYRING MAPPING & PRE-SHARED SECURITY KEYS
! ==============================================================================
crypto ikev2 keyring IPSEC_KEYRING
 peer OPNSENSE_FIREWALL
  address 192.168.200.10
  pre-shared-key crypto_zain
 exit
exit
!
! ==============================================================================
! PHASE 1: INTEGRATION PROFILE MATRIX
! ==============================================================================
crypto ikev2 profile IKEv2_PROF
 match identity remote address 192.168.200.10 255.255.255.255
 identity local address 192.168.200.20
 authentication remote pre-share
 authentication local pre-share
 keyring local IPSEC_KEYRING
exit
!
! ==============================================================================
! PHASE 2: TRANSFORM-SET PAYLOAD SPECIFICATIONS
! ==============================================================================
crypto ipsec transform-set ESP_AES256_SHA256 esp-aes 256 esp-sha256-hmac
 mode tunnel
exit
!
crypto ipsec profile IPSEC_PROF
 set transform-set ESP_AES256_SHA256
 set ikev2-profile IKEv2_PROF
exit
!
! ==============================================================================
! PHASE 2: CRYPTO MAP CONSTRUCTION & INTEREST TRAFFIC BINDING
! ==============================================================================
crypto map CMAP_TO_OPNSENSE 10 ipsec-isakmp
 set peer 192.168.200.10
 set transform-set ESP_AES256_SHA256
 set ikev2-profile IKEv2_PROF
 match address VPN_TRAFFIC_ACL
exit
!
! ==============================================================================
! INTERFACE CONFIGURATIONS & CRYPTO ATTACHMENTS
! ==============================================================================
interface FastEthernet0/0
 description WAN Link to OPNsense Firewall
 ip address 192.168.200.20 255.255.255.0
 duplex full
 crypto map CMAP_TO_OPNSENSE
 no shutdown
exit
!
interface GigabitEthernet1/0
 description Remote Offsite Storage Subnet (LAN)
 ip address 192.168.60.254 255.255.255.0
 negotiation auto
 no shutdown
exit
!
interface GigabitEthernet2/0
 no ip address
 negotiation auto
exit
!
interface GigabitEthernet3/0
 no ip address
 negotiation auto
exit
!
interface GigabitEthernet4/0
 no ip address
 negotiation auto
exit
!
interface GigabitEthernet5/0
 no ip address
 shutdown
 negotiation auto
exit
!
interface FastEthernet6/0
 no ip address
 shutdown
 speed auto
 duplex auto
exit
!
interface FastEthernet6/1
 no ip address
 shutdown
 speed auto
 duplex auto
exit
!
! ==============================================================================
! SYSTEM TUNABLES, STATIC ROUTING & ACCESS-LIST VALUES
! ==============================================================================
no ip http server
no ip http secure-server
!
! Return Path Static Route targeting Corporate Production LAN
ip route 192.168.40.0 255.255.255.0 192.168.200.10
!
! Extended ACL mapping Crypto Interest Traffic Categories
ip access-list extended VPN_TRAFFIC_ACL
 permit ip 192.168.60.0 0.0.0.255 192.168.40.0 0.0.0.255
exit
!
! ==============================================================================
! MANAGEMENT LINES CONSOLE SECURITY CONFIGURATIONS
! ==============================================================================
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 stopbits 1
exit
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 stopbits 1
exit
line vty 0 4
 login
exit
!
end
write memory
```
