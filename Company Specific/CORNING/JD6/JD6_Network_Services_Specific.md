# JD6 — Global Network Services Specific MCQs
### Role: IT Intern – Analyst, Global Network Services
> Role-specific framing: enterprise network ops, ServiceNow, monitoring, change management, network automation, global infrastructure
> 🟢 Easy | 🟡 Medium | 🔴 Hard
> Networking fundamentals → Shared_Networking_Windows.md
> Advanced Networking (CCNA) → Shared_Advanced_Networking.md
> Cisco & Arista → Shared_Cisco_Arista.md
> Network Monitoring → Shared_Network_Monitoring.md
> Network Automation → Shared_Network_Automation.md
> ITSM/ServiceNow → Shared_ITSM.md | Python → Shared_Python.md

---

## SECTION A: Enterprise Network Operations & Global Infrastructure (35 Questions)

**Q1.** 🟢 What is "enterprise networking" and how does it differ from home networking?
- A) Enterprise networking uses Wi-Fi; home networking uses Ethernet
- B) Enterprise networking supports thousands of users across multiple sites with requirements for redundancy, security, QoS, centralized management, and SLA-backed uptime — far more complex than home networks with a single router/switch
- C) Enterprise networking is managed by ISPs; home networking is self-managed
- D) They are identical — enterprise networking is just larger scale

**Answer: B**

---

**Q2.** 🟢 What is "network operations" in a Global Network Services team?
- A) Operating the financial operations of a network company
- B) The day-to-day activities of monitoring, maintaining, troubleshooting, and managing an organization's network infrastructure to ensure reliable, secure, and performant connectivity
- C) Network operations refers exclusively to hardware maintenance
- D) Operating the corporate intranet website

**Answer: B**

---

**Q3.** 🟢 What is a "NOC" (Network Operations Center)?
- A) A physical node on the corporate network
- B) A centralized facility (physical or virtual) where network engineers monitor, manage, and respond to network events and incidents across an organization's infrastructure — the nerve center for network operations
- C) A network optimization configuration tool
- D) A notification center for network alerts

**Answer: B**

---

**Q4.** 🟢 What is "network availability" and how is it typically measured?
- A) Whether the network is publicly accessible from the internet
- B) The percentage of time a network service is operational and accessible — measured as uptime (e.g., 99.99% = "four nines" = ~52 minutes downtime/year); a core SLA metric for enterprise networks
- C) The number of devices available on the network at any time
- D) The available bandwidth on a network link

**Answer: B**

---

**Q5.** 🟢 What is "network performance monitoring"?
- A) Measuring the performance reviews of network engineers
- B) Continuously measuring key network metrics (bandwidth utilization, latency, packet loss, jitter, interface errors) to detect degradation, forecast capacity needs, and validate SLAs
- C) Monitoring the speed of individual user devices
- D) A one-time network assessment before a major upgrade

**Answer: B**

---

**Q6.** 🟢 What is a "network change" in an enterprise operations context?
- A) Upgrading all network hardware simultaneously
- B) Any modification to network configuration, software, or infrastructure — managed through a formal change management process (ServiceNow CAB approval) to minimize risk of unplanned outages
- C) Changing the network provider (ISP)
- D) Replacing a failed network device as an emergency repair

**Answer: B**

---

**Q7.** 🟢 What is "ServiceNow" and how is it used in network operations?
- A) A network monitoring tool by Cisco
- B) An ITSM platform used to log, track, and manage network incidents, service requests, problems, and changes — providing workflow automation, SLA tracking, and audit trails for network operations
- C) A Cisco network management system
- D) A cloud network service by Microsoft

**Answer: B**

---

**Q8.** 🟡 What is "network redundancy" and why is it critical in enterprise networks?
- A) Having duplicate documentation for network configurations
- B) Designing the network with backup paths, devices, and links so that a single component failure doesn't cause a service outage — achieved through protocols like HSRP, spanning tree, LACP, and redundant WAN links
- C) Redundant IP addresses assigned to each device
- D) Backing up network configuration files to a server

**Answer: B**

---

