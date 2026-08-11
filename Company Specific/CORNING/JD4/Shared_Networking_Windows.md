# Shared — Networking & Windows Fundamentals MCQs
### Applies to: JD4 (Collaboration Services), any JD mentioning Windows, networking, IT troubleshooting
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What is an "IP address"?
- A) A physical address label on a network device
- B) A unique numerical label assigned to each device on a network — used to identify and communicate with it; comes in IPv4 (e.g., 192.168.1.1) and IPv6 formats
- C) An internet password for network access
- D) A port number for network services

**Answer: B**

---

**Q2.** 🟢 What is the difference between a "private IP address" and a "public IP address"?
- A) Private IPs are faster; public IPs are more secure
- B) Private IPs are used within a local network (e.g., 192.168.x.x) and not routable on the internet; public IPs are globally unique and routable on the internet — assigned by ISPs
- C) Public IPs are assigned by the router; private IPs by the ISP
- D) They are identical — the terms are interchangeable

**Answer: B**

---

**Q3.** 🟢 What does "DNS" stand for and what does it do?
- A) Data Network Security — encrypts network traffic
- B) Domain Name System — translates human-readable domain names (e.g., google.com) into IP addresses that computers use to route network traffic
- C) Dynamic Network Switching — manages network routing
- D) Distributed Node Service — distributes network load

**Answer: B**

---

**Q4.** 🟢 What does "DHCP" stand for and what does it do?
- A) Dynamic Host Configuration Protocol — automatically assigns IP addresses and network configuration (subnet mask, gateway, DNS) to devices joining a network
- B) Direct Hardware Control Protocol — manages hardware devices
- C) Dynamic HTML Control Protocol — manages web page rendering
- D) Distributed Host Cache Protocol — caches network data

**Answer: A**

---

**Q5.** 🟢 What is a "router" in networking?
- A) A device that connects devices within the same local network only
- B) A networking device that forwards data packets between different networks — directing traffic between a local network and the internet based on IP routing tables
- C) A device that amplifies Wi-Fi signals
- D) A firewall device that blocks unauthorized traffic

**Answer: B**

---

**Q6.** 🟢 What is a "subnet mask"?
- A) A physical mask placed over network cables for protection
- B) A 32-bit number that divides an IP address into network and host portions — determining which devices are on the same local network (e.g., 255.255.255.0 means the first 3 octets are the network)
- C) A security filter masking internal IP addresses from external networks
- D) A VLAN tag applied to network packets

**Answer: B**

---

**Q7.** 🟢 What is a "firewall" in networking?
- A) A physical wall in a data center preventing fire spread
- B) A security system (hardware or software) that monitors and controls incoming and outgoing network traffic based on predefined security rules — blocking unauthorized access
- C) A tool that prevents data loss from storage failure
- D) An antivirus program for endpoint devices

**Answer: B**

---

**Q8.** 🟢 What does "VPN" stand for and what is it used for?
- A) Virtual Private Network — creates an encrypted tunnel over the internet allowing remote users to securely access a private corporate network as if they were physically on-site
- B) Virtual Public Network — a shared network for multiple organizations
- C) Verified Private Node — a secure server for hosting websites
- D) Virtual Protocol Network — a networking standard for IoT devices

**Answer: A**

---

**Q9.** 🟡 What is "Active Directory" (AD)?
- A) A Microsoft tool for managing active network connections
- B) A Microsoft directory service for Windows domain networks — centralizing user account management, authentication, authorization, and group policy enforcement across an organization
- C) A database storing active IT support tickets
- D) A Microsoft 365 feature for managing SharePoint sites

**Answer: B**

---

**Q10.** 🟡 What is a "domain controller" (DC) in Active Directory?
- A) A device that controls network domain traffic
- B) A server running Active Directory Domain Services (AD DS) that authenticates and authorizes users and computers within a Windows domain — handling login requests and enforcing group policies
- C) A controller managing domain name registrations
- D) A network device managing DHCP for a domain

**Answer: B**

---

**Q11.** 🟡 What is "Group Policy" in Windows Active Directory?
- A) A policy governing the behavior of IT support groups
- B) A Windows feature allowing admins to centrally manage and configure operating system, application, and user settings for computers and users within an AD domain — enforced automatically at login or startup
- C) A policy for managing group mailboxes in Exchange
- D) A security policy for Microsoft 365 groups

**Answer: B**

---

**Q12.** 🟡 What is "LDAP" (Lightweight Directory Access Protocol)?
- A) A lightweight data archiving protocol for backups
- B) A protocol for accessing and managing directory information services (like Active Directory) — used for user authentication and querying directory data across networks
- C) A network protocol for loading web pages quickly
- D) A Microsoft-proprietary protocol for AD replication

**Answer: B**

---

