## Barcelona Router Configuration

### Interface Configuration
```bash
enable
configure terminal

mpls label protocol ldp
mpls traffic-eng tunnels

ip vrf SME
  rd 21200:1
  route-target export 21200:1
  route-target import 21200:1
!

class-map match-all Vl
  match ip dscp af21
!
policy-map OUT
  class Vl
    bandwidth 10000
!

interface Loopback0
  ip address 10.7.0.4 255.255.255.255
  ip ospf 1 area 0
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.94 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet1/0
  ip address 10.0.0.114 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet1/1
  ip address 10.0.0.109 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  service-policy output OUT
  ip rsvp bandwidth 100000 10000
  no shutdown
!

router ospf 1
 mpls traffic-eng router-id Loopback0
 mpls traffic-eng area 0
!
mpls ldp router-id Loopback0 force
!

router bgp 21200
  neighbor 10.7.0.6 remote-as 21200
  neighbor 10.7.0.6 update-source Loopback0
  neighbor 10.7.0.7 remote-as 21200
  neighbor 10.7.0.7 update-source Loopback0
  neighbor 10.7.0.8 remote-as 21200
  neighbor 10.7.0.8 update-source Loopback0
!
address-family vpnv4
  neighbor 10.7.0.6 activate
  neighbor 10.7.0.6 send-community both
  neighbor 10.7.0.7 activate
  neighbor 10.7.0.7 send-community both
  neighbor 10.7.0.8 activate
  neighbor 10.7.0.8 send-community both
!

address-family ipv4 vrf SME
  redistribute connected
exit-address-family
!
```