**Q9.** 🟡 What is "network segmentation" and what are its benefits?
- A) Dividing network documentation into separate sections
- B) Dividing a network into smaller, isolated segments (using VLANs, subnets, firewalls) — limiting broadcast domains, containing security breaches, improving performance, and enforcing access control between zones
- C) Segmenting network traffic by time of day
- D) Dividing bandwidth equally among all connected devices

**Answer: B**

---

**Q10.** 🟡 What is "Quality of Service" (QoS) in enterprise networking?
- A) A service quality guarantee from the ISP
- B) A set of techniques for managing and prioritizing network traffic — ensuring latency-sensitive applications (voice, video) get priority bandwidth over less critical traffic (file backups, bulk transfers), maintaining application performance
- C) A measurement of overall network quality reported monthly
- D) A Cisco feature that monitors service quality on switches

**Answer: B**

---

**Q11.** 🟡 What is a "network topology" and name the common enterprise types?
- A) The geographic location of network equipment
- B) The physical and logical arrangement of network devices and connections; common enterprise types include star (hub-and-spoke), hierarchical (core-distribution-access three-tier), ring, mesh, and hybrid
- C) The brand and model of network equipment deployed
- D) The IP addressing scheme used across the network

**Answer: B**

---

**Q12.** 🟡 What is the "three-tier hierarchical network model" (Core-Distribution-Access)?
- A) A three-layer security model for network access control
- B) A Cisco-defined enterprise network design with three layers: Core (high-speed backbone routing between sites), Distribution (policy enforcement, inter-VLAN routing, aggregation), Access (end-user device connectivity via switches) — each with defined roles for scalability and manageability
- C) A three-tier SLA model for network services
- D) A three-tier data center network model only

**Answer: B**

---

**Q13.** 🟡 What is "WAN" (Wide Area Network) vs. "LAN" (Local Area Network)?
- A) WAN uses wireless; LAN uses wired connections
- B) A LAN connects devices within a limited area (office, building) typically owned and managed by the organization; a WAN connects geographically distributed sites (cities, countries) typically over leased or carrier infrastructure
- C) WAN is for internet access; LAN is for internal servers only
- D) LAN supports more devices than WAN

**Answer: B**

---

**Q14.** 🟡 What is "SD-WAN" (Software-Defined WAN)?
- A) A type of WAN using satellite (SD = satellite dish) connections
- B) A network architecture using software-defined networking principles to manage and optimize WAN connections — abstracting the underlying transport (MPLS, broadband, LTE) and enabling centralized policy control, dynamic path selection, and application-aware routing
- C) A Cisco WAN product replacing MPLS entirely
- D) A software tool for documenting WAN configurations

**Answer: B**

---

**Q15.** 🟡 What is "MPLS" (Multiprotocol Label Switching)?
- A) Multiple Protocol Layer Security — a VPN encryption standard
- B) A high-performance WAN routing technique that forwards packets based on short label values rather than long IP addresses — providing fast, reliable, QoS-capable connectivity between enterprise sites; commonly used for private WAN services
- C) A switching protocol for enterprise LAN environments
- D) A multi-path load sharing protocol for routers

**Answer: B**

---

**Q16.** 🟡 What is "network baseline" and why is it important for monitoring?
- A) The base configuration template for all network devices
- B) A documented record of normal network performance metrics (bandwidth utilization, CPU, latency, error rates) during typical operation — used as a reference to detect anomalies and distinguish performance problems from normal behavior
- C) The minimum bandwidth guaranteed by the ISP
- D) The initial network design before any changes

**Answer: B**

---

**Q17.** 🟡 What is "packet loss" and what are common causes in enterprise networks?
- A) Physical loss of network cables or hardware
- B) The percentage of transmitted packets that fail to reach their destination — caused by network congestion, faulty hardware, misconfigured QoS, duplex mismatch, or failing links; even small percentages significantly degrade real-time applications
- C) Lost packets due to firewall dropping unauthorized traffic
- D) Packets discarded due to TTL expiration in routing loops

**Answer: B**

---

**Q18.** 🟡 What is "latency" and "jitter" in network performance?
- A) Latency is packet loss; jitter is network speed variation
- B) Latency is the delay for a packet to travel from source to destination (ms); jitter is the variation in latency between consecutive packets — both critically impact real-time applications like VoIP and video conferencing
- C) Latency is bandwidth; jitter is packet reordering
- D) They are the same metric measured differently

