# 🌐 Enterprise Network Security Infrastructure
## Multi-Zone Cisco ASA Firewall with OSPF Routing Protocol

<div align="center">

![Cisco](https://img.shields.io/badge/Cisco-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![ASA](https://img.shields.io/badge/ASA_5506X-FF0000?style=for-the-badge)
![OSPF](https://img.shields.io/badge/OSPF-00BCB4?style=for-the-badge)
![Security](https://img.shields.io/badge/Network_Security-4A154B?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Production_Ready-success?style=for-the-badge)

**Advanced enterprise-grade network architecture with zone-based firewall and dynamic routing**

*Complete implementation of ASA security appliance with OSPF protocol integration, multi-segment architecture, and DMZ isolation*

[View Topology](#-network-topology) • [Configuration Guide](#-complete-configuration-guide) • [Security Zones](#-security-architecture) • [OSPF Setup](#-ospf-configuration)

</div>

---

## 📋 Executive Summary

This project demonstrates a **production-ready enterprise network security infrastructure** implementing Cisco ASA 5506-X firewall with advanced features:

### 🎯 **Core Implementation**
- **Zone-Based Security Architecture** - Three-tier security model (IN/DMZ/OUT) with granular access control
- **Dynamic Routing Protocol** - OSPFv2 Area 0 for scalable network connectivity
- **Demilitarized Zone (DMZ)** - Isolated server farm for public-facing services
- **Static & Dynamic Routing Integration** - Hybrid routing architecture for enterprise scalability
- **Multi-Segment Network Design** - Four distinct network segments with controlled inter-VLAN routing
- **Enterprise-Grade Security** - Stateful packet inspection, zone-based policies, and connection tracking

### 📊 **Network Statistics**
```
Total Network Segments:    4
Routing Protocol:          OSPF Area 0
Security Zones:            3 (IN/DMZ/OUT)
Routers Deployed:          3 (Cisco 2911)
Switches:                  2 (Cisco 2960-24TT)
Firewall:                  1 (Cisco ASA 5506-X)
Total IP Networks:         5
Server Infrastructure:     3 (DMZ Zone)
Workstation Nodes:         4
```

---

## 🖼️ Network Topology

<div align="center">
  <img src="assets/topology-full.png" alt="Complete Enterprise Network Architecture" width="1000"/>
  <p><em>Multi-zone enterprise network with ASA firewall, OSPF backbone, and DMZ infrastructure</em></p>
</div>

### 🏗️ Architecture Overview

```
                                    INTERNET
                                        │
                                   [Router4]
                                  50.50.50.1
                                        │
                    ┌───────────────────┴───────────────────┐
                    │                                       │
              [OSPF Router]                          [ASA 5506-X]
             20.20.20.0/24                          Gi1/3 (OUT)
                    │                               Security: 0
                    │                                      │
            ┌───────┴────────┐                   ┌────────┴────────┐
            │                │                   │                 │
      [Router2]         [Router1]          Gi1/1 (IN)        Gi1/2 (DMZ)
    10.10.10.0/24     192.168.100.0/25    Security: 10      Security: 20
            │                │                   │                 │
        [PC4]          [Switch 2960]       [Switch 2960]     [DMZ Servers]
                       PC0, PC1, PC2              │           S1, S2, S3
                                            192.168.100.0/25  172.16.10.0/24
```

---

## 🔐 Security Architecture

### Zone Hierarchy & Trust Levels

<div align="center">

| Security Zone | Interface | Security Level | Trust Level | Purpose |
|:-------------:|:---------:|:--------------:|:-----------:|:--------|
| 🟢 **IN** | Gi1/1 | **10** | Highest | Internal corporate network |
| 🟡 **DMZ** | Gi1/2 | **20** | Medium | Public-facing servers |
| 🔴 **OUT** | Gi1/3 | **5** | Lowest | Internet gateway |

</div>

### Security Policy Matrix

```
┌─────────┬──────────┬──────────┬──────────┐
│ From/To │    IN    │   DMZ    │   OUT    │
├─────────┼──────────┼──────────┼──────────┤
│   IN    │    ✓     │    ✓     │    ✓     │
│   DMZ   │    ✗     │    ✓     │    ✓     │
│   OUT   │    ✗     │    ✗     │    ✓     │
└─────────┴──────────┴──────────┴──────────┘

✓ = Allowed by default (higher → lower security)
✗ = Denied by default (requires ACL)
```

### Traffic Flow Control

- **IN → DMZ**: ✅ Management access to DMZ servers
- **IN → OUT**: ✅ Internet access for internal users
- **DMZ → OUT**: ✅ Servers can access internet (updates, patches)
- **DMZ → IN**: ❌ **BLOCKED** - Servers cannot reach internal network
- **OUT → IN**: ❌ **BLOCKED** - Internet cannot access internal network
- **OUT → DMZ**: ⚠️ **CONTROLLED** - Port forwarding required for public services

---

## 🌍 Network Segments

### 🔹 **Segment 1: Internal LAN (IN Zone)**

**Network Information:**
```yaml
Network:          192.168.100.0/25
Subnet Mask:      255.255.255.128
Gateway:          ASA Gi1/1 (192.168.100.1)
Security Level:   10 (Trusted)
VLAN:             Native
Devices:          PC0, PC1, PC2
Switch:           Cisco 2960-24TT
```

**Connected Devices:**
| Device | Interface | IP Address | Default Gateway |
|--------|-----------|------------|-----------------|
| PC0 | Fa0 | 192.168.100.2 | 192.168.100.1 |
| PC1 | Fa0 | 192.168.100.3 | 192.168.100.1 |
| PC2 | Fa0 | 192.168.100.4 | 192.168.100.1 |
| Switch | VLAN1 | 192.168.100.254 | 192.168.100.1 |

**Purpose:** Corporate workstations, internal users, protected resources

---

### 🔹 **Segment 2: User Network (OSPF Area 0)**

**Network Information:**
```yaml
Network:          10.10.10.0/24
Subnet Mask:      255.255.255.0
Gateway:          Router2 (10.10.10.1)
Router Model:     Cisco 2911
OSPF Area:        0
Protocol:         OSPFv2
Devices:          PC4
```

**Router Configuration:**
| Interface | IP Address | Connected To | OSPF Network |
|-----------|------------|--------------|--------------|
| Fa0/0 | 10.10.10.1 | PC4 | 10.10.10.0/24 |
| Fa0/1 | 20.20.20.2 | OSPF Backbone | 20.20.20.0/24 |

**Purpose:** Secondary user segment with dynamic routing capability

---

### 🔹 **Segment 3: DMZ Zone (Server Farm)**

**Network Information:**
```yaml
Network:          172.16.10.0/24
Subnet Mask:      255.255.255.0
Gateway:          ASA Gi1/2 (172.16.10.1)
Security Level:   20 (Semi-Trusted)
Server Count:     3
Purpose:          Public-facing services
```

**Server Infrastructure:**
| Server | IP Address | Service Type | Port | Public Access |
|--------|------------|--------------|------|---------------|
| Server1 | 172.16.10.2 | Web Server | 80, 443 | Port Forward |
| Server2 | 172.16.10.3 | Database | 3306, 5432 | Internal Only |
| Server3 | 172.16.10.4 | Email/DNS | 25, 53 | Port Forward |

**Purpose:** Isolated server environment for public services, separated from internal network

---

### 🔹 **Segment 4: Public Network (OUT Zone)**

**Network Information:**
```yaml
Network:          50.50.50.0/24
Subnet Mask:      255.255.255.0
Gateway:          Router4 (50.50.50.1)
Security Level:   5 (Untrusted)
ISP Connection:   Simulated Internet
Purpose:          External connectivity
```

**Routing Configuration:**
```
ASA Gi1/3 → 50.50.50.2
    ↓
Router4 → 50.50.50.1
    ↓
INTERNET (Simulated)
```

**Purpose:** Internet gateway, external communication, ISP connection point

---

### 🔹 **Segment 5: OSPF Backbone Network**

**Network Information:**
```yaml
Network:          20.20.20.0/24
Subnet Mask:      255.255.255.0
Protocol:         OSPF Area 0
Router Count:     3
Purpose:          Inter-router communication
```

**OSPF Neighbors:**
| Router | Interface | IP Address | OSPF Priority | State |
|--------|-----------|------------|---------------|-------|
| Router1 | Fa0/1 | 20.20.20.1 | 1 | FULL |
| Router2 | Fa0/1 | 20.20.20.2 | 1 | FULL |
| Router3 | Fa0/1 | 20.20.20.3 | 1 | FULL |

**Purpose:** Dynamic routing backbone, OSPF adjacency, route distribution

---

## 🔧 Complete Configuration Guide

### 🔥 **Part 1: Cisco ASA 5506-X Firewall Setup**

#### Step 1: Initial Configuration

```cisco
ciscoasa> enable
ciscoasa# configure terminal

! Set hostname
ciscoasa(config)# hostname PERIMETER
PERIMETER(config)# 

! Configure domain name
PERIMETER(config)# domain-name enterprise.local

! Set enable password
PERIMETER(config)# enable password Str0ngP@ss123
```

#### Step 2: Interface Configuration (IN Zone)

```cisco
PERIMETER(config)# interface GigabitEthernet1/1
PERIMETER(config-if)# nameif IN
INFO: Security level for "IN" set to 0 by default.

PERIMETER(config-if)# security-level 10
PERIMETER(config-if)# ip address 192.168.100.1 255.255.255.128
PERIMETER(config-if)# description ** Internal LAN - Trusted Zone **
PERIMETER(config-if)# no shutdown
PERIMETER(config-if)# exit

! Verify interface
PERIMETER(config)# show interface GigabitEthernet1/1
```

#### Step 3: Interface Configuration (DMZ Zone)

```cisco
PERIMETER(config)# interface GigabitEthernet1/2
PERIMETER(config-if)# nameif DMZ
INFO: Security level for "DMZ" set to 0 by default.

PERIMETER(config-if)# security-level 20
PERIMETER(config-if)# ip address 172.16.10.1 255.255.255.0
PERIMETER(config-if)# description ** DMZ Zone - Server Farm **
PERIMETER(config-if)# no shutdown
PERIMETER(config-if)# exit

! Verify DMZ interface
PERIMETER(config)# show nameif
```

#### Step 4: Interface Configuration (OUT Zone)

```cisco
PERIMETER(config)# interface GigabitEthernet1/3
PERIMETER(config-if)# nameif OUT
INFO: Security level for "OUT" set to 0 by default.

PERIMETER(config-if)# security-level 5
PERIMETER(config-if)# ip address 50.50.50.2 255.255.255.0
PERIMETER(config-if)# description ** External Network - Internet Gateway **
PERIMETER(config-if)# no shutdown
PERIMETER(config-if)# exit
```

#### Step 5: Static Routing Configuration

```cisco
! Default route to Internet
PERIMETER(config)# route OUT 0.0.0.0 0.0.0.0 50.50.50.1 1

! Route to OSPF networks via OUT interface
PERIMETER(config)# route OUT 10.10.10.0 255.255.255.0 50.50.50.1
PERIMETER(config)# route OUT 20.20.20.0 255.255.255.0 50.50.50.1

! Verify routing table
PERIMETER# show route

Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, V - VPN
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, + - replicated route

Gateway of last resort is 50.50.50.1 to network 0.0.0.0

S*    0.0.0.0 0.0.0.0 [1/0] via 50.50.50.1, OUT
C     50.50.50.0 255.255.255.0 is directly connected, OUT
L     50.50.50.2 255.255.255.255 is directly connected, OUT
S     10.10.10.0 255.255.255.0 [1/0] via 50.50.50.1, OUT
S     20.20.20.0 255.255.255.0 [1/0] via 50.50.50.1, OUT
C     172.16.10.0 255.255.255.0 is directly connected, DMZ
L     172.16.10.1 255.255.255.255 is directly connected, DMZ
C     192.168.100.0 255.255.255.128 is directly connected, IN
L     192.168.100.1 255.255.255.255 is directly connected, IN
```

#### Step 6: NAT Configuration

```cisco
! Create network object for internal network
PERIMETER(config)# object network IN-NETWORK
PERIMETER(config-network-object)# subnet 192.168.100.0 255.255.255.128
PERIMETER(config-network-object)# nat (IN,OUT) dynamic interface
PERIMETER(config-network-object)# exit

! Create network object for DMZ network
PERIMETER(config)# object network DMZ-NETWORK
PERIMETER(config-network-object)# subnet 172.16.10.0 255.255.255.0
PERIMETER(config-network-object)# nat (DMZ,OUT) dynamic interface
PERIMETER(config-network-object)# exit

! Verify NAT configuration
PERIMETER# show nat
Manual NAT Policies (Section 1)
1 (IN) to (OUT) source dynamic IN-NETWORK interface
    translate_hits = 0, untranslate_hits = 0
2 (DMZ) to (OUT) source dynamic DMZ-NETWORK interface
    translate_hits = 0, untranslate_hits = 0
```

---

### 🔄 **Part 2: OSPF Router Configuration**

#### Router 1 Configuration (192.168.100.0/25 Segment)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname CORE-ROUTER-1

! Configure interfaces
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.100.254 255.255.255.128
Router(config-if)# description ** Connection to Internal LAN **
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 20.20.20.1 255.255.255.0
Router(config-if)# description ** OSPF Backbone Area 0 **
Router(config-if)# no shutdown
Router(config-if)# exit

! Configure OSPF
Router(config)# router ospf 10
Router(config-router)# router-id 1.1.1.1
Router(config-router)# network 192.168.100.0 0.0.0.127 area 0
Router(config-router)# network 20.20.20.0 0.0.0.255 area 0
Router(config-router)# passive-interface FastEthernet0/0
Router(config-router)# exit

! Save configuration
Router# write memory
```

#### Router 2 Configuration (10.10.10.0/24 Segment)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname USER-ROUTER-2

! Configure interfaces
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 10.10.10.1 255.255.255.0
Router(config-if)# description ** User Network Segment **
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 20.20.20.2 255.255.255.0
Router(config-if)# description ** OSPF Backbone Area 0 **
Router(config-if)# no shutdown
Router(config-if)# exit

! Configure OSPF
Router(config)# router ospf 10
Router(config-router)# router-id 2.2.2.2
Router(config-router)# network 10.10.10.0 0.0.0.255 area 0
Router(config-router)# network 20.20.20.0 0.0.0.255 area 0
Router(config-router)# passive-interface FastEthernet0/0
Router(config-router)# exit

! Configure default route
Router(config)# ip route 0.0.0.0 0.0.0.0 20.20.20.3

! Save configuration
Router# write memory
```

#### Router 3 Configuration (OSPF Backbone)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname BACKBONE-ROUTER-3

! Configure interfaces
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 20.20.20.3 255.255.255.0
Router(config-if)# description ** OSPF Backbone Area 0 **
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 50.50.50.3 255.255.255.0
Router(config-if)# description ** Connection to ASA OUT Zone **
Router(config-if)# no shutdown
Router(config-if)# exit

! Configure OSPF
Router(config)# router ospf 10
Router(config-router)# router-id 3.3.3.3
Router(config-router)# network 20.20.20.0 0.0.0.255 area 0
Router(config-router)# default-information originate
Router(config-router)# exit

! Configure default route
Router(config)# ip route 0.0.0.0 0.0.0.0 50.50.50.1

! Save configuration
Router# write memory
```

---

## 📡 OSPF Configuration

### OSPF Overview

**Protocol Details:**
```yaml
OSPF Version:        2 (OSPFv2)
Area:                0 (Backbone Area)
Process ID:          10
Authentication:      None (can be configured)
Router ID Method:    Manual assignment
Cost Metric:         Based on bandwidth
Hello Interval:      10 seconds
Dead Interval:       40 seconds
Network Type:        Broadcast
```

### OSPF Network Advertisement

```cisco
! Router 1 advertises:
network 192.168.100.0 0.0.0.127 area 0
network 20.20.20.0 0.0.0.255 area 0

! Router 2 advertises:
network 10.10.10.0 0.0.0.255 area 0
network 20.20.20.0 0.0.0.255 area 0

! Router 3 advertises:
network 20.20.20.0 0.0.0.255 area 0
```

### OSPF Verification Commands

```cisco
! View OSPF neighbors
Router# show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2          1   FULL/DR         00:00:35    20.20.20.2      FastEthernet0/1
3.3.3.3          1   FULL/BDR        00:00:38    20.20.20.3      FastEthernet0/1

! View OSPF database
Router# show ip ospf database

            OSPF Router with ID (1.1.1.1) (Process ID 10)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
1.1.1.1         1.1.1.1         245         0x80000003 0x00ABC1 2
2.2.2.2         2.2.2.2         180         0x80000004 0x00DEF2 2
3.3.3.3         3.3.3.3         220         0x80000002 0x001234 2

! View OSPF routes
Router# show ip route ospf
O    10.10.10.0/24 [110/2] via 20.20.20.2, 00:15:23, FastEthernet0/1
O    172.16.10.0/24 [110/3] via 20.20.20.3, 00:10:45, FastEthernet0/1

! View OSPF interface details
Router# show ip ospf interface FastEthernet0/1
FastEthernet0/1 is up, line protocol is up
  Internet address is 20.20.20.1/24, Area 0
  Process ID 10, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 1.1.1.1, Interface address 20.20.20.1
  Backup Designated Router (ID) 2.2.2.2, Interface address 20.20.20.2
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:08
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 2, Adjacent neighbor count is 2
    Adjacent with neighbor 2.2.2.2 (Backup Designated Router)
    Adjacent with neighbor 3.3.3.3
  Suppress hello for 0 neighbor(s)

! View OSPF protocol information
Router# show ip protocols
Routing Protocol is "ospf 10"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 1.1.1.1
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    192.168.100.0 0.0.0.127 area 0
    20.20.20.0 0.0.0.255 area 0
  Passive Interface(s):
    FastEthernet0/0
  Routing Information Sources:
    Gateway         Distance      Last Update
    2.2.2.2              110      00:15:23
    3.3.3.3              110      00:10:45
  Distance: (default is 110)
```

---

## 🖥️ CLI Configuration Screenshots

<div align="center">
  <img src="assets/asa-initial-config.png" alt="ASA Initial Configuration" width="850"/>
  <p><em>ASA hostname, password, and basic setup</em></p>
</div>

<div align="center">
  <img src="assets/asa-interface-config.png" alt="ASA Interface Configuration" width="850"/>
  <p><em>Three-zone interface configuration with security levels</em></p>
</div>

<div align="center">
  <img src="assets/asa-routing-config.png" alt="ASA Static Routing" width="850"/>
  <p><em>Static routes configuration for OSPF network access</em></p>
</div>

<div align="center">
  <img src="assets/ospf-router-config.png" alt="OSPF Router Configuration" width="850"/>
  <p><em>OSPF network advertisement and router-id configuration</em></p>
</div>

<div align="center">
  <img src="assets/ospf-neighbor-verification.png" alt="OSPF Neighbor Verification" width="850"/>
  <p><em>OSPF neighbor adjacency and routing table verification</em></p>
</div>

---

## 🧪 Testing & Verification

### Test Plan Overview

```
┌─────────────────────────────────────────────────────────────┐
│  Network Testing Matrix                                     │
├─────────────────────────────────────────────────────────────┤
│  ✓ Internal LAN Connectivity (192.168.100.0/25)           │
│  ✓ DMZ Server Accessibility (172.16.10.0/24)              │
│  ✓ Internet Gateway Connectivity (50.50.50.0/24)          │
│  ✓ OSPF Neighbor Adjacency (20.20.20.0/24)                │
│  ✓ User Network Routing (10.10.10.0/24)                   │
│  ✓ Inter-VLAN Routing via ASA                              │
│  ✓ NAT/PAT Functionality                                   │
│  ✓ Security Policy Enforcement                             │
└─────────────────────────────────────────────────────────────┘
```

### Scenario 1: Internal LAN Connectivity

**Test Objective:** Verify connectivity within Internal LAN segment

```bash
# From PC0 (192.168.100.2)
C:\> ping 192.168.100.3
Pinging 192.168.100.3 with 32 bytes of data:

Reply from 192.168.100.3: bytes=32 time=1ms TTL=128
Reply from 192.168.100.3: bytes=32 time<1ms TTL=128
Reply from 192.168.100.3: bytes=32 time<1ms TTL=128
Reply from 192.168.100.3: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.100.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

# Ping ASA inside interface
C:\> ping 192.168.100.1
Pinging 192.168.100.1 with 32 bytes of data:

Reply from 192.168.100.1: bytes=32 time=1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.100.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
```

**Expected Result:** ✅ 100% success rate

---

### Scenario 2: DMZ Server Access from Internal Network

**Test Objective:** Verify IN → DMZ traffic flow

```bash
# From PC0 (192.168.100.2) to Server1 (172.16.10.2)
C:\> ping 172.16.10.2
Pinging 172.16.10.2 with 32 bytes of data:

Reply from 172.16.10.2: bytes=32 time=2ms TTL=127
Reply from 172.16.10.2: bytes=32 time=1ms TTL=127
Reply from 172.16.10.2: bytes=32 time=1ms TTL=127
Reply from 172.16.10.2: bytes=32 time=1ms TTL=127

Ping statistics for 172.16.10.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),

# Test HTTP access to web server
C:\> telnet 172.16.10.2 80
Trying 172.16.10.2...
Connected to 172.16.10.2.
Escape character is '^]'.
```

**Expected Result:** ✅ Access granted (Security 10 → Security 20)

---

### Scenario 3: OSPF Network Reachability

**Test Objective:** Verify connectivity to OSPF networks

```bash
# From PC0 to User Network (10.10.10.0/24)
C:\> ping 10.10.10.2
Pinging 10.10.10.2 with 32 bytes of data:

Reply from 10.10.10.2: bytes=32 time=5ms TTL=126
Reply from 10.10.10.2: bytes=32 time=3ms TTL=126
Reply from 10.10.10.2: bytes=32 time=3ms TTL=126
Reply from 10.10.10.2: bytes=32 time=4ms TTL=126

Ping statistics for 10.10.10.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),

# Traceroute to show path
C:\> tracert
