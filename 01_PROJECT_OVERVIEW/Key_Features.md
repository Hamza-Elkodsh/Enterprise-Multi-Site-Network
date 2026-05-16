# Key Features

## Overview

This project implements a full-scale enterprise network simulation with the following
core features, each reflecting a real-world deployment scenario.

---

## 1. Multi-Site Enterprise Network

The network spans two physical locations — Headquarters and a Branch Office — connected
over a WAN link. This mirrors how real enterprise networks operate across offices,
data centers, or geographic regions.

- HQ hosts 4 departments and all centralized servers
- Branch hosts 2 departments with full access to HQ resources
- WAN link uses a dedicated `/30` subnet for point-to-point connectivity

---

## 2. VLAN Segmentation

Each department is placed in its own VLAN, providing:

- **Broadcast isolation** — traffic is contained within each department
- **Security boundaries** — departments cannot communicate unless explicitly allowed
- **Logical organization** — clean separation of IT, Admin, Sales, Support, and Operations

| VLAN | Department | Subnet |
|---|---|---|
| 10 | Admin | 192.168.1.0/24 |
| 20 | IT | 192.168.2.0/24 |
| 30 | Sales | 192.168.3.0/24 |
| 40 | Support | 192.168.4.0/24 |
| 50 | Operations | 192.168.5.0/24 |
| 100 | Servers | 192.168.10.0/24 |

---

## 3. Dynamic Routing with OSPF

OSPF (Open Shortest Path First) is configured on both routers to:

- Automatically share all network routes between HQ and Branch
- Eliminate the need for manual static routes
- Adapt to network changes without reconfiguration
- Establish a verified neighbor relationship between Router A and Router B

---

## 4. Inter-VLAN Routing

A Router-on-a-Stick configuration on the HQ router enables communication between VLANs:

- Subinterfaces are created for each VLAN on the router
- All inter-department traffic is routed through the core router
- ACLs are applied at the routing level to control traffic flow

---

## 5. Centralized Services

All shared services are hosted in the dedicated **Server VLAN (100)** at HQ:

| Server | Service | Purpose |
|---|---|---|
| DHCP Server | IP Address Assignment | Automatically assigns IPs to all departments |
| DNS Server | Name Resolution | Resolves internal hostnames to IP addresses |
| FTP Server | File Transfer | Secure file sharing for authorized departments |
| Web Server | HTTP Service | Internal web portal accessible network-wide |

---

## 6. Access Control Lists (ACLs)

ACLs are implemented to enforce the company's security policy:

- Restrict Sales from accessing the IT VLAN
- Limit Branch access to only Web and DNS servers
- Allow IT full access to all resources
- Deny unauthorized traffic by default

ACL rules are applied on router interfaces (inbound/outbound) for precise traffic control.

---

## 7. Automated IP Management (DHCP)

A centralized DHCP server eliminates manual IP configuration:

- Separate DHCP pools defined for each VLAN/subnet
- Default gateways, DNS server addresses, and lease times configured per pool
- End devices receive correct addressing automatically on startup

---

## 8. Scalable Design

The network is designed to accommodate future growth:

- Additional VLANs can be added without restructuring the existing design
- OSPF will automatically propagate new routes
- The `192.168.0.0/16` address space provides room for expansion
- Access switches can be added to accommodate more end devices

---

## 9. Full Documentation

Every aspect of the network is documented:

- IP addressing tables and VLAN assignments
- Step-by-step setup guide
- All device configurations (routers, switches, ACLs)
- Troubleshooting and maintenance guides
- Test results and verification outputs

---

## 10. Cisco Packet Tracer Ready

The complete simulation is packaged as a `.pkt` file:

- Open and run immediately in Cisco Packet Tracer
- All configurations are pre-loaded and active
- Simulation mode available for packet-level inspection
- Ideal for demonstration, study, and CCNA exam preparation
