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