**Answer: B**

---

**Q19.** 🟡 What is "network documentation" and why does it matter in global operations?
- A) Documentation of network team meeting minutes
- B) Accurate records of network topology, device inventories, IP addressing schemes, configurations, circuit details, and change history — essential for troubleshooting, onboarding, capacity planning, compliance, and disaster recovery across global sites
- C) Legal documentation for ISP service contracts
- D) A knowledge base of common network troubleshooting steps

**Answer: B**

---

**Q20.** 🟡 What is "mean time to repair" (MTTR) in network operations?
- A) The maximum time allowed to repair a network outage per SLA
- B) The average time from when a network incident is detected to when service is fully restored — a key operational metric measuring incident response and resolution efficiency
- C) The mean time between network repairs (scheduled maintenance)
- D) A metric for measuring network engineer performance

**Answer: B**

---

**Q21.** 🟡 What is "change freeze" / "change blackout" in network operations?
- A) A period when all network changes are automatically rolled back
- B) A defined period during which non-emergency network changes are prohibited — typically during business-critical periods (fiscal year-end, product launches, peak seasons) to minimize outage risk when the business cannot tolerate disruption
- C) A freeze on hiring new network engineers
- D) A network configuration backup taken before each change

**Answer: B**

---

**Q22.** 🟡 What is "network automation" and what problems does it solve in global operations?
- A) Automatically upgrading all network hardware on a schedule
- B) Using scripts, tools, and platforms (Python, Ansible, Netmiko) to automate repetitive network tasks (config deployment, compliance checking, reporting) — reducing manual errors, accelerating change delivery, and freeing engineers for complex work across hundreds of global devices
- C) Automating network monitoring alerts and emails
- D) Automatic network failover using redundant links

**Answer: B**

---

**Q23.** 🟡 What is "configuration management" in network operations?
- A) Managing the configuration of the ServiceNow ticketing system
- B) Tracking, controlling, and maintaining network device configurations — including version control of config files, detecting unauthorized changes (configuration drift), and ensuring devices match approved baseline configurations
- C) Managing network hardware warranty and support contracts
- D) Configuring network monitoring tool dashboards

**Answer: B**

---

**Q24.** 🔴 What is "network telemetry" and how does it modernize monitoring?
- A) A telecommunications network monitoring protocol
- B) The continuous, real-time streaming of operational data (metrics, state, events) from network devices using modern protocols (gRPC/gNMI, streaming telemetry) — replacing traditional polling-based SNMP with higher frequency, richer data for proactive monitoring and analytics
- C) A Cisco feature for remote device diagnostics
- D) Telemetry data sent by network users to Microsoft

**Answer: B**

---

**Q25.** 🔴 What is "intent-based networking" (IBN)?
- A) A network designed for specific business intentions
- B) A network management approach where administrators define high-level business intent (policy) and the system automatically translates it into device configurations, continuously verifies the network matches the intent, and remediates deviations — exemplified by Cisco DNA Center
- C) A networking philosophy prioritizing user intent in QoS
- D) A machine learning model predicting network issues

**Answer: B**

---

**Q26.** 🔴 What is "network access control" (NAC)?
- A) Controlling physical access to network equipment rooms
- B) A security framework that enforces policy-based access control on devices trying to connect to the network — verifying device identity, compliance (patched OS, antivirus), and user credentials before granting network access; examples: Cisco ISE, Arista CloudVision
- C) A firewall feature controlling inbound network access
- D) An Active Directory group policy for network shares

**Answer: B**

---

**Q27.** 🔴 What is "Zero Trust Network Access" (ZTNA) and how does it affect enterprise network design?
- A) A network design with zero redundancy to reduce cost
- B) A security model replacing implicit network trust — no user, device, or location is inherently trusted; access to resources is granted based on continuous verification of identity, device posture, and context — replacing VPN-centric remote access with identity-aware proxies
- C) A Cisco security product for enterprise networks
- D) A network design using zero physical network cables

**Answer: B**

