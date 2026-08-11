# Shared — Network Monitoring Tools MCQs
### Applies to: JD6 (Global Network Services) — SolarWinds, CACTI, SNMP, NetFlow, monitoring concepts
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What is "network monitoring" and what does it achieve?
- A) Monitoring employee internet usage on the network
- B) Continuously observing network devices, links, and services to measure performance, detect failures or degradation, generate alerts, and provide historical data for capacity planning and troubleshooting
- C) A process for monitoring network hardware warranties
- D) Auditing network configuration compliance

**Answer: B**

---

**Q2.** 🟢 What is "SNMP" (Simple Network Management Protocol)?
- A) A security protocol for managing network access
- B) An application-layer protocol for collecting and organizing information about managed network devices — agents on devices expose data (MIB) that a manager polls or receives traps from for monitoring purposes
- C) A simple network mapping protocol for topology discovery
- D) A protocol for managing network software patches

**Answer: B**

---

**Q3.** 🟢 What is an "SNMP trap"?
- A) A security trap catching unauthorized SNMP access
- B) An unsolicited notification sent proactively from a managed device (agent) to the SNMP manager when a significant event occurs (e.g., interface down, threshold exceeded) — enabling alert-driven monitoring without constant polling
- C) An SNMP query sent to check device status
- D) A firewall rule trapping unauthorized SNMP traffic

**Answer: B**

---

**Q4.** 🟢 What is "SolarWinds Network Performance Monitor" (NPM)?
- A) A Cisco network management tool
- B) A widely used commercial network monitoring platform — discovering network devices automatically, monitoring performance metrics (bandwidth, CPU, memory, interface status) via SNMP/WMI, and generating alerts and reports
- C) An open-source network monitoring tool
- D) A Microsoft tool for monitoring Windows network services

**Answer: B**

---

**Q5.** 🟢 What is "CACTI"?
- A) A Cisco network monitoring appliance
- B) An open-source network graphing and monitoring solution built on RRDTool — polling devices via SNMP and generating performance graphs (bandwidth, CPU, memory) for historical trend analysis
- C) A network monitoring commercial tool by SolarWinds
- D) A Python framework for network data collection

**Answer: B**

---

**Q6.** 🟢 What is "uptime" vs. "availability" in network monitoring?
- A) They are identical metrics
- B) Uptime measures how long a device or service has been continuously running since last restart; availability measures the percentage of total time a service is operational (including planned and unplanned downtime) — availability is the more meaningful SLA metric
- C) Uptime is measured in seconds; availability in percentages — but they measure the same thing
- D) Availability is measured per interface; uptime is per device

**Answer: B**

---

**Q7.** 🟡 What is "SNMP MIB" (Management Information Base)?
- A) A database storing SNMP community strings
- B) A hierarchical database defining the structure of information that can be managed on a network device — organized as a tree of OIDs (Object Identifiers); each OID represents a specific device attribute (e.g., interface speed, CPU utilization) that SNMP can read or set
- C) A management interface for configuring SNMP on devices
- D) A Cisco-specific SNMP database format

**Answer: B**

---

**Q8.** 🟡 What is the difference between SNMPv2c and SNMPv3?
- A) SNMPv2c supports more OIDs; SNMPv3 supports fewer but more securely
- B) SNMPv2c uses community strings for authentication (sent in plaintext — insecure); SNMPv3 adds authentication (MD5/SHA), encryption (DES/AES), and message integrity — making it the recommended version for production enterprise networks
- C) SNMPv3 is deprecated; SNMPv2c is the current standard
- D) They are identical except SNMPv3 uses TCP instead of UDP

**Answer: B**

---

**Q9.** 🟡 What is "NetFlow" (or "IPFIX") and what is it used for?
- A) A network protocol for flowing IP packets between sites
- B) A Cisco-developed (now IETF-standardized as IPFIX) protocol where routers/switches export metadata about IP traffic flows (source/dest IP, ports, protocol, bytes, packets) to a collector — used for traffic analysis, capacity planning, security anomaly detection, and billing
- C) A network flow balancing protocol for WAN links
- D) A tool for capturing full packet content for forensic analysis

