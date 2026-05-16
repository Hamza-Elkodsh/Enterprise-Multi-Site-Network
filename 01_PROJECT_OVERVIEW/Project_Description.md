# Project Description

## Enterprise Multi-Site Network Implementation

This project presents the full design and implementation of a professional enterprise network
that connects two office locations using Cisco networking technologies, simulated in
Cisco Packet Tracer.

---

## What This Project Is

The network simulates a real-world enterprise environment for a mid-sized company operating
across two physical sites — a Headquarters (HQ) and a Branch Office. It covers the complete
lifecycle of an enterprise network: planning, addressing, configuration, testing, and documentation.

The design follows industry best practices aligned with CCNA-level networking concepts, making
it suitable as a learning project, portfolio piece, or reference implementation.

---

## Scope of the Project

| Aspect | Details |
|---|---|
| Sites | 2 (Headquarters + Branch Office) |
| Departments | 6 (Admin, IT, Sales, Servers, Support, Operations) |
| VLANs | 6 VLANs across both sites |
| Routing Protocol | OSPF (Dynamic Routing) |
| Security | Access Control Lists (ACLs) |
| Services | DHCP, DNS, FTP, Web Server |
| Simulation Tool | Cisco Packet Tracer |

---

## Network Overview

### Headquarters (Location A)
The main office hosts four departments, each isolated in its own VLAN for security and
traffic management. A centralized server network provides shared services to all users
across both sites.

### Branch Office (Location B)
The branch hosts two departments connected to the main office via a WAN link between
the two routers. Branch users can access all centralized services at HQ.

### WAN Connectivity
Both sites are connected through a dedicated WAN link (`192.168.200.0/30`), enabling
full inter-site communication while maintaining logical separation between departments.

---

## Technologies Implemented

- **VLANs** — Department-level network segmentation
- **Inter-VLAN Routing** — Communication between VLANs via the core router
- **OSPF** — Dynamic routing for automatic route exchange between routers
- **ACLs** — Traffic filtering for security between departments and servers
- **DHCP** — Automatic IP address assignment for all end devices
- **DNS** — Domain name resolution for internal services
- **FTP** — File transfer service hosted at HQ
- **Web Server** — Internal web service accessible network-wide

---

## Author

**Hamza Mohamed**
Computer Science Student — Cyber Security
Passionate about Networking, Cybersecurity, and Enterprise Infrastructure.
