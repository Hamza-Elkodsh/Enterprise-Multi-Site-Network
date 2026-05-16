# Connectivity Tests

## Overview

This document records all connectivity tests performed to verify the enterprise
network is functioning correctly across both sites.

---

## Test Environment

| Property | Value |
|---|---|
| Simulation Tool | Cisco Packet Tracer |
| Test Date | 2026 |
| Author | Hamza Mohamed |
| Total Tests | 9 |
| Tests Passed | 9 |
| Tests Failed | 0 |

> Note: First packet loss (25%) on some inter-VLAN pings is normal in Packet Tracer
> due to ARP resolution on first contact. Subsequent packets succeed 100%.

---

## Test 1 — DHCP Assignment (PC0 - Admin)

**Objective:** Verify PC0 receives correct IP via DHCP from centralized DHCP server.

| Field | Expected | Actual | Result |
|---|---|---|---|
| IP Address | 192.168.1.10 | 192.168.1.10 | ✅ PASS |
| Subnet Mask | 255.255.255.0 | 255.255.255.0 | ✅ PASS |
| Default Gateway | 192.168.1.1 | 192.168.1.1 | ✅ PASS |
| DNS Server | 192.168.10.3 | 192.168.10.3 | ✅ PASS |

**Conclusion:** DHCP server correctly assigned all network parameters to PC0. ✅

---

## Test 2 — Intra-VLAN Communication (Admin VLAN 10)

**Objective:** Verify PC2 can communicate with PC1 within the same VLAN.

| From | To | Sent | Received | Lost | Result |
|---|---|---|---|---|---|
| PC2 | 192.168.1.11 | 4 | 4 | 0 (0%) | ✅ PASS |

```
Reply from 192.168.1.11: bytes=32 time<1ms TTL=128
Reply from 192.168.1.11: bytes=32 time<1ms TTL=128
Reply from 192.168.1.11: bytes=32 time<1ms TTL=128
Reply from 192.168.1.11: bytes=32 time=1ms TTL=128
Packets: Sent=4, Received=4, Lost=0 (0% loss)
```

**Conclusion:** Intra-VLAN communication working perfectly. ✅

---

## Test 3 — Inter-VLAN Communication (Admin → IT)

**Objective:** Verify Admin VLAN 10 can reach IT VLAN 20 via MLSwitch routing.

| From | To | Sent | Received | Lost | Result |
|---|---|---|---|---|---|
| PC2 (Admin) | 192.168.2.10 (IT) | 4 | 3 | 1 (25%) | ✅ PASS |

```
Request timed out. (ARP resolution — normal on first packet)
Reply from 192.168.2.10: bytes=32 time<1ms TTL=127
Reply from 192.168.2.10: bytes=32 time=1ms TTL=127
Reply from 192.168.2.10: bytes=32 time<1ms TTL=127
Packets: Sent=4, Received=3, Lost=1 (25% loss)
```

> TTL=127 confirms traffic is being routed by MLSwitch. ✅

**Conclusion:** Inter-VLAN routing working correctly via MLSwitch. ✅

---

## Test 4 — Inter-VLAN Communication (Admin → Sales)

**Objective:** Verify Admin VLAN 10 can reach Sales VLAN 30 via MLSwitch routing.

| From | To | Sent | Received | Lost | Result |
|---|---|---|---|---|---|
| PC2 (Admin) | 192.168.3.10 (Sales) | 4 | 3 | 1 (25%) | ✅ PASS |

```
Request timed out. (ARP resolution — normal on first packet)
Reply from 192.168.3.10: bytes=32 time<1ms TTL=127
Reply from 192.168.3.10: bytes=32 time=1ms TTL=127
Reply from 192.168.3.10: bytes=32 time<1ms TTL=127
Packets: Sent=4, Received=3, Lost=1 (25% loss)
```

**Conclusion:** Inter-VLAN routing working correctly between Admin and Sales. ✅

---

## Test 5 — Inter-Site Communication (Branch → HQ)

**Objective:** Verify Branch PC can reach HQ Admin VLAN across WAN link.

| From | To | Sent | Received | Lost | Result |
|---|---|---|---|---|---|
| Branch PC | 192.168.1.10 (Admin) | 4 | 3 | 1 (25%) | ✅ PASS |