**Answer: B**

---

**Q10.** 🟡 What is the difference between "NetFlow" and "packet capture" (PCAP)?
- A) NetFlow captures full packets; PCAP captures only metadata
- B) NetFlow exports flow metadata (who talked to whom, how much, when) — lightweight, always-on, scalable; packet capture (PCAP via Wireshark/tcpdump) captures full packet contents — detailed but storage-intensive, typically triggered for specific troubleshooting
- C) PCAP is vendor-specific; NetFlow is open standard
- D) They are identical — different terms for the same technology

**Answer: B**

---

**Q11.** 🟡 What is "Wireshark" used for in network operations?
- A) A wireless network design tool
- B) A free, open-source packet analyzer (protocol analyzer) capturing and inspecting network traffic in real time — decoding hundreds of protocols, filtering packets, and showing full packet details for deep troubleshooting of connectivity, protocol, and application issues
- C) A network monitoring dashboard tool
- D) A web-based network configuration management tool

**Answer: B**

---

**Q12.** 🟡 What is "ICMP" monitoring and what are its limitations?
- A) International Communication Monitoring Protocol — enterprise monitoring standard
- B) Using ICMP echo requests (ping) to check device reachability and measure round-trip latency — simple and universal, but limited: devices can be up but block ICMP, it doesn't reveal service availability or performance, and high packet loss thresholds miss intermittent issues
- C) A monitoring method using Cisco's ICMP implementation only
- D) ICMP monitoring captures all traffic for analysis

**Answer: B**

---

**Q13.** 🟡 What is "threshold alerting" in network monitoring?
- A) Alerting management when SLA thresholds are missed
- B) Configuring monitoring tools to generate alerts when a metric (bandwidth utilization, CPU, error rate, latency) crosses a defined threshold — enabling proactive notification of potential issues before they cause user impact
- C) Setting maximum bandwidth limits on network interfaces
- D) A firewall rule alerting on excessive traffic volumes

**Answer: B**

---

**Q14.** 🟡 What is "SolarWinds IPAM" (IP Address Manager)?
- A) An IP address monitoring tool
- B) A SolarWinds module for managing IP address space — tracking which IPs are in use, available, or reserved across subnets, integrating with DHCP and DNS for automated discovery and conflict detection
- C) An intrusion prevention and monitoring tool
- D) A SolarWinds module for managing internet peering

**Answer: B**

---

**Q15.** 🟡 What is "SolarWinds NCM" (Network Configuration Manager)?
- A) A network cost management tool by SolarWinds
- B) A SolarWinds module for automated network device configuration backup, change detection, compliance auditing (comparing configs against policies/standards), and bulk configuration deployment across Cisco, Arista, and other vendor devices
- C) A network capacity management module
- D) A Cisco network configuration manager

**Answer: B**

---

**Q16.** 🟡 What is "RRDTool" (Round Robin Database Tool)?
- A) A round-robin DNS management tool
- B) A high-performance data logging and graphing tool storing time-series data in fixed-size circular (round-robin) files — the engine behind CACTI and other monitoring tools for storing and graphing performance metrics over time
- C) A redundant routing database for OSPF
- D) A database tool for storing network inventory data

**Answer: B**

---

**Q17.** 🟡 What is "SNMP polling interval" and why does it matter?
- A) The time between SNMP version upgrades
- B) The frequency at which the monitoring system queries devices for metrics — shorter intervals provide finer granularity but increase device CPU load and network overhead; typical enterprise intervals are 5 minutes for performance, shorter for critical availability checks
- C) The interval between SNMP agent restarts
- D) The timeout period before an SNMP query is retried

**Answer: B**

---

**Q18.** 🟡 What is "synthetic monitoring" in network operations?
- A) Using artificial (synthetic) traffic generators for load testing
- B) Proactively simulating user transactions or network paths from specific locations to measure end-to-end performance and availability — detecting issues before real users are impacted, rather than relying solely on user-reported incidents
- C) Monitoring network performance using synthetic (fake) device data
- D) A monitoring approach using AI to synthesize alerts

