## Chicago Router Configuration

### Interface Configuration
```bash
enable
configure terminal
mpls ip
interface Loopback0
  ip address 10.7.0.5 255.255.255.255
  ip ospf 1 area 0
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.90 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet1/0
  ip address 10.0.0.113 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet2/0
  no shutdown
!
interface FastEthernet2/0.10
 encapsulation dot1Q 10
 ip address 10.10.0.3 255.255.255.0
 no shutdown
!
interface FastEthernet2/0.20
 encapsulation dot1Q 20
 ip address 10.20.0.3 255.255.255.0
 no shutdown
!
interface FastEthernet2/0.30
 encapsulation dot1Q 30
 ip address 10.30.0.3 255.255.255.0
 no shutdown
!
```