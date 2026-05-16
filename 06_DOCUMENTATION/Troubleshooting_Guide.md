# Troubleshooting Guide

## Overview

This guide covers the most common issues encountered in the enterprise network
and provides step-by-step solutions for each problem.

---

## Quick Diagnostic Checklist

Before diving into specific issues, run these commands first:

```
! On any router
show ip interface brief
show ip route
show ip ospf neighbor

! On any switch
show vlan brief
show interfaces status

! On any PC
ping [default gateway]
```

---

## Issue 1 — PC Cannot Get DHCP IP Address

**Symptom:** PC shows "DHCP request failed" or 0.0.0.0

### Step 1 — Check PC is set to DHCP
PC → Desktop → IP Configuration → confirm **DHCP is selected**

### Step 2 — Check DHCP Server is ON
DHCP Server → Services → DHCP → confirm **Service is ON**

### Step 3 — Check DHCP pool exists for that VLAN
Confirm pool exists with correct gateway:

| Department | Pool Name | Gateway |
|---|---|---|
| Admin | Admin_Pool | 192.168.1.1 |
| IT | IT_Pool | 192.168.2.1 |
| Sales | Sales_Pool | 192.168.3.1 |
| Support | Support_Pool | 192.168.4.1 |
| Operations | Operations_Pool | 192.168.5.1 |

### Step 4 — Check ip helper-address on gateway device
For HQ VLANs (on MLSwitch):
```
show run | section Vlan10
show run | section Vlan20
show run | section Vlan30
```
Must show `ip helper-address 192.168.10.2`

For Branch VLANs (on Router B):
```
show run | section GigabitEthernet0/0
show run | section GigabitEthernet0/1
```
Must show `ip helper-address 192.168.10.2`

### Step 5 — Check gateway can reach DHCP server
```
MLSwitch# ping 192.168.10.2
RouterB# ping 192.168.10.2
```
Both must succeed. If not, check routing.

### Step 6 — Check VLAN is active on switch
```
Switch1# show vlan brief
```
PC port must appear under correct VLAN number.

---

## Issue 2 — PC Cannot Ping Its Gateway

**Symptom:** ping to default gateway times out

### Step 1 — Check PC has correct static IP (for testing)
Set manually: IP=192.168.x.10, Mask=255.255.255.0, GW=192.168.x.1

### Step 2 — Check switch port is in correct VLAN
```
Switch1# show vlan brief
```
PC port (Fa0/1) must be listed under VLAN 10

### Step 3 — Check trunk port is correct
```
Switch1# show interfaces fastEthernet 0/5 switchport
```
Must show: Mode = trunk, Native VLAN = correct VLAN

### Step 4 — Check SVI is up on MLSwitch
```
MLSwitch# show ip interface brief
```
Vlan10 must show **up/up**

### Step 5 — Check native VLAN matches on both ends
```
MLSwitch# show run | section GigabitEthernet1/3
Switch1# show run | section fastEthernet 0/5
```
Both must have same native VLAN number

---

## Issue 3 — Inter-VLAN Routing Not Working

**Symptom:** PC in VLAN 10 cannot ping PC in VLAN 20

### Step 1 — Check ip routing is enabled on MLSwitch
```
MLSwitch# show run | include ip routing
```
Must show `ip routing`

### Step 2 — Check both SVIs are up
```
MLSwitch# show ip interface brief
```
Both Vlan10 and Vlan20 must be **up/up**

### Step 3 — Check routing table on MLSwitch
```
MLSwitch# show ip route
```
Both 192.168.1.0 and 192.168.2.0 must appear as connected

### Step 4 — Check ACL is not blocking traffic
```
MLSwitch# show ip access-lists
```
Check if any ACL is denying the traffic

---

## Issue 4 — Branch Cannot Reach HQ

**Symptom:** Branch PCs cannot ping HQ devices or servers

### Step 1 — Check WAN link is up
```
RouterB# show ip interface brief
```
Serial0/0/0 must be **up/up** with IP 192.168.200.2

### Step 2 — Check OSPF neighbor is FULL
```
RouterB# show ip ospf neighbor
```
Must show Router A (1.1.1.1) as **FULL**

### Step 3 — Check routing table on Router B
```
RouterB# show ip route
```
Must show routes to 192.168.1.0, 192.168.2.0, 192.168.3.0, 192.168.10.0

### Step 4 — Ping each hop from Router B
```
RouterB# ping 192.168.200.1    (Router A WAN)
RouterB# ping 192.168.100.1    (Router A LAN)
RouterB# ping 192.168.100.2    (MLSwitch)
RouterB# ping 192.168.10.2     (DHCP Server)
```
Identify exactly where the path breaks

### Step 5 — Check MLSwitch has return routes
```
MLSwitch# show ip route
```
Must show routes to 192.168.4.0 and 192.168.5.0

---

## Issue 5 — OSPF Neighbors Not Forming

**Symptom:** show ip ospf neighbor returns empty

### Step 1 — Check interfaces are up
```
RouterA# show ip interface brief
RouterB# show ip interface brief
```
WAN interfaces must be **up/up**

### Step 2 — Check OSPF is configured
```
RouterA# show run | section ospf
RouterB# show run | section ospf
```
Both must have `router ospf 1` with correct networks

### Step 3 — Check network statements match
Router A WAN network:
```
network 192.168.200.0 0.0.0.3 area 0
```
Router B WAN network:
```
network 192.168.200.0 0.0.0.3 area 0
```
Both must be in **same area (area 0)**

### Step 4 — Clear OSPF process
```
RouterA# clear ip ospf process
RouterB# clear ip ospf process
```
Type **yes** when prompted, then check neighbors again after 30 seconds

---

## Issue 6 — VLAN Not Active on Switch

**Symptom:** show vlan brief shows VLAN as inactive or missing

### Step 1 — Create VLAN manually
```
Switch1# configure terminal
Switch1(config)# vlan 10
Switch1(config-vlan)# name Admin
Switch1(config-vlan)# exit
```

### Step 2 — Assign ports to VLAN
```
Switch1(config)# interface fastEthernet 0/1
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit
```

### Step 3 — Verify
```
Switch1# show vlan brief
```
VLAN 10 must show as **active** with ports listed

---

## Issue 7 — Server Not Reachable

**Symptom:** Cannot ping DHCP/DNS/FTP/Web server

### Step 1 — Check server has correct static IP
Click server → Desktop → IP Configuration → confirm static IP

### Step 2 — Check server is connected to Switch4
Switch4 → show vlan brief → server port must be in VLAN 100

### Step 3 — Check MLSwitch Vlan100 SVI is up
```
MLSwitch# show ip interface brief
```
Vlan100 must be **up/up** with IP 192.168.10.1

### Step 4 — Ping server from MLSwitch
```
MLSwitch# ping 192.168.10.2
```
If this fails, check Switch4 VLAN 100 configuration

---

## Common Commands Reference

| Command | Purpose |
|---|---|
| `show ip interface brief` | Check interface status and IPs |
| `show ip route` | View routing table |
| `show ip ospf neighbor` | Check OSPF adjacencies |
| `show vlan brief` | Check VLAN status and ports |
| `show run | section ospf` | View OSPF config |
| `show run | section vlan` | View VLAN config |
| `show ip access-lists` | View ACL match counts |
| `ping [ip]` | Test connectivity |
| `clear ip ospf process` | Reset OSPF process |
| `write memory` | Save configuration |
