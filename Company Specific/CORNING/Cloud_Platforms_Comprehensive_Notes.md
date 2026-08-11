# Cloud Platforms — Comprehensive Study Notes
### For Corning OA Prep | JD2 (Data & AI Engineering), JD5 (Business Engagement)
> Covers: Cloud fundamentals, service models, AWS, Azure, GCP core services, networking, security, data/AI services, cost management, architecture patterns.

---

## 1. Cloud Computing Fundamentals

### 1.1 What is Cloud Computing?
Cloud computing is the delivery of computing services — servers, storage, databases, networking, software, analytics, AI — over the internet ("the cloud") on a **pay-as-you-go** basis. Instead of owning and managing physical data centers, you rent capacity from a cloud provider.

**Core characteristics (NIST definition):**
- **On-demand self-service:** Provision resources without human interaction with provider
- **Broad network access:** Accessible from anywhere over the internet
- **Resource pooling:** Provider's resources shared across many customers (multi-tenancy)
- **Rapid elasticity:** Scale up/down quickly, appearing unlimited to the user
- **Measured service:** Pay only for what you use (metered billing)

### 1.2 Cloud Service Models

**IaaS (Infrastructure as a Service):**
- Provider manages: Physical hardware, networking, hypervisor, storage
- Customer manages: OS, middleware, runtime, applications, data
- Examples: AWS EC2, Azure VMs, Google Compute Engine
- Best for: Maximum control, custom OS configs, lift-and-shift migrations

**PaaS (Platform as a Service):**
- Provider manages: Everything in IaaS + OS, middleware, runtime
- Customer manages: Applications and data only
- Examples: AWS Elastic Beanstalk, Azure App Service, Google App Engine, Heroku
- Best for: Developers who want to focus on code, not infrastructure

**SaaS (Software as a Service):**
- Provider manages: Everything — infrastructure, platform, and application
- Customer: Just uses the software (usually via browser)
- Examples: Microsoft 365, Salesforce, SAP S/4HANA Cloud, Google Workspace
- Best for: Standard business software, no customization needed

**Additional models:**
- **FaaS (Function as a Service) / Serverless:** Run individual functions triggered by events; no server management; pay per execution (AWS Lambda, Azure Functions, Google Cloud Functions)
- **CaaS (Container as a Service):** Run containerized workloads; provider manages container orchestration (AWS EKS, Azure AKS, Google GKE)
- **DBaaS (Database as a Service):** Managed databases — provider handles patching, backups, scaling (AWS RDS, Azure SQL, Google Cloud SQL)

### 1.3 Deployment Models

**Public Cloud:**
- Infrastructure owned and operated by cloud provider (AWS, Azure, GCP)
- Shared among many organizations (multi-tenant)
- Pay-as-you-go; no upfront infrastructure investment
- Best for: Most workloads, especially variable/unpredictable demand

