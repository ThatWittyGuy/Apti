# Shared — Advanced Networking MCQs (CCNA Level)
### Applies to: JD6 (Global Network Services) — routing, switching, protocols, subnetting beyond basics
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard
> Basic networking (IP, DNS, DHCP, OSI) → Shared_Networking_Windows.md

---

**Q1.** 🟢 What is "subnetting" and why is it used?
- A) Connecting multiple networks using a subnet device
- B) Dividing a larger IP network into smaller sub-networks — improving address utilization, reducing broadcast domains, enabling network segmentation, and applying security policies per segment
- C) A process for substituting failed network links
- D) Combining multiple IP addresses into a single range

**Answer: B**

---

**Q2.** 🟢 How many usable host addresses does a /24 subnet provide?
- A) 256
- B) 254 — a /24 has 256 addresses (2⁸), minus network address and broadcast address
- C) 255
- D) 252

**Answer: B**

---

**Q3.** 🟢 What is a "default gateway"?
- A) The default security setting on a network router
- B) The IP address of the router interface on a local subnet that hosts send traffic to when the destination is outside their subnet — the exit point from the local network
- C) The gateway used when no other gateway is configured
- D) The first IP address in a subnet always used as the gateway

**Answer: B**

---

**Q4.** 🟢 What is the difference between a "hub," a "switch," and a "router"?
- A) They are the same device with different names
- B) A hub broadcasts all traffic to all ports (Layer 1); a switch forwards frames intelligently to specific ports using MAC addresses (Layer 2); a router routes packets between different networks using IP addresses (Layer 3)
- C) A switch is wireless; a router is wired; a hub is optical
- D) Hubs are for LANs, switches for WANs, routers for the internet only

**Answer: B**

---

**Q5.** 🟢 What is a "MAC address"?
- A) An address used by Apple Mac computers on a network
- B) A unique 48-bit hardware address burned into a network interface card — used at Layer 2 (Data Link) for local network communication; written as six hex pairs (e.g., 00:1A:2B:3C:4D:5E)
- C) A network address assigned by DHCP
- D) A media access code used for wireless authentication

**Answer: B**

---

**Q6.** 🟢 What is "ARP" (Address Resolution Protocol)?
- A) A routing protocol for resolving AS paths
- B) A protocol that maps a known IP address to its corresponding MAC address on a local network — enabling Layer 3 to Layer 2 address resolution for local communication
- C) An authentication protocol for network access
- D) A protocol for resolving domain names to IP addresses

**Answer: B**

---

**Q7.** 🟡 What is "VLAN trunking" and which protocol is used?
- A) Combining multiple VLANs into one for simplicity
- B) Carrying traffic from multiple VLANs over a single physical link between switches — using IEEE 802.1Q tagging to identify which VLAN each frame belongs to; trunk ports carry all (or specified) VLANs
- C) A technique for encrypting VLAN traffic between switches
- D) A method for load-balancing traffic across multiple switch uplinks

**Answer: B**

---

**Q8.** 🟡 What is "STP" (Spanning Tree Protocol) and why is it needed?
- A) A protocol for managing secure traffic paths in VPNs
- B) A Layer 2 protocol (IEEE 802.1D) that prevents switching loops in redundant network topologies by electing a root bridge and blocking redundant paths — without it, broadcast storms would crash the network
- C) A protocol for routing traffic between spanning sites
- D) A protocol synchronizing time across network switches

**Answer: B**

---

**Q9.** 🟡 What is "RSTP" (Rapid Spanning Tree Protocol)?
- A) A replacement for STP using routing instead of switching
- B) An enhanced version of STP (IEEE 802.1W) that significantly reduces convergence time (from 30–50 seconds to near-instantaneous) by introducing port roles and states that allow faster failover to backup paths
- C) A protocol that runs STP on routers instead of switches
- D) A redundant STP instance for each VLAN

**Answer: B**

---