**Answer: B**

---

**Q19.** 🟡 What is "Nagios" / "Nagios XI" used for?
- A) A Cisco network management platform
- B) A widely used open-source (Nagios Core) and commercial (Nagios XI) IT infrastructure monitoring tool — monitoring hosts, services, and network devices with plugin-based extensibility, alerting, and dashboards
- C) A network automation tool for Cisco devices
- D) A log management and SIEM tool

**Answer: B**

---

**Q20.** 🟡 What is "Grafana" in a network monitoring context?
- A) A network graphing appliance by SolarWinds
- B) An open-source analytics and visualization platform that connects to time-series databases (InfluxDB, Prometheus, Elasticsearch) and renders interactive dashboards — widely used in modern network monitoring stacks alongside telemetry collectors
- C) A Cisco DNA Center visualization plugin
- D) A commercial network monitoring tool competing with SolarWinds

**Answer: B**

---

**Q21.** 🟡 What is "Prometheus" and how is it used in network monitoring?
- A) A commercial network monitoring platform by Cisco
- B) An open-source time-series metrics collection and alerting system — scraping metrics from exporters (node_exporter, SNMP exporter, network device exporters) at defined intervals and storing them for querying with PromQL and visualization in Grafana
- C) A network packet capture tool
- D) An open-source alternative to SolarWinds NCM

**Answer: B**

---

**Q22.** 🟡 What is "syslog" in network operations?
- A) A system log file stored on end-user devices
- B) A standard protocol (RFC 5424) for devices (routers, switches, firewalls) to send event and error log messages to a centralized syslog server — providing audit trail, security event logging, and troubleshooting data; messages have severity levels (0=Emergency to 7=Debug)
- C) A Cisco-proprietary logging system for IOS devices
- D) A system log viewer built into Windows Server

**Answer: B**

---

**Q23.** 🟡 What is a "network dashboard" in NOC operations?
- A) A car-style instrument panel controlling network devices
- B) A real-time visual display aggregating key network metrics, device status, alerts, and KPIs in a single view — enabling NOC operators to quickly assess the health of the network and identify issues requiring attention
- C) An executive report generated weekly from monitoring data
- D) A configuration management interface for network devices

**Answer: B**

---

**Q24.** 🔴 What is "gNMI" (gRPC Network Management Interface)?
- A) A general network monitoring index by Cisco
- B) A gRPC-based management protocol for network device configuration and telemetry — part of the OpenConfig ecosystem; supports capabilities negotiation, Get/Set operations, and streaming telemetry subscriptions; used by modern Cisco IOS XE, Arista EOS, and Junos devices
- C) A Google-developed network monitoring infrastructure tool
- D) A generic network management interface replacing SNMP v3

**Answer: B**

---

**Q25.** 🔴 What is "streaming telemetry" and how does it improve on SNMP polling?
- A) Streaming live video of network rack equipment
- B) A push-based model where devices continuously stream operational data (metrics, state) to a collector at high frequency using gRPC/gNMI or NETCONF — providing near-real-time visibility, lower collection latency, and richer data than polling-based SNMP; scales better for modern high-speed networks
- C) A Cisco feature streaming network logs to a SIEM
- D) Streaming NetFlow data to an external analytics platform

**Answer: B**

---

**Q26.** 🔴 What is "anomaly detection" in network monitoring?
- A) Detecting when a network device behaves abnormally during a change
- B) Using statistical baselines or ML models to automatically identify unusual patterns in network traffic or device metrics (e.g., sudden traffic spike, unexpected protocol, unusual time of activity) that may indicate security threats or failures — distinguishing from normal variation
- C) A SolarWinds feature for detecting configuration anomalies
- D) Detecting anomalies in network documentation

**Answer: B**

---

**Q27.** 🔴 What is "EEM" (Embedded Event Manager) on Cisco IOS?
- A) An enterprise event management system integrating with ServiceNow
- B) A Cisco IOS feature allowing automation of network tasks in response to events — writing TCL scripts or applets triggered by conditions (interface down, syslog message, threshold exceeded) to perform actions (send alert, restart process, make config change) without external tools
- C) An event management module in Cisco DNA Center
- D) A Cisco tool for managing network events in a NOC

