# Shared — Network Automation MCQs
### Applies to: JD6 (Global Network Services), JD5 (Business Engagement) — Python networking, Netmiko, NAPALM, Ansible, PowerShell
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What is "network automation" and why is it important?
- A) Automatically purchasing new network equipment
- B) Using scripts, APIs, and tools to automate repetitive network tasks (configuration deployment, compliance checking, monitoring, reporting) — reducing human error, accelerating change delivery, and enabling scalable management of large device fleets
- C) Automating network cable installation
- D) Using AI to replace network engineers entirely

**Answer: B**

---

**Q2.** 🟢 What is "Ansible" and how is it used in network automation?
- A) A Cisco network management platform
- B) An open-source, agentless automation tool using YAML-based playbooks to configure systems and network devices — connecting via SSH or APIs, requiring no agent installation on managed devices; widely used for network config automation with modules for Cisco, Arista, Juniper
- C) A Python library for network device connections
- D) A version control system for network configurations

**Answer: B**

---

**Q3.** 🟢 What is "Netmiko"?
- A) A network monitoring tool for Mikrotik devices
- B) An open-source Python library simplifying SSH connections to network devices across multiple vendors (Cisco IOS/XE/NX-OS, Arista EOS, Juniper, etc.) — abstracting connection handling, prompt detection, and command execution
- C) A Cisco-developed Python networking library
- D) A network simulation tool for testing automation scripts

**Answer: B**

---

**Q4.** 🟢 What is "NAPALM" (Network Automation and Programmability Abstraction Layer with Multivendor support)?
- A) A network security tool for blocking unauthorized access
- B) A Python library providing a unified, vendor-neutral API for interacting with network devices — abstracting vendor-specific differences (Cisco IOS, Arista EOS, Juniper JunOS) behind common functions for getting/setting configurations and operational data
- C) A network automation platform competing with Ansible
- D) A Python library for network packet analysis

**Answer: B**

---

**Q5.** 🟢 What is "Paramiko" in Python networking?
- A) A network parameter configuration tool
- B) A Python library implementing SSH protocol — providing a foundation for SSH connections to remote devices; Netmiko is built on top of Paramiko adding network-device-specific handling
- C) A Cisco Python library for IOS automation
- D) A SNMP library for Python network monitoring scripts

**Answer: B**

---

**Q6.** 🟢 What is "Jinja2" templating and why is it used in network automation?
- A) A Java-based network configuration language
- B) A Python templating engine enabling dynamic generation of text from templates with variables — used in network automation to generate device configurations from structured data, enabling consistent config deployment at scale (used by Ansible, AVD)
- C) A JSON-based interface for network APIs
- D) A JavaScript library for network visualization

**Answer: B**

---

**Q7.** 🟡 How would you connect to a Cisco IOS device using Netmiko in Python?
- A) `cisco = CiscoIOS(host='10.0.0.1', user='admin', password='pass')`
- B) Using `ConnectHandler(device_type='cisco_ios', host='10.0.0.1', username='admin', password='pass')` — then calling `send_command('show ip route')` for output or `send_config_set([...])` for configuration
- C) `paramiko.connect('10.0.0.1', username='admin', password='pass')`
- D) `netmiko.ssh_connect(ip='10.0.0.1', os='ios', creds=('admin','pass'))`

**Answer: B**

---

**Q8.** 🟡 What is the difference between `send_command()` and `send_config_set()` in Netmiko?
- A) `send_command()` is for Cisco; `send_config_set()` is for Arista
- B) `send_command()` executes a single show/exec command and returns output; `send_config_set()` enters config mode, sends a list of configuration commands, and exits config mode — returning the output of the config session
- C) `send_config_set()` saves the config automatically; `send_command()` does not
- D) They are identical — different names for the same function

**Answer: B**

---

**Q9.** 🟡 What is an "Ansible playbook"?
- A) A manual describing how to play the Ansible game
- B) A YAML file defining a set of plays — each targeting hosts and containing tasks (modules) to execute in order — the primary way to express automation logic in Ansible
- C) A Python script executed by Ansible
- D) An Ansible configuration file for setting global variables

