# Setup Guide

## Overview

This guide provides step-by-step instructions for setting up the Enterprise
Multi-Site Network from scratch in Cisco Packet Tracer.

---

## Prerequisites

| Requirement | Details |
|---|---|
| Software | Cisco Packet Tracer 8.0 or higher |
| File | enterprise_network.pkt |
| Knowledge | Basic Cisco IOS CLI commands |
| Time | Approximately 2-3 hours |

---

## Step 1 — Open the Project

1. Launch **Cisco Packet Tracer**
2. Go to **File → Open**
3. Navigate to `07_PACKET_TRACER/enterprise_network.pkt`
4. Click **Open**

The network topology will load with all devices visible across both sites.

---

## Step 2 — Verify Physical Connections

Before configuring, verify all cables are connected correctly:

### Headquarters (Location A)

| From | Port | To | Port |
|---|---|---|---|
| PC0 | Fa0 | Switch1 | Fa0/1 |
| PC1 | Fa0 | Switch1 | Fa0/2 |
| PC2 | Fa0 | Switch1 | Fa0/3 |
| PC19 | Fa0 | Switch1 | Fa0/4 |
| Switch1 | Fa0/5 | MLSwitch | Gi1/3 |
| PC3 | Fa0 | Switch2 | Fa0/1 |
| PC4 | Fa0 | Switch2 | Fa0/2 |
| PC5 | Fa0 | Switch2 | Fa0/3 |
| PC20 | Fa0 | Switch2 | Fa0/4 |
| Switch2 | Fa0/5 | MLSwitch | Gi1/4 |
| PC6 | Fa0 | Switch3 | Fa0/1 |
| PC7 | Fa0 | Switch3 | Fa0/2 |
| PC8 | Fa0 | Switch3 | Fa0/3 |
| PC18 | Fa0 | Switch3 | Fa0/4 |
| Switch3 | Fa0/5 | MLSwitch | Gi1/5 |
| DHCP Server | Fa0 | Switch4 | Fa0/1 |
| DNS Server | Fa0 | Switch4 | Fa0/2 |
| FTP Server | Fa0 | Switch4 | Fa0/3 |
| Web Server | Fa0 | Switch4 | Fa0/4 |
| Switch4 | Fa0/5 | MLSwitch | Gi1/6 |
| MLSwitch | Gi1/7 | Router A | Gi0/0 |
| Router A | Se0/0/0 | Router B | Se0/0/0 |

### Branch Office (Location B)

| From | Port | To | Port |
|---|---|---|---|
| PC12 | Fa0 | Switch5 | Fa0/1 |
| PC13 | Fa0 | Switch5 | Fa0/2 |
| PC15 | Fa0 | Switch5 | Fa0/3 |
| PC16 | Fa0 | Switch5 | Fa0/4 |
| Switch5 | Fa0/5 | Router B | Gi0/0 |
| PC9 | Fa0 | Switch6 | Fa0/1 |
| PC10 | Fa0 | Switch6 | Fa0/2 |
| PC11 | Fa0 | Switch6 | Fa0/3 |
| PC17 | Fa0 | Switch6 | Fa0/4 |
| Switch6 | Fa0/5 | Router B | Gi0/1 |

---

## Step 3 — Configure Server Static IPs

### DHCP Server (192.168.10.2)
Click DHCP Server → **Desktop → IP Configuration → Static**

| Field | Value |
|---|---|
| IP Address | 192.168.10.2 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.10.1 |
| DNS Server | 192.168.10.3 |

### DNS Server (192.168.10.3)
| Field | Value |
|---|---|
| IP Address | 192.168.10.3 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.10.1 |

### FTP Server (192.168.10.4)
| Field | Value |
|---|---|
| IP Address | 192.168.10.4 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.10.1 |

### Web Server (192.168.10.5)
| Field | Value |
|---|---|
| IP Address | 192.168.10.5 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.10.1 |

---

