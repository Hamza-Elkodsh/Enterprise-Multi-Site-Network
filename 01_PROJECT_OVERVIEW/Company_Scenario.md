# Company Scenario

## Background

**TechCore Solutions** is a mid-sized technology services company with operations across
two office locations. The company provides IT consulting, software support, and managed
services to enterprise clients.

As the company has grown, the need for a structured, secure, and scalable internal network
has become critical. The IT department has been tasked with redesigning the network
infrastructure to support current operations and future growth.

---

## Business Requirements

The company's management has defined the following network requirements:

- Each department must be isolated in its own network segment to limit broadcast traffic
  and reduce security risks
- All departments must be able to access centralized services (file sharing, web, DNS)
- The Branch Office must communicate seamlessly with Headquarters over a WAN link
- Unauthorized inter-department access must be restricted through firewall-like rules
- IP address management must be centralized and automated
- The design must be documented for future maintenance and onboarding

---

## Office Locations

### Location A — Headquarters (HQ)

The main office is the operational core of the company. It hosts the majority of staff
and all centralized infrastructure servers.

| Department | VLAN | Network | Role |
|---|---|---|---|
| Admin | 10 | 192.168.1.0/24 | Management and HR staff |
| IT | 20 | 192.168.2.0/24 | Internal IT and infrastructure team |
| Sales | 30 | 192.168.3.0/24 | Client-facing sales team |
| Servers | 100 | 192.168.100.0/24 | DHCP, DNS, FTP, and Web servers |

The HQ is managed by a **Core Switch** (Layer 2/3) and connected to **Router A**, which
handles inter-VLAN routing and WAN connectivity.

---

### Location B — Branch Office

The branch supports a smaller team focused on client support and internal operations.
Branch staff require access to HQ servers and inter-site communication.

| Department | VLAN | Network | Role |
|---|---|---|---|
| Support | 40 | 192.168.4.0/24 | Customer and technical support team |
| Operations | 50 | 192.168.5.0/24 | Internal operations and logistics |

The Branch is connected through **Router B**, which links back to HQ via the WAN link.

---

## WAN Link

Both offices are connected through a dedicated point-to-point WAN connection:

| Router | WAN Interface IP |
|---|---|
| Router A (HQ) | 192.168.200.1 |
| Router B (Branch) | 192.168.200.2 |

OSPF is used to dynamically share routing information between both sites, ensuring
all networks are reachable from both locations without manual static route maintenance.

---

## Security Policy

The IT department has defined the following security rules, enforced using ACLs:

- The **Sales** department cannot access the **IT** VLAN directly
- Only the **IT** department has full access to all servers
- **Admin** can access the Web and DNS server but not the FTP server
- **Branch** users can access the Web server and DNS only
- All other cross-department traffic follows a default deny policy where applicable

---

## Network Infrastructure Summary

| Device | Quantity | Location |
|---|---|---|
| Cisco Router | 2 | HQ (Router A), Branch (Router B) |
| Core Switch | 1 | HQ |
| Access Switches | 6 | HQ (3), Branch (2) |
| DHCP Server | 1 | HQ — Server VLAN |
| DNS Server | 1 | HQ — Server VLAN |
| FTP Server | 1 | HQ — Server VLAN |
| Web Server | 1 | HQ — Server VLAN |
| Department PCs | 4+ | One per department |