**Answer: B**

---

**Q28.** 🔴 What is "RSPAN" / "ERSPAN" in network monitoring?
- A) Remote Spanning Tree Protocol for WAN environments
- B) RSPAN (Remote SPAN) mirrors traffic from a source port to a monitoring port on a different switch within the same network; ERSPAN (Encapsulated RSPAN) encapsulates mirrored traffic in GRE and sends it across a routed network to a remote analyzer — enabling packet capture from remote locations
- C) Remote security policy analysis tools for network devices
- D) Cisco tools for monitoring SPAN (spanning tree) protocol

**Answer: B**

---

**Q29.** 🔴 What is "SolarWinds SAM" (Server & Application Monitor)?
- A) A security access management tool by SolarWinds
- B) A SolarWinds module extending monitoring beyond network devices to servers and applications — tracking Windows/Linux server metrics, application response times, service availability, and component health; integrating with NPM for correlated infrastructure visibility
- C) A SolarWinds module for managing software licenses
- D) A standalone SIEM tool by SolarWinds

**Answer: B**

---

**Q30.** 🔴 What is "network topology auto-discovery" in tools like SolarWinds?
- A) Automatically discovering which users are on the network
- B) A feature that automatically scans the network using SNMP, ICMP, and CDP/LLDP to discover all devices, build a network map showing device interconnections, and populate the inventory — reducing manual documentation effort and keeping topology current
- C) A DNS-based tool discovering all network hostnames
- D) A security scan identifying unauthorized network devices

**Answer: B**

---

**Q31.** 🟡 What does "interface utilization" metric indicate in network monitoring?
- A) How efficiently network engineers are utilizing their interfaces
- B) The percentage of an interface's maximum bandwidth currently in use — high utilization (>70–80%) sustained over time indicates potential congestion, packet drops, and need for capacity upgrade
- C) The number of devices connected to an interface
- D) The number of errors on a network interface

**Answer: B**

---

**Q32.** 🟡 What is "PRTG Network Monitor"?
- A) A Cisco network performance testing tool
- B) A commercial network monitoring platform by Paessler — providing agentless monitoring of network devices, servers, and applications via SNMP, WMI, NetFlow, and packet sniffing; with a sensor-based licensing model and web-based dashboards
- C) An open-source alternative to Nagios
- D) A packet capture tool competing with Wireshark

**Answer: B**

---

**Q33.** 🔴 What is "AIOps" applied to network monitoring?
- A) AI tools that automatically design network monitoring dashboards
- B) Applying machine learning to network monitoring data — correlating events from multiple sources, suppressing redundant alerts, automatically identifying root causes, predicting failures from trend data, and recommending or automating remediation — reducing alert fatigue and MTTR
- C) An AI-powered network configuration management tool
- D) Automated SNMP polling using AI to adjust intervals

**Answer: B**

---

**Q34.** 🔴 What is "sFlow" and how does it differ from NetFlow?
- A) A slow-flow sampling protocol for low-speed networks
- B) An open-standard traffic sampling protocol embedded in network hardware — sampling 1 in N packets and forwarding the sample with interface counters to a collector; vendor-neutral (unlike NetFlow which is Cisco-origin), supported natively by Arista and many other vendors
- C) A flow-based security monitoring protocol
- D) A software-based flow monitoring tool running on servers

**Answer: B**

---

**Q35.** 🔴 What is "network performance baselining" and how is it used operationally?
- A) Setting the base cost for network performance improvements
- B) Establishing a documented record of normal network behavior (utilization, latency, error rates, traffic patterns) during representative periods — used to detect deviations indicating problems, set meaningful alert thresholds, measure improvement after changes, and plan capacity
- C) A Cisco command setting performance baseline parameters
- D) The initial performance test run after a network installation

**Answer: B**

---
*35 Questions | Shared — Network Monitoring Tools.*
