# Enterprise Multi-Site Network Implementation

> A professional two-site enterprise network built in Cisco Packet Tracer, simulating real-world infrastructure with VLAN segmentation, OSPF routing, ACL security, and centralized services.

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-1D9E75?style=flat-square&logo=cisco&logoColor=white)
![OSPF](https://img.shields.io/badge/Routing-OSPF-378ADD?style=flat-square)
![VLANs](https://img.shields.io/badge/Segmentation-VLANs-7F77DD?style=flat-square)
![ACLs](https://img.shields.io/badge/Security-ACLs-888780?style=flat-square)
![CCNA](https://img.shields.io/badge/Concepts-CCNA-1D9E75?style=flat-square)

---

## 📌 Project Overview

This project demonstrates the design and implementation of a professional enterprise network connecting two office locations using Cisco networking technologies in Cisco Packet Tracer.

The network was built to simulate a real-world enterprise environment with:

- Multiple departments with isolated VLANs
- Dynamic routing using OSPF across a WAN link
- Access Control Lists (ACLs) for inter-department security
- Centralized server infrastructure (DHCP, DNS, FTP, Web)
- Full documentation and Packet Tracer simulation file

---

## 🏢 Network Topology

### Location A — Headquarters

| Department | VLAN | Network |
|------------|------|---------|
| Admin | 10 | 192.168.1.0/24 |
| IT | 20 | 192.168.2.0/24 |
| Sales | 30 | 192.168.3.0/24 |
| Servers | 100 | 192.168.10.0/24 |

### Location B — Branch Office

| Department | VLAN | Network |
|------------|------|---------|
| Support | 40 | 192.168.4.0/24 |
| Operations | 50 | 192.168.5.0/24 |

### WAN Link

| Device | IP Address |
|--------|------------|
| Router A (HQ) | 192.168.200.1 |
| Router B (Branch) | 192.168.200.2 |

Both locations communicate over a WAN connection between the two routers using `192.168.0.0/16` as the main network.

---

## ✨ Key Features

- ✅ Multi-site enterprise network across two locations
- ✅ VLAN segmentation per department
- ✅ Dynamic routing with OSPF
- ✅ Secure ACL-based traffic filtering
- ✅ Centralized DHCP, DNS, FTP, and Web services
- ✅ Scalable and fully documented design
- ✅ Ready-to-run Cisco Packet Tracer simulation

---

## 🛠️ Technologies Used

| Category | Technologies |
|----------|-------------|
| Switching | VLANs, Inter-VLAN Routing, Trunking |
| Routing | OSPF, Static Routes, WAN Connectivity |
| Security | Access Control Lists (ACLs) |
| Services | DHCP, DNS, FTP, Web Server |
| Tools | Cisco Packet Tracer, Subnetting |

---

## 🖥️ Equipment

| Device | Quantity | Details |
|--------|----------|---------|
| Cisco Routers | 2 | One per site |
| Core Switch | 1 | Headquarters |
| Access Switches | 3 | Department-level |
| Servers | 4 | DHCP, DNS, FTP, Web |
| PCs | Per dept. | One per department |

---

## 🔐 Security Implementation

ACLs are configured to:

- Restrict unauthorized communication between departments
- Control access to the server network (VLAN 100)
- Simulate enterprise-level traffic filtering
- Enforce the principle of least privilege across VLANs

---

## ⚙️ Configuration Summary

| Device | Approx. Lines |
|--------|--------------|
| Router A | ~45 |
| Router B | ~35 |
| Core Switch | ~30 |
| Access Switches | ~40 |
| **Total** | **~150** |

All configurations are included and ready to use in the `04_CONFIGURATION/` folder.

---

## 📂 Project Structure

```
ENTERPRISE_NETWORK_PROJECT/
│
├── README.md
├── LICENSE
│
├── 01_PROJECT_OVERVIEW/
│   ├── Project_Description.md
│   ├── Objectives.md
│   ├── Company_Scenario.md
│   └── Key_Features.md
│
├── 02_NETWORK_DESIGN/
│   ├── Network_Topology_Diagram.png
│   ├── Topology_Description.md
│   ├── Equipment_List.md
│   └── Physical_Layout.md
│
├── 03_IP_ADDRESSING/
│   ├── IP_Addressing_Plan.md
│   ├── Subnetting_Calculations.md
│   ├── IP_Address_Table.csv
│   └── VLAN_Assignment.md
│
├── 04_CONFIGURATION/
│   ├── Router_Config.txt
│   ├── Switch_Config.txt
│   ├── VLAN_Setup.txt
│   ├── Routing_Commands.txt
│   └── ACL_Rules.txt
│
├── 05_TESTING_RESULTS/
│   ├── Connectivity_Tests.md
│   ├── Ping_Results.txt
│   ├── Routing_Verification.md
│   └── Screenshots/
│
├── 06_DOCUMENTATION/
│   ├── Setup_Guide.md
│   ├── Troubleshooting_Guide.md
│   └── Maintenance_Guide.md
│
└── 07_PACKET_TRACER/
    ├── enterprise_network.pkt
    └── How_To_Open.md
```

---

## 🚀 How to Run

1. **Install Cisco Packet Tracer** — Download from the [NetAcad website](https://www.netacad.com)
2. **Open the project file** — Navigate to `07_PACKET_TRACER/enterprise_network.pkt`
3. **Start simulation** — Test ping connectivity, routing tables, VLAN communication, and ACL rules

---

## 🧪 Testing & Verification

The network was tested for:

- End-to-end connectivity across sites
- VLAN isolation and inter-VLAN routing
- OSPF neighbor relationships and route propagation
- ACL filtering between departments
- Server reachability (DHCP, DNS, FTP, Web)

Results and screenshots are included in `05_TESTING_RESULTS/`.

---

## 📖 Learning Objectives

This project helps practice:

- Enterprise network design and planning
- VLAN configuration and trunking
- Router and switch CLI configuration
- OSPF dynamic routing protocol
- Network security with ACLs
- Troubleshooting and verification techniques

---

## 👨‍💻 Author

**Hamza Mohamed**
Computer Science Student — Cyber Security
Passionate about Networking, Cybersecurity, and Enterprise Infrastructure.

---

## 📄 License

This project is for educational and learning purposes only.
