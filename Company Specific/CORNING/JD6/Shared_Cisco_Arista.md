# Shared — Cisco & Arista Technologies MCQs
### Applies to: JD6 (Global Network Services) — Cisco IOS/NX-OS, Arista EOS, CLI, enterprise configs
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What is "Cisco IOS" (Internetwork Operating System)?
- A) An internet operating system by Cisco for personal computers
- B) The proprietary operating system running on most Cisco routers and switches — providing the CLI (command-line interface) for configuring and managing Cisco network devices
- C) A Cisco cloud management platform
- D) An open-source OS for network devices

**Answer: B**

---

**Q2.** 🟢 What is the Cisco CLI "privileged exec mode" and how do you enter it?
- A) The default mode when logging into a Cisco device; no special command needed
- B) An elevated mode (indicated by `#` prompt) providing access to all show and configuration commands — entered by typing `enable` from user exec mode (`>`)
- C) A mode for viewing only — entered with `view` command
- D) A superuser mode entered with `sudo` command

**Answer: B**

---

**Q3.** 🟢 What does `show running-config` display on a Cisco device?
- A) The configuration saved in NVRAM (startup config)
- B) The currently active configuration in RAM — showing all settings applied to the device since last reload, including interfaces, routing protocols, ACLs, and VLANs
- C) A list of currently running processes on the device
- D) The current routing table entries

**Answer: B**

---

**Q4.** 🟢 What is the command to save the running configuration to NVRAM on a Cisco device?
- A) `save config`
- B) `copy running-config startup-config` (or `write memory` / `wr`)
- C) `commit running-config`
- D) `save running startup`

**Answer: B**

---

**Q5.** 🟢 What does `show ip interface brief` display?
- A) A brief summary of IP routing table entries
- B) A summary table of all interfaces showing IP address, status (up/down), and protocol status — the quickest way to check interface states on a Cisco router or switch
- C) Brief details of IP ACLs applied to interfaces
- D) A summary of IP DHCP assignments

**Answer: B**

---

**Q6.** 🟢 What is "Arista EOS" (Extensible Operating System)?
- A) A Cisco operating system for Arista-compatible switches
- B) Arista Networks' network operating system — built on a Linux kernel with a single binary image, supporting all Arista hardware; known for its programmability, stability, and support for open standards and APIs
- C) An open-source NOS competing with Cisco IOS
- D) An embedded OS for Arista's optical networking products

**Answer: B**

---

**Q7.** 🟢 What command shows the VLAN database on a Cisco switch?
- A) `show vlan database`
- B) `show vlan brief` — displays all VLANs, their names, status, and assigned ports
- C) `show switchport vlan`
- D) `show interface vlan`

**Answer: B**

---

**Q8.** 🟡 What is the difference between "access port" and "trunk port" on a Cisco switch?
- A) Access ports carry multiple VLANs; trunk ports carry one
- B) An access port carries traffic for a single VLAN (untagged — for end devices like PCs, printers); a trunk port carries tagged traffic for multiple VLANs (using 802.1Q) — used for switch-to-switch or switch-to-router links
- C) Trunk ports are faster than access ports
- D) Access ports support LACP; trunk ports do not

**Answer: B**

---

**Q9.** 🟡 What is the Cisco IOS command to configure a switch port as an access port on VLAN 10?
- A) `set port vlan 10`
- B) `switchport mode access` then `switchport access vlan 10` — entered in interface configuration mode
- C) `vlan access 10` in global config mode
- D) `interface vlan 10` then `switchport access`

**Answer: B**

---

**Q10.** 🟡 What is "CDP" (Cisco Discovery Protocol)?
- A) A routing protocol for Cisco device discovery
- B) A Cisco-proprietary Layer 2 protocol that enables Cisco devices to discover and share information (device type, IP address, IOS version, capabilities) with directly connected Cisco neighbors — useful for network topology mapping
- C) A configuration distribution protocol for Cisco networks
- D) A Cisco diagnostic protocol for performance monitoring

