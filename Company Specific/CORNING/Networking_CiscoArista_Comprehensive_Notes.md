# Networking & Cisco/Arista — Comprehensive Study Notes
### For Corning OA Prep | JD6 (Global Network Services)
> Covers: OSI/TCP-IP, IP addressing, switching, routing protocols, VLANs, WAN, security, wireless, Cisco IOS/NX-OS, Arista EOS, monitoring, automation, CCNA-level concepts.

---

## 1. Networking Fundamentals

### 1.1 Why Networking?
A computer network allows devices to communicate — sharing resources, data, and services. Enterprise networks connect thousands of devices across global offices, data centers, and remote workers securely and reliably.

### 1.2 OSI Model — 7 Layers
The OSI (Open Systems Interconnection) model is a conceptual framework describing how network communication works. Each layer has specific functions and talks to layers above and below it.

| Layer | Name | Function | Key Protocols/Devices |
|---|---|---|---|
| 7 | Application | User-facing network services | HTTP, HTTPS, FTP, SMTP, DNS, DHCP, SSH, SNMP |
| 6 | Presentation | Data formatting, encryption, compression | SSL/TLS, JPEG, MPEG |
| 5 | Session | Establish, manage, terminate sessions | NetBIOS, RPC |
| 4 | Transport | End-to-end delivery, error recovery, flow control | TCP, UDP, port numbers |
| 3 | Network | Logical addressing, routing between networks | IP, ICMP, OSPF, BGP, routers |
| 2 | Data Link | Physical addressing (MAC), framing, error detection | Ethernet, 802.1Q, switches, MAC addresses |
| 1 | Physical | Bits on wire/air; electrical signals | Cables, hubs, Wi-Fi radio, fiber |

**Mnemonic (top to bottom):** "All People Seem To Need Data Processing"

**Key device-layer mapping:**
- **Hub:** Layer 1 — repeats bits to all ports; creates collision domains
- **Switch:** Layer 2 — forwards frames based on MAC addresses; creates separate collision domains per port
- **Router:** Layer 3 — routes packets based on IP addresses between networks
- **Firewall:** Layer 3–7 — filters traffic based on rules

### 1.3 TCP/IP Model (Practical 4-layer Model)
The real-world model used on the internet:
| TCP/IP Layer | OSI Equivalent | Protocols |
|---|---|---|
| Application | 5, 6, 7 | HTTP, HTTPS, DNS, SMTP, FTP, SSH |
| Transport | 4 | TCP, UDP |
| Internet | 3 | IP, ICMP, ARP |
| Network Access | 1, 2 | Ethernet, Wi-Fi |

### 1.4 TCP vs. UDP
| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (3-way handshake) | Connectionless |
| Reliability | Guaranteed delivery, ordering, error recovery | Best-effort, no guarantee |
| Speed | Slower (overhead of reliability) | Faster (no overhead) |
| Flow control | Yes | No |
| Use cases | HTTP/S, FTP, SSH, email, databases | DNS, DHCP, video streaming, VoIP, gaming |

**TCP 3-way handshake:** SYN → SYN-ACK → ACK (establishes connection before data)

**TCP 4-way termination:** FIN → ACK → FIN → ACK

### 1.5 Common Port Numbers
| Port | Protocol | Service |
|---|---|---|
| 20/21 | TCP | FTP (data/control) |
| 22 | TCP | SSH |
| 23 | TCP | Telnet (insecure) |
| 25 | TCP | SMTP (email sending) |
| 53 | TCP/UDP | DNS |
| 67/68 | UDP | DHCP (server/client) |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 (email retrieval) |
| 143 | TCP | IMAP (email retrieval) |
| 161/162 | UDP | SNMP (get/trap) |
| 389 | TCP | LDAP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB (Windows file sharing) |
| 514 | UDP | Syslog |
| 3389 | TCP | RDP |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |

---

## 2. IP Addressing

### 2.1 IPv4 Addressing
- **Format:** 4 octets (bytes) of 8 bits each, written in dotted decimal: `192.168.1.100`
- **Range:** 0.0.0.0 to 255.255.255.255 (2³² = ~4.3 billion addresses)
- **Classes (historical):**
  - Class A: 1.0.0.0 – 126.255.255.255 (default /8 mask; 16M hosts per network)
  - Class B: 128.0.0.0 – 191.255.255.255 (default /16; 65K hosts)
  - Class C: 192.0.0.0 – 223.255.255.255 (default /24; 254 hosts)

**Private IP Ranges (RFC 1918 — not routable on internet):**
- `10.0.0.0/8` — Class A private (10.x.x.x)
- `172.16.0.0/12` — Class B private (172.16.x.x – 172.31.x.x)
- `192.168.0.0/16` — Class C private (192.168.x.x)

**Special addresses:**
- `127.0.0.1` — Loopback (localhost)
- `169.254.x.x` — APIPA (link-local, when DHCP fails)
- `0.0.0.0` — Default route (matches any network)
- `255.255.255.255` — Limited broadcast

### 2.2 Subnet Masking and CIDR
**Subnet Mask:** Defines which part of an IP address is the network and which is the host.
- `255.255.255.0` = /24 = 24 bits for network, 8 bits for hosts

**CIDR Notation:** `/prefix` where prefix = number of network bits.