**Answer: B**

---

**Q10.** 🟡 What is an "Ansible inventory"?
- A) A list of Ansible modules available for use
- B) A file (INI, YAML, or dynamic) defining the managed hosts and groups that Ansible targets — including connection details, variables, and group membership; can be static files or dynamically generated from network management systems
- C) An inventory of Ansible playbooks in a repository
- D) A list of all Ansible tasks completed in a run

**Answer: B**

---

**Q11.** 🟡 What is an "Ansible role"?
- A) A user permission level in Ansible Tower
- B) A structured, reusable unit of Ansible automation — organizing tasks, variables, templates, handlers, and files into a standardized directory structure for a specific function (e.g., a "cisco_base_config" role) enabling code reuse across playbooks
- C) The role an Ansible playbook plays in a workflow
- D) A job template in Ansible Automation Platform

**Answer: B**

---

**Q12.** 🟡 What is "Ansible Tower" / "AWX" / "Ansible Automation Platform"?
- A) A network tower device managed by Ansible
- B) A web-based UI, REST API, and job scheduler for Ansible — enabling centralized execution of playbooks with RBAC, credential management, audit logging, and scheduling; AWX is the open-source upstream; Ansible Automation Platform is the Red Hat enterprise product
- C) An Ansible plugin for managing network towers (cell towers)
- D) A version control platform for Ansible playbooks

**Answer: B**

---

**Q13.** 🟡 What is "YANG" data modeling and why is it important for network automation?
- A) Yet Another Networking GUI — a visualization tool
- B) A data modeling language (RFC 6020/7950) defining the structure, types, and constraints of configuration and operational data on network devices — enabling programmatic, model-driven automation via NETCONF/RESTCONF/gNMI rather than screen-scraping CLI output
- C) A Cisco proprietary data format for device configurations
- D) A YAML extension for network configuration templates

**Answer: B**

---

**Q14.** 🟡 What is "RESTCONF" in network automation?
- A) A REST API for configuring OSPF routing protocols
- B) An IETF protocol (RFC 8040) providing a RESTful HTTP interface to YANG-modeled data on network devices — using JSON or XML payloads; simpler than NETCONF (SSH/XML) for developers familiar with REST APIs; supported on Cisco IOS XE and Arista EOS
- C) A REST API replacement for Cisco CLI
- D) A configuration file format for REST-based network tools

**Answer: B**

---

**Q15.** 🟡 What is "Git" used for in network automation?
- A) A network configuration backup tool
- B) Version control for network automation code (Python scripts, Ansible playbooks, Jinja2 templates, YAML data) — enabling collaboration, change tracking, rollback, peer review via pull requests, and CI/CD pipeline integration for network changes as code
- C) A tool for pulling network device configurations automatically
- D) A network graphing tool for topology documentation

**Answer: B**

---

**Q16.** 🟡 What is "infrastructure as code" (IaC) applied to networking (Network as Code)?
- A) Writing code that manages network billing infrastructure
- B) Treating network configurations, policies, and topology as code — stored in version control, peer-reviewed, tested in CI/CD pipelines, and deployed automatically — bringing software engineering practices to network operations for consistency, auditability, and speed
- C) Converting network diagrams into executable code
- D) A programming language for network hardware firmware

**Answer: B**

---

**Q17.** 🟡 What is a "network CI/CD pipeline" for configuration changes?
- A) A continuous integration pipeline for monitoring network changes
- B) An automated pipeline where network configuration changes (committed to Git) trigger: syntax validation → linting → automated testing in a lab/simulation → peer review → staged deployment to production — reducing human error and enabling consistent, auditable change delivery
- C) A pipeline for continuously integrating new network devices
- D) A Cisco DNA Center feature for automating network changes

**Answer: B**

---

