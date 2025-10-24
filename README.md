# University Network Topology
## About
This project represents a topology for a University with fail-safe network that includes HSRP on L3 switches, 2 DHCPs servers, Cisco ASA, IPSec, ACLs, SSH, WLC, VoIP, Server Farm.

In this topology I configured HQ and branch. Between HQ and Branch I configured IPSec for secure connection. Routing is implemented using OSPF and static routing with the ISP. Details on security, routing, and infrastructure are provided below.

## Topology
![Entire Topology](https://github.com/gwill1337/University-Network/blob/main/Images/Network.png?raw=true)

## Security
Topology has Cisco ASA firewall in HQ and branch thats confugured with:
* **Servers in DMZ**   
* **ACLs**   
* **OSPF**
* **IPSec connectinon between HQ and Branch**   
* **NAT**

Every switches and routers configured with:
* **Switchport Nonegotiate**
* **STP Portfast on all access ports**
* **BPDU Guard on all access ports**
* **Blackhole Vlan and shutdown on all unusable ports**
* **SSH with ACL**

### Details:

1. **Servers in DMZ** - Public facing servers are placed in a separate network segment to isolate them from the internal LAN and reduce security risks.
2. **ACLs** - Define and enforce traffic filtering rules to allow or deny specific connections based on IP, protocol, and port.
3. **OSPF** - Dynamic routing protocol used between routers; authentication is configured to secure routing updates.
4. **IPSec** - Establishes an encrypted VPN tunnel between HQ and Branch to ensure data confidentiality and integrity.
5. **NAT** - Translates internal private IP addresses to public ones, protecting internal devices and managing external connectivity.
6. **Nonegotiate** - Disables DTP negotiation to prevent unauthorized trunk formation and VLAN hopping attacks.
7. **STP Portfast** - Enables immediate transition of access ports to the forwarding state, improving connection speed for end devices.
8. **BPDU Guard** - Automatically disables a port if unexpected BPDUs are received, protecting against potential topology manipulation.
9. **SSH with ACL** - Provides secure remote management and restricts SSH access to authorized IP addresses only.

## IP Table
#### HQ:
Name          | Vlan          |     IP
:---------: | :----------:  | :----------:
MNG         | 10              | 192.168.10.0/24
ADM  | 15 | 192.168.20.0/24
LAN | 20 | 172.11.0.0/16
WLan | 99 | 1.10.0.0/16
VoIP | 199 | 172.40.1.0/24
Blackhole | 999 | *
#### Branch:
Name         | Vlan          |     IP
:---------: | :----------:  | :----------:
AMD | 15 | 192.168.21.0/24
LAN | 120 | 172.20.0.0/16
WLan | 99 | 1.11.0.0/16
VoIP | 199 | 172.40.1.0/24
Blackhole | 999 | *

#### Else:
Name         |     IP
:---------:  | :----------:
SRV | 10.10.10.0/24

[P.s ](#else) Here better use mask /28 for SRV so as not to use the entire network but in my case CPT doesn't want work with this mask.

## HQ
### Topology
![HQ Topology](https://github.com/gwill1337/University-Network/blob/main/Images/HQ.png?raw=true)
### Devices:
* 1 x Cisco 2911 Router
* 1 x Cisco 2811 Router 
* 1 x Cisco ASA Firewall
* 2 x Layer 3 Switch
* 8 x Layer 2 Switches
* 1 x WLC 2504
* 6 x LAP
* 7 x Servers
* 1 x Ip Phone
* 1 x Printer
* PCs

**L3 Switch:** Configured with HSRP, OSPF, Inter Vlan Routing and between them EtherChannel with LACP.   
**Cisco 2811 Router:** performs the default gateway for VoIP devices.     
**Cisco ASA:** Configured with IPSec, OSPF, ACLs, DMZ zone for servers and NAT with dynamic interface.  
*P.s. In .pkt file Cisco ASAs with ACL that allows all traffic because ASA doesn't want to pass my Web server even with permit ports 80 and 443.*



### Servers

Server      |     Function
---------   |    :----------:
DHCP 1        | For DHCP IPs for HQ and Branch
DHCP 2       | DHCP if DHCP 1 falls
NTP/SYSLOG         | Time synchronization for HQ and branch Network diveces, and this same one for logging
DNS         | Resolves company domain + forwards to Google DNS (8.8.8.8)
WEB         | University website
FTP         | Data storage

## Branch
### Topology
![Branch Topology](https://github.com/gwill1337/University-Network/blob/main/Images/Branch.png?raw=true)
### Devices:
* 1 x Cisco 2911 Router
* 1 x Cisco 2811 Router  
* 2 x Layer 3 Switch
* 4 x Layer 2 Switches
* 4 x LAP
* 1 x IP Phone
* 1 x Printer
* PCs

**L3 Switch:** Configured with HSRP, OSPF, Inter Vlan Routing and between them EtherChannel with LACP.   
**Cisco ASA:** Configured with IPSec, OSPF, ACLs and NAT with dynamic interface.  
*P.s. In .pkt file Cisco ASAs with ACL that allows all traffic because ASA doesn't want to pass my Web server even with permit ports 80 and 443.*
