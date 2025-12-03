---
## 📡 Topology Overview

The network contains several interconnected segments:

### 🔹 **1. Internal LAN (IN Zone)**
- Subnet: **192.168.100.0/25**
- Connected to switch and routed to ASA (security level 10)
- Hosts internal clients (PC0, PC1)

### 🔹 **2. User LAN**
- Subnet: **10.10.10.0/24**
- Connected to Router2 and participates in OSPF

### 🔹 **3. Public Network + WAN**
- Subnet: **50.50.50.0/24**
- Connected to Router4 → provides external connectivity

### 🔹 **4. ASA Firewall (Cisco ASA 5506-X)**
Interfaces configured:
- **Gi1/1 — IN Zone** (security level 10)  
- **Gi1/2 — DMZ Zone** (security level 20)  
- **Gi1/3 — OUT Zone** (security level 5)

ASA functions:
- Packet inspection  
- Interface zoning  
- Static routing toward upstream routers  
- Traffic flow control between LAN ↔ DMZ ↔ Public Network  

### 🔹 **5. Partial DMZ Segment**
- Subnet: **172.16.10.0/24**
- Hosts multiple servers:
  - Server1  
  - Server2  
  - Server3  

### 🔹 **6. OSPF Area 0**
OSPF is configured between:
- Router 2911  
- Router 10.10.10.x  
- Router on 20.20.20.x segment

The ASA handles static routes pointing towards the OSPF backbone router.
---

## 📸 Topology Preview
![Topology Diagram](/assets/Topology.png)

---

## ⚙️ Key Features

### 🔐 **Firewall Zoning**
- Different security levels for IN, DMZ, and OUT  
- Controlled access between zones  

### 🛰️ **Dynamic Routing (OSPF)**
- Multi-router OSPF area 0 configuration  
- Redistributes internal subnets to ensure full path connectivity  

### 🖥️ **Server Deployment in DMZ**
- DMZ hosts web/database/test servers  
- Isolated from internal network using ASA rules  

### 🔁 **Static Routing (ASA)**
- OUT route → towards WAN (20.20.20.x)  
- INTERNAL route → ASA acts as gateway for inside and DMZ  

### 🔐 **Basic Security Configuration**
- ASA password  
- User account creation  
- Interface configuration + security levels  

---

## 🧪 Technologies Used

- **Cisco ASA 5506-X**  
- **Cisco 2911 Routers**  
- **Cisco 2960 Switches**  
- **OSPFv2 Routing Protocol**  
- **Static Routes**  
- **Security Level Zoning**  
- **DMZ Deployment**  
- **Packet Tracer (Simulation Tool)**  

---

## 🖥️ CLI Demonstrations

### 🔹 ASA Password + User Configuration
![ASA Password Configuration](/assets/passwordcliconfig.png)

### 🔹 OSPF Router Configuration
![OSPF CLI](/assets/ospfcli.png)

### 🔹 OSPF Neighbor / Routing Table
![OSPF Detailed CLI](/assets/OSPFCLI1.png)

---

## 📁 Included Files
- `cisco.pkt` — full topology and configuration  
- CLI screenshots and firewall configs (saved in `/assets`)  

---

## 🚀 Purpose of the Project
This project simulates a real-world enterprise network scenario used for:
- Learning firewall configuration  
- Understanding OSPF routing across multiple segments  
- Practicing network segmentation and DMZ isolation  
- Preparing for CCNA/CCNP Security & Enterprise certifications  
- Improving troubleshooting and topology design skills  

---
