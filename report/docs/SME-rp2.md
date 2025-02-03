## RP2 Configuration

### Interface Configuration
```bash
enable
configure terminal

snmp-server community public RO
snmp-server location Porto


mpls label protocol ldp
mpls traffic-eng tunnels

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
interface Tunnel0
  ip unnumbered Loopback0
  tunnel mode mpls traffic-eng
  tunnel destination 10.7.0.6
  tunnel mpls traffic-eng autoroute announce
  tunnel mpls traffic-eng priority 7 7
  tunnel mpls traffic-eng bandwidth 10000
  tunnel mpls traffic-eng path-option 1 dynamic
  no shutdown
!
interface Tunnel1
  ip unnumbered Loopback0
  tunnel mode mpls traffic-eng
  tunnel destination 10.7.0.8
  tunnel mpls traffic-eng autoroute announce
  tunnel mpls traffic-eng priority 7 7
  tunnel mpls traffic-eng bandwidth 10000
  tunnel mpls traffic-eng path-option 1 dynamic
  no shutdown
!
interface FastEthernet0/0
  ip address 10.0.0.106 255.255.255.252
  ip ospf 1 area 0
  mpls ip
  mpls traffic-eng tunnels
  ip rsvp bandwidth 100000 10000
  no shutdown
!
interface FastEthernet1/0
  ip vrf forwarding SME
  ip address 10.0.1.1 255.255.255.0
  no shutdown
!

router ospf 1
 mpls traffic-eng router-id Loopback0
 mpls traffic-eng area 0
!
router bgp 21200
  bgp log-neighbor-changes
  neighbor 10.7.0.6 remote-as 21200
  neighbor 10.7.0.6 update-source Loopback0
  neighbor 10.7.0.8 remote-as 21200
  neighbor 10.7.0.8 update-source Loopback0
  neighbor 10.7.0.4 remote-as 21200
  neighbor 10.7.0.4 update-source Loopback0
!
address-family vpnv4
  neighbor 10.7.0.6 activate
  neighbor 10.7.0.6 send-community both
  neighbor 10.7.0.8 activate
  neighbor 10.7.0.8 send-community both
  neighbor 10.7.0.4 activate
  neighbor 10.7.0.4 send-community both
!
address-family ipv4 vrf SME
  redistribute connected
exit-address-family
!
```