## Step 4 — Configure DHCP Server Pools

Click DHCP Server → **Services → DHCP → Service ON**

Add the following pools one by one:

| Pool Name | Default Gateway | DNS Server | Start IP | Subnet Mask | Max Users |
|---|---|---|---|---|---|
| Admin_Pool | 192.168.1.1 | 192.168.10.3 | 192.168.1.10 | 255.255.255.0 | 245 |
| IT_Pool | 192.168.2.1 | 192.168.10.3 | 192.168.2.10 | 255.255.255.0 | 245 |
| Sales_Pool | 192.168.3.1 | 192.168.10.3 | 192.168.3.10 | 255.255.255.0 | 245 |
| Support_Pool | 192.168.4.1 | 192.168.10.3 | 192.168.4.10 | 255.255.255.0 | 245 |
| Operations_Pool | 192.168.5.1 | 192.168.10.3 | 192.168.5.10 | 255.255.255.0 | 245 |

Click **Add** after each pool then **Save**.

---

## Step 5 — Configure DNS Server

Click DNS Server → **Services → DNS → DNS Service ON**

Add the following records:

| Name | Type | Address |
|---|---|---|
| dhcp.techcore.local | A Record | 192.168.10.2 |
| dns.techcore.local | A Record | 192.168.10.3 |
| ftp.techcore.local | A Record | 192.168.10.4 |
| www.techcore.local | A Record | 192.168.10.5 |

---

## Step 6 — Configure MLSwitch

Click MLSwitch → **CLI tab** and paste:

```
enable
configure terminal
ip routing
service dhcp
vlan 10
name Admin
exit
vlan 20
name IT
exit
vlan 30
name Sales
exit
vlan 100
name Servers
exit
interface vlan 10
ip address 192.168.1.1 255.255.255.0
ip helper-address 192.168.10.2
no shutdown
exit
interface vlan 20
ip address 192.168.2.1 255.255.255.0
ip helper-address 192.168.10.2
no shutdown
exit
interface vlan 30
ip address 192.168.3.1 255.255.255.0
ip helper-address 192.168.10.2
no shutdown
exit
interface vlan 100
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
interface GigabitEthernet1/3
switchport mode trunk
switchport trunk allowed vlan 10
switchport trunk native vlan 10
no shutdown
exit
interface GigabitEthernet1/4
switchport mode trunk
switchport trunk allowed vlan 20
switchport trunk native vlan 20
no shutdown
exit
interface GigabitEthernet1/5
switchport mode trunk
switchport trunk allowed vlan 30
switchport trunk native vlan 30
no shutdown
exit
interface GigabitEthernet1/6
switchport mode trunk
switchport trunk allowed vlan 100
switchport trunk native vlan 100
no shutdown
exit
interface GigabitEthernet1/7
no switchport
ip address 192.168.100.2 255.255.255.252
no shutdown
exit
ip route 0.0.0.0 0.0.0.0 192.168.100.1
ip route 192.168.4.0 255.255.255.0 192.168.100.1
ip route 192.168.5.0 255.255.255.0 192.168.100.1
ip route 192.168.200.0 255.255.255.252 192.168.100.1
router ospf 1
router-id 3.3.3.3
network 192.168.1.0 0.0.0.255 area 0
network 192.168.2.0 0.0.0.255 area 0
network 192.168.3.0 0.0.0.255 area 0
network 192.168.10.0 0.0.0.255 area 0
network 192.168.100.0 0.0.0.3 area 0
exit
end
write memory
```

---

## Step 7 — Configure Access Switches (HQ)

Apply the following to each switch:

### Switch1 (Admin)
```
enable
configure terminal
hostname Switch1
vlan 10
name Admin
exit
interface range fastEthernet 0/1 - 4
switchport mode access
switchport access vlan 10
no shutdown
exit
interface fastEthernet 0/5
switchport mode trunk
switchport trunk allowed vlan 10
switchport trunk native vlan 10
no shutdown
exit
interface range fastEthernet 0/6 - 24
shutdown
exit
interface vlan 10
ip address 192.168.1.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1
end
write memory
```