```
Request timed out. (ARP resolution — normal on first packet)
Reply from 192.168.1.10: bytes=32 time=1ms TTL=125
Reply from 192.168.1.10: bytes=32 time=1ms TTL=125
Reply from 192.168.1.10: bytes=32 time=1ms TTL=125
Packets: Sent=4, Received=3, Lost=1 (25% loss)
```

> TTL=125 confirms traffic crosses multiple hops:
> Branch PC → Router B → Router A → MLSwitch → HQ PC ✅

**Conclusion:** Inter-site WAN communication working correctly. ✅

---

## Test 6 — OSPF Neighbors (Router A)

**Objective:** Verify Router A OSPF adjacencies with MLSwitch and Router B.

| Neighbor ID | State | Interface | Result |
|---|---|---|---|
| 3.3.3.3 (MLSwitch) | FULL/DR | GigabitEthernet0/0 | ✅ PASS |
| 2.2.2.2 (Router B) | FULL/- | Serial0/0/0 | ✅ PASS |

**Conclusion:** Router A has full OSPF adjacency with both neighbors. ✅

---

## Test 7 — OSPF Neighbors (Router B)

**Objective:** Verify Router B OSPF adjacency with Router A.

| Neighbor ID | State | Interface | Result |
|---|---|---|---|
| 1.1.1.1 (Router A) | FULL/- | Serial0/0/0 | ✅ PASS |

**Conclusion:** Router B has full OSPF adjacency with Router A. ✅

---

## Test 8 — Routing Table (Router A)

**Objective:** Verify Router A has routes to all networks.

| Network | Type | Next Hop | Result |
|---|---|---|---|
| 192.168.1.0/24 | S (Static) | 192.168.100.2 | ✅ |
| 192.168.2.0/24 | S (Static) | 192.168.100.2 | ✅ |
| 192.168.3.0/24 | S (Static) | 192.168.100.2 | ✅ |
| 192.168.4.0/24 | O (OSPF) | 192.168.200.2 | ✅ |
| 192.168.5.0/24 | O (OSPF) | 192.168.200.2 | ✅ |
| 192.168.10.0/24 | S (Static) | 192.168.100.2 | ✅ |
| 192.168.100.0/30 | C (Connected) | GigabitEthernet0/0 | ✅ |
| 192.168.200.0/30 | C (Connected) | Serial0/0/0 | ✅ |

**Conclusion:** Router A has complete routing table — all networks reachable. ✅

---

## Test 9 — Routing Table (Router B)

**Objective:** Verify Router B has routes to all networks.

| Network | Type | Next Hop | Result |
|---|---|---|---|
| 192.168.1.0/24 | S (Static) | 192.168.200.1 | ✅ |
| 192.168.2.0/24 | S (Static) | 192.168.200.1 | ✅ |
| 192.168.3.0/24 | S (Static) | 192.168.200.1 | ✅ |
| 192.168.4.0/24 | C (Connected) | GigabitEthernet0/0 | ✅ |
| 192.168.5.0/24 | C (Connected) | GigabitEthernet0/1 | ✅ |
| 192.168.10.0/24 | S (Static) | 192.168.200.1 | ✅ |
| 192.168.100.0/30 | S (Static) | 192.168.200.1 | ✅ |
| 192.168.200.0/30 | C (Connected) | Serial0/0/0 | ✅ |

**Conclusion:** Router B has complete routing table — all networks reachable. ✅

---

## Final Summary

| Test | Description | Result |
|---|---|---|
| 1 | DHCP Assignment | ✅ PASS |
| 2 | Intra-VLAN Communication | ✅ PASS |
| 3 | Inter-VLAN Admin → IT | ✅ PASS |
| 4 | Inter-VLAN Admin → Sales | ✅ PASS |
| 5 | Inter-Site Branch → HQ | ✅ PASS |
| 6 | OSPF Neighbors Router A | ✅ PASS |
| 7 | OSPF Neighbors Router B | ✅ PASS |
| 8 | Routing Table Router A | ✅ PASS |
| 9 | Routing Table Router B | ✅ PASS |
