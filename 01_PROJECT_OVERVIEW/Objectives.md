# Project Objectives

## Primary Goal

Design, configure, and verify a fully functional multi-site enterprise network that reflects
real-world networking practices, using Cisco devices in Cisco Packet Tracer.

---

## Technical Objectives

### 1. Network Design
- Design a scalable and logical network topology for two office locations
- Define a structured IP addressing plan using the `192.168.0.0/16` address space
- Segment the network into VLANs for each department

### 2. VLAN Configuration
- Create and name VLANs on all switches
- Assign correct access ports to each department VLAN
- Configure trunk links between switches and routers

### 3. Inter-VLAN Routing
- Enable communication between VLANs using Router-on-a-Stick (subinterfaces)
- Verify that devices in different VLANs can communicate where permitted

### 4. WAN Connectivity
- Connect Headquarters and Branch Office using a point-to-point WAN link
- Assign WAN IP addresses: Router A (`192.168.200.1`) and Router B (`192.168.200.2`)

### 5. Dynamic Routing with OSPF
- Configure OSPF on both routers
- Advertise all networks so both sites have full routing knowledge
- Verify OSPF neighbor relationships and routing tables

### 6. DHCP Configuration
- Set up a DHCP server at HQ to serve IP addresses to all departments
- Configure DHCP pools for each VLAN/subnet
- Verify automatic IP assignment on end devices

### 7. Server Integration
- Deploy and configure DNS, FTP, and Web servers on the server VLAN
- Verify all servers are reachable from both HQ and Branch departments

### 8. Security with ACLs
- Write and apply Access Control Lists to:
  - Restrict unauthorized inter-department communication
  - Control access to sensitive server resources
  - Filter traffic on WAN-facing interfaces
- Verify ACL rules with live testing

### 9. Testing & Verification
- Perform end-to-end ping tests across all VLANs and sites
- Verify OSPF routing tables on both routers
- Confirm DHCP leases and server accessibility
- Document all test results with output evidence

### 10. Documentation
- Produce complete project documentation covering setup, configuration, and troubleshooting
- Organize all files in the defined project folder structure

---

## Learning Objectives

By completing this project, the following CCNA-aligned skills are practiced:

| Skill Area | Topics Covered |
|---|---|
| Network Design | Topology planning, IP subnetting, VLAN strategy |
| Switching | VLAN creation, trunk/access ports, STP awareness |
| Routing | OSPF configuration, route verification, inter-VLAN routing |
| Security | Standard and extended ACLs, traffic filtering |
| Services | DHCP, DNS, FTP, HTTP server setup |
| Troubleshooting | Ping, show commands, routing table analysis |
| Documentation | Technical writing, configuration archiving |

---

## Success Criteria

The project is considered complete when:

- All end devices receive IP addresses via DHCP
- All devices within the same VLAN can communicate
- Inter-VLAN routing works between permitted VLANs
- Branch Office devices can reach HQ resources
- OSPF is active and both routers share full routing tables
- ACL rules correctly permit and deny traffic as designed
- All servers (DHCP, DNS, FTP, Web) are reachable from authorized hosts
- All tests pass and results are documented
