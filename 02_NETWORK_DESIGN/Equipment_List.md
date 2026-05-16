# Equipment List

## Summary

| Device Type | Model | Quantity |
|---|---|---|
| Router | Cisco 2911 | 2 |
| Access Switch | Cisco 2960-24TT | 6 |
| Server | Cisco Server-PT | 4 |
| PC | Cisco PC-PT | 16+ |

---

## Routers

| Device Name | Model | Location | Role | WAN IP |
|---|---|---|---|---|
| Router A | Cisco 2911 | Headquarters | Inter-VLAN routing, OSPF, WAN | 192.168.200.1 |
| Router B | Cisco 2911 | Branch Office | OSPF, WAN gateway for Branch | 192.168.200.2 |

### Router A — Subinterfaces (Inter-VLAN Routing)

| Subinterface | VLAN | IP Address | Department |
|---|---|---|---|
| Fa0/0.10 | 10 | 192.168.1.1 | Admin |
| Fa0/0.20 | 20 | 192.168.2.1 | IT |
| Fa0/0.30 | 30 | 192.168.3.1 | Sales |
| Fa0/0.100 | 100 | 192.168.10.1 | Servers |

### Router B — Subinterfaces (Inter-VLAN Routing)

| Subinterface | VLAN | IP Address | Department |
|---|---|---|---|
| Fa0/0.40 | 40 | 192.168.4.1 | Support |
| Fa0/0.50 | 50 | 192.168.5.1 | Operations |

---

## Switches

| Device Name | Model | Location | VLAN | Department |
|---|---|---|---|---|
| Switch1 | Cisco 2960-24TT | HQ — Admin Area | 10 | Admin |
| Switch2 | Cisco 2960-24TT | HQ — IT Room | 20 | IT |
| Switch3 | Cisco 2960-24TT | HQ — Sales Area | 30 | Sales |
| Switch4 | Cisco 2960-24TT | HQ — Server Room | 100 | Servers |
| Switch5 | Cisco 2960-24TT | Branch — Support Area | 40 | Support |
| Switch6 | Cisco 2960-24TT | Branch — Operations Area | 50 | Operations |

---

## Servers

All servers are located at HQ on VLAN 100 (192.168.10.0/24), connected to Switch4.

| Device Name | Role | IP Address | Service |
|---|---|---|---|
| DHCP Server | IP Address Assignment | 192.168.10.2 | DHCP |
| DNS Server | Name Resolution | 192.168.10.3 | DNS |
| FTP Server | File Transfer | 192.168.10.4 | FTP |
| Web Server | Internal Web Portal | 192.168.10.5 | HTTP |

---

## End Devices (PCs)

### Headquarters

| PC | Switch | VLAN | Department | IP Assignment |
|---|---|---|---|---|
| PC0 | Switch1 | 10 | Admin | DHCP |
| PC1 | Switch1 | 10 | Admin | DHCP |
| PC2 | Switch1 | 10 | Admin | DHCP |
| PC19 | Switch1 | 10 | Admin | DHCP |
| PC3 | Switch2 | 20 | IT | DHCP |
| PC4 | Switch2 | 20 | IT | DHCP |
| PC5 | Switch2 | 20 | IT | DHCP |
| PC20 | Switch2 | 20 | IT | DHCP |
| PC6 | Switch3 | 30 | Sales | DHCP |
| PC7 | Switch3 | 30 | Sales | DHCP |
| PC8 | Switch3 | 30 | Sales | DHCP |
| PC18 | Switch3 | 30 | Sales | DHCP |

### Branch Office

| PC | Switch | VLAN | Department | IP Assignment |
|---|---|---|---|---|
| PC12 | Switch5 | 40 | Support | DHCP |
| PC13 | Switch5 | 40 | Support | DHCP |
| PC15 | Switch5 | 40 | Support | DHCP |
| PC16 | Switch5 | 40 | Support | DHCP |
| PC9 | Switch6 | 50 | Operations | DHCP |
| PC10 | Switch6 | 50 | Operations | DHCP |
| PC11 | Switch6 | 50 | Operations | DHCP |
| PC17 | Switch6 | 50 | Operations | DHCP |

---

## Device Count Summary

| Category | Count |
|---|---|
| Routers | 2 |
| Access Switches | 6 |
| Servers | 4 |
| HQ PCs | 12 |
| Branch PCs | 8 |
| **Total Devices** | **32** |
