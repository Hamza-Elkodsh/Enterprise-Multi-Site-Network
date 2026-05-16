# Subnetting Calculations

## Overview

All department subnets are carved from the `192.168.0.0/16` address space using
a fixed `/24` prefix length. The WAN link uses a `/30` prefix to minimize
address waste on the point-to-point connection.

---

## How /24 Subnetting Works

```
IP Address:   192.168.1.0
Subnet Mask:  255.255.255.0  (/24)

Binary breakdown:
  192      .  168      .  1        .  0
  11000000 . 10101000 . 00000001 . 00000000
  [────────── Network (24 bits) ──────────] [── Host (8 bits) ──]

Total addresses:     2^8  = 256
Usable host range:   256 - 2 = 254  (subtract network + broadcast)
Network address:     192.168.1.0
First usable host:   192.168.1.1
Last usable host:    192.168.1.254
Broadcast address:   192.168.1.255
```

---

## Department Subnet Calculations

### VLAN 10 — Admin (192.168.1.0/24)

| Field | Value |
|---|---|
| Network Address | 192.168.1.0 |
| Subnet Mask | 255.255.255.0 |
| CIDR | /24 |
| First Host | 192.168.1.1 (Gateway) |
| DHCP Start | 192.168.1.10 |
| DHCP End | 192.168.1.254 |
| Broadcast | 192.168.1.255 |
| Total Hosts | 254 |
| Hosts In Use | 4 PCs + 1 Gateway = 5 |
| Remaining | 249 |

---

### VLAN 20 — IT (192.168.2.0/24)

| Field | Value |
|---|---|
| Network Address | 192.168.2.0 |
| Subnet Mask | 255.255.255.0 |
| CIDR | /24 |
| First Host | 192.168.2.1 (Gateway) |
| DHCP Start | 192.168.2.10 |
| DHCP End | 192.168.2.254 |
| Broadcast | 192.168.2.255 |
| Total Hosts | 254 |
| Hosts In Use | 4 PCs + 1 Gateway = 5 |
| Remaining | 249 |

---

### VLAN 30 — Sales (192.168.3.0/24)

| Field | Value |
|---|---|
| Network Address | 192.168.3.0 |
| Subnet Mask | 255.255.255.0 |
| CIDR | /24 |
| First Host | 192.168.3.1 (Gateway) |
| DHCP Start | 192.168.3.10 |
| DHCP End | 192.168.3.254 |
| Broadcast | 192.168.3.255 |
| Total Hosts | 254 |
| Hosts In Use | 4 PCs + 1 Gateway = 5 |
| Remaining | 249 |

---

### VLAN 40 — Support (192.168.4.0/24)

| Field | Value |
|---|---|
| Network Address | 192.168.4.0 |
| Subnet Mask | 255.255.255.0 |
| CIDR | /24 |
| First Host | 192.168.4.1 (Gateway) |
| DHCP Start | 192.168.4.10 |
| DHCP End | 192.168.4.254 |
| Broadcast | 192.168.4.255 |
| Total Hosts | 254 |
| Hosts In Use | 4 PCs + 1 Gateway = 5 |
| Remaining | 249 |

---

### VLAN 50 — Operations (192.168.5.0/24)

| Field | Value |
|---|---|
| Network Address | 192.168.5.0 |
| Subnet Mask | 255.255.255.0 |
| CIDR | /24 |
| First Host | 192.168.5.1 (Gateway) |
| DHCP Start | 192.168.5.10 |
| DHCP End | 192.168.5.254 |
| Broadcast | 192.168.5.255 |
| Total Hosts | 254 |
| Hosts In Use | 4 PCs + 1 Gateway = 5 |
| Remaining | 249 |

---

### VLAN 100 — Servers (192.168.10.0/24)

| Field | Value |
|---|---|
| Network Address | 192.168.10.0 |
| Subnet Mask | 255.255.255.0 |
| CIDR | /24 |
| First Host | 192.168.10.1 (Gateway) |
| Static Range | 192.168.10.2 — 192.168.10.10 |
| Broadcast | 192.168.10.255 |
| Total Hosts | 254 |
| Hosts In Use | 4 Servers + 1 Gateway = 5 |
| Remaining | 249 |

> No DHCP pool on this subnet — all server IPs are statically assigned.

---

## WAN Link Calculation (192.168.200.0/30)

A `/30` subnet provides exactly 2 usable host addresses — perfect for a
point-to-point link between two routers.

```
IP Address:   192.168.200.0
Subnet Mask:  255.255.255.252  (/30)

Binary breakdown:
  192      .  168      .  200      .  0
  11000000 . 10101000 . 11001000 . 00000000
  [──────────────── Network (30 bits) ──────────────────] [Host]

Total addresses:   2^2 = 4
Usable hosts:      4 - 2 = 2
Network address:   192.168.200.0
Router A (HQ):     192.168.200.1
Router B (Branch): 192.168.200.2
Broadcast:         192.168.200.3
```

| Field | Value |
|---|---|
| Network | 192.168.200.0 |
| Subnet Mask | 255.255.255.252 |
| CIDR | /30 |
| Router A | 192.168.200.1 |
| Router B | 192.168.200.2 |
| Broadcast | 192.168.200.3 |
| Usable Hosts | 2 |

---

## All Subnets at a Glance

| VLAN | Department | Network | Mask | Gateway | Usable Hosts |
|---|---|---|---|---|---|
| 10 | Admin | 192.168.1.0/24 | 255.255.255.0 | 192.168.1.1 | 254 |
| 20 | IT | 192.168.2.0/24 | 255.255.255.0 | 192.168.2.1 | 254 |
| 30 | Sales | 192.168.3.0/24 | 255.255.255.0 | 192.168.3.1 | 254 |
| 40 | Support | 192.168.4.0/24 | 255.255.255.0 | 192.168.4.1 | 254 |
| 50 | Operations | 192.168.5.0/24 | 255.255.255.0 | 192.168.5.1 | 254 |
| 100 | Servers | 192.168.10.0/24 | 255.255.255.0 | 192.168.10.1 | 254 |
| N/A | WAN Link | 192.168.200.0/30 | 255.255.255.252 | N/A | 2 |
