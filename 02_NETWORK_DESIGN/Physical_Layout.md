# Physical Layout

## Overview

The network is physically distributed across two office locations. Each location has
its own router, switches, and end devices. The two sites are connected via a WAN link.

---

## Location A — Headquarters (HQ)

The HQ contains four separate areas, each with its own switch and department PCs.
A dedicated server room houses all centralized services.

```
┌─────────────────────────────────────────────────────────────────┐
│                  LOCATION A — HEADQUARTERS                       │
│                                                                  │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐         │
│  │  Admin Area  │   │   IT Room    │   │  Sales Area  │         │
│  │              │   │              │   │              │         │
│  │  PC0,PC1     │   │  PC3,PC4     │   │  PC6,PC7     │         │
│  │  PC2,PC19    │   │  PC5,PC20    │   │  PC8,PC18    │         │
│  │              │   │              │   │              │         │
│  │  [Switch1]   │   │  [Switch2]   │   │  [Switch3]   │         │
│  │  VLAN 10     │   │  VLAN 20     │   │  VLAN 30     │         │
│  └──────┬───────┘   └──────┬───────┘   └──────┬───────┘         │
│         │                  │                  │                  │
│         └──────────────────┴──────────────────┘                  │
│                            │                                     │
│                       [Router A]                                 │
│                    192.168.200.1                                 │
│                            │                                     │
│                       [Switch4]                                  │
│                       VLAN 100                                   │
│                            │                                     │
│  ┌─────────────────────────────────────────────┐                 │
│  │              Server Room                     │                │
│  │  DHCP Server  DNS Server  FTP Server  Web    │                │
│  │  .10.2        .10.3       .10.4        .10.5 │                │
│  └─────────────────────────────────────────────┘                 │
└─────────────────────────────────────────────────────────────────┘
```

### HQ Room Breakdown

| Room | Switch | VLAN | Devices |
|---|---|---|---|
| Admin Area | Switch1 | 10 | PC0, PC1, PC2, PC19 |
| IT Room | Switch2 | 20 | PC3, PC4, PC5, PC20 |
| Sales Area | Switch3 | 30 | PC6, PC7, PC8, PC18 |
| Server Room | Switch4 | 100 | DHCP, DNS, FTP, Web |

---

## Location B — Branch Office

The Branch has two separate department areas, each with its own switch connecting
directly to Router B.

```
┌─────────────────────────────────────────────┐
│           LOCATION B — BRANCH OFFICE         │
│                                              │
│                  [Router B]                  │
│               192.168.200.2                  │
│                  /        \                  │
│                 /          \                 │
│  ┌─────────────┐            ┌─────────────┐  │
│  │Support Area │            │  Operations │  │
│  │             │            │    Area     │  │
│  │ PC12, PC13  │            │  PC9, PC10  │  │
│  │ PC15, PC16  │            │  PC11, PC17 │  │
│  │             │            │             │  │
│  │  [Switch5]  │            │  [Switch6]  │  │
│  │  VLAN 40    │            │  VLAN 50    │  │
│  └─────────────┘            └─────────────┘  │
└─────────────────────────────────────────────┘
```

### Branch Room Breakdown

| Room | Switch | VLAN | Devices |
|---|---|---|---|
| Support Area | Switch5 | 40 | PC12, PC13, PC15, PC16 |
| Operations Area | Switch6 | 50 | PC9, PC10, PC11, PC17 |

---

## WAN Connection Between Sites

```
┌─────────────────┐                    ┌──────────────────┐
│  HEADQUARTERS   │                    │  BRANCH OFFICE   │
│                 │                    │                  │
│    Router A     │◄══ WAN LINK ══════►│    Router B      │
│ 192.168.200.1   │  192.168.200.0/30  │  192.168.200.2   │
└─────────────────┘                    └──────────────────┘
```

---

## Cabling Summary

| Connection | Cable Type | From | To |
|---|---|---|---|
| Switch1 → Router A | Straight-through / Trunk | Switch1 (Fa) | Router A (Fa0/0) |
| Switch2 → Router A | Straight-through / Trunk | Switch2 (Fa) | Router A (Fa0/0) |
| Switch3 → Router A | Straight-through / Trunk | Switch3 (Fa) | Router A (Fa0/0) |
| Switch4 → Router A | Straight-through / Trunk | Switch4 (Fa) | Router A (Fa0/0) |
| Switch5 → Router B | Straight-through / Trunk | Switch5 (Fa) | Router B (Fa0/0) |
| Switch6 → Router B | Straight-through / Trunk | Switch6 (Fa) | Router B (Fa0/0) |
| Router A → Router B | Serial / WAN | Router A (Se) | Router B (Se) |
| PCs → Switches | Straight-through | PC (Fa) | Switch (Fa) |
| Servers → Switch4 | Straight-through | Server (Fa) | Switch4 (Fa) |