Repeat same pattern for Switch2 (VLAN 20), Switch3 (VLAN 30), Switch4 (VLAN 100).

---

## Step 8 — Configure Router A

```
enable
configure terminal
hostname RouterA
interface GigabitEthernet0/0
ip address 192.168.100.1 255.255.255.252
no shutdown
exit
interface serial 0/0/0
ip address 192.168.200.1 255.255.255.252
no shutdown
exit
ip route 192.168.1.0 255.255.255.0 192.168.100.2
ip route 192.168.2.0 255.255.255.0 192.168.100.2
ip route 192.168.3.0 255.255.255.0 192.168.100.2
ip route 192.168.10.0 255.255.255.0 192.168.100.2
ip route 192.168.4.0 255.255.255.0 192.168.200.2
ip route 192.168.5.0 255.255.255.0 192.168.200.2
router ospf 1
router-id 1.1.1.1
network 192.168.100.0 0.0.0.3 area 0
network 192.168.200.0 0.0.0.3 area 0
exit
end
write memory
```

---

## Step 9 — Configure Router B

```
enable
configure terminal
hostname RouterB
interface serial 0/0/0
ip address 192.168.200.2 255.255.255.252
no shutdown
exit
interface GigabitEthernet0/0
ip address 192.168.4.1 255.255.255.0
ip helper-address 192.168.10.2
no shutdown
exit
interface GigabitEthernet0/1
ip address 192.168.5.1 255.255.255.0
ip helper-address 192.168.10.2
no shutdown
exit
ip route 192.168.1.0 255.255.255.0 192.168.200.1
ip route 192.168.2.0 255.255.255.0 192.168.200.1
ip route 192.168.3.0 255.255.255.0 192.168.200.1
ip route 192.168.10.0 255.255.255.0 192.168.200.1
ip route 192.168.100.0 255.255.255.252 192.168.200.1
router ospf 1
router-id 2.2.2.2
network 192.168.4.0 0.0.0.255 area 0
network 192.168.5.0 0.0.0.255 area 0
network 192.168.200.0 0.0.0.3 area 0
exit
end
write memory
```

---

## Step 10 — Configure Branch Switches

### Switch5 (Support)
```
enable
configure terminal
hostname Switch5
vlan 40
name Support
exit
interface range fastEthernet 0/1 - 5
switchport mode access
switchport access vlan 40
no shutdown
exit
interface range fastEthernet 0/6 - 24
shutdown
exit
interface vlan 40
ip address 192.168.4.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.4.1
end
write memory
```

### Switch6 (Operations)
```
enable
configure terminal
hostname Switch6
vlan 50
name Operations
exit
interface range fastEthernet 0/1 - 5
switchport mode access
switchport access vlan 50
no shutdown
exit
interface range fastEthernet 0/6 - 24
shutdown
exit
interface vlan 50
ip address 192.168.5.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.5.1
end
write memory
```

---

## Step 11 — Set PCs to DHCP

On each PC → **Desktop → IP Configuration → select DHCP**

Verify each PC receives the correct IP:

| PC | Expected IP |
|---|---|
| PC0-PC19 (Admin) | 192.168.1.10-13 |
| PC3-PC20 (IT) | 192.168.2.10-13 |
| PC6-PC18 (Sales) | 192.168.3.10-13 |
| PC12-PC16 (Support) | 192.168.4.10-13 |
| PC9-PC17 (Operations) | 192.168.5.10-13 |

---

## Step 12 — Verify Network

Run these verification commands:

```
RouterA# show ip ospf neighbor
RouterA# show ip route
RouterB# show ip ospf neighbor
RouterB# show ip route
MLSwitch# show vlan brief
MLSwitch# show ip route
```

All tests should pass as documented in `05_TESTING_RESULTS/`.
