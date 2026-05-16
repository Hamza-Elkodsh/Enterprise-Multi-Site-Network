# Routing Verification

## Overview

This document verifies that routing is correctly configured and functioning
across the entire enterprise network including OSPF adjacencies, routing tables,
and end-to-end reachability between all sites.

---

## Routing Architecture Summary

| Device | Role | Routing Method |
|---|---|---|
| MLSwitch | HQ Core — VLAN Gateway | OSPF + Static Routes |
| Router A | HQ Edge — WAN Gateway | OSPF + Static Routes |
| Router B | Branch Edge — WAN Gateway | OSPF + Static Routes |

---

## 1. OSPF Verification

### Router A — OSPF Neighbors

```
RouterA#show ip ospf neighbor

Neighbor ID   Pri   State     Dead Time   Address          Interface
3.3.3.3        1    FULL/DR   00:00:38    192.168.100.2    GigabitEthernet0/0
2.2.2.2        0    FULL/-    00:00:39    192.168.200.2    Serial0/0/0
```

| Neighbor | Router ID | State | Meaning |
|---|---|---|---|
| MLSwitch | 3.3.3.3 | FULL/DR | Full adjacency — MLSwitch is DR ✅ |
| Router B | 2.2.2.2 | FULL/- | Full adjacency on serial link ✅ |

**Conclusion:** Router A has established full OSPF adjacency with both neighbors. ✅

---

### Router B — OSPF Neighbors

```
RouterB#show ip ospf neighbor

Neighbor ID   Pri   State     Dead Time   Address          Interface
1.1.1.1        0    FULL/-    00:00:34    192.168.200.1    Serial0/0/0
```

| Neighbor | Router ID | State | Meaning |
|---|---|---|---|
| Router A | 1.1.1.1 | FULL/- | Full adjacency on serial WAN link ✅ |

**Conclusion:** Router B has established full OSPF adjacency with Router A. ✅

---

## 2. Routing Table Verification

### Router A — Full Routing Table

```
RouterA#show ip route

S    192.168.1.0/24  [1/0] via 192.168.100.2
S    192.168.2.0/24  [1/0] via 192.168.100.2
S    192.168.3.0/24  [1/0] via 192.168.100.2
O    192.168.4.0/24  [110/65] via 192.168.200.2, Serial0/0/0
O    192.168.5.0/24  [110/65] via 192.168.200.2, Serial0/0/0
S    192.168.10.0/24 [1/0] via 192.168.100.2
C    192.168.100.0/30 — GigabitEthernet0/0
C    192.168.200.0/30 — Serial0/0/0
```

| Network | Route Type | Status |
|---|---|---|
| 192.168.1.0 (Admin) | Static via MLSwitch | ✅ |
| 192.168.2.0 (IT) | Static via MLSwitch | ✅ |
| 192.168.3.0 (Sales) | Static via MLSwitch | ✅ |
| 192.168.4.0 (Support) | OSPF via Router B | ✅ |
| 192.168.5.0 (Operations) | OSPF via Router B | ✅ |
| 192.168.10.0 (Servers) | Static via MLSwitch | ✅ |
| 192.168.100.0 (LAN link) | Connected | ✅ |
| 192.168.200.0 (WAN link) | Connected | ✅ |

**Conclusion:** Router A has complete routing knowledge of all 8 networks. ✅

---

### Router B — Full Routing Table

```
RouterB#show ip route

S    192.168.1.0/24  [1/0] via 192.168.200.1
S    192.168.2.0/24  [1/0] via 192.168.200.1
S    192.168.3.0/24  [1/0] via 192.168.200.1
C    192.168.4.0/24  — GigabitEthernet0/0
C    192.168.5.0/24  — GigabitEthernet0/1
S    192.168.10.0/24 [1/0] via 192.168.200.1
S    192.168.100.0/30 [1/0] via 192.168.200.1
C    192.168.200.0/30 — Serial0/0/0
```

| Network | Route Type | Status |
|---|---|---|
| 192.168.1.0 (Admin) | Static via Router A | ✅ |
| 192.168.2.0 (IT) | Static via Router A | ✅ |
| 192.168.3.0 (Sales) | Static via Router A | ✅ |
| 192.168.4.0 (Support) | Connected | ✅ |
| 192.168.5.0 (Operations) | Connected | ✅ |
| 192.168.10.0 (Servers) | Static via Router A | ✅ |
| 192.168.100.0 (LAN link) | Static via Router A | ✅ |
| 192.168.200.0 (WAN link) | Connected | ✅ |

**Conclusion:** Router B has complete routing knowledge of all 8 networks. ✅

---

## 3. End-to-End Reachability

### Packet Path Analysis

#### HQ PC to Branch PC
```
PC0 (192.168.1.10)
  → Switch1 (VLAN 10)
  → MLSwitch (routes to 192.168.100.1)
  → Router A (routes to 192.168.200.2)
  → Router B (routes to 192.168.4.0)
  → Switch5
  → PC12 (192.168.4.10)

TTL hops: 3 (TTL=125 confirmed in ping tests)
```

#### Branch PC to HQ Server
```
PC12 (192.168.4.10)
  → Switch5
  → Router B (routes to 192.168.200.1)
  → Router A (routes to 192.168.100.2)
  → MLSwitch (routes to VLAN 100)
  → Switch4
  → DHCP Server (192.168.10.2)
```

---

## 4. Verification Summary

| Check | Result |
|---|---|
| OSPF Router A ↔ MLSwitch | ✅ FULL |
| OSPF Router A ↔ Router B | ✅ FULL |
| OSPF Router B ↔ Router A | ✅ FULL |
| Router A knows all 8 networks | ✅ |
| Router B knows all 8 networks | ✅ |
| HQ → Branch reachability | ✅ |
| Branch → HQ reachability | ✅ |
| Branch → DHCP Server | ✅ |
| All PCs get DHCP IPs | ✅ |
