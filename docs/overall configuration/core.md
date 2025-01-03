## Core Router Configuration

### Interface Configuration
```bash
enable
configure terminal
mpls ip
interface Loopback0
  ip address 10.7.0.1 255.255.255.255
  ip ospf 1 area 0
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.81 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
interface FastEthernet1/0
  ip address 10.0.0.85 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
  interface FastEthernet1/1
  ip address 10.0.0.93 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
  interface FastEthernet2/0
  ip address 10.0.0.89 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  no shutdown
!
# policy-map SME-QoS
#   class class-default
#     bandwidth 10000
# !
# interface FastEthernet0/0
#   service-policy output SME-QoS
# !
# interface FastEthernet1/0
#   service-policy output SME-QoS
# !
# interface FastEthernet1/1
#   service-policy output SME-QoS
# !
```