**Q13.** 🟡 What is "TCP/IP" and why is it fundamental to networking?
- A) Technical Control Protocol / Internet Protocol — a hardware standard
- B) Transmission Control Protocol / Internet Protocol — the foundational protocol suite of the internet; TCP ensures reliable, ordered delivery of data; IP handles addressing and routing — together enabling all internet communication
- C) Terminal Connection Protocol / Internal Protocol — for LAN communication
- D) Transfer and Compression Protocol — for file transfer only

**Answer: B**

---

**Q14.** 🟡 What is the difference between TCP and UDP?
- A) TCP is for internet traffic; UDP is for local network traffic
- B) TCP is connection-oriented, ensuring reliable, ordered, error-checked delivery (used for web, email, file transfer); UDP is connectionless, offering faster but unreliable delivery with no guarantee of order or receipt (used for video streaming, DNS, VoIP)
- C) UDP is more secure than TCP
- D) They are identical protocols with different names

**Answer: B**

---

**Q15.** 🟡 What is a "port number" in networking?
- A) A physical connector on a network switch
- B) A 16-bit number identifying a specific process or service on a device — allowing multiple services to run on the same IP address (e.g., HTTP=80, HTTPS=443, SMTP=25, RDP=3389)
- C) A slot number on a network rack
- D) The number of devices connected to a router

**Answer: B**

---

**Q16.** 🟡 What is "HTTPS" and how does it differ from "HTTP"?
- A) HTTPS is faster than HTTP because it compresses data
- B) HTTPS (HTTP Secure) encrypts web traffic using TLS/SSL between browser and server — protecting data in transit from eavesdropping and tampering; HTTP sends data in plaintext
- C) HTTPS is used only for financial websites; HTTP is for all others
- D) They are identical — browsers automatically convert HTTP to HTTPS

**Answer: B**

---

**Q17.** 🟡 What is a "VLAN" (Virtual Local Area Network)?
- A) A virtual LAN created by connecting multiple physical networks
- B) A logical segmentation of a physical network into separate broadcast domains — allowing devices on the same physical switch to be grouped as if on separate networks, improving security and reducing congestion
- C) A virtual network adapter in a virtual machine
- D) A Wi-Fi network separate from the wired LAN

**Answer: B**

---

**Q18.** 🟡 What is "NAT" (Network Address Translation)?
- A) Network Automation Tool — for configuring routers
- B) A method where a router maps multiple private IP addresses to one (or few) public IP addresses — allowing many devices on a private network to share a single public IP for internet access
- C) A network authentication technique for VPN connections
- D) Natural Address Table — a routing protocol

**Answer: B**

---

**Q19.** 🟡 What is "ping" used for in network troubleshooting?
- A) Measuring the physical distance between two network devices
- B) A command (`ping <IP or hostname>`) that sends ICMP echo requests to a target and measures round-trip time — used to check if a host is reachable and measure network latency
- C) A tool for testing firewall rules
- D) A command for resetting network adapter settings

**Answer: B**

---

**Q20.** 🟡 What does `ipconfig` (Windows) / `ifconfig` (Linux) show?
- A) The list of installed network applications
- B) The current network configuration of a device — including IP address, subnet mask, default gateway, and DNS server for each network adapter
- C) The list of active network connections and open ports
- D) The routing table of the device

**Answer: B**

---

**Q21.** 🟡 What is "nslookup" used for?
- A) Looking up network switch configurations
- B) A command-line tool for querying DNS servers — verifying DNS resolution (mapping a hostname to an IP or vice versa) and diagnosing DNS issues
- C) A tool for looking up MAC addresses on a network
- D) A command for viewing network interface statistics

**Answer: B**

---

**Q22.** 🟡 What is "traceroute" / "tracert" (Windows) used for?
- A) Tracing the route of files through a network share
- B) A command that maps the path (hops) a packet takes from source to destination — showing each intermediate router and latency at each hop, useful for diagnosing where network slowdowns or failures occur
- C) A tool for tracing virus propagation through a network
- D) A command for monitoring real-time network bandwidth usage

**Answer: B**

---

**Q23.** 🟡 What is "Remote Desktop Protocol" (RDP)?
- A) A protocol for remotely downloading files from a server
- B) A Microsoft protocol (port 3389) enabling users to remotely connect to and control another Windows computer over a network — commonly used for remote IT support and server administration
- C) A protocol for remote database access
- D) A redundant data path protocol for network resilience

**Answer: B**

---

**Q24.** 🟡 What is "Windows Task Manager" used for?
- A) Managing scheduled Windows tasks and batch jobs
- B) A system monitoring tool showing running processes, CPU/memory/disk/network usage, startup programs, and services — used to identify resource bottlenecks, terminate unresponsive applications, and diagnose performance issues
- C) A tool for managing Windows user accounts
- D) A file manager for organizing Windows system files

**Answer: B**

---

**Q25.** 🟡 What is the "Windows Event Viewer"?
- A) A viewer for watching video events recorded by Windows
- B) A Windows administrative tool that displays detailed logs of system, security, and application events — used to diagnose errors, security incidents, startup failures, and application crashes
- C) A tool for viewing real-time network traffic on Windows
- D) A Windows feature for reviewing user activity logs

