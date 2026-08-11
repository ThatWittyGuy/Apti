# Shared — Cloud Platforms MCQs
### Applies to: JD2 (Data & AI Engineering), any JD mentioning cloud, AWS, Azure, GCP
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What are the three main cloud service models?
- A) Public, Private, Hybrid
- B) IaaS (Infrastructure as a Service), PaaS (Platform as a Service), SaaS (Software as a Service)
- C) Compute, Storage, Network
- D) On-Premise, Cloud, Edge

**Answer: B**

---

**Q2.** 🟢 What is IaaS?
- A) Software delivered over the internet on a subscription basis
- B) Cloud-provided virtualized computing infrastructure (VMs, storage, networking) that users manage — the cloud handles the physical hardware
- C) A platform for building apps without managing servers
- D) A managed database service

**Answer: B**

---

**Q3.** 🟢 What is SaaS?
- A) A cloud model where you manage your own servers
- B) Fully managed software delivered over the internet — the vendor manages everything; users just use the application (e.g., Gmail, Salesforce, SAP S/4HANA Cloud)
- C) A platform providing tools for developers
- D) A storage-as-a-service offering

**Answer: B**

---

**Q4.** 🟢 Which of the following is Amazon's cloud platform?
- A) Microsoft Azure
- B) AWS (Amazon Web Services)
- C) Google Cloud Platform (GCP)
- D) IBM Cloud

**Answer: B**

---

**Q5.** 🟢 What is "cloud storage" vs. "local storage"?
- A) Cloud storage is faster; local storage is cheaper
- B) Cloud storage stores data on remote servers accessed via the internet, offering scalability, redundancy, and remote access; local storage stores data on physical devices at your location
- C) Cloud storage is encrypted; local is not
- D) Local storage uses SSDs; cloud uses tapes

**Answer: B**

---

**Q6.** 🟢 What does "scalability" mean in cloud computing?
- A) The ability to reduce cloud costs by downgrading servers
- B) The ability to increase or decrease computing resources (up/down or in/out) to match demand — a key advantage of cloud over fixed on-premise infrastructure
- C) The physical size of data center servers
- D) The ability to scale a machine learning model

**Answer: B**

---

**Q7.** 🟢 What is a "virtual machine" (VM) in cloud computing?
- A) A physical server dedicated to one customer
- B) A software emulation of a physical computer running its own OS and applications on shared physical hardware — the foundation of IaaS
- C) A containerized application runtime
- D) A serverless function execution environment

**Answer: B**

---

**Q8.** 🟡 What is the difference between "vertical scaling" and "horizontal scaling"?
- A) Vertical scaling adds more servers; horizontal scaling upgrades a single server
- B) Vertical scaling (scale up) increases resources of a single server (more CPU/RAM); horizontal scaling (scale out) adds more servers to distribute load — horizontal is more resilient
- C) They are the same concept in different cloud providers
- D) Vertical is for storage; horizontal is for compute

**Answer: B**

---

**Q9.** 🟡 What is Amazon S3?
- A) Amazon's third-generation server product
- B) Amazon Simple Storage Service — an object storage service offering highly scalable, durable storage for any type of data (files, backups, data lake files) accessed via HTTP/REST APIs
- C) Amazon's SQL database service
- D) Amazon's container orchestration service

**Answer: B**

---

**Q10.** 🟡 What is Amazon EC2?
- A) Amazon's 2nd-generation cloud platform
- B) Amazon Elastic Compute Cloud — a service providing resizable virtual machines (instances) in the cloud, allowing users to run applications on AWS infrastructure
- C) Amazon's managed container service
- D) Amazon's serverless function execution service

**Answer: B**

---

**Q11.** 🟡 What is "serverless computing"? Give an AWS example.
- A) Running servers without an operating system; example: EC2
- B) A model where the cloud provider manages all server infrastructure and automatically scales; developers deploy code (functions) triggered by events without provisioning servers; example: AWS Lambda
- C) Computing without any cloud provider; example: on-premise
- D) A model where servers run without power; example: AWS Graviton

**Answer: B**

---

**Q12.** 🟡 What is Microsoft Azure's equivalent of AWS S3?
- A) Azure SQL Database
- B) Azure Blob Storage (Azure Data Lake Storage Gen2 for analytics workloads)
- C) Azure Virtual Machines
- D) Azure Cosmos DB

**Answer: B**

---