**Answer: B**

---

**Q11.** 🟡 What is "LLDP" (Link Layer Discovery Protocol)?
- A) A Cisco-proprietary version of CDP
- B) An IEEE 802.1AB open-standard neighbor discovery protocol — vendor-neutral equivalent of CDP; used by Arista, Juniper, and other vendors to advertise device information to directly connected neighbors
- C) A link-layer data plane protocol for packet forwarding
- D) A lightweight protocol for detecting link failures

**Answer: B**

---

**Q12.** 🟡 What is the command to view OSPF neighbor relationships on a Cisco router?
- A) `show ospf neighbors`
- B) `show ip ospf neighbor` — displays OSPF neighbor table showing neighbor ID, state (Full = fully adjacent), interface, and dead timer
- C) `show routing ospf`
- D) `debug ospf adjacency`

**Answer: B**

---

**Q13.** 🟡 What is "Cisco NX-OS"?
- A) A next-generation version of Cisco IOS for all platforms
- B) A data center-focused Cisco operating system running on Nexus switches — optimized for high-density, high-performance data center environments with features like vPC (Virtual Port Channel), FabricPath, and enhanced programmability
- C) A Cisco OS exclusively for firewalls
- D) A cloud management OS for Cisco Meraki devices

**Answer: B**

---

**Q14.** 🟡 What is "vPC" (Virtual Port Channel) on Cisco Nexus?
- A) A virtual private cloud integration for Nexus switches
- B) A Cisco NX-OS feature allowing a device to form a port channel (LAG) to two separate Nexus switches simultaneously — eliminating Spanning Tree blocked ports while providing full bandwidth and redundancy; both Nexus switches appear as one logical device
- C) A virtual port configuration for VM connectivity
- D) A Cisco protocol for virtual cluster management

**Answer: B**

---

**Q15.** 🟡 What is "Arista MLAG" (Multi-Chassis Link Aggregation)?
- A) A multi-layer aggregation feature for Arista data centers
- B) Arista's equivalent of Cisco vPC — allowing two Arista switches to act as a single logical switch for LACP link aggregation, eliminating STP-blocked uplinks while providing redundancy and full bandwidth utilization
- C) A management link aggregation feature for Arista switches
- D) A multi-layer group for Arista routing protocols

**Answer: B**

---

**Q16.** 🟡 What makes Arista EOS programmable compared to traditional network OSes?
- A) Arista EOS supports Python macros in the CLI
- B) Arista EOS runs on a Linux kernel exposing standard Linux tools, supports eAPI (HTTP/HTTPS JSON-RPC API), pyeapi (Python library), and OpenConfig/YANG data models — enabling programmatic management via scripts, Ansible, and streaming telemetry natively
- C) Arista EOS supports GUI-based configuration only
- D) Arista EOS uses YAML configuration files replacing CLI

**Answer: B**

---

**Q17.** 🟡 What is "Cisco DNA Center" (now Cisco Catalyst Center)?
- A) A Cisco DNA sequencing research platform
- B) Cisco's centralized network management and intent-based networking platform — providing automated network provisioning, policy enforcement, assurance (AI-powered troubleshooting), and software image management for enterprise campus and branch networks
- C) A Cisco cloud security management platform
- D) A Cisco data center management tool for Nexus switches

**Answer: B**

---

**Q18.** 🟡 What is "Cisco Meraki"?
- A) A Cisco CLI tool for managing remote devices
- B) A cloud-managed networking platform (acquired by Cisco) providing centralized, web-based management of switches, wireless APs, security appliances, and cameras — designed for simplicity with all management through a cloud dashboard
- C) A Cisco hardware line for small businesses only
- D) A Cisco product competing with Arista in data centers

**Answer: B**

---

**Q19.** 🟡 What is the Cisco IOS command to view the routing table?
- A) `show routes`
- B) `show ip route` — displays the routing table showing all known networks, their administrative distance, metric, next-hop, and outgoing interface; prefixed with codes (C=Connected, S=Static, O=OSPF, B=BGP, etc.)
- C) `show routing table`
- D) `display ip routing`