---

**Q28.** 🔴 What is "network forensics" in the context of incident investigation?
- A) A legal investigation of network equipment theft
- B) The capture, recording, and analysis of network traffic and logs to reconstruct events, identify the source and scope of security incidents or performance problems, and provide evidence for post-incident review
- C) Forensic recovery of data from failed network storage
- D) A Cisco tool for analyzing network configuration history

**Answer: B**

---

**Q29.** 🔴 What is "BGP" used for in a global enterprise network?
- A) Basic Gateway Protocol — a simple routing protocol for small networks
- B) Border Gateway Protocol — the internet's routing protocol used by enterprises for inter-AS routing, controlling how traffic enters and exits the network through multiple ISPs (multihoming), enabling path selection and traffic engineering at the WAN edge
- C) A backup routing protocol when OSPF fails
- D) A LAN routing protocol for large campus networks

**Answer: B**

---

**Q30.** 🔴 What is "route summarization" (route aggregation) and why is it used?
- A) Summarizing routing table information in documentation
- B) Combining multiple specific routes into a single, less-specific advertisement — reducing routing table size, decreasing routing protocol overhead, improving convergence time, and limiting the propagation of topology changes
- C) A routing protocol feature summarizing traffic statistics
- D) Summarizing BGP routes for reporting purposes

**Answer: B**

---

**Q31.** 🔴 What is "network capacity planning"?
- A) Planning the physical capacity of the data center
- B) The process of analyzing current network utilization trends and forecasting future demand — ensuring sufficient bandwidth, device capacity, and infrastructure are provisioned to meet business growth without over-provisioning unnecessarily
- C) Planning the capacity of the network operations team
- D) Calculating the number of IP addresses needed for a new site

**Answer: B**

---

**Q32.** 🔴 What is "DDoS" (Distributed Denial of Service) and how do enterprises defend against it?
- A) A type of network misconfiguration causing distributed outages
- B) An attack flooding a target with massive traffic from many sources to exhaust resources and deny service — defended using upstream scrubbing centers, rate limiting, black-hole routing, anycast diffusion, and DDoS protection services (Cloudflare, AWS Shield)
- C) A distributed database outage affecting network services
- D) A denial of service caused by misconfigured routing protocols

**Answer: B**

---

**Q33.** 🔴 What is "AIOps for networking" and why is it gaining adoption?
- A) AI tools that automatically design network topologies
- B) Applying AI/ML to network operations data (telemetry, logs, events) to detect anomalies, predict failures before they occur, correlate related alerts into single incidents, and recommend or automate remediation — reducing alert fatigue and MTTR in large global networks
- C) Automated network equipment ordering using AI
- D) An AI tool for writing network documentation

**Answer: B**

---

**Q34.** 🔴 What is "network slicing" in the context of modern enterprise networking?
- A) Dividing a network cable into multiple strands
- B) A network architecture technique (prominent in 5G and SDN) creating multiple logical, isolated network instances on shared physical infrastructure — each with guaranteed resources and characteristics tailored for specific use cases (IoT, latency-critical apps, bulk data)
- C) A Cisco feature for segmenting VLANs on a switch
- D) A technique for splitting network management between teams

**Answer: B**

---

**Q35.** 🔴 What is "network observability" vs. traditional "network monitoring"?
- A) They are identical — observability is the modern term for monitoring
- B) Traditional monitoring tracks predefined metrics (is the device up? is bandwidth over threshold?); network observability goes further — combining metrics, logs, traces, and topology context to understand *why* anomalies occur, enabling proactive diagnosis of unknown issues rather than just alerting on known thresholds
- C) Network observability is only for cloud networks; monitoring is for on-premise
- D) Observability is a Cisco-specific term; monitoring is vendor-neutral

**Answer: B**

---
*End of JD6 role-specific file — 35 Questions.*
*Advanced Networking → Shared_Advanced_Networking.md*
*Cisco & Arista → Shared_Cisco_Arista.md*
*Network Monitoring → Shared_Network_Monitoring.md*
*Network Automation → Shared_Network_Automation.md*
*ITSM/ServiceNow → Shared_ITSM.md | Python → Shared_Python.md*