**Q18.** 🟡 What is "nornir" in Python network automation?
- A) A Nordic network automation standard
- B) A Python-native automation framework providing multi-threaded/async execution of tasks across many devices simultaneously — more Pythonic than Ansible for developers who prefer pure Python workflows; often combined with Netmiko, NAPALM, or RESTCONF
- C) A network normalization library for data parsing
- D) A Cisco Python SDK for IOS XE automation

**Answer: B**

---

**Q19.** 🟡 What is "TextFSM" / "NTC Templates" in network automation?
- A) A text file system for storing network templates
- B) A Python library (TextFSM) and community template library (NTC Templates) for parsing structured data from unstructured CLI output — converting `show` command text into Python data structures for programmatic processing without screen-scraping logic
- C) A Cisco tool for formatting network configuration files
- D) A text-based network simulation tool

**Answer: B**

---

**Q20.** 🟡 What is "pyATS" / "Genie" (Cisco)?
- A) A Python advanced testing suite for Cisco Meraki
- B) Cisco's Python-based network test automation framework (pyATS) with a parser/feature library (Genie) — enabling automated network testing, configuration validation, and operational state verification across Cisco platforms; widely used for network CI/CD and validation
- C) A Python library for Cisco APIC management
- D) A Cisco GUI tool for automated network audits

**Answer: B**

---

**Q21.** 🟡 What is PowerShell used for in network automation?
- A) Powering network switches remotely
- B) A cross-platform scripting and automation framework — used in network contexts to interact with Windows-based network tools (RSAT, AD, DHCP/DNS servers), automate network configuration via REST APIs, process CSV/XML network data, and integrate with Microsoft ecosystem tools
- C) A Cisco CLI alternative on Windows platforms
- D) A power management protocol for network devices

**Answer: B**

---

**Q22.** 🟡 What is "idempotency" in the context of network automation?
- A) Running the same automation script only once
- B) The property that applying the same automation script multiple times produces the same result as applying it once — critical for reliable automation: if the desired state is already present, the tool makes no changes; Ansible is designed to be idempotent
- C) A security property ensuring network scripts are tamper-proof
- D) A performance property ensuring automation runs at the same speed each time

**Answer: B**

---

**Q23.** 🔴 What is "OpenConfig" in network automation?
- A) An open-source network configuration tool
- B) A vendor-neutral, community-driven initiative defining standard YANG data models for network device configuration and telemetry — enabling consistent, model-driven automation and monitoring across multi-vendor networks (Cisco, Arista, Juniper) using NETCONF/gNMI
- C) An open configuration standard for Cisco devices
- D) A Cisco open-source network management project

**Answer: B**

---

**Q24.** 🔴 What is "Batfish" used for in network automation?
- A) A Python library for batch processing of network data
- B) An open-source network configuration analysis tool — parsing device configurations and simulating network behavior to answer questions about routing, reachability, and ACL impact without touching live devices; enables pre-change validation and network testing in CI/CD pipelines
- C) A batch job scheduler for network automation tasks
- D) A network simulation tool for testing physical topologies

**Answer: B**

---

**Q25.** 🔴 What is "Terraform" in a network automation context?
- A) A network device firmware update tool
- B) A HashiCorp IaC tool for provisioning and managing infrastructure via declarative HCL configuration — used for network automation to provision cloud network resources (VPCs, subnets, security groups, load balancers) and increasingly physical network devices via provider plugins
- C) A Python framework for network configuration management
- D) A Cisco tool for configuring Terraform-based switches

**Answer: B**

---

**Q26.** 🔴 What is "Salt" (SaltStack) in network automation?
- A) A network security encryption tool
- B) An event-driven automation and configuration management platform — using a master-minion architecture (or masterless/proxy minion for network devices); supports network device automation via proxy minions communicating with devices over SSH or APIs
- C) A Python library for adding salted hashes to network credentials
- D) A Cisco network automation platform

**Answer: B**

---

**Q27.** 🔴 What is "Scrapli" in Python network automation?
- A) A screen-scraping library for network device CLIs
- B) A modern, async-capable Python library for connecting to network devices via SSH, Telnet, or NETCONF — designed with a clean, consistent API and support for async execution; positioned as a faster, more modern alternative to Netmiko
- C) A scripting API for Cisco and Arista devices
- D) A Python library for scraping network monitoring dashboards

