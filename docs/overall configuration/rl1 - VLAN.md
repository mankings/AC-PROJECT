## RL1 Configuration

### Interface Configuration
```bash
configure
set interfaces ethernet eth1 vif 10 address 10.10.0.4/24
set interfaces ethernet eth1 vif 20 address 10.20.0.4/24
set interfaces ethernet eth1 vif 30 address 10.30.0.4/24
set protocols static route 0.0.0.0/0 next-hop 10.10.0.1
delete firewall
commit
save
```


```
interfaces {
    bridge br10 {
        address 10.10.0.254/24
        description vlan10
        member {
            interface eth1.10 {
            }
            interface vxlan10 {
            }
        }
        traffic-policy {
            out VLAN10-MARK
        }
    }
    bridge br20 {
        address 10.20.0.254/24
        description vlan20
        member {
            interface eth1.20 {
            }
            interface vxlan20 {
            }
        }
        traffic-policy {
            out VLAN20-MARK
        }
    }
    bridge br30 {
        address 10.30.0.254/24
        description vlan30
        member {
            interface eth1.30 {
            }
            interface vxlan30 {
            }
        }
        traffic-policy {
            out VLAN30-MARK
        }
    }
    dummy dum0 {
        address 10.0.0.249/32
    }
    ethernet eth0 {
        address 10.0.0.29/30
        hw-id 0c:76:47:7c:00:00
    }
    ethernet eth1 {
        hw-id 0c:76:47:7c:00:01
        vif 10 {
        }
        vif 20 {
        }
        vif 30 {
        }
    }
    ethernet eth2 {
        hw-id 0c:76:47:7c:00:02
    }
    ethernet eth3 {
        hw-id 0c:76:47:7c:00:03
    }
    ethernet eth4 {
        hw-id 0c:76:47:7c:00:04
    }
    ethernet eth5 {
        hw-id 0c:76:47:7c:00:05
    }
    loopback lo {
    }
    vxlan vxlan10 {
        mtu 1500
        source-address 10.0.0.249
        vni 10
    }
    vxlan vxlan20 {
        mtu 1500
        source-address 10.0.0.249
        vni 20
    }
    vxlan vxlan30 {
        mtu 1500
        source-address 10.0.0.249
        vni 30
    }
}
protocols {
    bgp {
        address-family {
            l2vpn-evpn {
                advertise-all-vni
            }
        }
        neighbor 10.0.0.247 {
            peer-group evpn
        }
        neighbor 10.0.0.250 {
            peer-group evpn
        }
        parameters {
            router-id 10.0.0.249
        }
        peer-group evpn {
            address-family {
                l2vpn-evpn {
                    nexthop-self {
                    }
                    route-reflector-client
                }
            }
            remote-as 21200
            update-source dum0
        }
        system-as 21200
    }
    ospf {
        area 0 {
            network 10.0.0.28/30
            network 10.0.0.249/32
        }
    }
}
system {
    config-management {
        commit-revisions 100
    }
    conntrack {
        modules {
            ftp
            h323
            nfs
            pptp
            sip
            sqlnet
            tftp
        }
    }
    console {
        device ttyS0 {
            speed 115200
        }
    }
    host-name RL1
    login {
        user vyos {
            authentication {
                encrypted-password ****************
                plaintext-password ****************
            }
        }
    }
    ntp {
        server time1.vyos.net {
        }
        server time2.vyos.net {
        }
        server time3.vyos.net {
        }
    }
    syslog {
        global {
            facility all {
                level info
            }
            facility protocols {
                level debug
            }
        }
    }
}
traffic-policy {
    shaper VLAN10-MARK {
        bandwidth 1gbit
        class 10 {
            bandwidth 1gbit
            burst 15k
            match ip {
                ip {
                    source {
                        address 10.10.0.0/24
                    }
                }
            }
            queue-type fair-queue
            set-dscp AF21
        }
        default {
            bandwidth 500mbit
            burst 15k
            queue-type fair-queue
        }
    }
    shaper VLAN20-MARK {
        bandwidth 1gbit
        class 20 {
            bandwidth 1gbit
            burst 15k
            match ip {
                ip {
                    source {
                        address 10.20.0.0/24
                    }
                }
            }
            queue-type fair-queue
            set-dscp AF21
        }
        default {
            bandwidth 500mbit
            burst 15k
            queue-type fair-queue
        }
    }
    shaper VLAN30-MARK {
        bandwidth 1gbit
        class 30 {
            bandwidth 1gbit
            burst 15k
            match ip {
                ip {
                    source {
                        address 10.30.0.0/24
                    }
                }
            }
            queue-type fair-queue
            set-dscp AF21
        }
        default {
            bandwidth 500mbit
            burst 15k
            queue-type fair-queue
        }
    }
}

```