**Q10.** 🟡 What is "LACP" (Link Aggregation Control Protocol)?
- A) A protocol for managing link-layer access control
- B) An IEEE 802.3ad protocol for bundling multiple physical links between two devices into a single logical link (port channel/LAG) — increasing bandwidth and providing redundancy; LACP dynamically negotiates the bundle
- C) A Cisco protocol for managing access port configurations
- D) A load balancing protocol for WAN links

**Answer: B**

---

**Q11.** 🟡 What is "Inter-VLAN routing" and how is it achieved?
- A) Routing between VLANs on different physical networks
- B) Enabling communication between devices in different VLANs — achieved via a Layer 3 switch (using SVIs — Switched Virtual Interfaces) or a router with sub-interfaces ("router-on-a-stick"), as VLANs are isolated Layer 2 domains by default
- C) A Cisco feature routing VLAN traffic over MPLS
- D) Configuring VLANs to allow inter-department communication automatically

**Answer: B**

---

**Q12.** 🟡 What is "OSPF" (Open Shortest Path First)?
- A) An open-source firewall protocol
- B) A link-state Interior Gateway Protocol (IGP) that uses Dijkstra's SPF algorithm to calculate the shortest path to each destination — routers share Link State Advertisements (LSAs) to build a complete topology map; widely used in enterprise networks
- C) A distance-vector routing protocol similar to RIP
- D) A BGP extension for multi-area enterprise routing

**Answer: B**

---

**Q13.** 🟡 What is "EIGRP" (Enhanced Interior Gateway Routing Protocol)?
- A) An open-standard distance-vector routing protocol
- B) A Cisco-developed advanced distance-vector (hybrid) routing protocol using DUAL algorithm for loop-free, fast-converging routing — supports unequal-cost load balancing; proprietary to Cisco but now partially open standard
- C) A link-state protocol similar to OSPF
- D) An exterior gateway protocol for inter-AS routing

**Answer: B**

---

**Q14.** 🟡 What is the difference between "static routing" and "dynamic routing"?
- A) Static routing is automated; dynamic requires manual configuration
- B) Static routing uses manually configured routes that don't change automatically — simple, predictable, low overhead; dynamic routing uses protocols (OSPF, EIGRP, BGP) to automatically discover and update routes as topology changes
- C) Dynamic routing is only for WAN; static is only for LAN
- D) Static routing is faster; dynamic routing is more secure

**Answer: B**

---

**Q15.** 🟡 What is "BGP" (Border Gateway Protocol)?
- A) A basic routing protocol for small enterprise networks
- B) The internet's path vector Exterior Gateway Protocol — routing between Autonomous Systems (AS); used by enterprises for multihoming (multiple ISP connections) and internet edge routing; highly scalable with rich policy controls
- C) A backup routing protocol when OSPF fails in enterprise
- D) A LAN routing protocol for large campus environments

**Answer: B**

---

**Q16.** 🟡 What is "NAT" overload (PAT — Port Address Translation)?
- A) A security feature blocking unauthorized inbound connections
- B) A form of NAT mapping multiple private IP addresses to a single public IP by using different source port numbers — enabling an entire organization to share one public IP address for internet access
- C) A network address translation method only used in IPv6
- D) A load balancing technique distributing traffic across multiple servers

**Answer: B**

---

**Q17.** 🟡 What is "ACL" (Access Control List) in networking?
- A) A list of authorized users for network access
- B) A set of permit/deny rules applied to a router or switch interface that filter traffic based on criteria (source/destination IP, protocol, port) — used for security, traffic filtering, and QoS classification
- C) An Active Directory group controlling LAN access
- D) A configuration list for network device management access

**Answer: B**

---

**Q18.** 🟡 What is the difference between "standard ACL" and "extended ACL" in Cisco?
- A) Standard ACLs are for switches; extended are for routers
- B) Standard ACLs filter only on source IP address (applied close to destination); extended ACLs filter on source and destination IP, protocol, and port — providing much more granular control (applied close to source)
- C) Extended ACLs support IPv6; standard support only IPv4
- D) Standard ACLs are numbered 100–199; extended are 1–99

**Answer: B**

---

