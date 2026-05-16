# Maintenance Guide

## Overview

This guide covers routine maintenance tasks, network monitoring procedures,
and expansion guidelines for the enterprise network.

---

## 1. Routine Maintenance Tasks

### Daily Checks

| Task | Command | Expected Result |
|---|---|---|
| Check all interfaces up | `show ip interface brief` | All active interfaces up/up |
| Verify OSPF neighbors | `show ip ospf neighbor` | All neighbors FULL state |
| Check routing tables | `show ip route` | All 8 networks present |
| Verify DHCP leases | DHCP Server → Services → DHCP | Active leases for all departments |

### Weekly Checks

| Task | Command | Purpose |
|---|---|---|
| Check VLAN status | `show vlan brief` | Confirm all VLANs active |
| Review ACL counters | `show ip access-lists` | Monitor blocked traffic |
| Check interface errors | `show interfaces` | Detect physical issues |
| Verify server services | Ping each server | Confirm availability |

---

## 2. Saving Configurations

Always save configuration after any changes:

```
! On any device
write memory
```

Or alternatively:
```
copy running-config startup-config
```

To verify saved config:
```
show startup-config
```

---

## 3. Adding a New PC to Existing VLAN

### Step 1 — Connect PC to correct switch
Plug PC into an available port on the department switch.

### Step 2 — Verify port is in correct VLAN
```
Switch1# show vlan brief
```

### Step 3 — If port is not in VLAN, assign it
```
Switch1# configure terminal
Switch1(config)# interface fastEthernet 0/6
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# no shutdown
Switch1(config-if)# exit
Switch1(config)# end
Switch1# write memory
```

### Step 4 — Set PC to DHCP
PC → Desktop → IP Configuration → select **DHCP**

### Step 5 — Verify IP assignment
PC should receive IP in correct range automatically.

---

## 4. Adding a New VLAN

### Step 1 — Create VLAN on MLSwitch
```
MLSwitch# configure terminal
MLSwitch(config)# vlan 60
MLSwitch(config-vlan)# name NewDepartment
MLSwitch(config-vlan)# exit
```

### Step 2 — Create SVI on MLSwitch
```
MLSwitch(config)# interface vlan 60
MLSwitch(config-if)# ip address 192.168.6.1 255.255.255.0
MLSwitch(config-if)# ip helper-address 192.168.10.2
MLSwitch(config-if)# no shutdown
MLSwitch(config-if)# exit
```

### Step 3 — Add DHCP Pool on DHCP Server
DHCP Server → Services → DHCP → Add new pool:

| Field | Value |
|---|---|
| Pool Name | NewDept_Pool |
| Default Gateway | 192.168.6.1 |
| DNS Server | 192.168.10.3 |
| Start IP | 192.168.6.10 |
| Subnet Mask | 255.255.255.0 |
| Max Users | 245 |

### Step 4 — Connect new access switch
```
MLSwitch(config)# interface GigabitEthernet1/8
MLSwitch(config-if)# switchport mode trunk
MLSwitch(config-if)# switchport trunk allowed vlan 60
MLSwitch(config-if)# switchport trunk native vlan 60
MLSwitch(config-if)# no shutdown
MLSwitch(config-if)# exit
```

### Step 5 — Update OSPF if needed
```
MLSwitch(config)# router ospf 1
MLSwitch(config-router)# network 192.168.6.0 0.0.0.255 area 0
MLSwitch(config-router)# exit
```

---

## 5. Changing a Device IP Address

### Step 1 — Remove old IP
```
RouterA(config)# interface GigabitEthernet0/0
RouterA(config-if)# no ip address
```

### Step 2 — Assign new IP
```
RouterA(config-if)# ip address [new-ip] [subnet-mask]
RouterA(config-if)# no shutdown
RouterA(config-if)# exit
```

### Step 3 — Update static routes if needed
```
RouterA(config)# no ip route [old-network] [mask] [old-next-hop]
RouterA(config)# ip route [network] [mask] [new-next-hop]
```

### Step 4 — Save and verify
```
RouterA# write memory
RouterA# show ip route
```

---

## 6. Resetting a Device to Default

### Router/Switch Full Reset
```
enable
write erase
reload
```
Type **yes** when prompted. Device will reload with blank config.

### Clear Only Routing Table
```
clear ip route *
```

### Clear Only OSPF
```
clear ip ospf process
```

---

## 7. Monitoring ACL Traffic

To see how many packets each ACL rule is matching:
```
MLSwitch# show ip access-lists
RouterB# show ip access-lists
```

To reset ACL counters:
```
MLSwitch# clear ip access-list counters
```

---

## 8. Backup and Restore

### Backup Configuration
In Packet Tracer:
1. Click device → CLI
2. Type `show running-config`
3. Copy all output to a text file
4. Save as `DeviceName_backup.txt`

### Restore Configuration
1. Open saved text file
2. Click device → CLI
3. Enter `configure terminal`
4. Paste configuration
5. Type `end` then `write memory`

---

## 9. Network Expansion Guidelines

| Expansion | Action Required |
|---|---|
| Add PC to existing VLAN | Connect to switch, set DHCP |
| Add new department at HQ | New VLAN + SVI + switch + DHCP pool |
| Add new department at Branch | New interface on Router B + switch + DHCP pool |
| Add second Branch site | New router + WAN link + OSPF network statements |
| Add new server | Connect to Switch4 VLAN 100, set static IP |

---

## 10. IP Address Reference

| Device | IP Address | Role |
|---|---|---|
| MLSwitch Vlan10 | 192.168.1.1 | Admin Gateway |
| MLSwitch Vlan20 | 192.168.2.1 | IT Gateway |
| MLSwitch Vlan30 | 192.168.3.1 | Sales Gateway |
| MLSwitch Vlan100 | 192.168.10.1 | Server Gateway |
| MLSwitch Gi1/7 | 192.168.100.2 | Link to Router A |
| Router A Gi0/0 | 192.168.100.1 | Link to MLSwitch |
| Router A Se0/0/0 | 192.168.200.1 | WAN to Router B |
| Router B Se0/0/0 | 192.168.200.2 | WAN to Router A |
| Router B Gi0/0 | 192.168.4.1 | Support Gateway |
| Router B Gi0/1 | 192.168.5.1 | Operations Gateway |
| DHCP Server | 192.168.10.2 | DHCP Service |
| DNS Server | 192.168.10.3 | DNS Service |
| FTP Server | 192.168.10.4 | FTP Service |
| Web Server | 192.168.10.5 | HTTP Service |
