# IP Addressing Plan

## Overview

The entire network uses the `192.168.0.0/16` private address space, divided into
smaller `/24` subnets — one per department/VLAN. This provides up to 254 usable
hosts per subnet, which is more than enough for each department.

---

## Address Space Summary

| Property | Value |
|---|---|
| Main Network | 192.168.0.0/16 |
| Subnet Mask | 255.255.0.0 |
| Subnetting Strategy | One /24 subnet per VLAN |
| Total Subnets Used | 7 (6 VLANs + 1 WAN) |
| Usable Hosts Per Subnet | 254 |

---

## Headquarters — IP Addressing

### VLAN 10 — Admin

| Property | Value |
|---|---|
| Network | 192.168.1.0/24 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |
| DHCP Range | 192.168.1.10 — 192.168.1.254 |
| Broadcast | 192.168.1.255 |

### VLAN 20 — IT

| Property | Value |
|---|---|
| Network | 192.168.2.0/24 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.2.1 |
| DHCP Range | 192.168.2.10 — 192.168.2.254 |
| Broadcast | 192.168.2.255 |

### VLAN 30 — Sales

| Property | Value |
|---|---|
| Network | 192.168.3.0/24 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.3.1 |
| DHCP Range | 192.168.3.10 — 192.168.3.254 |
| Broadcast | 192.168.3.255 |

### VLAN 100 — Servers

| Property | Value |
|---|---|
| Network | 192.168.10.0/24 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.10.1 |
| Static Range | 192.168.10.2 — 192.168.10.10 |
| Broadcast | 192.168.10.255 |

> Servers use **static IP addresses** — no DHCP on this subnet.

---

## Branch Office — IP Addressing

### VLAN 40 — Support

| Property | Value |
|---|---|
| Network | 192.168.4.0/24 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.4.1 |
| DHCP Range | 192.168.4.10 — 192.168.4.254 |
| Broadcast | 192.168.4.255 |

### VLAN 50 — Operations

| Property | Value |
|---|---|
| Network | 192.168.5.0/24 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.5.1 |
| DHCP Range | 192.168.5.10 — 192.168.5.254 |
| Broadcast | 192.168.5.255 |

---

## WAN Link — IP Addressing

| Property | Value |
|---|---|
| Network | 192.168.200.0/30 |
| Subnet Mask | 255.255.255.252 |
| Router A (HQ) | 192.168.200.1 |
| Router B (Branch) | 192.168.200.2 |
| Broadcast | 192.168.200.3 |
| Usable Hosts | 2 (exactly right for point-to-point) |

> A `/30` subnet is used for the WAN link because only 2 IP addresses are needed —
> one for each router end. This avoids wasting addresses.

---

## Gateway Summary (Router Subinterfaces)

All default gateways are the `.1` address of each subnet, assigned to Router A
or Router B subinterfaces.

| VLAN | Department | Gateway | Router |
|---|---|---|---|
| 10 | Admin | 192.168.1.1 | Router A |
| 20 | IT | 192.168.2.1 | Router A |
| 30 | Sales | 192.168.3.1 | Router A |
| 100 | Servers | 192.168.10.1 | Router A |
| 40 | Support | 192.168.4.1 | Router B |
| 50 | Operations | 192.168.5.1 | Router B |
