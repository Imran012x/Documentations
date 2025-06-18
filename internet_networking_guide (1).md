# Complete Networking Encyclopedia: Protocols, Devices, Implementation & Troubleshooting

## Table of Contents
1. [Network Evolution Timeline](#network-evolution-timeline)
2. [Complete Protocol Classification](#complete-protocol-classification)
3. [All Network Devices & Their Evolution](#all-network-devices--their-evolution)
4. [Protocol Implementation & Configuration](#protocol-implementation--configuration)
5. [Network Monitoring & Management](#network-monitoring--management)
6. [Network Troubleshooting Guide](#network-troubleshooting-guide)
7. [Network Security Protocols](#network-security-protocols)
8. [Modern Networking Technologies](#modern-networking-technologies)
9. [Network Performance Optimization](#network-performance-optimization)
10. [Real-World Network Scenarios](#real-world-network-scenarios)

---

## Network Evolution Timeline

### Pre-1960s: Telegraph Era
**Why it existed**: Need for long-distance communication
**Technology**: Electrical signals over wires
**Limitation**: Point-to-point only

### 1960s-1970s: Birth of Networking
**1962**: **ARPANET** concept - First packet-switched network
**1971**: **Email** invented (@tomlinson)
**1973**: **TCP/IP** protocol suite developed
**1974**: Term "Internet" first used

### 1980s: Protocol Standardization
**1983**: **ARPANET** adopts TCP/IP
**1984**: **DNS** system implemented
**1986**: **NSFNET** backbone created
**1988**: **IRC** (Internet Relay Chat) developed

### 1990s: World Wide Web Era
**1990**: **HTTP** protocol created
**1991**: **World Wide Web** goes public
**1993**: **Mosaic** browser released
**1995**: **SSL** security protocol
**1995**: **IPv6** development begins

### 2000s: Broadband & Mobile
**2003**: **WiFi** becomes mainstream
**2004**: **MPLS** for traffic engineering
**2007**: **iPhone** brings mobile internet
**2008**: **LTE** mobile standard

### 2010s-Present: Cloud & IoT Era
**2011**: **IPv6** World Launch Day
**2012**: **SDN** (Software Defined Networking)
**2015**: **5G** development
**2020**: **WiFi 6** standard
**2023**: **IPv6** adoption accelerates

---

## Complete Protocol Classification

### By OSI Layer Classification

#### **Physical Layer Protocols**
```
Purpose: Electrical/Optical signal transmission
Evolution: Telegraph → Telephone → Digital → Fiber → Wireless

1. Wired Physical Standards:
   - Ethernet (IEEE 802.3): 10Mbps → 100Gbps+
   - Token Ring (IEEE 802.5): Legacy IBM standard
   - FDDI (Fiber Distributed Data Interface): 100Mbps fiber
   - ATM (Asynchronous Transfer Mode): Cell-based switching
   - SONET/SDH: Fiber optic transmission
   - T1/E1: Digital trunk lines (1.544/2.048 Mbps)
   - T3/E3: Higher capacity trunk (44.736/34.368 Mbps)
   - OC-x: Optical Carrier levels (OC-1 = 51.84 Mbps)

2. Wireless Physical Standards:
   - IEEE 802.11 (WiFi): a/b/g/n/ac/ax (WiFi 6/6E/7)
   - IEEE 802.15 (Bluetooth): 1.0 → 5.4 + LE (Low Energy)
   - IEEE 802.16 (WiMAX): Broadband wireless
   - Cellular: 1G → 2G → 3G → 4G → 5G → 6G (research)
   - Satellite: VSAT, LEO, GEO constellations
   - Infrared (IrDA): Point-to-point communication
   - Radio Frequency: Various bands and protocols
```

#### **Data Link Layer Protocols**
```
Purpose: Frame formatting, error detection, flow control

1. LAN Protocols:
   - Ethernet II: Most common frame format
   - IEEE 802.3: Original Ethernet standard
   - IEEE 802.2 (LLC): Logical Link Control
   - Point-to-Point Protocol (PPP): Serial connections
   - High-Level Data Link Control (HDLC): Synchronous protocol
   - Frame Relay: Packet-switched WAN
   - ATM: Cell-based switching (53-byte cells)

2. WAN Protocols:
   - PPP (Point-to-Point Protocol): Dial-up and leased lines
   - PPPoE (PPP over Ethernet): DSL connections
   - HDLC: Default on Cisco serial interfaces
   - Frame Relay: Legacy packet-switched network
   - X.25: Early packet-switched protocol
   - ISDN: Integrated Services Digital Network
   - SLIP (Serial Line Internet Protocol): Legacy

3. Wireless Data Link:
   - IEEE 802.11 MAC: WiFi medium access control
   - Bluetooth protocols: HCI, L2CAP, SDP
   - Cellular protocols: GSM, UMTS, LTE, 5G NR
```

#### **Network Layer Protocols**
```
Purpose: Routing, addressing, path determination

1. Internet Protocol Suite:
   - IPv4: 32-bit addressing (4.3 billion addresses)
   - IPv6: 128-bit addressing (340 undecillion addresses)
   - ICMPv4/ICMPv6: Error reporting and diagnostics
   - IGMP: Internet Group Management Protocol
   - ARP: Address Resolution Protocol (IPv4)
   - RARP: Reverse Address Resolution Protocol
   - NDP: Neighbor Discovery Protocol (IPv6)

2. Routing Protocols:
   Interior Gateway Protocols (IGP):
   - RIP v1/v2: Distance vector, 15 hop limit
   - RIPng: RIP for IPv6
   - OSPF v2/v3: Link-state, hierarchical areas
   - IS-IS: Intermediate System to Intermediate System
   - EIGRP: Cisco hybrid protocol
   - IGRP: Legacy Cisco protocol

   Exterior Gateway Protocols (EGP):
   - BGP-4: Border Gateway Protocol for internet routing
   - EGP: Original exterior gateway protocol (obsolete)

3. Tunneling Protocols:
   - GRE: Generic Routing Encapsulation
   - IPIP: IP in IP tunneling
   - L2TP: Layer 2 Tunneling Protocol
   - PPTP: Point-to-Point Tunneling Protocol
   - IPSec: IP Security tunneling
   - MPLS: Multiprotocol Label Switching

4. Quality of Service:
   - DiffServ: Differentiated Services
   - IntServ: Integrated Services
   - RSVP: Resource Reservation Protocol
   - DSCP: Differentiated Services Code Point
```

#### **Transport Layer Protocols**
```
Purpose: End-to-end communication, reliability, flow control

1. Connection-Oriented:
   - TCP: Transmission Control Protocol
     * Reliable, ordered delivery
     * Flow control, congestion control
     * 3-way handshake
     * Segments and acknowledgments

2. Connectionless:
   - UDP: User Datagram Protocol
     * Fast, lightweight
     * No reliability guarantees
     * Used for real-time applications

3. Specialized Transport:
   - SCTP: Stream Control Transmission Protocol
     * Multi-homing support
     * Message-oriented
     * Partial reliability options
   
   - DCCP: Datagram Congestion Control Protocol
     * Unreliable but congestion-controlled
   
   - RUDP: Reliable UDP
     * UDP with reliability features

4. Real-Time Transport:
   - RTP: Real-time Transport Protocol
   - RTCP: RTP Control Protocol
   - SRTP: Secure RTP
```

#### **Session Layer Protocols**
```
Purpose: Dialog control, session management

1. Session Management:
   - NetBIOS: Network Basic Input/Output System
   - RPC: Remote Procedure Call
   - SQL Sessions: Database session management
   - SMB: Server Message Block
   - NFS: Network File System

2. Authentication Sessions:
   - Kerberos: Network authentication protocol
   - RADIUS: Remote Authentication Dial-In User Service
   - TACACS+: Terminal Access Controller Access-Control System
   - LDAP: Lightweight Directory Access Protocol
```

#### **Presentation Layer Protocols**
```
Purpose: Data formatting, encryption, compression

1. Data Representation:
   - ASCII: American Standard Code for Information Interchange
   - EBCDIC: Extended Binary Coded Decimal Interchange Code
   - Unicode: Universal character encoding
   - MIME: Multipurpose Internet Mail Extensions
   - XDR: External Data Representation

2. Compression:
   - JPEG: Image compression
   - MPEG: Video compression
   - ZIP: File compression
   - GZIP: GNU zip compression

3. Encryption:
   - SSL/TLS: Secure Sockets Layer/Transport Layer Security
   - PGP: Pretty Good Privacy
   - S/MIME: Secure/Multipurpose Internet Mail Extensions
```

#### **Application Layer Protocols**
```
Purpose: Network services to applications

1. Web Protocols:
   - HTTP: HyperText Transfer Protocol
   - HTTPS: HTTP Secure (HTTP over TLS)
   - HTTP/2: Binary, multiplexed HTTP
   - HTTP/3: HTTP over QUIC
   - WebSocket: Full-duplex communication
   - REST: Representational State Transfer
   - SOAP: Simple Object Access Protocol
   - GraphQL: Query language for APIs

2. File Transfer:
   - FTP: File Transfer Protocol
   - FTPS: FTP over SSL/TLS
   - SFTP: SSH File Transfer Protocol
   - TFTP: Trivial File Transfer Protocol
   - SCP: Secure Copy Protocol
   - rsync: Remote synchronization
   - BitTorrent: Peer-to-peer file sharing

3. Email Protocols:
   - SMTP: Simple Mail Transfer Protocol
   - POP3: Post Office Protocol version 3
   - IMAP: Internet Message Access Protocol
   - MAPI: Messaging Application Programming Interface
   - Exchange: Microsoft email server protocol

4. Directory Services:
   - DNS: Domain Name System
   - LDAP: Lightweight Directory Access Protocol
   - Active Directory: Microsoft directory service
   - NIS: Network Information Service
   - X.500: International directory standard

5. Network Management:
   - SNMP: Simple Network Management Protocol
   - CMIP: Common Management Information Protocol
   - WMI: Windows Management Instrumentation
   - Netconf: Network Configuration Protocol
   - YANG: Data modeling language

6. Time Synchronization:
   - NTP: Network Time Protocol
   - SNTP: Simple Network Time Protocol
   - PTP: Precision Time Protocol (IEEE 1588)

7. Remote Access:
   - SSH: Secure Shell
   - Telnet: Telecommunication Network
   - RDP: Remote Desktop Protocol
   - VNC: Virtual Network Computing
   - TeamViewer: Commercial remote access
   - Citrix ICA: Independent Computing Architecture

8. Voice/Video:
   - SIP: Session Initiation Protocol
   - H.323: Video conferencing standard
   - RTP/RTCP: Real-time Transport Protocol
   - WebRTC: Web Real-Time Communication
   - Skype Protocol: Proprietary P2P
   - Zoom Protocol: Proprietary video conferencing

9. Network Configuration:
   - DHCP: Dynamic Host Configuration Protocol
   - DHCPv6: DHCP for IPv6
   - BOOTP: Bootstrap Protocol
   - RARP: Reverse Address Resolution Protocol
   - Wake-on-LAN: Remote wake up protocol

10. Network Discovery:
    - ARP: Address Resolution Protocol
    - LLDP: Link Layer Discovery Protocol
    - CDP: Cisco Discovery Protocol
    - SSDP: Simple Service Discovery Protocol
    - Bonjour: Apple's zero-configuration networking
    - UPnP: Universal Plug and Play
```

---

## All Network Devices & Their Evolution

### **Layer 1 Devices (Physical Layer)**

#### **1. Repeater** - The Signal Amplifier
**Evolution**: 1960s telegraphs → 1980s Ethernet → Modern fiber amplifiers
**Purpose**: Regenerate and amplify signals
**Why Created**: Overcome signal degradation over distance
**Modern Use**: Fiber optic amplifiers, WiFi range extenders

```
Configuration Example:
Physical Repeater: No configuration (passive device)
WiFi Extender:
SSID: MainNetwork_EXT
Password: Same as main network
Placement: Halfway between router and dead zone
```

#### **2. Hub** - The Collision Generator
**Evolution**: 1980s → Obsolete by 2000s
**Purpose**: Connect multiple devices in star topology
**Why Obsolete**: Half-duplex, single collision domain, security issues
**Replacement**: Switches

```
Problems with Hubs:
- All devices share bandwidth
- Collisions increase with more devices
- No security (all traffic visible to all ports)
- Half-duplex only
```

### **Layer 2 Devices (Data Link Layer)**

#### **3. Bridge** - The Network Separator
**Evolution**: 1980s → Evolved into switches
**Purpose**: Connect two network segments, reduce collision domains
**Learning**: Builds MAC address table
**Modern Equivalent**: Switch

```
Bridge Operation:
1. Receives frame
2. Checks source MAC → learns port
3. Checks destination MAC
4. Forwards or filters based on location
```

#### **4. Switch** - The Intelligent Traffic Director
**Evolution**: 1990s → Present (Layer 2 → Layer 3 → Layer 4-7)
**Purpose**: Intelligent frame switching, collision domain separation

**Types of Switches**:
```
1. Unmanaged Switch:
   - Plug and play
   - No configuration
   - Basic functionality
   - Consumer/small office use

2. Managed Switch:
   - Full configuration options
   - VLANs, QoS, monitoring
   - Enterprise features
   - Remote management

3. Layer 3 Switch:
   - Routing capabilities
   - IP routing between VLANs
   - Higher performance than routers
   - Core network use

4. PoE Switch:
   - Power over Ethernet
   - Powers IP phones, APs, cameras
   - Reduces cabling requirements
```

**Configuration Examples**:
```bash
# Cisco Switch Basic Configuration
Switch(config)# hostname CoreSwitch
Switch(config)# enable secret cisco123
Switch(config)# service password-encryption

# VLAN Configuration
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config)# vlan 20
Switch(config-vlan)# name Marketing

# Port Configuration
Switch(config)# interface range fastethernet 0/1-10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10

# Trunk Configuration
Switch(config)# interface gigabitethernet 0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20

# Spanning Tree Protocol
Switch(config)# spanning-tree mode rapid-pvst
Switch(config)# spanning-tree vlan 10 priority 4096
```

### **Layer 3 Devices (Network Layer)**

#### **5. Router** - The Path Finder
**Evolution**: 1980s → Present (Hardware → Software → Cloud)
**Purpose**: Route packets between different networks

**Types of Routers**:
```
1. Edge Router:
   - Connects to ISP
   - Internet gateway
   - NAT, firewall functions

2. Core Router:
   - High-speed backbone routing
   - MPLS, BGP protocols
   - Service provider networks

3. Branch Router:
   - Remote office connectivity
   - VPN capabilities
   - WAN optimization

4. Virtual Router:
   - Software-based routing
   - Cloud environments
   - SDN implementations

5. Wireless Router:
   - Combined router/AP/switch
   - Home/small office use
   - WiFi integration
```

**Advanced Router Configuration**:
```bash
# Cisco Router Advanced Configuration
Router(config)# hostname BorderRouter
Router(config)# ip domain-name company.com

# Interface Configuration with IPv6
Router(config)# interface GigabitEthernet0/0
Router(config-if)# description "WAN to ISP"
Router(config-if)# ip address dhcp
Router(config-if)# ipv6 address autoconfig
Router(config-if)# ipv6 enable
Router(config-if)# no shutdown

# Multiple Routing Protocols
Router(config)# router ospf 1
Router(config-router)# router-id 1.1.1.1
Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
Router(config-router)# redistribute bgp 65001 subnets

Router(config)# router bgp 65001
Router(config-router)# neighbor 203.0.113.1 remote-as 65000
Router(config-router)# network 192.168.1.0 mask 255.255.255.0

# Quality of Service
Router(config)# class-map match-all VoIP
Router(config-cmap)# match protocol rtp
Router(config)# policy-map QoS-Policy
Router(config-pmap)# class VoIP
Router(config-pmap-c)# priority percent 30

# VPN Configuration
Router(config)# crypto isakmp policy 10
Router(config-isakmp)# authentication pre-share
Router(config-isakmp)# encryption aes 256
Router(config-isakmp)# hash sha256
Router(config-isakmp)# group 14
```

### **Specialized Network Devices**

#### **6. Firewall** - The Security Guardian
**Evolution**: 1990s packet filters → Modern NGFW (Next-Gen Firewalls)
**Purpose**: Network security, access control

**Types**:
```
1. Packet Filtering Firewall:
   - Layer 3/4 filtering
   - IP addresses, ports, protocols
   - Stateless operation

2. Stateful Firewall:
   - Connection state tracking
   - Return traffic allowed
   - More secure than packet filtering

3. Application Layer Firewall:
   - Layer 7 inspection
   - Application-specific rules
   - Deep packet inspection

4. Next-Generation Firewall (NGFW):
   - IPS integration
   - Application awareness
   - User identity
   - SSL inspection
```

**Firewall Configuration**:
```bash
# Cisco ASA Firewall Configuration
firewall(config)# hostname CompanyFirewall

# Security Levels
firewall(config)# interface GigabitEthernet0/0
firewall(config-if)# nameif outside
firewall(config-if)# security-level 0
firewall(config-if)# ip address dhcp

firewall(config)# interface GigabitEthernet0/1
firewall(config-if)# nameif inside
firewall(config-if)# security-level 100
firewall(config-if)# ip address 192.168.1.1 255.255.255.0

# Access Control Lists
firewall(config)# access-list OUTSIDE_IN extended permit tcp any host 192.168.1.10 eq 80
firewall(config)# access-list OUTSIDE_IN extended permit tcp any host 192.168.1.10 eq 443
firewall(config)# access-list OUTSIDE_IN extended deny ip any any log
firewall(config)# access-group OUTSIDE_IN in interface outside

# NAT Configuration
firewall(config)# object network LAN
firewall(config-network-object)# subnet 192.168.1.0 255.255.255.0
firewall(config-network-object)# nat (inside,outside) dynamic interface

# VPN Configuration
firewall(config)# group-policy VPN_POLICY internal
firewall(config-group-policy)# vpn-tunnel-protocol ssl-client
firewall(config-group-policy)# split-tunnel-policy tunnelspecified
firewall(config-group-policy)# split-tunnel-network-list SPLIT_TUNNEL
```

#### **7. Load Balancer** - The Traffic Distributor
**Evolution**: 1990s hardware → Software → Cloud load balancers
**Purpose**: Distribute traffic across multiple servers

**Types**:
```
1. Layer 4 Load Balancer:
   - Transport layer balancing
   - IP and port-based decisions
   - High performance

2. Layer 7 Load Balancer:
   - Application layer balancing
   - HTTP header inspection
   - Content-based routing

3. Global Load Balancer:
   - DNS-based load balancing
   - Geographic distribution
   - Disaster recovery
```

**Load Balancer Configuration**:
```bash
# F5 BIG-IP Configuration
tmsh create ltm pool web_pool members add { 192.168.1.10:80 192.168.1.11:80 192.168.1.12:80 }
tmsh create ltm virtual web_virtual destination 203.0.113.10:80 pool web_pool

# HAProxy Configuration
global
    daemon
    maxconn 4096

defaults
    mode http
    timeout connect 5s
    timeout client 50s
    timeout server 50s

frontend web_frontend
    bind *:80
    default_backend web_servers

backend web_servers
    balance roundrobin
    server web1 192.168.1.10:80 check
    server web2 192.168.1.11:80 check
    server web3 192.168.1.12:80 check
```

#### **8. Proxy Server** - The Intermediary
**Evolution**: 1990s caching → Modern security proxies
**Purpose**: Intermediate requests, caching, security

**Types**:
```
1. Forward Proxy:
   - Client-side proxy
   - Internet access control
   - Caching and filtering

2. Reverse Proxy:
   - Server-side proxy
   - Load balancing
   - SSL termination

3. Transparent Proxy:
   - No client configuration
   - Traffic interception
   - Policy enforcement
```

#### **9. Intrusion Detection/Prevention System (IDS/IPS)**
**Evolution**: 1990s signature-based → Modern AI/ML systems
**Purpose**: Detect and prevent network attacks

**Configuration Example**:
```bash
# Snort IDS Configuration
var HOME_NET 192.168.1.0/24
var EXTERNAL_NET !$HOME_NET

# Detection Rules
alert tcp $EXTERNAL_NET any -> $HOME_NET 22 (msg:"SSH Brute Force Attack"; detection_filter:track by_src, count 5, seconds 60; sid:1000001;)
alert tcp $EXTERNAL_NET any -> $HOME_NET 80 (msg:"SQL Injection Attempt"; content:"union select"; nocase; sid:1000002;)
```

### **Wireless Network Devices**

#### **10. Wireless Access Point (AP)** - The WiFi Broadcaster
**Evolution**: 1990s → WiFi 7 (802.11be)
**Purpose**: Wireless network access

**Types**:
```
1. Standalone AP:
   - Independent operation
   - Web-based management
   - Small deployments

2. Controller-based AP:
   - Centralized management
   - Coordinated operation
   - Enterprise deployments

3. Mesh AP:
   - Self-healing network
   - Dynamic routing
   - Extended coverage
```

**WiFi Standards Evolution**:
```
IEEE 802.11   (1997): 2 Mbps
IEEE 802.11b  (1999): 11 Mbps
IEEE 802.11a  (1999): 54 Mbps
IEEE 802.11g  (2003): 54 Mbps
IEEE 802.11n  (2009): 600 Mbps (WiFi 4)
IEEE 802.11ac (2013): 6.93 Gbps (WiFi 5)
IEEE 802.11ax (2019): 9.6 Gbps (WiFi 6)
IEEE 802.11be (2024): 46 Gbps (WiFi 7)
```

#### **11. Wireless Controller** - The WiFi Orchestra Conductor
**Purpose**: Centralized wireless management
**Features**: Configuration, monitoring, security, roaming

```bash
# Cisco WLC Configuration
(Cisco Controller) > config wlan create 1 Guest-Network Guest-Network
(Cisco Controller) > config wlan security wpa enable 1
(Cisco Controller) > config wlan security wpa akm psk enable 1
(Cisco Controller) > config wlan security wpa akm psk set-key ascii GuestPassword123 1
```

### **WAN Devices**

#### **12. CSU/DSU** - The Digital Translator
**Evolution**: 1970s → Integrated into routers
**Purpose**: Digital signal conversion for WAN connections
**Modern Status**: Mostly integrated into routers

#### **13. Modem** - The Signal Converter
**Evolution**: 1950s acoustic → Cable → DSL → Fiber
**Purpose**: Modulate/demodulate signals for transmission

**Types**:
```
1. Dial-up Modem: 56 Kbps (obsolete)
2. DSL Modem: 1-100 Mbps
3. Cable Modem: 10 Mbps - 1 Gbps
4. Fiber Modem (ONT): 100 Mbps - 10 Gbps
5. Satellite Modem: 12-100 Mbps
6. Cellular Modem: 1 Mbps - 1 Gbps (5G)
```

### **Network Appliances**

#### **14. Network Attached Storage (NAS)**
**Purpose**: Centralized file storage
**Protocols**: SMB/CIFS, NFS, FTP, HTTP

#### **15. Unified Threat Management (UTM)**
**Purpose**: Multiple security functions in one device
**Features**: Firewall, IPS, Antivirus, VPN, Web filtering

#### **16. SD-WAN Appliance**
**Purpose**: Software-defined WAN connectivity
**Features**: Dynamic path selection, centralized policies

---

## Protocol Implementation & Configuration

### **Network Protocol Configuration Hierarchy**

#### **1. Physical Layer Setup**
```bash
# Interface Physical Configuration
Router(config)# interface GigabitEthernet0/0
Router(config-if)# duplex full
Router(config-if)# speed 1000
Router(config-if)# no shutdown

# Cable Diagnostics
Router# show interfaces GigabitEthernet0/0
Router# test cable-diagnostics tdr interface GigabitEthernet0/0
```

#### **2. Data Link Layer Configuration**
```bash
# Ethernet Frame Configuration
Router(config-if)# encapsulation dot1q 100
Router(config-if)# frame-relay encapsulation
Router(config-if)# ppp authentication chap

# Switching Configuration
Switch(config)# mac address-table aging-time 600
Switch(config)# mac address-table static aaaa.bbbb.cccc vlan 10 interface fastethernet0/1
```

#### **3. Network Layer Implementation**

**IPv4 Configuration**:
```bash
# Basic IPv4
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# ip helper-address 192.168.1.100

# Secondary IP
Router(config-if)# ip address 192.168.2.1 255.255.255.0 secondary

# DHCP Server
Router(config)# ip dhcp pool LAN
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# lease 7 0 0
```

**IPv6 Configuration**:
```bash
# IPv6 Global Configuration
Router(config)# ipv6 unicast-routing
Router(config)# ipv6 cef

# Interface IPv6
Router(config-if)# ipv6 address 2001:db8:1::1/64
Router(config-if)# ipv6 address autoconfig
Router(config-if)# ipv6 enable

# IPv6 DHCP
Router(config)# ipv6 dhcp pool LAN-v6
Router(config-dhcpv6)# address prefix 2001:db8:1::/64
Router(config-dhcpv6)# dns-server 2001:4860:4860::8888
```

#### **4. Routing Protocol Configuration**

**OSPF Implementation**:
```bash
# OSPFv2 Configuration
Router(config)# router ospf 1
Router(config-router)# router-id 1.1.1.1
Router(config-router)# log-adjacency-changes detail
Router(config-router)# area 0 authentication message-digest
Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
Router(config-router)# network 10.0.0.0 0.0.0.3 area 1
Router(config-router)# default-information originate

# OSPFv3 for IPv6
Router(config)# ipv6 router ospf 1
Router(config-rtr)# router-id 1.1.1.1
Router(config-rtr)# log-adjacency-changes detail

Router(config-if)# ipv6 ospf 1 area 0
```

**BGP Implementation**:
```bash
# BGP Configuration
Router(config)# router bgp 65100
Router(config-router)# bgp router-id 1.1.1.1
Router(config-router)# bgp log-neighbor-changes

# eBGP Neighbor
Router(config-router)# neighbor 203.0.113.1 remote-as 65200
Router(config-router)# neighbor 203.0.113.1 description "ISP Connection"
Router(config-router)# neighbor 203.0.113.1 password BGPSecret123

# iBGP Neighbor
Router(config-router)# neighbor 10.0.0.2 remote-as 65100
Router(config-router)# neighbor 10.0.0.2 update-source loopback0
Router(config-router)# neighbor 10.0.0.2 next-hop-self

# Network Advertisement
Router(config-router)# network 192.168.1.0 mask 255.255.255.0
Router(config-router)# redistribute ospf 1

# Route Filtering
Router(config)# ip prefix-list ALLOW-OUT permit 192.168.1.0/24
Router(config-router)# neighbor 203.0.113.1 prefix-list ALLOW-OUT out
```

**EIGRP Implementation**:
```bash
# EIGRP Configuration
Router(config)# router eigrp 100
Router(config-router)# eigrp router-id 1.1.1.1
Router(config-router)# network 192.168.1.0 0.0.0.255
Router(config-router)# network 10.0.0.0 0.0.0.3
Router(config-router)# no auto-summary

# EIGRP Metrics Tuning
Router(config-if)# bandwidth 1000
Router(config-if)# delay 100
Router(config-if)# ip hello-interval eigrp 100 30
Router(config-if)# ip hold-time eigrp 100 90

# EIGRP Authentication
Router(config)# key chain EIGRP-KEYS
Router(config-keychain)# key 1
Router(config-keychain-key)# key-string EIGRPSecret123
Router(config-if)# ip authentication mode eigrp 100 md5
Router(config-if)# ip authentication key-chain eigrp 100 EIGRP-KEYS
```

#### **5. Transport Layer Configuration**

**TCP Optimization**:
```bash
# TCP Window Scaling
Router(config)# ip tcp window-size 65536
Router(config)# ip tcp selective-ack
Router(config)# ip tcp timestamp

# TCP Connection Limits
Router(config)# ip tcp synwait-time 10
Router(config)# ip tcp path-mtu-discovery
```

**UDP Configuration**:
```bash
# UDP Broadcast Forwarding
Router(config-if)# ip helper-address 192.168.1.100
Router(config)# ip forward-protocol udp 69
Router(config)# ip forward-protocol udp 137
```

#### **6. Application Layer Services**

**DNS Configuration**:
```bash
# DNS Server Setup (BIND9)
zone "company.com" {
    type master;
    file "/etc/bind/db.company.com";
    allow-update { none; };
};

# DNS Records
$TTL    604800
@       IN      SOA     ns1.company.com. admin.company.com. (
                        2023061801      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
@       IN      NS      ns1.company.com.
@       IN      A       203.0.113.10
www     IN      A       203.0.113.10
mail    IN      A       203.0.113.20
ftp     IN      A       203.0.113.30
```

**Web Server Configuration**:
```bash
# Apache HTTP Configuration
<VirtualHost *:80>
    ServerName www.company.com
    DocumentRoot /var/www/html
    ErrorLog /var/log/apache2/error.log
    CustomLog /var/log/apache2/access.log combined
    
    # Redirect to HTTPS
    Redirect permanent / https://www.company.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName www.company.com
    DocumentRoot /var/www/html
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/company.crt
    SSLCertificateKeyFile /etc/ssl/private/company.key
    
    # Security Headers
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
    Header always set X-Frame-Options DENY
    Header always set X-Content-Type-Options nosniff
</VirtualHost>
```

**Email Server Configuration**:
```bash
# Postfix SMTP Configuration
# /etc/postfix/main.cf
myhostname = mail.company.com
mydomain = company.com
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
mynetworks = 192.168.1.0/24, 127.0.0.0/8

# TLS Configuration
smtpd_tls_cert_file = /etc/ssl/certs/mail.crt
smtpd_tls_key_file = /etc/ssl/private/mail.key
smtpd_use_tls = yes
smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache

# Dovecot IMAP/POP3 Configuration
# /etc/dovecot/dovecot.conf
protocols = imap pop3 lmtp
mail_location = maildir:~/Maildir
ssl_cert = </etc/ssl/certs/mail.crt
ssl_key = </etc/ssl/private/mail.key
```

---

## Network Monitoring & Management

### **SNMP (Simple Network Management Protocol) Implementation**

#### **SNMP Versions Comparison**:
```
SNMPv1 (1988):
- Community-based security
- Plain text transmission
- Limited error handling
- 32-bit counters

SNMPv2c (1993):
- Improved error handling
- 64-bit counters
- Bulk operations
- Still community-based security

SNMPv3 (1998):
- User-based security
- Authentication and encryption
- Access control
- Message integrity
```

#### **SNMP Configuration**:
```bash
# Cisco Router SNMP Configuration
Router(config)# snmp-server community public RO
Router(config)# snmp-server community private RW
Router(config)# snmp-server location "Data Center Rack 42"
Router(config)# snmp-server contact "admin@company.com"

# SNMPv3 Configuration
Router(config)# snmp-server group ADMIN v3 priv
Router(config)# snmp-server user admin ADMIN v3 auth sha AuthPass123 priv aes 128 PrivPass123
Router(config)# snmp-server host 192.168.1.100 version 3 priv admin

# SNMP Traps
Router(config)# snmp-server enable traps
Router(config)# snmp-server enable traps interface
Router(config)# snmp-server enable traps ospf
```

### **Network Monitoring Tools & Implementation**

#### **1. Nagios Monitoring**:
```bash
# Host Definition
define host {
    use                     linux-server
    host_name               web-server-01
    alias                   Web Server 01
    address                 192.168.1.10
    max_check_attempts      5
    check_period            24x7
    notification_interval   30
    notification_period     24x7
}

# Service Definition
define service {
    use                     generic-service
    host_name               web-server-01
    service_description     HTTP
    check_command           check_http
    max_check_attempts      3
    normal_check_interval   5
    retry_check_interval    1
}
```

#### **2. Zabbix Monitoring**:
```bash
# Zabbix Agent Configuration
Server=192.168.1.100
ServerActive=192.168.1.100
Hostname=web-server-01
RefreshActiveChecks=120
UserParameter=mysql.ping,mysqladmin -uroot ping | grep -c alive
```

#### **3. PRTG Network Monitor**:
```xml
<!-- Custom Sensor Configuration -->
<sensor>
    <name>Bandwidth Utilization</name>
    <kind>snmp</kind>
    <host>192.168.1.1</host>
    <community>public</community>
    <oid>1.3.6.1.2.1.2.2.1.10.1</oid>
    <interval>300</interval>
</sensor>
```

### **Network Flow Analysis**

#### **NetFlow Configuration**:
```bash
# Cisco NetFlow Configuration
Router(config)# ip flow-export version 9
Router(config)# ip flow-export destination 192.168.1.100 2055
Router(config)# ip flow-export source loopback0

Router(config-if)# ip flow ingress
Router(config-if)# ip flow egress
```

#### **sFlow Configuration**:
```bash
# HP/Aruba Switch sFlow
switch(config)# sflow 1 destination 192.168.1.100
switch(config)# sflow 1 polling ethernet all 20
switch(config)# sflow 1 sampling ethernet all 512
```

### **Log Management**

#### **Syslog Configuration**:
```bash
# Cisco Syslog Configuration
Router(config)# logging host 192.168.1.100
Router(config)# logging trap informational
Router(config)# logging facility local0
Router(config)# logging source-interface loopback0

# Linux Syslog-ng Configuration
source s_network {
    udp(ip(0.0.0.0) port(514));
    tcp(ip(0.0.0.0) port(514));
};

destination d_cisco {
    file("/var/log/cisco.log");
};

filter f_cisco {
    host("192.168.1.1");
};

log {
    source(s_network);
    filter(f_cisco);
    destination(d_cisco);
};
```

---

## Network Troubleshooting Guide

### **OSI Layer Troubleshooting Methodology**

#### **Layer 1 - Physical Layer Issues**

**Common Problems**:
- Cable faults
- Port failures
- Power issues
- Environmental factors

**Troubleshooting Steps**:
```bash
# Check Physical Interface Status
Router# show interfaces GigabitEthernet0/0
Router# show interfaces status

# Cable Diagnostics
Router# test cable-diagnostics tdr interface GigabitEthernet0/0
Switch# show cable-diagnostics tdr interface GigabitEthernet0/1

# Power Status (PoE)
Switch# show power inline
Switch# show environment power

# Optical Interface Diagnostics
Router# show interfaces transceiver
Router# show controllers dwdm-carrier
```

**Tools**:
- Cable tester
- Optical power meter
- Network analyzer
- Multimeter

#### **Layer 2 - Data Link Layer Issues**

**Common Problems**:
- Spanning Tree loops
- VLAN misconfigurations
- MAC address conflicts
- Frame errors

**Troubleshooting Steps**:
```bash
# Spanning Tree Analysis
Switch# show spanning-tree
Switch# show spanning-tree interface GigabitEthernet0/1 detail
Switch# show spanning-tree root
Switch# show spanning-tree blockedports

# VLAN Troubleshooting
Switch# show vlan brief
Switch# show interfaces trunk
Switch# show interfaces switchport

# MAC Address Table
Switch# show mac address-table
Switch# show mac address-table aging-time
Switch# show mac address-table count

# Interface Statistics
Switch# show interfaces GigabitEthernet0/1 counters errors
Switch# show interfaces GigabitEthernet0/1 counters

# Frame Relay Troubleshooting
Router# show frame-relay pvc
Router# show frame-relay lmi
Router# show frame-relay map
```

#### **Layer 3 - Network Layer Issues**

**Common Problems**:
- Routing table issues
- IP address conflicts
- Subnet mask errors
- ARP problems

**Troubleshooting Steps**:
```bash
# IP Configuration Verification
Router# show ip interface brief
Router# show ip interface GigabitEthernet0/0
Router# show ip route
Router# show ip route summary

# ARP Troubleshooting
Router# show arp
Router# clear arp-cache
Router# debug arp

# ICMP Troubleshooting
Router# ping 8.8.8.8
Router# ping 8.8.8.8 source loopback0
Router# traceroute 8.8.8.8

# Routing Protocol Troubleshooting
Router# show ip ospf neighbor
Router# show ip ospf database
Router# show ip eigrp neighbors
Router# show ip bgp summary

# IPv6 Troubleshooting
Router# show ipv6 interface brief
Router# show ipv6 route
Router# ping ipv6 2001:4860:4860::8888
```

#### **Layer 4 - Transport Layer Issues**

**Common Problems**:
- Port blocking
- TCP connection issues
- UDP packet loss
- Firewall rules

**Troubleshooting Steps**:
```bash
# TCP Connection Analysis
Router# show tcp brief
Router# show tcp statistics

# Port Testing
telnet 192.168.1.10 80
nmap -p 80,443,22 192.168.1.10

# Firewall Rule Analysis
firewall# show access-list
firewall# show conn
firewall# show nat

# Quality of Service
Router# show policy-map interface GigabitEthernet0/0
Router# show class-map
```

#### **Layer 5-7 - Session/Presentation/Application Issues**

**Common Problems**:
- DNS resolution failures
- Certificate issues
- Application timeouts
- Authentication problems

**Troubleshooting Steps**:
```bash
# DNS Troubleshooting
nslookup www.google.com
dig www.google.com
host www.google.com

# HTTP/HTTPS Testing
curl -I http://www.google.com
openssl s_client -connect www.google.com:443

# SMTP Testing
telnet mail.company.com 25
EHLO test.com
MAIL FROM: test@test.com
RCPT TO: user@company.com

# Application Layer Analysis
netstat -an
ss -tuln
lsof -i :80
```

### **Network Performance Troubleshooting**

#### **Bandwidth Testing**:
```bash
# iperf3 Testing
# Server
iperf3 -s

# Client
iperf3 -c 192.168.1.100 -t 60 -i 5

# UDP Testing
iperf3 -c 192.168.1.100 -u -b 100M

# Multiple Connections
iperf3 -c 192.168.1.100 -P 4
```

#### **Latency Analysis**:
```bash
# Ping Statistics
ping -c 100 8.8.8.8
ping -i 0.2 -c 50 192.168.1.1

# MTR Network Diagnostics
mtr -r -c 10 8.8.8.8

# Traceroute with Timestamps
traceroute -T -p 80 www.google.com
```

#### **Packet Capture Analysis**:
```bash
# tcpdump Capture
tcpdump -i eth0 -w capture.pcap host 192.168.1.10
tcpdump -i eth0 port 80 and host 192.168.1.10

# Wireshark CLI (tshark)
tshark -i eth0 -f "host 192.168.1.10" -w capture.pcap
tshark -r capture.pcap -Y "http.response.code == 404"

# Network Statistics
netstat -i
cat /proc/net/dev
```

---

## Network Security Protocols

### **VPN Protocols & Implementation**

#### **1. IPSec VPN**

**IPSec Components**:
- **AH (Authentication Header)**: Authentication and integrity
- **ESP (Encapsulating Security Payload)**: Encryption and authentication
- **IKE (Internet Key Exchange)**: Key management

**Site-to-Site IPSec Configuration**:
```bash
# Cisco Site-to-Site VPN
Router(config)# crypto isakmp policy 10
Router(config-isakmp)# authentication pre-share
Router(config-isakmp)# encryption aes 256
Router(config-isakmp)# hash sha256
Router(config-isakmp)# group 14
Router(config-isakmp)# lifetime 86400

Router(config)# crypto isakmp key VPNSecret123 address 203.0.113.100

Router(config)# crypto ipsec transform-set STRONG esp-aes 256 esp-sha256-hmac
Router(config-crypto-trans)# mode tunnel

Router(config)# crypto map VPN_MAP 10 ipsec-isakmp
Router(config-crypto-map)# set peer 203.0.113.100
Router(config-crypto-map)# set transform-set STRONG
Router(config-crypto-map)# match address VPN_TRAFFIC

Router(config)# ip access-list extended VPN_TRAFFIC
Router(config-ext-nacl)# permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255

Router(config-if)# crypto map VPN_MAP
```

#### **2. SSL/TLS VPN**

**OpenVPN Server Configuration**:
```bash
# OpenVPN Server Config
port 1194
proto udp
dev tun
ca ca.crt
cert server.crt
key server.key
dh dh2048.pem
server 10.8.0.0 255.255.255.0
ifconfig-pool-persist ipp.txt
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 8.8.4.4"
keepalive 10 120
tls-auth ta.key 0
cipher AES-256-CBC
user nobody
group nogroup
persist-key
persist-tun
status openvpn-status.log
log-append /var/log/openvpn.log
verb 3
```

#### **3. WireGuard VPN**

**WireGuard Configuration**:
```bash
# Server Configuration
[Interface]
PrivateKey = <server-private-key>
Address = 10.0.0.1/24
ListenPort = 51820
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

[Peer]
PublicKey = <client-public-key>
AllowedIPs = 10.0.0.2/32

# Client Configuration
[Interface]
PrivateKey = <client-private-key>
Address = 10.0.0.2/24
DNS = 8.8.8.8

[Peer]
PublicKey = <server-public-key>
Endpoint = server.example.com:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

### **Network Access Control (NAC)**

#### **802.1X Authentication**:
```bash
# Cisco Switch 802.1X Configuration
Switch(config)# aaa new-model
Switch(config)# aaa authentication dot1x default group radius
Switch(config)# aaa authorization network default group radius

Switch(config)# dot1x system-auth-control
Switch(config)# radius server RADIUS-SERVER
Switch(config-radius-server)# address ipv4 192.168.1.100 auth-port 1812 acct-port 1813
Switch(config-radius-server)# key RadiusSecret123

Switch(config-if)# dot1x port-control auto
Switch(config-if)# dot1x host-mode multi-auth
Switch(config-if)# authentication periodic
Switch(config-if)# authentication timer reauthenticate 3600
```

### **Network Segmentation**

#### **VLAN Implementation**:
```bash
# Advanced VLAN Configuration
Switch(config)# vlan 10
Switch(config-vlan)# name Finance
Switch(config)# vlan 20
Switch(config-vlan)# name Marketing
Switch(config)# vlan 30
Switch(config-vlan)# name Guest

# Private VLANs
Switch(config)# vlan 100
Switch(config-vlan)# private-vlan primary
Switch(config)# vlan 101
Switch(config-vlan)# private-vlan isolated
Switch(config)# vlan 102
Switch(config-vlan)# private-vlan community

Switch(config)# vlan 100
Switch(config-vlan)# private-vlan association 101,102
```

#### **Access Control Lists (ACLs)**:
```bash
# Standard ACL
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# access-list 1 deny any

# Extended ACL
Router(config)# ip access-list extended FIREWALL
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq 80
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq 443
Router(config-ext-nacl)# permit udp 192.168.1.0 0.0.0.255 any eq 53
Router(config-ext-nacl)# deny ip any any log

# Time-based ACL
Router(config)# time-range BUSINESS-HOURS
Router(config-time-range)# periodic weekdays 8:00 to 18:00

Router(config)# ip access-list extended TIME-BASED
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq 80 time-range BUSINESS-HOURS
```

---

## Modern Networking Technologies

### **Software Defined Networking (SDN)**

#### **OpenFlow Protocol**:
```python
# OpenFlow Controller (Ryu Framework)
from ryu.base import app_manager
from ryu.controller import ofp_event
from ryu.controller.handler import CONFIG_DISPATCHER, MAIN_DISPATCHER
from ryu.controller.handler import set_ev_cls
from ryu.ofproto import ofproto_v1_3

class SimpleSwitch13(app_manager.RyuApp):
    OFP_VERSIONS = [ofproto_v1_3.OFP_VERSION]

    def __init__(self, *args, **kwargs):
        super(SimpleSwitch13, self).__init__(*args, **kwargs)
        self.mac_to_port = {}

    @set_ev_cls(ofp_event.EventOFPSwitchFeatures, CONFIG_DISPATCHER)
    def switch_features_handler(self, ev):
        datapath = ev.msg.datapath
        ofproto = datapath.ofproto
        parser = datapath.ofproto_parser

        match = parser.OFPMatch()
        actions = [parser.OFPActionOutput(ofproto.OFPP_CONTROLLER,
                                          ofproto.OFPCML_NO_BUFFER)]
        self.add_flow(datapath, 0, match, actions)
```

#### **Network Function Virtualization (NFV)**:
```yaml
# OpenStack Heat Template for VNF
heat_template_version: 2018-08-31

description: Virtual Firewall Service

resources:
  firewall_instance:
    type: OS::Nova::Server
    properties:
      name: virtual-firewall
      image: { get_param: firewall_image }
      flavor: { get_param: firewall_flavor }
      networks:
        - network: { get_param: management_network }
        - network: { get_param: internal_network }
        - network: { get_param: external_network }
      user_data_format: RAW
      user_data: |
        #!/bin/bash
        # Firewall configuration script
        iptables -t nat -A POSTROUTING -o eth2 -j MASQUERADE
        iptables -A FORWARD -i eth1 -o eth2 -j ACCEPT
        iptables -A FORWARD -i eth2 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

### **Intent-Based Networking (IBN)**

#### **Cisco DNA Center Configuration**:
```json
{
  "name": "Security_Policy",
  "description": "Block social media during business hours",
  "policy": {
    "source": {
      "scalableGroups": ["Employees"]
    },
    "destination": {
      "applications": ["Facebook", "Twitter", "Instagram"]
    },
    "action": "DENY",
    "schedule": {
      "days": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
      "times": ["08:00-18:00"]
    }
  }
}
```

### **Network Automation**

#### **Ansible Network Automation**:
```yaml
# Ansible Playbook for Network Configuration
---
- name: Configure Cisco Routers
  hosts: cisco_routers
  gather_facts: no
  connection: network_cli
  
  tasks:
    - name: Configure OSPF
      ios_config:
        lines:
          - router ospf 1
          - router-id {{ router_id }}
          - network {{ network }} {{ wildcard }} area {{ area }}
          - log-adjacency-changes detail
        parents: router ospf 1
        
    - name: Configure Interfaces
      ios_interfaces:
        config:
          - name: "{{ item.name }}"
            description: "{{ item.description }}"
            enabled: true
            mtu: 1500
      loop: "{{ interfaces }}"
      
    - name: Configure VLANs
      ios_vlans:
        config:
          - vlan_id: "{{ item.id }}"
            name: "{{ item.name }}"
            state: active
      loop: "{{ vlans }}"
```

#### **Python Network Automation**:
```python
# Netmiko Network Automation
from netmiko import ConnectHandler
import json

# Device connection parameters
device = {
    'device_type': 'cisco_ios',
    'host': '192.168.1.1',
    'username': 'admin',
    'password': 'password',
    'secret': 'enable_password',
}

# Connect to device
connection = ConnectHandler(**device)
connection.enable()

# Configuration commands
config_commands = [
    'interface GigabitEthernet0/1',
    'description Configured by Python',
    'ip address 192.168.100.1 255.255.255.0',
    'no shutdown',
    'exit',
    'router ospf 1',
    'network 192.168.100.0 0.0.0.255 area 0',
]

# Send configuration
output = connection.send_config_set(config_commands)
print(output)

# Save configuration
save_output = connection.save_config()
print(save_output)

connection.disconnect()
```

### **Cloud Networking**

#### **AWS VPC Configuration**:
```json
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Resources": {
    "VPC": {
      "Type": "AWS::EC2::VPC",
      "Properties": {
        "CidrBlock": "10.0.0.0/16",
        "EnableDnsHostnames": true,
        "EnableDnsSupport": true,
        "Tags": [
          {
            "Key": "Name",
            "Value": "Production-VPC"
          }
        ]
      }
    },
    "PublicSubnet": {
      "Type": "AWS::EC2::Subnet",
      "Properties": {
        "VpcId": {"Ref": "VPC"},
        "CidrBlock": "10.0.1.0/24",
        "AvailabilityZone": "us-west-2a",
        "MapPublicIpOnLaunch": true
      }
    },
    "PrivateSubnet": {
      "Type": "AWS::EC2::Subnet",
      "Properties": {
        "VpcId": {"Ref": "VPC"},
        "CidrBlock": "10.0.2.0/24",
        "AvailabilityZone": "us-west-2b"
      }
    }
  }
}
```

---

## Network Performance Optimization

### **Quality of Service (QoS) Implementation**

#### **Traffic Classification**:
```bash
# Cisco QoS Configuration
Router(config)# class-map match-all VOICE
Router(config-cmap)# match protocol rtp
Router(config-cmap)# match dscp ef

Router(config)# class-map match-all VIDEO
Router(config-cmap)# match protocol http url "*video*"
Router(config-cmap)# match dscp af41

Router(config)# class-map match-all CRITICAL_DATA
Router(config-cmap)# match access-group 101
Router(config-cmap)# match dscp af31

Router(config)# class-map match-all BULK_DATA
Router(config-cmap)# match protocol ftp
Router(config-cmap)# match dscp af11
```

#### **Traffic Shaping and Policing**:
```bash
# Policy Map Configuration
Router(config)# policy-map WAN_POLICY
Router(config-pmap)# class VOICE
Router(config-pmap-c)# priority percent 30
Router(config-pmap-c)# set dscp ef

Router(config-pmap)# class VIDEO
Router(config-pmap-c)# bandwidth percent 25
Router(config-pmap-c)# set dscp af41

Router(config-pmap)# class CRITICAL_DATA
Router(config-pmap-c)# bandwidth percent 25
Router(config-pmap-c)# set dscp af31

Router(config-pmap)# class BULK_DATA
Router(config-pmap-c)# bandwidth percent 10
Router(config-pmap-c)# set dscp af11

Router(config-pmap)# class class-default
Router(config-pmap-c)# bandwidth percent 10
Router(config-pmap-c)# random-detect dscp-based

# Apply Policy
Router(config-if)# service-policy output WAN_POLICY
```

### **Load Balancing Algorithms**

#### **Different Load Balancing Methods**:
```bash
# Round Robin
Router(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1
Router(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.2

# Weighted Round Robin
Router(config)# ip sla 1
Router(config-ip-sla)# icmp-echo 192.168.1.1
Router(config-ip-sla-echo)# frequency 10
Router(config)# ip sla schedule 1 life forever start-time now

Router(config)# track 1 ip sla 1
Router(config-track)# delay down 10 up 5

Router(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1 track 1
Router(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.2 10
```

### **Network Optimization Techniques**

#### **TCP Optimization**:
```bash
# Linux TCP Tuning
# /etc/sysctl.conf
net.core.rmem_default = 31457280
net.core.rmem_max = 67108864
net.core.wmem_default = 31457280
net.core.wmem_max = 67108864
net.core.somaxconn = 65535
net.core.netdev_max_backlog = 65535
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_rmem = 4096 65536 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_congestion_control = bbr
```

#### **Buffer Management**:
```bash
# Cisco Buffer Tuning
Router(config-if)# tx-ring-limit 512
Router(config-if)# hold-queue 300 out
Router(config-if)# hold-queue 300 in

# Interface Queue Management
Router(config-if)# random-detect
Router(config-if)# random-detect precedence 0 10 20 10
Router(config-if)# random-detect precedence 1 15 25 10
```

---

## Real-World Network Scenarios

### **Enterprise Network Design**

#### **Three-Tier Architecture**:
```
Core Layer (Layer 3 Switching):
- High-speed switching
- Routing between distribution layers
- Redundancy and load balancing

Distribution Layer (Layer 3 Switching):
- Aggregation point for access switches
- Policy enforcement
- VLAN routing

Access Layer (Layer