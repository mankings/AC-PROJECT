## RP2 Configuration

### Interface Configuration
```bash
enable
configure terminal
mpls ip
ip vrf SME
  rd 21200:1
  route-target export 21200:1
  route-target import 21200:1
!
interface Loopback0
  ip address 10.7.0.7 255.255.255.255
  ip ospf 1 area 0
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.106 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet1/0
  ip vrf forwarding SME1
  ip address 10.0.1.1 255.255.255.0
!
router bgp 21200 
  neighbor 10.7.0.5 remote-as 21200
  neighbor 10.7.0.5 update-source Loopback0
  neighbor 10.7.0.8 remote-as 21200
  neighbor 10.7.0.8 update-source Loopback0
!
address-family vpnv4
  neighbor 10.7.0.5 activate
  neighbor 10.7.0.5 send-community both
  neighbor 10.7.0.8 activate
  neighbor 10.7.0.8 send-community both
!
address-family ipv4 vrf SME
  redistribute connected
  redistribute static
!
policy-map SME-QoS
  class class-default
    bandwidth 10000
!
interface FastEthernet0/0
  service-policy output SME1-QoS

```