**Private Cloud:**
- Dedicated infrastructure for one organization
- Can be on-premise (customer's data center) or hosted by provider
- More control, better compliance for regulated industries
- Higher cost (dedicated hardware), less elasticity

**Hybrid Cloud:**
- Combination of public cloud + private cloud/on-premise
- Connected via VPN or dedicated link (AWS Direct Connect, Azure ExpressRoute)
- Workloads move between environments based on need (burst to cloud, keep sensitive data on-prem)
- Most common in large enterprises

**Multi-Cloud:**
- Use two or more public cloud providers (e.g., AWS for compute, GCP for ML, Azure for M365 integration)
- Avoids vendor lock-in, leverages best-of-breed services
- Adds operational complexity

### 1.4 CapEx vs. OpEx Model
- **On-Premise (CapEx):** Large upfront capital expenditure for hardware; depreciate over years; provisioned for peak capacity (waste during low usage)
- **Cloud (OpEx):** No upfront cost; pay monthly subscription/usage; scale to actual demand
- **Financial benefit:** Cloud moves IT spending from capital budget to operational budget; improves cash flow

### 1.5 Scalability, Elasticity, and High Availability

**Scalability:** Ability to handle increased load by adding capacity.
- **Vertical Scaling (Scale Up):** Add more CPU/RAM to an existing server. Simple but has limits and causes downtime.
- **Horizontal Scaling (Scale Out):** Add more servers behind a load balancer. Virtually unlimited; more resilient.

**Elasticity:** Automatically scaling up AND down based on demand in real-time. Cloud key feature — you don't pay for idle capacity.

**High Availability (HA):** System remains operational despite component failures. Achieved through:
- Redundant components (no single point of failure)
- Multiple availability zones
- Auto-healing (auto-replace failed instances)
- Target: 99.9% (8.7 hrs/year downtime) to 99.999% (5.3 min/year)

**Fault Tolerance:** System continues operating even when components fail — stronger than HA (zero downtime, not just minimal).

**Disaster Recovery (DR):**
- **RTO (Recovery Time Objective):** Maximum acceptable downtime
- **RPO (Recovery Point Objective):** Maximum acceptable data loss (how old can the backup be?)
- DR strategies: Backup & Restore (slow, cheap) → Pilot Light → Warm Standby → Multi-Site Active-Active (fast, expensive)

---

## 2. Amazon Web Services (AWS)

### 2.1 AWS Global Infrastructure
- **Regions:** Geographic areas (us-east-1, eu-west-1, ap-southeast-1) — 30+ globally
- **Availability Zones (AZs):** 2–6 discrete data centers per region, each with independent power/cooling/networking
- **Edge Locations:** CDN endpoints for CloudFront — 400+ globally
- **Best practice:** Deploy across multiple AZs for high availability; across multiple regions for disaster recovery

### 2.2 AWS Compute

**EC2 (Elastic Compute Cloud):**
- Virtual machines ("instances") in the cloud
- Choose: instance type (CPU, RAM, GPU), OS (Linux, Windows), storage, network
- **Instance families:** t (general purpose/burstable), m (general purpose), c (compute optimized), r (memory optimized), g/p (GPU), i (storage optimized)
- **Purchasing options:**
  - On-Demand: Pay per hour/second; no commitment; most flexible
  - Reserved: 1 or 3 year commitment; up to 72% discount
  - Spot: Bid on unused capacity; up to 90% discount; can be interrupted
  - Savings Plans: Flexible commitment; similar discounts to Reserved

**Auto Scaling:**
- Automatically add/remove EC2 instances based on demand
- Scaling policies: target tracking (keep CPU at 70%), step scaling, scheduled
- Requires: Launch Template + Auto Scaling Group + Load Balancer

**Elastic Load Balancer (ELB):**
- Distribute traffic across multiple EC2 instances/containers
- Types: Application Load Balancer (HTTP/HTTPS, path-based routing), Network Load Balancer (TCP, ultra-high performance), Gateway Load Balancer (third-party appliances)

**AWS Lambda:**
- Serverless function execution — upload code, define trigger, run
- Pay per invocation + duration (milliseconds)
- Max 15 minutes execution time
- Triggers: API Gateway (HTTP), S3 events, DynamoDB streams, SQS, EventBridge (schedule)
- Languages: Python, Node.js, Java, Go, .NET, Ruby, custom runtime

**ECS / EKS:**
- **ECS (Elastic Container Service):** AWS-native container orchestration; run Docker containers at scale
- **EKS (Elastic Kubernetes Service):** Managed Kubernetes; run Kubernetes workloads without managing control plane
- **Fargate:** Serverless container runtime for ECS/EKS — no EC2 instances to manage

### 2.3 AWS Storage

**S3 (Simple Storage Service):**
- Object storage for any amount of data (files, backups, static websites, data lakes)
- **Buckets:** Containers for objects; globally unique names
- **Objects:** Files with key (filename), value (data), metadata
- **Storage classes:** Standard (frequently accessed), Standard-IA (infrequent access, lower cost), Glacier (archival, retrieval minutes to hours), Glacier Deep Archive (cheapest, 12hr retrieval)
- **Features:** Versioning (keep all versions), lifecycle rules (auto-transition between classes), replication (cross-region or same-region), bucket policies (access control), encryption
- **Durability:** 99.999999999% (11 nines) — S3 is designed to never lose an object

**EBS (Elastic Block Store):**
- Block storage volumes attached to EC2 instances — like a virtual hard drive
- Types: gp3 (general purpose SSD), io2 (high-performance IOPS), st1 (throughput HDD), sc1 (cold HDD)
- Single AZ — data automatically replicated within AZ

**EFS (Elastic File System):**
- Managed NFS (Network File System) — shared file storage accessible from multiple EC2 instances simultaneously
- Scales automatically; pay for storage used

**FSx:**
- Managed file systems: FSx for Windows File Server (SMB), FSx for Lustre (HPC), FSx for NetApp ONTAP

### 2.4 AWS Networking

**VPC (Virtual Private Cloud):**
- Logically isolated virtual network within AWS
- Define: IP address range (CIDR), subnets, route tables, gateways
- **Public subnet:** Has route to Internet Gateway → EC2 instances can have public IPs and receive internet traffic
- **Private subnet:** No direct internet route → for databases, app servers; use NAT Gateway for outbound-only internet access
- **Best practice:** Multi-AZ with public/private subnets in each AZ

**Key VPC components:**
- **Internet Gateway (IGW):** Enable internet access for public subnets
- **NAT Gateway:** Enable private subnet instances to access internet (outbound only)
- **Security Groups:** Stateful firewall at instance level (allow rules only; deny is implicit)
- **Network ACLs:** Stateless firewall at subnet level (explicit allow AND deny rules)
- **VPC Peering:** Connect two VPCs (same or different accounts/regions)
- **Transit Gateway:** Central hub connecting many VPCs and on-premise networks
- **Direct Connect:** Dedicated private network connection from on-premise to AWS (not over internet) — lower latency, consistent bandwidth

**Route 53:**
- AWS's DNS service
- Domain registration, DNS routing (simple, weighted, latency-based, failover, geolocation)
- Health checks — route traffic away from unhealthy endpoints

**CloudFront:**
- AWS's CDN (Content Delivery Network)
- Cache content at 400+ edge locations globally
- Reduce latency for static content (images, CSS, JS) and accelerate API calls
- Integrated with S3 (origin), EC2, ALB, Lambda@Edge (run code at edge)

### 2.5 AWS Databases

**RDS (Relational Database Service):**
- Managed relational databases: MySQL, PostgreSQL, Oracle, SQL Server, MariaDB
- AWS manages: patching, backups (automated), Multi-AZ failover, read replicas
- **Multi-AZ:** Synchronous standby replica in different AZ — automatic failover (HA)
- **Read Replicas:** Asynchronous copies for read scaling (up to 15 read replicas)

**Aurora:**
- AWS's cloud-native relational database — MySQL and PostgreSQL compatible
- Up to 5x faster than MySQL, 3x faster than PostgreSQL
- 6-way replication across 3 AZs — highly available
- **Aurora Serverless:** Automatically scales capacity to zero when not in use

**DynamoDB:**
- Fully managed NoSQL database (key-value and document)
- Single-digit millisecond performance at any scale
- Serverless — scales automatically, pay per capacity or on-demand
- Use for: session management, gaming leaderboards, shopping carts, high-frequency reads/writes

**ElastiCache:**
- Managed in-memory caching: Redis or Memcached
- Cache frequently accessed data to reduce database load (reduce latency from ms to µs)

**Redshift:**
- Managed cloud data warehouse (OLAP)
- Columnar storage + MPP (Massively Parallel Processing)
- Query petabytes of data using SQL
- Integrates with S3 (Redshift Spectrum — query S3 data without loading it)

### 2.6 AWS Data & AI Services

**AWS Glue:**
- Serverless ETL service
- Crawlers: auto-discover schema of S3/RDS data → populate Glue Data Catalog
- ETL jobs: PySpark or Python scripts running on managed Spark infrastructure
- Data Catalog: Central metadata repository (schema, partition info) used by Athena, Redshift Spectrum, EMR

**Athena:**
- Serverless interactive SQL query service for S3 data
- Query CSV, JSON, Parquet, ORC files in S3 using standard SQL
- Pay per data scanned (optimize by using Parquet + partitioning)
- No infrastructure to manage

**EMR (Elastic MapReduce):**
- Managed big data platform: Apache Spark, Hadoop, Hive, Presto
- For large-scale batch data processing
- More control than Glue; requires cluster management

**Kinesis:**
- Real-time data streaming and processing
- **Kinesis Data Streams:** Ingest and process real-time data (clickstreams, logs, transactions)
- **Kinesis Data Firehose:** Automatically load streaming data to S3, Redshift, Elasticsearch (no custom code)
- **Kinesis Data Analytics:** SQL or Apache Flink on streaming data

**SageMaker:**
- End-to-end ML platform on AWS
- **Studio:** Jupyter-based IDE for ML development
- **Training:** Managed distributed model training
- **Autopilot:** AutoML — automatic model selection and tuning
- **Model Registry:** Track and manage model versions
- **Endpoints:** Deploy models as real-time REST APIs
- **Feature Store:** Centralized feature management
- **Clarify:** Detect bias and explain model predictions
- **Pipelines:** MLOps orchestration

**AWS Bedrock:**
- Fully managed service to access foundation models (Anthropic Claude, Amazon Titan, Stability AI, Meta Llama) via API
- Build GenAI apps without managing ML infrastructure
- Includes: Knowledge Bases (RAG), Agents (tool-using LLMs), Guardrails (safety)

---

## 3. Microsoft Azure

### 3.1 Azure Global Infrastructure
- **Regions:** 60+ globally (Azure has more regions than any other cloud)
- **Availability Zones:** 3 physically separate zones per region
- **Region Pairs:** Each region paired with another for disaster recovery (e.g., East US ↔ West US)
- **Sovereign Clouds:** Azure Government (US), Azure China — physically separated for compliance

### 3.2 Azure Core Compute

**Azure Virtual Machines:**
- IaaS VMs with extensive size options
- **Availability Sets:** Spread VMs across fault domains (different rack/power) and update domains
- **VM Scale Sets:** Auto-scaling group of identical VMs
- **Azure Spot VMs:** Low-priority, interruptible VMs at discounted price (like AWS Spot)

**Azure App Service:**
- PaaS platform for web apps, REST APIs, mobile backends
- Languages: .NET, Java, Python, Node.js, PHP, Ruby
- Built-in: scaling, CI/CD integration, custom domains, SSL

**Azure Functions:**
- Serverless compute — same concept as AWS Lambda
- Triggers: HTTP, Timer, Blob storage, Service Bus, Event Hub, CosmosDB

**Azure Kubernetes Service (AKS):**
- Managed Kubernetes — control plane managed by Azure
- Integrates with Azure AD, ACR, Monitor, DevOps

**Azure Container Instances (ACI):**
- Run individual containers without managing servers or Kubernetes
- Pay per second; fastest way to run a container in Azure

### 3.3 Azure Storage

**Azure Blob Storage:**
- Object storage (equivalent to AWS S3)
- **Access tiers:** Hot (frequent), Cool (infrequent, lower cost), Archive (rarely accessed, lowest cost)
- **Azure Data Lake Storage Gen2 (ADLS Gen2):** Blob Storage with hierarchical namespace — optimized for big data analytics workloads (Hadoop compatible)

**Azure Managed Disks:** Block storage for Azure VMs (equivalent to AWS EBS)

**Azure Files:** Managed file shares (SMB/NFS protocol) accessible from VMs and on-premise

**Azure Queue Storage:** Simple message queue for decoupling applications

### 3.4 Azure Networking

**Azure Virtual Network (VNet):**
- Equivalent to AWS VPC — isolated virtual network
- Subnets, Network Security Groups (NSGs), Route Tables
- **VNet Peering:** Connect two VNets (low latency, same/different regions)
- **Azure Bastion:** Browser-based RDP/SSH to VMs without public IP
- **ExpressRoute:** Dedicated private connection from on-premise to Azure (like AWS Direct Connect)
- **Azure VPN Gateway:** Site-to-site or point-to-site VPN connections

**Azure Load Balancer:** Layer 4 TCP/UDP load balancing

**Azure Application Gateway:** Layer 7 HTTP/HTTPS load balancer with WAF (Web Application Firewall)

**Azure Front Door:** Global CDN + load balancer + WAF — routes to nearest healthy backend worldwide

**Azure DNS:** Managed DNS service

### 3.5 Azure Databases

**Azure SQL Database:** Managed SQL Server in the cloud — PaaS; automatic backups, patching, HA

**Azure SQL Managed Instance:** Near 100% SQL Server compatibility — for lift-and-shift migrations

**Azure Cosmos DB:**
- Globally distributed, multi-model NoSQL database
- APIs: SQL (document), MongoDB, Cassandra, Gremlin (graph), Table
- Single-digit millisecond latency; auto-scaling throughput
- Turnkey global distribution — replicate to multiple regions with one click

**Azure Database for PostgreSQL/MySQL:** Managed open-source databases

**Azure Cache for Redis:** Managed Redis caching service

### 3.6 Azure Data & AI Services

**Azure Data Factory (ADF):**
- Cloud ETL/ELT and data integration service
- Visual pipeline designer + 90+ built-in connectors
- Equivalents: AWS Glue + AWS Step Functions combined

**Azure Synapse Analytics:**
- Unified analytics platform combining:
  - **Dedicated SQL Pool:** Data warehouse (formerly SQL DW) — MPP columnar analytics
  - **Serverless SQL Pool:** Query data lake files with SQL (like Athena)
  - **Apache Spark Pool:** Managed Spark for big data processing
  - **Data Integration:** ADF-like pipelines built in
  - **Synapse Link:** Zero-ETL connection to Azure Cosmos DB and other operational stores

**Azure Databricks:**
- First-party Databricks integration on Azure (jointly developed by Microsoft and Databricks)
- Managed Apache Spark + Delta Lake + MLflow
- Deeply integrated with Azure storage, networking, Active Directory

**Azure Stream Analytics:**
- Serverless real-time stream processing (SQL-based)
- Process IoT, log, clickstream data in real time

**Azure Machine Learning:**
- End-to-end ML platform
- **Designer:** No-code drag-and-drop ML pipeline builder
- **Automated ML (AutoML):** Automatic algorithm and hyperparameter selection
- **Compute clusters:** Managed training infrastructure
- **Managed Online Endpoints:** Deploy models as REST APIs
- **Responsible AI dashboard:** Bias detection, explainability, error analysis

**Azure OpenAI Service:**
- Access to OpenAI models (GPT-4, GPT-4o, DALL-E, Whisper, Embeddings) via Azure
- Enterprise features: private deployment, no data used for training, compliance, SLA
- Integration with Azure security, networking (private endpoints), monitoring

**Azure AI Services (Cognitive Services):**
- Pre-built AI APIs: Vision (OCR, face detection, image analysis), Speech (STT/TTS), Language (NLP, translation, sentiment), Decision (anomaly detection, personalizer)
- No ML expertise needed — call API, get result

### 3.7 Azure Identity & Security

**Microsoft Entra ID (Azure AD):**
- Identity platform for Azure and M365 (covered in M365 notes)
- RBAC on Azure resources based on Entra ID roles

**Azure RBAC:**
- Role-based access control for Azure resources
- Built-in roles: Owner, Contributor, Reader, service-specific roles
- Assigned at: Management Group, Subscription, Resource Group, or Resource level

**Azure Key Vault:**
- Centrally store and manage secrets (API keys, passwords), encryption keys, and certificates
- Apps retrieve secrets at runtime via SDK — never hardcoded

**Microsoft Defender for Cloud:**
- Cloud Security Posture Management (CSPM) — assess and improve security configuration
- Cloud Workload Protection — detect threats in VMs, containers, databases, storage

---

## 4. Google Cloud Platform (GCP)

### 4.1 GCP Core Infrastructure

**Regions and Zones:** Similar to AWS/Azure — regions (us-central1), zones within regions (us-central1-a)

**GCP Compute:**
- **Compute Engine:** IaaS VMs
- **Google Kubernetes Engine (GKE):** Managed Kubernetes — often considered best-in-class K8s offering
- **Cloud Run:** Serverless containers — deploy any containerized app; scales to zero
- **Cloud Functions:** Serverless functions (AWS Lambda equivalent)
- **App Engine:** PaaS for web apps

**GCP Storage:**
- **Cloud Storage:** Object storage (AWS S3 equivalent); storage classes: Standard, Nearline, Coldline, Archive
- **Persistent Disk:** Block storage for VMs
- **Filestore:** Managed NFS file storage

### 4.2 GCP Data & Analytics (GCP's Strongest Area)

**BigQuery:**
- GCP's flagship fully managed, serverless data warehouse
- Columnar storage + distributed query execution
- Pay per query (TB of data scanned) or flat-rate capacity
- **Features:** Streaming inserts, ML in SQL (BigQuery ML — train models with SELECT statements), geospatial analytics, BI Engine (in-memory acceleration), federated queries (query external sources)
- Integrates natively with Looker, Vertex AI, Dataflow

**Cloud Dataflow:**
- Managed Apache Beam runner for stream and batch processing
- Fully serverless — auto-scales
- Equivalent: AWS Kinesis Data Analytics / Azure Stream Analytics for streaming; AWS Glue / Azure Data Factory for batch

**Cloud Dataproc:**
- Managed Hadoop and Spark clusters
- Faster cluster startup than AWS EMR
- Equivalent: AWS EMR, Azure HDInsight

**Cloud Pub/Sub:**
- Asynchronous messaging service (event streaming)
- Publishers push messages → Subscribers pull/receive
- Equivalent: AWS SNS/SQS, Azure Service Bus, Apache Kafka

**Looker / Looker Studio:**
- **Looker:** Enterprise BI platform (acquired 2019); semantic modeling layer (LookML)
- **Looker Studio (formerly Data Studio):** Free self-service reporting tool (like Power BI Desktop)

**Cloud Composer:**
- Managed Apache Airflow — workflow orchestration
- Equivalent: AWS MWAA, Azure Managed Airflow

### 4.3 GCP AI/ML (GCP's Leading Area Alongside Data)

**Vertex AI:**
- Unified ML platform (replaces AI Platform)
- **AutoML:** Train models on tabular, image, text, video data without coding
- **Custom Training:** Use any framework (TensorFlow, PyTorch, scikit-learn) on managed infrastructure
- **Feature Store:** Managed feature repository
- **Model Registry:** Track and manage models
- **Endpoints:** Deploy models for online prediction
- **Pipelines:** MLOps orchestration with Kubeflow Pipelines or TFX
- **Explainable AI:** Feature attributions for predictions
- **Model Monitoring:** Detect drift in production

**Vertex AI Gemini (Google's LLM):**
- Access Gemini Ultra, Pro, Flash, Nano via API
- Multimodal: text, image, audio, video, code
- Grounding: connect to Google Search for up-to-date information

**Cloud Natural Language API:** Sentiment, entity, syntax analysis on text

**Vision AI:** Image classification, object detection, OCR

**Speech-to-Text / Text-to-Speech:** Audio processing APIs

---

## 5. Cloud Architecture Patterns

### 5.1 Well-Architected Framework
All three major cloud providers publish architecture best practices:
- **AWS Well-Architected:** 6 pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability
- **Azure Well-Architected:** Reliability, Security, Cost Optimization, Operational Excellence, Performance Efficiency
- **GCP Architecture Framework:** Similar pillars

### 5.2 Core Architecture Patterns

**Three-Tier Architecture:**
- Presentation (web/app servers) → Application (API servers) → Data (databases)
- Each tier scaled independently; communication through load balancers

**Microservices:**
- Application decomposed into small, independent services
- Each service: owns its data, deployed independently, communicates via API/events
- Benefits: independent scaling, fault isolation, team autonomy
- Challenges: distributed system complexity, network latency, data consistency

**Serverless Architecture:**
- No server management; pay only for execution
- API Gateway → Lambda → DynamoDB
- Event-driven: S3 upload → Lambda processes → stores result

**Event-Driven Architecture:**
- Services communicate via events (messages) through a message broker
- Publishers and subscribers decoupled — publisher doesn't know who's listening
- Tools: Kafka, AWS EventBridge, Azure Service Bus, GCP Pub/Sub

**CQRS (Command Query Responsibility Segregation):**
- Separate read model from write model
- Optimize reads and writes independently

### 5.3 Cloud Security Architecture

**Defense in Depth:**
Multiple security layers — if one fails, others protect:
- Network (VPC, firewall, WAF)
- Identity (IAM, MFA, Conditional Access)
- Data (encryption at rest, in transit)
- Application (OWASP practices, input validation)
- Monitoring (SIEM, anomaly detection)

**Encryption:**
- **In transit:** TLS/SSL for all API calls and data movement
- **At rest:** AES-256 encryption for stored data; provider-managed or customer-managed keys (CMK)
- **Key management:** AWS KMS, Azure Key Vault, GCP Cloud KMS

**IAM Best Practices (all clouds):**
- Principle of least privilege — minimum access needed
- Use roles (service accounts) not long-lived access keys for applications
- Enable MFA for all human users
- Rotate access keys regularly
- Audit access via CloudTrail/Activity Log/Cloud Audit Logs

**Shared Responsibility Model:**
| Responsibility | IaaS | PaaS | SaaS |
|---|---|---|---|
| Physical security | Provider | Provider | Provider |
| Network/Hypervisor | Provider | Provider | Provider |
| OS | Customer | Provider | Provider |
| Runtime/Middleware | Customer | Provider | Provider |
| Application | Customer | Customer | Provider |
| Data | Customer | Customer | Customer (mostly) |
| Identity | Shared | Shared | Shared |

### 5.4 Cost Management

**Cost Optimization Strategies:**
- **Right-sizing:** Use instance types appropriately sized for workload — not over-provisioned
- **Reserved/Committed Use:** Commit to 1–3 years for 40–72% discount on predictable workloads
- **Spot/Preemptible/Spot VMs:** For fault-tolerant batch workloads — 60–90% discount
- **Auto-scaling:** Only run what you need when you need it
- **Storage tiering:** Move infrequently accessed data to cheaper storage classes
- **Delete idle resources:** Unused EC2, unattached EBS volumes, idle load balancers

**FinOps (Cloud Financial Operations):**
- Practice bringing financial accountability to cloud spending
- Three phases: Inform (visibility) → Optimize (efficiency) → Operate (governance)
- Tools: AWS Cost Explorer, Azure Cost Management, GCP Cost Management, CloudHealth, Spot.io

**Tagging strategy:** Tag every resource with: project, environment, owner, cost-center → enables cost allocation and accountability

---

## 6. Cloud Services Comparison Table

| Service Category | AWS | Azure | GCP |
|---|---|---|---|
| Compute (VMs) | EC2 | Virtual Machines | Compute Engine |
| Serverless Functions | Lambda | Azure Functions | Cloud Functions |
| Containers (Managed K8s) | EKS | AKS | GKE |
| Object Storage | S3 | Blob Storage | Cloud Storage |
| Block Storage | EBS | Managed Disks | Persistent Disk |
| Managed Relational DB | RDS | Azure SQL Database | Cloud SQL |
| NoSQL | DynamoDB | Cosmos DB | Firestore / Bigtable |
| Data Warehouse | Redshift | Azure Synapse | BigQuery |
| Managed Spark | EMR | Azure Databricks / HDInsight | Dataproc |
| Stream Processing | Kinesis | Azure Stream Analytics | Dataflow |
| ETL / Data Integration | AWS Glue | Azure Data Factory | Cloud Dataflow |
| ML Platform | SageMaker | Azure Machine Learning | Vertex AI |
| LLM / Foundation Models | Bedrock | Azure OpenAI Service | Vertex AI Gemini |
| CDN | CloudFront | Azure Front Door | Cloud CDN |
| DNS | Route 53 | Azure DNS | Cloud DNS |
| VPN/Private Connect | Direct Connect | ExpressRoute | Cloud Interconnect |
| Identity | IAM | Entra ID / Azure RBAC | Cloud IAM |
| Secret Management | AWS Secrets Manager / KMS | Azure Key Vault | Secret Manager / Cloud KMS |
| Monitoring | CloudWatch | Azure Monitor | Cloud Monitoring |
| SIEM | AWS Security Hub | Microsoft Sentinel | Chronicle |
| Message Queue | SQS | Azure Service Bus | Cloud Pub/Sub |
| Workflow Orchestration | Step Functions / MWAA | Logic Apps / ADF | Cloud Composer |

---

## 7. Key Cloud Terms Quick Reference

| Term | Meaning |
|---|---|
| IaaS | Infrastructure as a Service — rent VMs, storage, networking |
| PaaS | Platform as a Service — rent platform for app deployment; no OS management |
| SaaS | Software as a Service — use software over internet; no infrastructure management |
| Region | Geographic area with multiple data centers |
| Availability Zone (AZ) | Physically separate data center within a region |
| VPC / VNet | Isolated virtual network in the cloud |
| Security Group | Stateful instance-level firewall (AWS) |
| NACL | Network Access Control List — stateless subnet-level firewall (AWS) |
| Load Balancer | Distributes traffic across multiple servers |
| Auto Scaling | Automatically adjust number of instances based on demand |
| CDN | Content Delivery Network — cache content close to users |
| Serverless | Run code without managing servers; pay per execution |
| IAM | Identity and Access Management — who can do what |
| KMS | Key Management Service — manage encryption keys |
| Object Storage | Store files as objects (S3, Blob, Cloud Storage) |
| Block Storage | Raw storage volumes attached to VMs (EBS, Managed Disks) |
| Data Lake | Store raw data of any format at scale (S3 + structure) |
| Data Warehouse | Structured analytical store optimized for SQL queries |
| MPP | Massively Parallel Processing — query execution across many nodes |
| Spot / Preemptible | Spare cloud capacity at deep discount; can be interrupted |
| Reserved Instance | Commit to 1–3 years for significant discount |
| FinOps | Cloud financial management discipline — visibility, optimization, governance |
| Shared Responsibility | Division of security duties between cloud provider and customer |
| Lift and Shift | Move application to cloud with minimal changes (IaaS) |
| Cloud Native | Designed specifically to leverage cloud capabilities (microservices, containers, serverless) |
| Multi-tenancy | Multiple customers sharing same underlying infrastructure (isolated logically) |
| SLA | Service Level Agreement — provider's uptime guarantee (usually 99.9%–99.99%) |

---
*End of Cloud Platforms Comprehensive Notes — ~640 lines.*
*Read alongside: Shared_Cloud_Platforms.md for MCQ practice.*