**Q19.** 🟡 What is "HSRP" (Hot Standby Router Protocol)?
- A) A high-speed routing protocol for enterprise cores
- B) A Cisco First Hop Redundancy Protocol (FHRP) providing gateway redundancy — two or more routers share a virtual IP and MAC address; if the active router fails, the standby takes over automatically, maintaining connectivity for end devices
- C) A spanning tree extension for router redundancy
- D) A protocol synchronizing routing tables between redundant routers

**Answer: B**

---

**Q20.** 🟡 What is "DHCP snooping" and why is it a security feature?
- A) A feature for monitoring DHCP traffic for performance
- B) A switch security feature that validates DHCP messages and builds a binding table of legitimate DHCP assignments — preventing rogue DHCP servers from assigning incorrect IP addresses to clients (DHCP starvation/spoofing attacks)
- C) A tool for snooping on DHCP logs for troubleshooting
- D) A Cisco feature restricting DHCP to specific VLANs

**Answer: B**

---

**Q21.** 🟡 What is "Dynamic ARP Inspection" (DAI)?
- A) A feature for dynamically updating ARP tables
- B) A switch security feature that validates ARP packets against the DHCP snooping binding table — dropping ARP packets with invalid IP-to-MAC mappings to prevent ARP spoofing/poisoning attacks
- C) A tool for inspecting ARP performance on the network
- D) A dynamic ARP caching mechanism for faster resolution

**Answer: B**

---

**Q22.** 🟡 What is "Port Security" on a Cisco switch?
- A) Configuring a firewall on each switch port
- B) A feature that restricts which MAC addresses can connect to a switch port — limiting the number of allowed MAC addresses and shutting down or dropping traffic from unauthorized devices, preventing MAC flooding attacks
- C) A security certificate required for switch management access
- D) Blocking all ports not actively in use

**Answer: B**

---

**Q23.** 🔴 What is "OSPF area" design and why is area 0 (backbone) significant?
- A) Areas are geographic zones for managing router locations
- B) OSPF uses areas to limit LSA flooding scope — all areas must connect to Area 0 (backbone), which interconnects all other areas; non-backbone areas exchange routes through Area 0, reducing routing table size and SPF computation in large networks
- C) Area 0 is reserved for internet-facing routers
- D) Areas divide OSPF routers by manufacturer (Cisco, Arista)

**Answer: B**

---

**Q24.** 🔴 What is "route redistribution" in multi-protocol networks?
- A) Redistributing routing tables evenly across all routers
- B) The process of taking routes from one routing protocol (e.g., OSPF) and injecting them into another (e.g., EIGRP or BGP) — required when multiple routing protocols coexist; must be carefully managed to avoid routing loops and suboptimal paths
- C) Balancing routing load across multiple equal-cost paths
- D) Redistributing IP address assignments between subnets

**Answer: B**

---

**Q25.** 🔴 What is "IPv6" and what are the key differences from IPv4?
- A) The 6th version of the internet protocol with minor improvements
- B) A 128-bit addressing protocol (vs. IPv4's 32-bit) providing virtually unlimited addresses — eliminating NAT necessity, mandatory IPSec support, simplified header, stateless address autoconfiguration (SLAAC), no broadcast (uses multicast), and integrated neighbor discovery replacing ARP
- C) An IPv4 extension adding security features
- D) A protocol exclusively for mobile network devices

**Answer: B**

---

**Q26.** 🔴 What is "MPLS" label switching and how does it work?
- A) A security protocol using multiple labels for authentication
- B) A data forwarding technique attaching short fixed-length labels to packets at the network edge — routers (LSRs) forward packets based on labels (not IP lookups) through predetermined Label Switched Paths (LSPs), enabling fast, QoS-aware, traffic-engineered WAN transport
- C) A switching protocol replacing Ethernet in data centers
- D) A multi-layer protocol stack for enterprise applications

**Answer: B**

---

**Q27.** 🔴 What is "ECMP" (Equal-Cost Multi-Path) routing?
- A) An encryption protocol for multi-path VPN connections
- B) A routing strategy where traffic to a destination is distributed across multiple equal-cost paths simultaneously — increasing throughput and providing redundancy; supported by OSPF, EIGRP, and BGP with specific configuration
- C) A protocol ensuring the most efficient (cheapest) single path is always used
- D) A Cisco feature for equal distribution of VLANs across switches