**Answer: B**

---

**Q26.** 🟡 What is "Windows Registry"?
- A) A list of registered Windows software licenses
- B) A hierarchical database in Windows storing low-level configuration settings for the OS, hardware, and installed applications — edited via `regedit`; incorrect edits can cause system instability
- C) A system tool for registering new hardware drivers
- D) A Windows backup of user profile settings

**Answer: B**

---

**Q27.** 🔴 What is "OSI model" and name its 7 layers?
- A) Operating System Interface model; layers: Hardware, Software, OS, App, User, Admin, Physical
- B) Open Systems Interconnection model — a conceptual framework for network communication in 7 layers: Physical, Data Link, Network, Transport, Session, Presentation, Application
- C) Open Source Interface model; layers: IP, TCP, HTTP, DNS, FTP, SMTP, TLS
- D) Operational Systems Integration model; 7 layers of IT infrastructure

**Answer: B**

---

**Q28.** 🔴 At which OSI layer does a "switch" operate, and what does it use to forward frames?
- A) Layer 3 (Network); uses IP addresses
- B) Layer 2 (Data Link); uses MAC addresses — a switch builds a MAC address table by learning which device is on which port, then forwards frames only to the correct port
- C) Layer 1 (Physical); uses electrical signals
- D) Layer 4 (Transport); uses port numbers

**Answer: B**

---

**Q29.** 🔴 What is "SSL/TLS" and where is it used?
- A) Server-Side Logic / Transport Layer System — used in web servers
- B) Secure Sockets Layer / Transport Layer Security — cryptographic protocols that encrypt communications between clients and servers (used in HTTPS, email, VPN) — TLS is the modern successor to SSL
- C) System Security Layer / Terminal Layer Security — used in VPN only
- D) Standard Security Link / Transfer Layer Standard — a networking standard

**Answer: B**

---

**Q30.** 🔴 What is "NTFS" and how does it differ from "FAT32"?
- A) NTFS is for SSDs; FAT32 is for HDDs
- B) NTFS (New Technology File System) is Windows' primary file system supporting large files (>4GB), permissions, encryption, journaling, and compression; FAT32 is older, limited to 4GB max file size but more universally compatible with non-Windows devices
- C) FAT32 is more secure than NTFS; NTFS is faster
- D) They are identical for practical purposes

**Answer: B**

---

**Q31.** 🔴 What is "PowerShell" and how does it differ from Command Prompt (CMD)?
- A) PowerShell is a GUI tool; CMD is command-line only
- B) PowerShell is a more powerful cross-platform shell and scripting language built on .NET — supporting object-oriented pipelines, advanced scripting, and access to Windows Management Instrumentation (WMI) and COM objects; CMD is a legacy command interpreter with basic scripting only
- C) CMD is for administration; PowerShell is for development only
- D) They are functionally identical with different syntax

**Answer: B**

---

**Q32.** 🔴 What is "Windows Defender" and what protection does it provide?
- A) A physical security tool for Windows data centers
- B) Microsoft's built-in security suite (antivirus, anti-malware, firewall, endpoint detection) providing real-time protection against viruses, ransomware, spyware, and other threats — now part of Microsoft Defender for Endpoint in enterprise
- C) A Windows feature blocking unauthorized software installation only
- D) A tool for managing Windows update and patch deployment

**Answer: B**

---

**Q33.** 🔴 What is "BitLocker" in Windows?
- A) A Windows tool for locking user accounts after failed login attempts
- B) A Windows full-disk encryption feature that encrypts entire drives using AES — protecting data on lost or stolen devices; in enterprise environments managed via Active Directory or Microsoft Intune
- C) A browser tool for locking browser settings
- D) A password manager built into Windows

**Answer: B**

---

**Q34.** 🔴 What is "Wi-Fi 6" (802.11ax) and how does it improve on Wi-Fi 5 (802.11ac)?
- A) Wi-Fi 6 is the 6 GHz band; Wi-Fi 5 is the 5 GHz band
- B) Wi-Fi 6 (802.11ax) improves on Wi-Fi 5 by adding OFDMA (efficient multi-user scheduling), MU-MIMO (more simultaneous streams), BSS Coloring (reduced interference), and Target Wake Time (better battery life) — significantly improving performance in dense environments like offices
- C) Wi-Fi 6 is faster but only works outdoors; Wi-Fi 5 works indoors
- D) They are identical — Wi-Fi 6 is just a marketing rebrand

**Answer: B**

---

**Q35.** 🔴 What is "zero-day vulnerability"?
- A) A software bug that causes a system to crash on day zero of deployment
- B) A security vulnerability in software that is unknown to the vendor and has no patch available — actively exploited by attackers before the vendor can issue a fix, making it particularly dangerous
- C) A vulnerability with a CVSS score of zero (no risk)
- D) A security flaw that has been patched on the same day it was discovered

**Answer: B**

---
*35 Questions | Shared — Networking & Windows Fundamentals.*