**Answer: B**

---

**Q20.** 🟡 What is "administrative distance" (AD) in Cisco routing?
- A) The distance (in hops) to the administrative network
- B) A value (0–255) rating the trustworthiness of a routing information source — lower AD = more preferred; used when multiple protocols advertise the same route (e.g., Connected=0, Static=1, EIGRP=90, OSPF=110, BGP=20 external)
- C) The maximum number of hops a routing protocol supports
- D) The administrative cost of managing a routing protocol

**Answer: B**

---

**Q21.** 🟡 What is the Arista EOS `eAPI` used for?
- A) An Arista emergency API for disaster recovery
- B) Arista's HTTP/HTTPS-based management API that accepts JSON-formatted commands and returns JSON responses — enabling programmatic configuration and monitoring of Arista devices from Python scripts, automation tools, and orchestration platforms
- C) An encrypted API for secure Arista device management
- D) An Arista API exclusively for CloudVision integration

**Answer: B**

---

**Q22.** 🟡 What is "Arista CloudVision"?
- A) A cloud storage solution for Arista network backups
- B) Arista's network management and automation platform — providing network-wide telemetry, configuration management, zero-touch provisioning, compliance auditing, and workflow automation for Arista devices; analogous to Cisco DNA Center
- C) A cloud monitoring service for non-Arista networks
- D) A virtualization platform for running Arista EOS in VMs

**Answer: B**

---

**Q23.** 🟡 What does the Cisco command `debug ip ospf events` do?
- A) Disables OSPF event logging for troubleshooting
- B) Enables real-time OSPF event debugging output to the console — showing adjacency state changes, LSA events, and SPF calculations; must be used carefully in production as it generates significant output and consumes CPU
- C) Displays a static summary of OSPF events from the log
- D) Debugs IP addressing conflicts with OSPF interfaces

**Answer: B**

---

**Q24.** 🔴 What is "Cisco IOS XE" and how does it differ from classic IOS?
- A) IOS XE is the exclusive edition of IOS for enterprise
- B) IOS XE is a modular, Linux-based version of Cisco IOS running IOS as a daemon on a Linux kernel — providing improved stability (process isolation), programmability (NETCONF/YANG, gRPC, Python), and support for modern automation tooling while maintaining IOS CLI compatibility
- C) IOS XE is exclusively for Cisco ASR routers
- D) IOS XE replaced IOS XR for service provider platforms

**Answer: B**

---

**Q25.** 🔴 What is "NETCONF/YANG" in the context of Cisco and Arista?
- A) A network configuration file format replacing IOS configs
- B) NETCONF is an IETF network management protocol using XML over SSH for configuration and state retrieval; YANG is the data modeling language defining the structure of configuration and operational data — together enabling programmatic, model-driven network automation on modern Cisco IOS XE and Arista EOS devices
- C) A Cisco-proprietary protocol for device configuration backups
- D) Network Configuration / Yet Another Network Gateway — a routing protocol

**Answer: B**

---

**Q26.** 🔴 What is "Cisco IOS XR" and for what environment is it designed?
- A) An extended range version of IOS for campus networks
- B) A highly scalable, microkernel-based Cisco OS designed for service provider and large enterprise core routers (ASR 9000, NCS series) — providing carrier-grade reliability, in-service software upgrades (ISSU), and rich automation support
- C) A cross-platform IOS version compatible with Arista hardware
- D) An experimental release of IOS for testing new features

**Answer: B**

---

**Q27.** 🔴 What is "Cisco APIC-EM" / "Cisco DNA Center" Path Trace used for?
- A) Tracing the physical cable path between two devices
- B) A network assurance tool that traces the actual forwarding path a packet takes between two endpoints across the network — showing each hop, applied ACLs, QoS policies, and potential issues — dramatically accelerating troubleshooting
- C) A route tracing tool running on Cisco IOS (equivalent to traceroute)
- D) A Cisco tool tracing configuration changes to their source