**Quick subnet reference:**
| CIDR | Subnet Mask | # Hosts | # Subnets from /24 |
|---|---|---|---|
| /24 | 255.255.255.0 | 254 | 1 |
| /25 | 255.255.255.128 | 126 | 2 |
| /26 | 255.255.255.192 | 62 | 4 |
| /27 | 255.255.255.224 | 30 | 8 |
| /28 | 255.255.255.240 | 14 | 16 |
| /29 | 255.255.255.248 | 6 | 32 |
| /30 | 255.255.255.252 | 2 | 64 (point-to-point links) |

**Usable hosts = 2ⁿ - 2** where n = host bits (subtract network address and broadcast address)

**Subnetting example:** `192.168.10.0/24` divided into /26 subnets:
- Subnet 1: 192.168.10.0/26 (hosts: .1–.62; broadcast: .63)
- Subnet 2: 192.168.10.64/26 (hosts: .65–.126; broadcast: .127)
- Subnet 3: 192.168.10.128/26 (hosts: .129–.190; broadcast: .191)
- Subnet 4: 192.168.10.192/26 (hosts: .193–.254; broadcast: .255)

### 2.3 IPv6 Addressing
- **Format:** 8 groups of 4 hexadecimal digits: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- **Abbreviation rules:** Leading zeros omitted; consecutive all-zero groups replaced with `::` (once)
  - `2001:db8:85a3::8a2e:370:7334`
- **Size:** 128 bits = 2¹²⁸ ≈ 3.4×10³⁸ addresses (essentially unlimited)
- **No NAT needed** — every device can have a public address
- **No broadcast** — uses multicast instead
- **SLAAC (Stateless Address Autoconfiguration):** Devices auto-configure IPv6 address from router advertisement

**Important IPv6 prefixes:**
- `::1` — IPv6 loopback (equivalent to 127.0.0.1)
- `fe80::/10` — Link-local (auto-configured; not routed)
- `fc00::/7` — Unique local (private; like RFC 1918)
- `2000::/3` — Global unicast (public, routable internet addresses)
- `ff00::/8` — Multicast

### 2.4 NAT (Network Address Translation)
**Why NAT:** Private IP addresses can't route on the internet. NAT translates private IPs to public IPs at the router.

**Types:**
- **Static NAT:** One private IP ↔ one public IP (1:1 mapping)
- **Dynamic NAT:** Pool of public IPs assigned to private IPs as needed
- **PAT (Port Address Translation) / NAT Overload:** Many private IPs share one public IP, differentiated by port numbers. This is what home/enterprise NAT uses. `192.168.1.x:port → 203.0.113.1:unique_port`

---

## 3. Switching

### 3.1 How Switches Work
A switch operates at Layer 2 using **MAC (Media Access Control) addresses** to make forwarding decisions.

**MAC Address Table (CAM Table):**
- Switch learns MAC addresses by observing source MAC on incoming frames
- Associates MAC with the port the frame arrived on
- When forwarding, looks up destination MAC → sends only to that port
- Unknown destination → flood to all ports (except source port)
- Broadcast (FF:FF:FF:FF:FF:FF) → flood to all ports

**ARP (Address Resolution Protocol):**
- Maps known IP address → unknown MAC address
- Device broadcasts: "Who has IP 192.168.1.1?"
- Device with that IP replies: "I have 192.168.1.1, my MAC is AA:BB:CC:DD:EE:FF"
- Result stored in ARP cache for a few minutes

### 3.2 VLANs (Virtual Local Area Networks)
**What:** Logically segment a physical switch into separate broadcast domains. Devices in different VLANs cannot communicate directly — they need a router/Layer 3 switch.

