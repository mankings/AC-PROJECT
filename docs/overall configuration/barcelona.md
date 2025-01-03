## Barcelona Router Configuration

### Interface Configuration
```bash
enable
configure terminal
mpls ip
interface Loopback0
  ip address 10.7.0.4 255.255.255.255
  ip ospf 1 area 0
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.94 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet1/0
  ip address 10.0.0.114 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet1/1
  ip address 10.0.0.109 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
policy-map SME-QoS
  class class-default
    bandwidth 10000
!
interface FastEthernet1/1
  service-policy output SME-QoS
!
```