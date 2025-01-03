## Lisbon Router Configuration

### Interface Configuration
```bash
enable
configure terminal
mpls ip
interface Loopback0
  ip address 10.7.0.2 255.255.255.255
  ip ospf 1 area 0
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.82 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet1/0
  ip address 10.0.0.97 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet1/1
  ip address 10.0.0.101 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet2/0
  no shutdown
!
interface FastEthernet2/0.10
  encapsulation dot1Q 10
  ip address 10.10.0.1 255.255.255.0
  no shutdown
!
interface FastEthernet2/0.20
  encapsulation dot1Q 20
  ip address 10.20.0.1 255.255.255.0
  no shutdown
!
interface FastEthernet2/0.30
  encapsulation dot1Q 30
  ip address 10.30.0.1 255.255.255.0
  no shutdown
!
policy-map SME-QoS
  class class-default
    bandwidth 10000
!
interface FastEthernet0/0
  service-policy output SME-QoS
!
```