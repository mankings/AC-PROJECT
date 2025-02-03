## Core Router Configuration

### Interface Configuration
```bash
enable
configure terminal

snmp-server community public RO
snmp-server location Core

mpls label protocol ldp
mpls traffic-eng tunnels

class-map match-all Vl
  match ip dscp af21 
class-map match-any Vlans
!
policy-map OUT
 class Vl
  bandwidth 10000
!

interface Loopback0
  ip address 10.7.0.1 255.255.255.255
  ip ospf 1 area 0
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.81 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet1/0
  ip address 10.0.0.85 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet1/1
  ip address 10.0.0.93 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet2/0
  ip address 10.0.0.89 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet3/0
  ip address 10.0.0.121 255.255.255.252
  ip ospf 1 area 0
  no shutdown
!

router ospf 1
  mpls traffic-eng router-id Loopback0
  mpls traffic-eng area 0
!
router bgp 21200
  bgp log-neighbor-changes
!
mpls ldp router-id Loopback0 force
!
```