**Q13.** 🟡 What is Google BigQuery?
- A) Google's virtual machine service
- B) A fully managed, serverless cloud data warehouse by Google that enables fast SQL analytics over massive datasets — billed by query data scanned
- C) Google's object storage service
- D) Google's real-time streaming platform

**Answer: B**

---

**Q14.** 🟡 What is "Azure Data Factory" used for?
- A) A factory simulation tool on Azure
- B) A cloud-based ETL/ELT and data integration service for orchestrating data movement and transformation pipelines across cloud and on-premise sources
- C) An Azure VM provisioning tool
- D) A managed SQL database service on Azure

**Answer: B**

---

**Q15.** 🟡 What is the difference between a "public cloud," "private cloud," and "hybrid cloud"?
- A) Public cloud is free; private and hybrid cost money
- B) Public cloud is shared infrastructure operated by a provider (AWS/Azure/GCP); private cloud is dedicated infrastructure for one organization; hybrid cloud combines both, with workloads running across them
- C) Private cloud is always on-premise; public is always off-site
- D) Hybrid cloud only refers to multi-cloud setups

**Answer: B**

---

**Q16.** 🟡 What is a "container" in cloud computing (e.g., Docker)?
- A) A physical storage box for servers
- B) A lightweight, portable unit that packages an application and all its dependencies together, ensuring consistent behavior across environments — lighter than a VM as it shares the host OS kernel
- C) A VM with a graphical interface
- D) A cloud storage bucket for files

**Answer: B**

---

**Q17.** 🟡 What is "Kubernetes" (K8s)?
- A) A programming language for cloud apps
- B) An open-source container orchestration system that automates deployment, scaling, load balancing, and management of containerized applications across a cluster of machines
- C) A cloud provider competing with AWS
- D) A monitoring tool for cloud infrastructure

**Answer: B**

---

**Q18.** 🟡 What is "AWS Lambda"?
- A) Amazon's machine learning service
- B) AWS's serverless compute service that runs code in response to events (HTTP request, file upload, schedule) without provisioning servers — billed per execution duration
- C) Amazon's container management service
- D) A cryptographic key management service

**Answer: B**

---

**Q19.** 🟡 What is "Amazon Redshift"?
- A) Amazon's NoSQL database service
- B) AWS's fully managed, petabyte-scale cloud data warehouse optimized for analytical SQL queries using columnar storage and massively parallel processing (MPP)
- C) Amazon's object storage service
- D) Amazon's real-time streaming service

**Answer: B**

---

**Q20.** 🟡 What is "Azure Synapse Analytics"?
- A) A real-time monitoring tool for Azure
- B) Microsoft's unified analytics platform combining data warehousing (dedicated SQL pool), big data processing (Spark), and data integration in one workspace
- C) An Azure VM type for high-performance computing
- D) Azure's NoSQL database service

**Answer: B**

---

**Q21.** 🟡 What is "cloud-native" application development?
- A) Applications developed by cloud provider employees
- B) Designing and building applications specifically for cloud environments — using containers, microservices, serverless, declarative APIs, and CI/CD to leverage cloud elasticity and resilience
- C) Applications that run only offline
- D) Migrating legacy applications to the cloud unchanged

**Answer: B**

---

**Q22.** 🟡 What is "auto-scaling" in cloud computing?
- A) Automatically upgrading cloud software versions
- B) A feature that automatically adjusts the number of compute resources (VMs, containers) up or down based on current demand — optimizing cost and performance
- C) Automatic data backup scaling
- D) Automatically scaling storage when 80% full

**Answer: B**

---

**Q23.** 🟡 What is a "CDN" (Content Delivery Network) in cloud?
- A) A cloud database network
- B) A geographically distributed network of servers that caches and delivers content (web pages, videos, files) from locations close to end users — reducing latency
- C) A cloud deployment network for containers
- D) A network of cloud data centers for backup

**Answer: B**

---

**Q24.** 🔴 What is the "shared responsibility model" in cloud security?
- A) Cloud providers share security costs with customers
- B) A framework defining which security responsibilities belong to the cloud provider (physical security, hypervisor, network) and which belong to the customer (data, access management, app security) — varies by IaaS/PaaS/SaaS
- C) A model where security is entirely the cloud provider's responsibility
- D) A shared billing model for cloud security tools

**Answer: B**

---