**Answer: B**

---

**Q28.** 🔴 What is "event-driven automation" in network operations?
- A) Automation scripts that run on a fixed schedule
- B) Automation triggered by real-time network events (interface down, threshold exceeded, syslog message, telemetry anomaly) — enabling self-healing networks that automatically respond to conditions without waiting for human intervention; exemplified by Ansible EDA (Event-Driven Ansible)
- C) Network events triggering change management approvals automatically
- D) Automated reporting triggered by end-of-month events

**Answer: B**

---

**Q29.** 🔴 What is "Ansible EDA" (Event-Driven Ansible)?
- A) An enhanced deployment automation tool for Ansible
- B) An Ansible component that listens to event sources (webhooks, Kafka, syslog, monitoring alerts) and triggers Ansible playbooks or rulebook actions in response — enabling automated, real-time network remediation at scale
- C) An Ansible module for managing enterprise data centers
- D) An Ansible extension for Arista device automation

**Answer: B**

---

**Q30.** 🔴 What is "model-driven telemetry" vs. "SNMP polling" from an automation perspective?
- A) Model-driven telemetry uses SNMP; polling uses gRPC
- B) Model-driven telemetry (gNMI/gRPC streaming) pushes YANG-modeled data from devices to collectors at high frequency without being asked — enabling real-time automation triggers; SNMP polling requests data at fixed intervals — reactive, lower frequency, harder to use for real-time automation
- C) SNMP polling is more accurate; model-driven is faster but less reliable
- D) They are identical from an automation perspective

**Answer: B**

---

**Q31.** 🟡 What is the typical workflow for automating a network configuration change using Python + Netmiko?
- A) Write config → push directly to all devices → verify manually
- B) Define change in structured data → generate config using Jinja2 template → connect to device with Netmiko → send pre-change show commands → apply config via `send_config_set()` → send post-change show commands → compare pre/post → log results → save config
- C) SSH to device → type commands manually → use Python to document what was done
- D) Use Python to generate a change ticket in ServiceNow → implement manually

**Answer: B**

---

**Q32.** 🟡 What is "dry run" / "check mode" in Ansible network automation?
- A) Running Ansible in a dry (offline) environment
- B) An Ansible execution mode (`--check`) that simulates what changes would be made without actually applying them to devices — enabling validation of playbook logic and expected changes before production execution
- C) Testing an Ansible playbook on a single device before all devices
- D) A mode running Ansible with reduced verbosity

**Answer: B**

---

**Q33.** 🔴 What is "network digital twin" in the context of automation testing?
- A) A duplicate physical network for testing changes
- B) A software model of a physical network — replicating topology, device configurations, and routing behavior in a simulation environment (e.g., Cisco Modeling Labs, GNS3, EVE-NG) — enabling automation scripts and configuration changes to be tested without risking production outages
- C) A digital replica of network monitoring dashboards
- D) An AI model trained to predict network behavior

**Answer: B**

---

**Q34.** 🔴 What is "Cisco Modeling Labs" (CML) used for?
- A) A Cisco tool for modeling network billing costs
- B) A network simulation platform by Cisco enabling engineers to build virtual network topologies running actual Cisco IOS/NX-OS/IOS XE/IOS XR images — used for testing automation scripts, validating configurations, training, and proof-of-concept development without physical hardware
- C) A 3D modeling tool for data center physical layout
- D) A Cisco tool for creating network architecture documentation

**Answer: B**

---

**Q35.** 🔴 What is "intent-driven automation" in modern network operations?
- A) Automation driven by network engineer intent (manual scripts)
- B) An automation approach where operators specify the desired network state or policy (intent) in high-level terms — and the system automatically translates intent into device-specific configurations, validates compliance, and remediates deviations; going beyond task automation to outcome-based management
- C) Automating network changes based on business intent documents
- D) A Cisco DNA Center feature for intent-based routing

**Answer: B**

---
*35 Questions | Shared — Network Automation.*
