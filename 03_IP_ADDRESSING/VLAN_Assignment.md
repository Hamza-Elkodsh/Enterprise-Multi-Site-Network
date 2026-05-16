# VLAN Assignment

## Overview

VLANs (Virtual Local Area Networks) are used to logically segment the network by
department. Each department is assigned a unique VLAN ID, isolating its broadcast
domain and providing a security boundary between departments.

---

## VLAN Table

| VLAN ID | Name | Location | Network | Switch | Purpose |
|---|---|---|---|---|---|
| 10 | Admin | HQ | 192.168.1.0/24 | Switch1 | Management and HR staff |
| 20 | IT | HQ | 192.168.2.0/24 | Switch2 | Internal IT and infrastructure |
| 30 | Sales | HQ | 192.168.3.0/24 | Switch3 | Client-facing sales team |
| 100 | Servers | HQ | 192.168.10.0/24 | Switch4 | DHCP, DNS, FTP, Web servers |
| 40 | Support | Branch | 192.168.4.0/24 | Switch5 | Customer and technical support |
| 50 | Operations | Branch | 192.168.5.0/24 | Switch6 | Internal operations and logistics |

---

## VLAN to Port Assignment

### Switch1 — Admin (HQ)

| Port | Mode | VLAN | Device |
|---|---|---|---|
| Fa0/1 | Access | 10 | PC0 |
| Fa0/2 | Access | 10 | PC1 |
| Fa0/3 | Access | 10 | PC2 |
| Fa0/4 | Access | 10 | PC19 |
| Fa0/24 | Trunk | All | Router A |

### Switch2 — IT (HQ)

| Port | Mode | VLAN | Device |
|---|---|---|---|
| Fa0/1 | Access | 20 | PC3 |
| Fa0/2 | Access | 20 | PC4 |
| Fa0/3 | Access | 20 | PC5 |
| Fa0/4 | Access | 20 | PC20 |
| Fa0/24 | Trunk | All | Router A |

### Switch3 — Sales (HQ)

| Port | Mode | VLAN | Device |
|---|---|---|---|
| Fa0/1 | Access | 30 | PC6 |
| Fa0/2 | Access | 30 | PC7 |
| Fa0/3 | Access | 30 | PC8 |
| Fa0/4 | Access | 30 | PC18 |
| Fa0/24 | Trunk | All | Router A |

### Switch4 — Servers (HQ)

| Port | Mode | VLAN | Device |
|---|---|---|---|
| Fa0/1 | Access | 100 | DHCP Server |
| Fa0/2 | Access | 100 | DNS Server |
| Fa0/3 | Access | 100 | FTP Server |
| Fa0/4 | Access | 100 | Web Server |
| Fa0/24 | Trunk | All | Router A |

### Switch5 — Support (Branch)

| Port | Mode | VLAN | Device |
|---|---|---|---|
| Fa0/1 | Access | 40 | PC12 |
| Fa0/2 | Access | 40 | PC13 |
| Fa0/3 | Access | 40 | PC15 |
| Fa0/4 | Access | 40 | PC16 |
| Fa0/24 | Trunk | All | Router B |

### Switch6 — Operations (Branch)

| Port | Mode | VLAN | Device |
|---|---|---|---|
| Fa0/1 | Access | 50 | PC9 |
| Fa0/2 | Access | 50 | PC10 |
| Fa0/3 | Access | 50 | PC11 |
| Fa0/4 | Access | 50 | PC17 |
| Fa0/24 | Trunk | All | Router B |

---

## Port Modes Explained

| Mode | Description | Used For |
|---|---|---|
| **Access** | Carries traffic for one VLAN only | PC and server connections |
| **Trunk** | Carries traffic for multiple VLANs | Switch-to-router uplinks |

---

## VLAN Design Notes

- **VLAN IDs 10, 20, 30** are sequential for HQ departments — easy to remember
- **VLAN 100** is used for servers — a high number to clearly separate it from user VLANs
- **VLAN 40, 50** continue the sequence for Branch departments
- All switch uplinks to routers are configured as **trunk ports** using 802.1Q encapsulation
- Router subinterfaces use **dot1q encapsulation** matching each VLAN ID
