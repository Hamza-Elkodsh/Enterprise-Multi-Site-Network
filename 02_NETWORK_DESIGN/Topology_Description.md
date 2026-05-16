# Network Topology Description

## Overview

The network follows a **hierarchical multi-switch topology** spanning two physical locations
connected via a WAN link. Each site uses a dedicated router and multiple access switches —
one per department — providing clean segmentation, scalability, and fault isolation.

---

## Topology Type

| Property | Value |
|---|---|
| Topology Type | Hierarchical Star |
| Design Pattern | Multi-Switch per Site |
| Routing Protocol | OSPF (Dynamic) |
| WAN Connection | Point-to-Point |
| Segmentation | VLAN per Department |

---

## Location A — Headquarters (HQ)

Router A is the central device at HQ. All 4 access switches connect directly to Router A,
which handles inter-VLAN routing via subinterfaces (Router-on-a-Stick).

```
                        Router A (HQ)
                    WAN IP: 192.168.200.1
                    /      |       |      \
                  Sw1     Sw2     Sw3     Sw4
                (Admin)   (IT)  (Sales) (Servers)
                VLAN 10  VLAN 20 VLAN 30 VLAN 100
```

### HQ Switches and Connected Devices

| Switch | VLAN | Department | Network | Connected Devices |
|---|---|---|---|---|
| Switch1 | 10 | Admin | 192.168.1.0/24 | PC0, PC1, PC2, PC19 |
| Switch2 | 20 | IT | 192.168.2.0/24 | PC3, PC4, PC5, PC20 |
| Switch3 | 30 | Sales | 192.168.3.0/24 | PC6, PC7, PC8, PC18 |
| Switch4 | 100 | Servers | 192.168.10.0/24 | DHCP, DNS, FTP, Web Server |

---

## Location B — Branch Office

Router B is the central device at the Branch. Both access switches connect directly
to Router B. The branch communicates with HQ through the WAN link between the two routers.

```
                      Router B (Branch)
                    WAN IP: 192.168.200.2
                         /         \
                       Sw5          Sw6
                    (Support)    (Operations)
                    VLAN 40       VLAN 50
```

### Branch Switches and Connected Devices

| Switch | VLAN | Department | Network | Connected Devices |
|---|---|---|---|---|
| Switch5 | 40 | Support | 192.168.4.0/24 | PC12, PC13, PC15, PC16 |
| Switch6 | 50 | Operations | 192.168.5.0/24 | PC9, PC10, PC11, PC17 |

---

## WAN Link

Both routers are connected via a point-to-point WAN link using a `/30` subnet.

```
    Router A ══════════════════════════ Router B
  192.168.200.1    WAN Link           192.168.200.2
                192.168.200.0/30
```

| Device | Interface | IP Address |
|---|---|---|
| Router A (HQ) | Serial/WAN | 192.168.200.1 |
| Router B (Branch) | Serial/WAN | 192.168.200.2 |

---

## Full Network Diagram (Text)

```
Location A — Headquarters
─────────────────────────────────────────────────
                    Router A
                192.168.200.1 (WAN)
               /      |      |      \
             Sw1      Sw2    Sw3    Sw4
           (Admin)   (IT)  (Sales)(Servers)
           VLAN10   VLAN20 VLAN30  VLAN100
           /24       /24    /24     /24

                        ║ WAN Link
                        ║ 192.168.200.0/30

Location B — Branch Office
─────────────────────────────────────────────────
                    Router B
                192.168.200.2 (WAN)
                   /          \
                 Sw5            Sw6
              (Support)      (Operations)
              VLAN 40          VLAN 50
               /24              /24
```

---

## Key Design Decisions

### One Switch Per Department
Each department has its own dedicated access switch. This provides:
- Fault isolation — a failed switch only affects one department
- Clean VLAN boundaries — no cross-department traffic on the same switch
- Easy scalability — add more PCs without affecting other departments

### Router Handles Inter-VLAN Routing
Router A uses subinterfaces (Router-on-a-Stick) to route traffic between VLANs.
This eliminates the need for a Layer 3 core switch while keeping the design simple
and easy to understand.

### Dedicated Server Switch
The 4 servers (DHCP, DNS, FTP, Web) are connected to a dedicated Switch4 on
VLAN 100. This isolates server traffic from department traffic and makes access
control easier to enforce via ACLs.

### Symmetric Branch Design
The Branch mirrors the HQ design pattern — each department gets its own switch
connected directly to the site router. This consistency makes configuration,
troubleshooting, and documentation straightforward.