**Answer: B**

---

**Q28.** 🔴 What is "Cisco TrustSec" (SGT — Security Group Tags)?
- A) A Cisco physical security product for data centers
- B) A Cisco security architecture that assigns Security Group Tags (SGTs) to users/devices based on identity and context — enforcing access policy based on group membership rather than IP addresses; providing scalable micro-segmentation without complex ACLs
- C) A Cisco certificate management system
- D) A Cisco firewall feature for enterprise security

**Answer: B**

---

**Q29.** 🔴 What is "Arista AVD" (Ansible AVD / Arista Validated Design)?
- A) An Arista video delivery network design
- B) A community-supported Ansible collection providing validated, opinionated network designs and automation workflows for deploying Arista-based data center and campus fabrics — enabling infrastructure-as-code for Arista networks with structured YAML-based input
- C) An Arista hardware validation and diagnostics tool
- D) An Arista API for virtual device management

**Answer: B**

---

**Q30.** 🔴 What is "Cisco SDA" (Software-Defined Access)?
- A) A software-defined approach to WAN connectivity
- B) Cisco's intent-based campus network solution using DNA Center as controller, LISP for host mobility, VXLAN for overlay, and TrustSec for policy — providing automated provisioning, network-wide segmentation, and assurance for enterprise campus networks
- C) Software-defined access control for Cisco firewalls
- D) A Cisco SD-WAN product for branch office access

**Answer: B**

---

**Q31.** 🟡 What does `show interfaces` display on a Cisco device?
- A) A list of all configured interfaces and their IP addresses
- B) Detailed statistics for each interface — including input/output rates, error counters (CRC, input errors, output drops, collisions), line protocol status, and MTU — critical for diagnosing interface-level problems
- C) A summary of interface speed and duplex settings only
- D) The interface configuration from the running-config

**Answer: B**

---

**Q32.** 🟡 What is "duplex mismatch" and what problems does it cause?
- A) A mismatch between two switch firmware versions
- B) A misconfiguration where one end of a link is set to full-duplex and the other to half-duplex — causing late collisions, CRC errors, and significantly reduced throughput; detected via `show interfaces` showing increasing error counters
- C) A mismatch in VLAN configuration between two ports
- D) A speed mismatch between connected interfaces

**Answer: B**

---

**Q33.** 🔴 What is "In-Service Software Upgrade" (ISSU) on Cisco/Arista?
- A) Upgrading software in-service (during business hours)
- B) The ability to upgrade a network device's operating system while maintaining traffic forwarding — achieving near-zero downtime during software upgrades on platforms supporting stateful switchover (SSO) and non-stop forwarding (NSF)
- C) A Cisco software feature for installing updates in-service hours only
- D) An Arista feature for rolling back software upgrades in service

**Answer: B**

---

**Q34.** 🔴 What is "Cisco Catalyst" vs. "Cisco Nexus" switching platform?
- A) Catalyst is for data centers; Nexus is for enterprise campus
- B) Cisco Catalyst switches are designed for enterprise campus/branch LAN environments (access, distribution, core layers); Cisco Nexus switches are designed for data center environments — optimized for high-density, low-latency server connectivity and east-west traffic
- C) They are the same platform with different branding
- D) Nexus supports only IPv6; Catalyst supports IPv4 and IPv6

**Answer: B**

---

**Q35.** 🔴 What is "Zero Touch Provisioning" (ZTP) in Cisco/Arista?
- A) A provisioning method requiring zero network engineers
- B) An automated device bootstrapping process where a new or replaced network device automatically downloads and applies its configuration from a server (DHCP → TFTP/HTTP) upon first boot — eliminating manual CLI configuration and enabling rapid deployment at scale
- C) A Cisco feature allowing zero-configuration wireless access
- D) A security feature provisioning certificates with zero manual steps

**Answer: B**

---
*35 Questions | Shared — Cisco & Arista Technologies.*