**Answer: B**

---

**Q28.** 🔴 What is "GRE" (Generic Routing Encapsulation) tunnel?
- A) A generic routing encryption protocol for VPNs
- B) A tunneling protocol encapsulating packets inside IP packets — creating a virtual point-to-point link between two endpoints to carry traffic across an IP network; commonly used to tunnel non-IP traffic or connect OSPF areas over non-OSPF networks; often combined with IPSec for encryption
- C) A Cisco routing protocol for encrypted WANs
- D) A generic route export format for sharing routing tables

**Answer: B**

---

**Q29.** 🔴 What is "IPSec" and its two main modes?
- A) IP Security — a routing protocol ensuring secure path selection
- B) A suite of protocols providing authentication and encryption for IP packets — two modes: Transport mode (encrypts only payload, keeps original IP header — for end-to-end) and Tunnel mode (encrypts entire packet including header, adds new IP header — for VPN gateways)
- C) Internet Protocol Security — a firewall protocol
- D) An IP extension adding security certificates to packets

**Answer: B**

---

**Q30.** 🔴 What is "SDN" (Software-Defined Networking)?
- A) A type of networking using software-based switches
- B) An architecture separating the control plane (centralized SDN controller making routing decisions) from the data plane (network devices forwarding packets) — enabling programmatic, centralized network management and automation at scale
- C) A networking approach using software to replace physical hardware
- D) A network design using software documentation standards

**Answer: B**

---

**Q31.** 🔴 What is "VXLAN" (Virtual Extensible LAN)?
- A) An extended version of VLAN supporting more IDs
- B) A network virtualization overlay protocol encapsulating Layer 2 frames in UDP packets — extending Layer 2 segments across Layer 3 networks, supporting up to 16 million segments (vs. VLAN's 4,096), widely used in data centers and SD-WAN overlays
- C) A Cisco VLAN extension for enterprise campuses
- D) A virtual LAN protocol replacing 802.1Q in data centers

**Answer: B**

---

**Q32.** 🔴 What is "BFD" (Bidirectional Forwarding Detection)?
- A) A backup forwarding detection protocol for OSPF
- B) A lightweight, high-speed hello protocol that rapidly detects link or path failures between adjacent network devices — operating independently of routing protocols and notifying them of failures much faster than protocol-native hello timers
- C) A firewall detection mechanism for bidirectional traffic
- D) A Cisco protocol for detecting broadcast forwarding issues

**Answer: B**

---

**Q33.** 🔴 What is "PBR" (Policy-Based Routing)?
- A) A routing protocol based on network policies
- B) A routing mechanism that routes packets based on administrator-defined policies (source IP, application, QoS marking) rather than destination IP alone — enabling traffic engineering like sending video over high-bandwidth paths and VoIP over low-latency paths
- C) A Cisco protocol for BGP policy management
- D) A port-based routing feature for VLAN interfaces

**Answer: B**

---

**Q34.** 🔴 What is "LISP" (Locator/ID Separation Protocol)?
- A) A scripting language for network configuration
- B) A routing architecture separating the endpoint identity (EID) from its location (RLOC) — enabling more scalable routing tables, seamless host mobility across subnets, and flexible multi-cloud/SD-WAN deployments; used in Cisco SDA (Software-Defined Access)
- C) A lightweight protocol for IoT sensor networking
- D) A loop inspection protocol for Spanning Tree

**Answer: B**

---

**Q35.** 🔴 What is "QoS DSCP marking" and how is it used?
- A) A quality scoring system for network equipment
- B) DSCP (Differentiated Services Code Point) is a 6-bit field in the IP header used to classify and mark packets with a priority level — network devices honor these markings to apply QoS policies (queue prioritization, traffic shaping) ensuring voice/video gets priority over bulk data
- C) A Cisco encryption marking for secure traffic
- D) A diagnostic code in network packets indicating errors

**Answer: B**

---
*35 Questions | Shared — Advanced Networking (CCNA Level).*
