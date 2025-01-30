## Porto Router Configuration

### Interface Configuration
```bash
enable
configure terminal

mpls label protocol ldp
mpls traffic-eng tunnels

class-map match-all Vl
 match ip dscp af21 
!
policy-map OUT
 class Vl
  bandwidth 10000
!

interface Loopback0
  ip address 10.7.0.3 255.255.255.255
  ip ospf 1 area 0
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.86 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet1/0 
  ip address 10.0.0.98 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet1/1
  ip address 10.0.0.105 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet2/0
  no shutdown
!
interface FastEthernet2/0.10
 encapsulation dot1Q 10
 ip address 10.10.0.2 255.255.255.0
 no shutdown
!
interface FastEthernet2/0.20
 encapsulation dot1Q 20
 ip address 10.20.0.2 255.255.255.0
 no shutdown
!
interface FastEthernet2/0.30
 encapsulation dot1Q 30
 ip address 10.30.0.2 255.255.255.0
 no shutdown
!

router ospf 1
  mpls traffic-eng router-id Loopback0
  mpls traffic-eng area 0
!
mpls ldp router-id Loopback0 force
!
```