**Why use VLANs:**
- Security isolation (HR can't reach Engineering network)
- Performance (reduce broadcast domain size)
- Organization (logical grouping by department)
- Flexibility (VLAN membership by software config, not physical port)

**VLAN Tagging — IEEE 802.1Q:**
- 4-byte tag inserted into Ethernet frame header
- Contains: TPID (0x8100), PCP (priority), DEI, **VID (VLAN ID 1–4094)**
- Maximum 4,094 VLANs (VXLAN extends this to 16M)

**Access Ports:** Carry ONE VLAN; frames are untagged. Used for end devices (PCs, printers, IP phones).

**Trunk Ports:** Carry MULTIPLE VLANs; frames are tagged with 802.1Q. Used for switch-to-switch and switch-to-router links.

**Native VLAN:** On a trunk port, one VLAN is "native" — frames for it are sent untagged. Default is VLAN 1. Security best practice: change native VLAN to an unused VLAN.

### 3.3 Inter-VLAN Routing
Devices in different VLANs need Layer 3 routing to communicate:

**Router-on-a-Stick:**
- One router physical port configured as trunk
- Sub-interfaces created: `interface g0/0.10`, `encapsulation dot1q 10`, `ip address 192.168.10.1 255.255.255.0`
- Simple but single point of congestion (all inter-VLAN traffic through one link)

**Layer 3 Switch with SVIs (Switched Virtual Interfaces):**
- Switch performs routing internally
- SVI: Virtual interface for each VLAN (`interface vlan 10`, `ip address 192.168.10.1 255.255.255.0`)
- Much faster than router-on-a-stick; most enterprise switches use this

### 3.4 Spanning Tree Protocol (STP)
**Problem:** Redundant switch links create Layer 2 loops → broadcast storms → network crash.

**STP (IEEE 802.1D):** Prevents loops by blocking redundant paths.
- **Root Bridge election:** Switch with lowest Bridge ID (priority + MAC address) becomes root. All ports on root bridge are **Designated** (forwarding).
- **Root Port:** Non-root switch's port with lowest cost path to root bridge (forwarding).
- **Designated Port:** Per segment, the port with lowest cost to root (forwarding).
- **Blocked Port:** All other ports (blocking — discards frames, listens for BPDUs only).

**STP port states:** Blocking → Listening (15s) → Learning (15s) → Forwarding. Total: 30–50 seconds to converge.

**RSTP (IEEE 802.1W — Rapid STP):** Improved STP with near-instant convergence (seconds).
- New port roles: Alternate port (backup blocked port), Backup port
- Proposal/Agreement mechanism replaces slow timer-based convergence
- Backward compatible with STP

**PVST+ / RPVST+ (Cisco):** Run separate STP instance per VLAN — allows load balancing across redundant links.

**MSTP (IEEE 802.1S):** Multiple VLANs mapped to instances — reduces number of STP instances compared to PVST+.

### 3.5 Link Aggregation (LACP / EtherChannel)
**Problem:** Single uplinks are bottlenecks; redundant links blocked by STP.

**Solution:** Bundle multiple physical links into one logical link.
- **LACP (IEEE 802.3ad):** Dynamic negotiation protocol (open standard). Modes: active (initiate) or passive (respond).
- **PAgP (Cisco proprietary):** Auto or desirable modes.
- **Static EtherChannel:** No negotiation protocol (on/on).

**Benefits:** Increased bandwidth (sum of member links), redundancy (failure of one link doesn't break bundle), STP sees logical link (no blocking).

---

## 4. Routing

### 4.1 How Routers Work
Routers operate at Layer 3, forwarding packets between different IP networks using a **routing table**.

**Routing table entries contain:**
- Destination network (IP prefix/mask)
- Next-hop IP address or exit interface
- Metric (cost/distance)
- Administrative distance (trustworthiness of source)
- Route type (C=Connected, S=Static, O=OSPF, B=BGP, etc.)

**Longest prefix match:** Router always uses the most specific (longest) matching route.

### 4.2 Administrative Distance (AD)
When multiple routing sources advertise the same network, router prefers **lower AD**:
| Route Source | AD |
|---|---|
| Connected | 0 |
| Static | 1 |
| EIGRP summary | 5 |
| External BGP (eBGP) | 20 |
| EIGRP | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| Internal BGP (iBGP) | 200 |

### 4.3 Static Routing
- Manually configured routes
- `ip route 10.0.0.0 255.0.0.0 192.168.1.1` (destination network/mask → next hop)
- **Default route:** `ip route 0.0.0.0 0.0.0.0 [next-hop]` — matches everything; used for internet-bound traffic
- Simple, predictable, no overhead; doesn't adapt to topology changes
- Used for: small networks, specific path control, stub networks

### 4.4 OSPF (Open Shortest Path First)
**Type:** Link-State Interior Gateway Protocol (IGP)
**Algorithm:** Dijkstra's Shortest Path First (SPF)

**How OSPF works:**
1. **Hello packets:** Discover and maintain neighbor relationships (sent every 10s on broadcast, 30s on serial)
2. **Adjacency formation:** Neighbors become adjacent (exchange full topology info)
3. **LSA (Link State Advertisement):** Each router floods topology information to all others
4. **LSDB (Link State Database):** Every router builds identical complete topology map
5. **SPF calculation:** Dijkstra runs on LSDB → each router calculates shortest path to every destination
6. **Routing table:** Best paths installed

**OSPF Areas:**
- Reduces LSA flooding by creating hierarchical areas
- **Area 0 (Backbone):** All areas must connect to area 0 (directly or via virtual link)
- **ABR (Area Border Router):** Connects two areas; summarizes routes
- **ASBR (Autonomous System Boundary Router):** Connects OSPF to external routing domains

**OSPF cost:** 100Mbps / interface bandwidth (default reference 100Mbps; configure `auto-cost reference-bandwidth 10000` for 10Gbps networks)

**OSPF DR/BDR election (on multi-access networks):**
- Designated Router (DR) and Backup DR elected to reduce LSA flooding on broadcast segments
- Highest priority wins (default 1); tie-broken by highest Router ID
- Once elected, stable (non-preemptive by default)

**OSPF Router ID:** Highest loopback IP > highest active interface IP. Configure explicitly: `router-id 1.1.1.1`

### 4.5 EIGRP (Enhanced Interior Gateway Routing Protocol)
**Type:** Advanced Distance Vector (Cisco hybrid) IGP
**Algorithm:** DUAL (Diffusing Update Algorithm) — guarantees loop-free paths

**Key features:**
- Rapid convergence — maintains backup paths (Feasible Successors) instantly usable if primary fails
- Partial updates — only send changes, not full table
- Supports unequal-cost load balancing (variance command)
- Supports multiple Layer 3 protocols (IPv4, IPv6, AppleTalk — legacy)
- Now partially open standard (RFC 7868)

**EIGRP metric:** Composite of bandwidth + delay (by default). Optional: load, reliability, MTU.

**EIGRP tables:**
- Neighbor table: Directly connected EIGRP neighbors
- Topology table: All paths learned from neighbors
- Routing table: Best paths (Successor routes) installed

### 4.6 BGP (Border Gateway Protocol)
**Type:** Path Vector Exterior Gateway Protocol (EGP)
**Purpose:** Inter-AS routing — the internet's routing protocol

**Why BGP is different:**
- Designed for routing between Autonomous Systems (AS) — separate organizations
- Policy-driven (not just shortest path) — you choose which paths to prefer, which to advertise
- Scalability — handles the full internet routing table (~1M routes)
- Slow convergence (by design — stability over speed)

**BGP types:**
- **iBGP:** BGP between routers within the same AS — full mesh required (or route reflectors)
- **eBGP:** BGP between routers in different ASes — the internet routing protocol

**Key BGP concepts:**
- **AS Number (ASN):** Unique identifier for each AS (1–65535 public; 64512–65535 private)
- **BGP Attributes:** Used for path selection and policy:
  - **Weight (Cisco proprietary):** Higher preferred; local to router
  - **LOCAL_PREF:** Higher preferred; propagated within AS; indicates preferred exit
  - **AS_PATH:** List of ASes the route passed through; shorter preferred; loop prevention
  - **MED (Multi-Exit Discriminator):** Lower preferred; suggests preferred entry into AS to external neighbors
  - **NEXT_HOP:** IP address of next-hop router for reaching the prefix

**BGP path selection order (memorize):** Weight → Local Pref → Locally Originated → AS_PATH length → Origin → MED → eBGP over iBGP → IGP metric to next-hop → Router ID

**Enterprise BGP use:** Multihoming (multiple ISP connections) for redundancy and traffic engineering

### 4.7 First Hop Redundancy Protocols (FHRP)
**Problem:** End devices have one default gateway configured. If that router fails, they lose connectivity.

**HSRP (Hot Standby Router Protocol — Cisco proprietary):**
- Two+ routers share a virtual IP and virtual MAC
- Active router handles traffic; Standby router monitors
- If Active fails, Standby takes over (sub-second with tuned timers)
- Preemption: higher priority router can take active role back
- Version 2: supports IPv6, more groups

**VRRP (Virtual Router Redundancy Protocol — IEEE 802.3):** Open standard equivalent of HSRP

**GLBP (Gateway Load Balancing Protocol — Cisco):** Load balances across multiple routers (all active) while providing redundancy

---

## 5. WAN Technologies

### 5.1 MPLS (Multiprotocol Label Switching)
- High-performance WAN forwarding based on **labels** (short fixed-length values) instead of IP addresses
- Routers (Label Switched Routers — LSRs) swap labels at each hop through a predetermined path (LSP — Label Switched Path)
- **Benefits:** Fast forwarding (label lookup faster than IP lookup), QoS-capable, traffic engineering, VPN support
- **MPLS VPN:** Service providers use MPLS to create private Layer 3 VPNs for enterprise customers — sites appear connected to private network

### 5.2 SD-WAN (Software-Defined WAN)
**Problem:** Traditional WAN = expensive MPLS + complexity. Branch offices need secure internet access too.

**SD-WAN solution:**
- Abstract the WAN transport — works over any link (MPLS, broadband, LTE, satellite)
- Centralized controller manages all WAN edge devices
- **Application-aware routing:** Route specific applications over best path (VoIP over low-latency, backups over cheap broadband)
- **Automatic failover:** If MPLS fails, instantly use broadband (sub-second)
- **Centralized policy:** Deploy consistent policies across all branches from one place
- **Security:** Built-in firewall, encryption (IPSec), segmentation

**Vendors:** Cisco Viptela, Cisco Meraki, VMware VeloCloud, Fortinet, Aruba

### 5.3 VPN Technologies
**Site-to-Site VPN:** Permanent encrypted tunnel between two sites (router to router)
- Protocol: IPSec (most common)
- Used when dedicated MPLS not available or too expensive

**Remote Access VPN (SSL/TLS):** Individual user connects to corporate network
- Cisco AnyConnect, GlobalProtect — install client, authenticate, tunnel to VPN gateway
- **Split tunneling:** Only corporate traffic goes through VPN; internet traffic goes directly (performance optimization)
- **Full tunnel:** All traffic through VPN (security, compliance requirement)

**IPSec:** Suite of protocols for encrypting and authenticating IP packets
- **IKE (Internet Key Exchange):** Negotiate security parameters and exchange encryption keys
- **AH (Authentication Header):** Authentication only (no encryption)
- **ESP (Encapsulating Security Payload):** Encryption + authentication (most common)
- **Tunnel mode:** Encrypts entire original packet + new IP header (site-to-site)
- **Transport mode:** Encrypts only payload + original header (end-to-end)

---

## 6. Network Security

### 6.1 Access Control Lists (ACLs)
Packet filters applied to router/switch interfaces:

**Standard ACL (1–99, 1300–1999):** Filter on source IP only
- Apply **close to destination** (affects all traffic from that source)

**Extended ACL (100–199, 2000–2699):** Filter on source IP, destination IP, protocol, port
- Apply **close to source** (stop traffic early before it traverses network)
- `access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 80` (allow HTTP from 192.168.1.x to anywhere)

**Named ACLs:** More descriptive, easier to edit (add/remove individual entries without rewriting entire ACL)

**ACL processing:** Top-down — first match wins. Implicit deny all at end of every ACL.

**Wildcard mask:** Inverse of subnet mask. `0.0.0.255` = match any host in any subnet with same first 3 octets.

### 6.2 Network Security Features

**DHCP Snooping:**
- Switch security feature — builds binding table of legitimate DHCP assignments (IP + MAC + port + VLAN)
- Blocks DHCP server messages on untrusted ports (only uplinks to legitimate DHCP servers are trusted)
- Prevents rogue DHCP servers and DHCP starvation attacks

**Dynamic ARP Inspection (DAI):**
- Validates ARP packets against DHCP snooping binding table
- Drops ARP replies that don't match binding table → prevents ARP spoofing/poisoning

**Port Security:**
- Restrict which MAC addresses can access a switch port
- Maximum MAC count per port
- Violation action: Shutdown (disable port), Restrict (drop + log), Protect (drop silently)
- **Sticky MAC:** Dynamically learn MACs and add them to running config

**802.1X (Port-Based Network Access Control):**
- Authentication required before network access granted
- Three parties: Supplicant (end device), Authenticator (switch), Authentication Server (RADIUS)
- Works with Cisco ISE (Identity Services Engine) or other RADIUS servers
- Can assign VLAN based on user/device identity

**AAA (Authentication, Authorization, Accounting):**
- **Authentication:** Who are you? (username/password, certificates)
- **Authorization:** What can you do? (privilege level, allowed commands)
- **Accounting:** What did you do? (audit log)
- Implemented via RADIUS or TACACS+ protocols

### 6.3 Firewall Types
- **Stateful:** Tracks connection state; allows return traffic automatically; most common
- **Stateless (ACL-based):** Each packet evaluated independently; no state tracking
- **Next-Generation Firewall (NGFW):** Deep packet inspection, application awareness, intrusion prevention (IPS), SSL inspection, URL filtering (Cisco ASA/Firepower, Palo Alto, Fortinet)

---

## 7. Cisco IOS — CLI Reference

### 7.1 IOS Modes
```
Router>           ← User EXEC mode (limited show commands)
Router# enable    ← Privileged EXEC mode (all show commands, debug)
Router(config)# configure terminal  ← Global Config mode
Router(config-if)# interface g0/0   ← Interface Config mode
Router(config-router)# router ospf 1 ← Routing protocol config mode
Router(config)# exit / end / Ctrl+Z  ← Return to privileged EXEC
```

### 7.2 Essential Show Commands
```bash
show running-config          # Active configuration in RAM
show startup-config          # Configuration saved in NVRAM
show ip interface brief      # Quick interface status (IP, up/down)
show interfaces              # Detailed stats (errors, drops, utilization)
show ip route                # IP routing table
show ip route ospf           # OSPF routes only
show ip ospf neighbor        # OSPF neighbor table (check Full state)
show ip ospf database        # OSPF LSDB
show ip bgp summary          # BGP neighbor summary
show ip bgp                  # BGP routing table
show vlan brief              # VLAN database (switches)
show interfaces trunk        # Trunk port status and VLANs
show spanning-tree           # STP status and port roles
show mac address-table       # MAC address table (switches)
show arp                     # ARP cache
show ip protocols            # Active routing protocols summary
show version                 # IOS version, uptime, hardware
show cdp neighbors detail    # Directly connected Cisco neighbors
show lldp neighbors detail   # LLDP (open standard) neighbors
```

### 7.3 Common Configuration Commands
```bash
# Hostname
hostname CORE-SW-01

# Interface configuration
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
 description "Link to Distribution SW"

# Save configuration
copy running-config startup-config   # or: write memory / wr

# VLAN configuration (switch)
vlan 10
 name FINANCE
interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 10

# Trunk port
interface GigabitEthernet0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30

# OSPF
router ospf 1
 router-id 1.1.1.1
 network 192.168.0.0 0.0.255.255 area 0
 passive-interface GigabitEthernet0/0  ! Don't send hellos on this interface

# Static route
ip route 10.0.0.0 255.0.0.0 192.168.1.254
ip route 0.0.0.0 0.0.0.0 203.0.113.1   ! Default route

# ACL
ip access-list extended BLOCK_TELNET
 deny tcp any any eq 23
 permit ip any any
interface GigabitEthernet0/1
 ip access-group BLOCK_TELNET in
```

### 7.4 IOS Versions
- **IOS (Classic):** Original monolithic OS — still on older routers/switches
- **IOS XE:** Linux kernel + IOS daemon — modular, modern, programmable (Catalyst 9000, ASR routers)
- **IOS XR:** Microkernel, carrier-grade, ISSU (In-Service Software Upgrade) support (ASR 9000, NCS — service providers and large enterprise)
- **NX-OS:** Data center focused; runs on Nexus switches; vPC, FabricPath support
- **All support:** NETCONF, RESTCONF, gNMI, Python (IOS XE, NX-OS, IOS XR)

---

## 8. Arista EOS

### 8.1 What Makes EOS Different
- **Single binary image:** All features in one image; no separate add-ons to install
- **Linux kernel:** Arista EOS runs on standard Linux — full Linux tools available (bash, Python, tcpdump)
- **Stability:** If control plane crashes, data plane keeps forwarding (decoupled)
- **Programmability (first-class):**
  - **eAPI:** HTTP/HTTPS REST API — send JSON commands, receive JSON responses
  - **OpenConfig / YANG models:** Standard model-driven management
  - **Streaming telemetry (native):** gNMI/gRPC streaming out of the box
  - **Python in CLI:** Run Python scripts directly from EOS CLI
  - **NAPALM, Ansible, pyeapi support:** Rich automation ecosystem

### 8.2 Arista CLI Similarities to Cisco
Arista EOS CLI is **very similar to Cisco IOS** — intentional design choice for easy migration:
```bash
show ip route
show interfaces
show vlan
show running-config
show bgp summary
configure terminal
interface Ethernet1
  ip address 10.0.0.1/24
  no shutdown
```
Key differences:
- Arista uses `/prefix` notation directly in CLI: `ip address 10.0.0.1/24` (Cisco uses `ip address 10.0.0.1 255.255.255.0`)
- `show ip bgp` vs Arista `show bgp`
- `commit` required in some Arista config modes (network rollback support)

### 8.3 Arista eAPI
HTTP/HTTPS JSON-based API for programmatic management:
```python
import requests
import json

payload = {
    "jsonrpc": "2.0",
    "method": "runCmds",
    "params": {
        "format": "json",
        "cmds": ["show version", "show interfaces"]
    },
    "id": "1"
}

response = requests.post(
    "https://192.168.1.1/command-api",
    auth=("admin", "password"),
    json=payload,
    verify=False
)
data = response.json()
```

**pyeapi:** Python library wrapping eAPI for cleaner code:
```python
import pyeapi
node = pyeapi.connect(host='192.168.1.1', username='admin', password='password')
result = node.execute(['show version'])
```

### 8.4 Arista CloudVision
- **Network management and automation platform** for Arista devices
- **Zero Touch Provisioning (ZTP):** New device boots, gets config from CloudVision automatically
- **Network-wide telemetry:** Real-time streaming telemetry from all devices → historical data, dashboards
- **Change control:** Workflow-driven network change management with rollback
- **Compliance auditing:** Verify configs against standards
- **Studio:** Template-based configuration deployment
- Equivalent to Cisco DNA Center / Catalyst Center

### 8.5 Arista MLAG (Multi-Chassis Link Aggregation)
- Two Arista switches appear as one logical switch to connected devices
- Downstream device sees one LACP partner with two physical links
- **Peer-link:** High-bandwidth link between MLAG peer switches (carries control traffic and some data)
- **MLAG Domain:** Identifies the MLAG pair; must match on both peers
- Eliminates STP-blocked ports → full bandwidth utilization + redundancy
- Equivalent to Cisco vPC on Nexus

### 8.6 Arista Certifications
- **ACE (Arista Certified Engineer):** Entry level (L2, L3, basic EOS)
- **ACE-L (Arista Certified Engineer — L3):** Routing, BGP, EVPN
- **ACE-P (Arista Certified Professional):** Advanced, data center, automation
- CCNA holders can transition to ACE with focused study on EOS CLI differences

---

## 9. Network Monitoring

### 9.1 SNMP (Simple Network Management Protocol)
**Components:**
- **SNMP Manager (NMS):** Polling system (SolarWinds, CACTI, Zabbix)
- **SNMP Agent:** Runs on managed device (router, switch) — responds to polls
- **MIB (Management Information Base):** Hierarchical database of variables (OIDs) the agent exposes
- **OID (Object Identifier):** Unique identifier for each variable (e.g., `1.3.6.1.2.1.1.5.0` = sysName)

**SNMP Operations:**
- **GET:** Manager requests value of specific OID
- **GETNEXT/GETBULK:** Retrieve multiple OIDs efficiently (table walking)
- **SET:** Manager changes a value on device
- **TRAP:** Device proactively sends alert to manager (unsolicited) — e.g., link down
- **INFORM:** Like TRAP but requires acknowledgment

**SNMP versions:**
- **v1:** Original; community strings; insecure
- **v2c:** Improved performance (GETBULK); still community strings (plaintext)
- **v3:** Adds authentication (MD5/SHA) and encryption (AES/DES) — use this in production

**Community strings (v1/v2c):** Password-like strings; read-only (`public`) and read-write (`private`). NEVER use defaults in production.

### 9.2 SolarWinds NPM (Network Performance Monitor)
- Commercial monitoring platform for enterprise networks
- **Auto-discovery:** Scans network via SNMP, ICMP, CDP/LLDP → discovers all devices
- **Interface monitoring:** Bandwidth utilization, errors, discards per interface
- **Node monitoring:** CPU, memory, availability per device
- **NetFlow analysis:** SolarWinds NTA (Network Traffic Analyzer) for flow-based analysis
- **Alerts:** Threshold-based alerting (email, SMS, ticketing integration)
- **Topology maps:** Visual network maps auto-generated from discovery
- **NCM (Network Configuration Manager):** Backup configs, detect changes, compliance auditing

### 9.3 CACTI
- Open-source network graphing tool built on RRDTool
- Poll devices via SNMP → store time-series data in RRD files → generate graphs
- Primarily used for: interface utilization over time, CPU/memory trends
- Web-based UI for configuring data sources, graphs, templates
- Less feature-rich than SolarWinds but free

### 9.4 NetFlow / IPFIX
- Routers/switches export **metadata about IP flows** to a collector
- Flow = 5-tuple: source IP + destination IP + source port + destination port + protocol
- Data collected: bytes, packets, start/end time, interface
- **Use cases:** Bandwidth analysis (who's using the bandwidth), capacity planning, security (detect anomalous traffic patterns), application visibility, billing

**NetFlow vs. SNMP:**
- SNMP tells you: how much bandwidth total on an interface
- NetFlow tells you: what applications/hosts are consuming that bandwidth

### 9.5 Streaming Telemetry
Modern alternative to SNMP polling:
- **Push-based:** Device streams data continuously without being asked
- **Protocol:** gNMI (gRPC Network Management Interface) or NETCONF
- **Data model:** YANG-modeled data (structured, consistent across vendors)
- **Frequency:** Sub-second resolution vs SNMP's minimum ~30 second intervals
- **Consumers:** InfluxDB + Grafana, Elastic Stack, vendor tools (CloudVision, Cisco Crosswork)

---

## 10. Network Automation

### 10.1 Why Automate Networks?
- **Scale:** Managing 500 devices manually is impossible; automation scales to thousands
- **Consistency:** Automated configs are identical; manual configs drift
- **Speed:** Deploy changes in minutes across all devices; not days of manual work
- **Error reduction:** No typos, no missed devices
- **Compliance:** Automatically verify configs match policy; detect drift

### 10.2 Python for Network Automation

**Netmiko (most common for SSH-based automation):**
```python
from netmiko import ConnectHandler

device = {
    'device_type': 'cisco_ios',      # or 'arista_eos', 'cisco_nxos', etc.
    'host': '192.168.1.1',
    'username': 'admin',
    'password': 'password',
}

with ConnectHandler(**device) as net_connect:
    output = net_connect.send_command('show ip route')
    print(output)
    
    # Configuration commands
    config_commands = [
        'interface GigabitEthernet0/1',
        'description AUTOMATED',
        'no shutdown'
    ]
    net_connect.send_config_set(config_commands)
    net_connect.save_config()
```

**NAPALM (vendor-neutral API):**
```python
from napalm import get_network_driver

driver = get_network_driver('ios')      # or 'eos', 'junos', 'nxos'
device = driver('192.168.1.1', 'admin', 'password')
device.open()

# Getters - return structured data (not text)
facts = device.get_facts()              # hostname, vendor, model, OS version
interfaces = device.get_interfaces()   # all interface details
bgp_neighbors = device.get_bgp_neighbors()

# Configuration management
device.load_merge_candidate(filename='new_config.txt')
print(device.compare_config())         # Diff: what would change
device.commit_config()                 # Apply changes
device.close()
```

**TextFSM / NTC Templates (parse CLI output):**
```python
import textfsm

# Parse 'show ip route' into structured data
with open('show_ip_route.textfsm') as template:
    fsm = textfsm.TextFSM(template)
    result = fsm.ParseText(cli_output)
    # result is a list of lists with parsed values
```

### 10.3 Ansible for Networks

**Key concepts:**
- **Agentless:** No software installed on network devices; uses SSH or API
- **Idempotent:** Running playbook multiple times gives same result as running once
- **Inventory:** Define devices and groups in INI or YAML files
- **Modules:** Network-specific modules (`cisco.ios.ios_config`, `arista.eos.eos_command`, `junipernetworks.junos.junos_config`)
- **Playbooks:** YAML files defining automation tasks

**Example playbook:**
```yaml
---
- name: Configure OSPF on Cisco routers
  hosts: cisco_routers
  gather_facts: no
  
  tasks:
    - name: Show current OSPF neighbors
      cisco.ios.ios_command:
        commands: show ip ospf neighbor
      register: ospf_output
      
    - name: Configure OSPF
      cisco.ios.ios_config:
        lines:
          - router ospf 1
          - router-id {{ inventory_hostname_short }}
          - network 0.0.0.0 255.255.255.255 area 0
        
    - name: Save configuration
      cisco.ios.ios_config:
        save_when: always
```

### 10.4 NETCONF / RESTCONF / YANG
**YANG:** Data modeling language defining structure of network configuration and state data

**NETCONF (RFC 6241):**
- SSH-based protocol; XML payload
- Operations: get-config, edit-config, copy-config, commit, lock, unlock
- Used when full configuration management needed with transactional commit

**RESTCONF (RFC 8040):**
- HTTP/HTTPS REST API; JSON or XML payload
- Simpler than NETCONF for developers familiar with REST
- Supports: GET (read), PUT (replace), POST (create), PATCH (modify), DELETE

**Python with NETCONF (ncclient):**
```python
from ncclient import manager

with manager.connect(host='192.168.1.1', port=830, username='admin', 
                     password='pass', hostkey_verify=False) as m:
    # Get running config
    config = m.get_config(source='running')
    print(config.xml)
```

### 10.5 OpenConfig
Vendor-neutral YANG data models defining standard structure for:
- Interface configuration and statistics
- BGP, OSPF, IS-IS configuration
- QoS, VLANs, LLDP, LACP
- Telemetry subscriptions

Supported by: Cisco IOS XE, IOS XR, NX-OS; Arista EOS; Juniper JunOS; Nokia SR OS

Goal: write automation code once, run against any vendor's devices

---

## 11. Wireless Networking

### 11.1 Wi-Fi Standards
| Standard | Max Speed | Frequency | Key Feature |
|---|---|---|---|
| 802.11a | 54 Mbps | 5 GHz | Early 5GHz |
| 802.11b | 11 Mbps | 2.4 GHz | Early 2.4GHz |
| 802.11g | 54 Mbps | 2.4 GHz | Combined |
| 802.11n (Wi-Fi 4) | 600 Mbps | 2.4/5 GHz | MIMO |
| 802.11ac (Wi-Fi 5) | 3.5 Gbps | 5 GHz | MU-MIMO, beamforming |
| 802.11ax (Wi-Fi 6/6E) | 9.6 Gbps | 2.4/5/6 GHz | OFDMA, BSS Coloring, TWT (battery), dense environments |
| 802.11be (Wi-Fi 7) | 46 Gbps | 2.4/5/6 GHz | Multi-Link Operation |

**2.4 GHz vs 5 GHz:**
- 2.4 GHz: Longer range, better penetration through walls; more interference (microwaves, Bluetooth, older Wi-Fi); only 3 non-overlapping channels (1, 6, 11)
- 5 GHz: Shorter range; less interference; 25 non-overlapping channels; faster throughput

### 11.2 Wireless Security
- **WEP:** Broken and deprecated — never use
- **WPA/WPA2:** Significant improvement; WPA2 with AES (CCMP) = current standard
- **WPA3:** Latest; more secure handshake (SAE), stronger encryption, easier IoT onboarding
- **802.1X (Enterprise mode):** RADIUS-based authentication — each user/device authenticates individually; no shared PSK

---

## 12. Quick Reference Cheat Sheet

### Troubleshooting Methodology
1. **Identify the problem** — what exactly is failing? Who is affected?
2. **Gather information** — show commands, ping, traceroute, check logs
3. **Establish baseline** — compare to known good state
4. **Identify possible causes** — OSI layer by layer (start at layer 1)
5. **Test theory** — make one change at a time
6. **Confirm resolution** — verify fix worked
7. **Document** — record root cause and resolution

**OSI layer-by-layer troubleshooting:**
- Layer 1: `show interfaces` — is it up/up? Any physical errors? Check cable, SFP
- Layer 2: `show mac address-table` — is MAC learned? `show spanning-tree` — any blocking?
- Layer 3: `show ip route` — is route present? `ping` to default gateway
- Layer 4+: Telnet/curl to test port connectivity; check ACLs, firewall rules

### Key Terms Quick Reference
| Term | Meaning |
|---|---|
| MAC | Media Access Control — Layer 2 hardware address (48-bit) |
| ARP | Address Resolution Protocol — maps IP to MAC |
| CDP | Cisco Discovery Protocol — Layer 2 neighbor discovery |
| LLDP | Link Layer Discovery Protocol — open standard neighbor discovery |
| STP | Spanning Tree Protocol — prevents Layer 2 loops |
| RSTP | Rapid STP — faster convergence version of STP |
| LACP | Link Aggregation Control Protocol — bundle multiple links into one |
| OSPF | Open Shortest Path First — link-state IGP using Dijkstra |
| EIGRP | Enhanced IGRP — Cisco advanced distance vector IGP |
| BGP | Border Gateway Protocol — internet EGP, path vector |
| HSRP | Hot Standby Router Protocol — Cisco gateway redundancy |
| VRRP | Virtual Router Redundancy Protocol — open standard gateway redundancy |
| ACL | Access Control List — packet filter rules |
| NAT/PAT | Network/Port Address Translation — private to public IP mapping |
| MPLS | Multiprotocol Label Switching — label-based WAN forwarding |
| SD-WAN | Software-Defined WAN — centralized WAN management over any transport |
| SNMP | Simple Network Management Protocol — network monitoring |
| NetFlow | Cisco flow export protocol — traffic analysis metadata |
| gNMI | gRPC Network Management Interface — streaming telemetry |
| YANG | Yet Another Next Generation — data modeling language |
| NETCONF | Network Configuration Protocol — SSH-based config management |
| NX-OS | Cisco data center OS for Nexus switches |
| EOS | Extensible Operating System — Arista's Linux-based NOS |
| eAPI | Arista's HTTP/HTTPS JSON-RPC management API |
| MLAG | Multi-Chassis Link Aggregation — Arista's multi-chassis LAG |
| vPC | Virtual Port Channel — Cisco Nexus multi-chassis LAG |
| ZTP | Zero Touch Provisioning — automatic device bootstrapping |
| ISSU | In-Service Software Upgrade — upgrade with no traffic disruption |
| CCNA | Cisco Certified Network Associate — foundational Cisco certification |

---
*End of Networking & Cisco/Arista Comprehensive Notes — ~680 lines.*
*Read alongside: Shared_Networking_Windows.md, Shared_Advanced_Networking.md, Shared_Cisco_Arista.md, Shared_Network_Monitoring.md, Shared_Network_Automation.md for MCQ practice.*