**Q25.** 🔴 What is "IAM" (Identity and Access Management) in cloud platforms?
- A) A network security scanning tool
- B) A framework of policies and services for managing user identities, authentication, and fine-grained authorization to cloud resources — following least-privilege principles
- C) An AI model management service
- D) An internet access monitoring tool

**Answer: B**

---

**Q26.** 🔴 What is a "VPC" (Virtual Private Cloud)?
- A) A virtual personal computer in the cloud
- B) A logically isolated virtual network within a public cloud where you can define your own IP ranges, subnets, routing, and security rules — providing network-level isolation for cloud resources
- C) A private cloud data center
- D) A VPN connection to an on-premise network

**Answer: B**

---

**Q27.** 🔴 What is "multi-cloud" strategy and what are its tradeoffs?
- A) Using multiple cloud accounts with the same provider
- B) Using two or more cloud providers (e.g., AWS + Azure) for different workloads — reducing vendor lock-in and increasing resilience, but adding operational complexity and integration challenges
- C) Connecting cloud to multiple on-premise data centers
- D) A multi-region deployment on one cloud provider

**Answer: B**

---

**Q28.** 🔴 What is "FinOps" (Cloud Financial Operations)?
- A) A financial software product running on cloud
- B) A practice combining finance, operations, and engineering to bring financial accountability to cloud spending — using visibility, optimization, and governance to maximize business value per cloud dollar
- C) A billing tool provided by cloud vendors
- D) A financial analytics platform on AWS

**Answer: B**

---

**Q29.** 🔴 What is "AWS Glue" used for?
- A) A cloud networking service
- B) A fully managed serverless ETL service on AWS that discovers, catalogs, and transforms data for analytics — including a data catalog, crawlers, and Spark-based job execution
- C) A glue-code framework for microservices
- D) An API gateway service on AWS

**Answer: B**

---

**Q30.** 🔴 What is the difference between "availability zone" (AZ) and "region" in AWS/Azure/GCP?
- A) Region is a single data center; AZ is a group of regions
- B) A region is a geographic area with multiple isolated AZs; an AZ is one or more discrete data centers within a region with independent power/cooling/networking — deploying across AZs improves fault tolerance
- C) AZs are only used in AWS; regions are used in all providers
- D) They are the same concept with different names

**Answer: B**

---

**Q31.** 🔴 What is "Azure Machine Learning" and what does it provide?
- A) An AI chatbot service from Microsoft
- B) A cloud platform for end-to-end ML lifecycle management — including data labeling, experiment tracking, automated ML, model training at scale, deployment, and monitoring
- C) A machine learning library for Python on Azure VMs
- D) An Azure service for running Jupyter notebooks only

**Answer: B**

---

**Q32.** 🔴 What is "Google Cloud Dataflow"?
- A) A data governance tool on GCP
- B) A fully managed, serverless stream and batch data processing service on GCP based on Apache Beam — used for ETL, real-time analytics, and event-driven pipelines
- C) A cloud database for IoT data
- D) GCP's equivalent of Azure DevOps

**Answer: B**

---

**Q33.** 🔴 What is "object storage" vs. "block storage" vs. "file storage" in cloud?
- A) They are three names for the same storage type
- B) Object storage (e.g., S3) stores files as objects with metadata — great for unstructured data at scale; block storage (e.g., EBS) provides raw storage volumes like hard disks — great for databases and VMs; file storage (e.g., EFS) provides shared network file systems
- C) Block storage is for backups; object is for databases; file is for ML
- D) Object storage is on-premise; block and file are cloud-only

**Answer: B**

---

**Q34.** 🔴 What is "Infrastructure as Code" (IaC) and give an example tool?
- A) Writing code that runs on cloud infrastructure
- B) Managing and provisioning cloud infrastructure through machine-readable configuration files rather than manual processes — enabling versioning, repeatability, and automation; example: Terraform, AWS CloudFormation
- C) A programming language designed for infrastructure engineers
- D) A cloud provider's SDK for managing resources

**Answer: B**

---

**Q35.** 🔴 What is "cloud elasticity" vs. "cloud scalability"?
- A) They are identical concepts
- B) Elasticity is the ability to automatically and dynamically provision/deprovision resources in real time based on demand (pay for what you use now); scalability is the ability to handle increased load by adding capacity — elasticity implies automation and fine-grained cost control
- C) Elasticity is for storage; scalability is for compute
- D) Scalability is automatic; elasticity requires manual intervention

**Answer: B**

---
*35 Questions | Shared across cloud-focused JDs.*
