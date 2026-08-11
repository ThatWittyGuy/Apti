# SAP — Comprehensive Study Notes
### For Corning OA Prep | All 6 JDs
> Covers: SAP history, S/4HANA, HANA DB, Fiori, BTP, FICO, MM, SD, Analytics Cloud, Datasphere, Activate, BW, AI/ML in SAP, and everything in between.

---

## 1. SAP — Company & Product Overview

**SAP SE** (Systems, Applications & Products in Data Processing) is a German multinational software company founded in 1972. It is the world's largest enterprise software vendor by revenue, with ~22% of the global ERP market.

**SAP's Mission:** Help organizations run their business processes — finance, procurement, manufacturing, sales, HR — efficiently and intelligently through integrated software.

**Key SAP Product Families:**
- **SAP S/4HANA** — Core ERP (the heart of SAP)
- **SAP BTP** — Business Technology Platform (cloud platform for extensions, integration, AI, analytics)
- **SAP SuccessFactors** — Cloud HCM (HR, payroll, talent)
- **SAP Ariba** — Procurement / supply chain
- **SAP Concur** — Travel & expense management
- **SAP Customer Experience (CX)** — CRM / sales / commerce
- **SAP Integrated Business Planning (IBP)** — Supply chain planning
- **SAP Analytics Cloud (SAC)** — Cloud BI, planning, predictive
- **SAP Datasphere** — Data management / data fabric

---

## 2. SAP Evolution: From R/2 to S/4HANA

| Version | Year | Key Feature |
|---|---|---|
| SAP R/2 | 1979 | Mainframe-based ERP |
| SAP R/3 | 1992 | Client-server architecture; 3-tier; massive enterprise adoption |
| SAP ECC (ERP Central Component) | 2004 | Enhanced R/3; added more modules; supports multiple databases |
| SAP S/4HANA | 2015 | In-memory HANA DB only; simplified data model; real-time; Fiori UI |

**SAP ECC End of Mainstream Maintenance:** 2027 (extended to 2030 with extra fee). This is driving the global wave of S/4HANA migrations — including at Corning.

---

## 3. SAP HANA Database

### 3.1 What is SAP HANA?
SAP HANA (High-performance ANalytic Appliance) is SAP's proprietary **in-memory, column-store database** released in 2010. It is the only database on which SAP S/4HANA runs.

### 3.2 Key Technical Characteristics

**In-Memory Storage:**
- Data lives primarily in RAM, not on disk
- RAM access is ~100,000x faster than disk
- Enables real-time analytics without pre-aggregating data

**Columnar Storage:**
- Data stored column-by-column (not row-by-row)
- When you run `SELECT SUM(revenue) FROM sales`, only the revenue column is read — not all rows
- Dramatic reduction in data scanned → much faster analytical queries
- High compression rates (similar values in a column compress better)

**Row Store (also exists in HANA):**
- OLTP transactions (inserts, updates) use row store
- HANA is a hybrid — columnar for analytics, row store for transactional

**Multi-Model:**
- Relational (SQL), graph, spatial, document (JSON), text search — all in one engine
- No need for separate specialist databases

**Eliminating Aggregates:**
- Traditional databases pre-computed aggregates (summary tables) for performance
- HANA is fast enough to compute aggregations on the fly from raw data
- S/4HANA removed hundreds of aggregate/index tables (e.g., BSIS, BSAS, FAGLFLEXT) — simplifying the data model dramatically

### 3.3 HANA vs. Traditional Databases

| Feature | Traditional DB (Oracle, SQL Server) | SAP HANA |
|---|---|---|
| Storage | Disk-based | In-memory primary |
| Layout | Row-store | Columnar + Row hybrid |
| Aggregates | Pre-computed summary tables needed | Computed real-time from raw data |
| Analytics | Separate OLAP/DW needed | Unified OLTP + OLAP |
| Speed | Seconds/minutes for reports | Sub-second |

### 3.4 HANA Cloud
- Fully managed, cloud-native version of SAP HANA as a service on BTP
- Elastic scaling — scale up/down on demand
- Pay-as-you-go
- Foundation for SAP Datasphere

---

## 4. SAP S/4HANA — Deep Dive

### 4.1 What is S/4HANA?
SAP S/4HANA (SAP Business Suite 4 SAP HANA) is SAP's next-generation ERP system built exclusively on SAP HANA. Released in 2015, it is SAP's strategic ERP product replacing ECC.

**"S/4"** = Business Suite 4 (the 4th generation of SAP's Business Suite)
**"HANA"** = Runs exclusively on SAP HANA database

### 4.2 Key Differences: ECC vs. S/4HANA

| Aspect | SAP ECC | SAP S/4HANA |
|---|---|---|
| Database | Any (Oracle, SQL Server, DB2, HANA) | SAP HANA only |
| Data model | Complex, many aggregate/index tables | Simplified Universal Journal (ACDOCA) |
| UI | SAP GUI (old-style transactions) | SAP Fiori (modern, web-based, responsive) |
| Analytics | Separate BW/BO system needed | Embedded real-time analytics via CDS views |
| Business Partner | Separate Customer & Vendor masters | Unified Business Partner (BP) |
| Material Ledger | Optional | Mandatory |
| FI-CO Reconciliation | Required (separate ledgers) | Eliminated (Universal Journal) |
| AI/ML | Minimal | Deeply embedded |

### 4.3 The Universal Journal (ACDOCA)
The most important architectural innovation in S/4HANA Finance.

**What it is:** A single, central database table that stores all accounting entries across:
- FI (Financial Accounting)
- CO (Controlling)
- AA (Asset Accounting)
- ML (Material Ledger)
- PS (Project Systems)

**Why it matters:**
- In ECC, FI and CO were separate ledgers that required periodic reconciliation
- In S/4HANA, one posting writes to ACDOCA once — FI and CO are always in sync
- No more reconciliation between FI and CO
- Real-time reporting on any financial dimension without data warehouse
- Enables "any dimension, any time" reporting

**ACDOCA key fields:** Company Code, Fiscal Year, Posting Period, GL Account, Cost Center, Profit Center, Segment, Functional Area, Currency fields (transaction, local, group), quantities

### 4.4 Deployment Options

**S/4HANA On-Premise:**
- Installed on customer's own servers
- Full customization flexibility
- Customer manages upgrades, infrastructure
- Best for: complex processes, high customization, data sovereignty needs

**S/4HANA Private Cloud (PCE — Private Cloud Edition):**
- Runs on SAP-managed or hyperscaler (AWS/Azure/GCP) cloud
- More customization than public cloud
- SAP manages infrastructure and upgrades
- Customer manages application

**S/4HANA Public Cloud (formerly Cloud ERP):**
- Full SaaS — SAP manages everything
- Quarterly automatic updates
- Fit-to-standard approach — minimal customization
- Best for: standardized processes, fast implementation, lower TCO
- Limitation: less flexibility

**RISE with SAP:**
- A bundled commercial offering combining S/4HANA Private Cloud + BTP services + support + tools (Business Process Intelligence, SAP Signavio)
- Positioned as SAP's end-to-end cloud transformation package
- Not a product itself — a commercial/contractual model

### 4.5 SAP Organizational Structure in S/4HANA

**Client:** The entire SAP system — one client = one company's complete data environment (e.g., all of Corning's SAP data)

**Company Code:** The basic legal entity for financial accounting — has its own complete set of financial statements. Example: Corning Incorporated (US), Corning Germany GmbH

**Controlling Area:** The management accounting boundary — typically spans one or more company codes

**Plant:** A physical or logical operating location (factory, warehouse, distribution center)

**Storage Location:** Sub-unit of a plant for inventory tracking

**Sales Organization:** Responsible entity for selling products

**Purchasing Organization:** Responsible entity for procurement

**Business Area:** An optional organizational unit for segment reporting across company codes

**Segment:** IFRS 8 / US GAAP reporting segment (e.g., Optical Communications, Display Technologies at Corning)

### 4.6 SAP Activate Methodology (S/4HANA Implementation)
SAP's official agile implementation methodology. Six phases:

**1. Discover**
- Understand SAP Best Practices content
- Business case development
- Organizational readiness
- Access to SAP's trial systems

**2. Prepare**
- Project team formation and governance
- Infrastructure setup (system provisioning)
- Project standards and tools
- Initial training

**3. Explore (Fit-to-Standard Workshops)**
- The most critical phase for S/4HANA
- Run SAP Best Practice processes in the system (as delivered)
- Business teams see the standard system first
- Document gaps (what doesn't fit standard)
- Design decisions made: accept standard OR justify deviation
- Goal: maximize standard adoption, minimize custom development

**4. Realize (Build in Sprints)**
- Configure the system based on Explore decisions
- Custom development only for approved gaps
- Data migration object preparation
- Integration development
- Unit testing and integration testing
- Iterative sprints (agile)

**5. Deploy**
- User Acceptance Testing (UAT) by business users
- End-user training
- Cutover planning (how to switch from old to new system)
- Data migration cutover rehearsals
- Go/no-go decision
- Actual go-live

**6. Run**
- Hypercare period (intensive post-go-live support)
- Transition to BAU (Business as Usual) support
- Continuous improvement
- Monitoring and optimization

### 4.7 SAP System Landscape & Transports
Standard three-tier landscape:

**DEV → QAS → PRD**

**Transport Request:**
- A container packaging configuration changes or development objects
- Created in DEV, released, imported to QAS for testing, then imported to PRD
- Provides control, audit trail, and separation of environments
- Managed through STMS (SAP Transport Management System)

**Correction and Transport System (CTS):**
- The SAP mechanism managing transport requests
- Everything that changes in DEV is tracked in a transport

---

## 5. SAP Fiori

### 5.1 What is SAP Fiori?
SAP Fiori is SAP's **design language and user experience (UX) framework** for modern, role-based SAP applications. Introduced in 2013, it replaced the dated SAP GUI with a clean, responsive, web-based interface.

### 5.2 Fiori Design Principles (The 5 Fs)
1. **Role-based** — each user sees only what's relevant to their job
2. **Responsive** — works on desktop, tablet, and mobile
3. **Simple** — one user, one use case, one screen
4. **Coherent** — consistent design across all apps
5. **Delightful** — pleasant user experience

### 5.3 Fiori Launchpad
The central entry point — like a home screen — showing tiles (app shortcuts) organized by role and catalog. Users click tiles to launch specific apps.

### 5.4 Types of Fiori Apps
**Transactional Apps:** Create/change/display business documents (e.g., Create Purchase Order, Post Journal Entry)

**Analytical Apps:** Real-time KPI monitoring and reporting (e.g., Days Payable Outstanding, Cash Flow Analysis)

**Fact Sheet Apps:** Overview of master data (e.g., Supplier Fact Sheet, Material Overview)

**SAP GUI transactions wrapped in Fiori:** Some classic transactions accessible via Fiori shell

### 5.5 Fiori Technical Foundation
- Built on **SAPUI5** (SAP's JavaScript UI framework based on OpenUI5/jQuery)
- Runs in any modern browser
- Backend services exposed via **OData** (REST-based API standard)
- Content managed via **Fiori Launchpad Designer** and **Site Manager**

### 5.6 OData in Fiori
**OData (Open Data Protocol):** A REST-based standard for exposing business data as APIs. SAP Fiori apps communicate with the S/4HANA backend via OData services.
- Read data: HTTP GET
- Create data: HTTP POST
- Update: HTTP PUT/PATCH
- Delete: HTTP DELETE
- Returns JSON or XML

---

## 6. SAP FICO — Financial Accounting & Controlling

### 6.1 SAP FI (Financial Accounting)

**Purpose:** External financial reporting — record all business transactions and produce statutory financial statements.

#### 6.1.1 General Ledger (FI-GL)
- Central repository of all accounting data
- Every business event posts a document to the GL
- In S/4HANA: the **Universal Journal (ACDOCA)** IS the GL

**Key Concepts:**
- **Chart of Accounts (COA):** List of all GL accounts (structured numbering system)
- **Fiscal Year Variant:** Defines accounting periods (Jan–Dec, Apr–Mar, 4-4-5)
- **Posting Period:** Which periods are open for posting (controlled by period variants)
- **Document Types:** KR = vendor invoice, KZ = vendor payment, DR = customer invoice, DZ = customer payment, SA = GL posting
- **Posting Keys:** 2-digit code determining debit/credit and account type (e.g., 40 = GL debit, 50 = GL credit, 31 = vendor credit)

**Parallel Accounting / Parallel Ledgers (critical for global companies):**
- S/4HANA supports multiple accounting ledgers simultaneously
- **Leading Ledger (0L):** Primary ledger — typically IFRS/US GAAP
- **Non-Leading Ledgers:** Local GAAP ledgers (e.g., different fiscal year, different depreciation)
- Each ledger can have own currency, fiscal year variant, accounting principles
- Eliminates need for manual adjustment entries between standards

**Document Splitting:**
- Automatically splits accounting line items by financial dimensions (profit center, segment)
- Enables zero-balance financial statements at profit center/segment level
- Configured via splitting rules — which characteristics are split and how

#### 6.1.2 Accounts Payable (FI-AP)
- Manages amounts owed to vendors
- Vendor Master: key data enabling AP processing (payment terms, bank details, tax info)
- **Business Partner (BP):** In S/4HANA, vendors/customers are unified as Business Partners

**AP Process:**
1. Vendor invoice received → posted in SAP (transaction FB60 or MIRO)
2. Three-way match verified (PO + GR + Invoice)
3. Invoice approved / payment block released
4. **Automatic Payment Run (F110):** Program selects due invoices, creates payment documents, generates payment file for bank
5. Bank payment processed → bank statement uploaded → reconciliation

**Key AP Concepts:**
- **Payment terms:** Define due dates and early payment discounts (e.g., 2/10 Net 30 = 2% discount if paid within 10 days, otherwise full amount due in 30 days)
- **Dunning:** Automated reminders to customers about overdue invoices (same concept on AR side)
- **GR/IR Clearing Account:** Temporary account holding value between goods receipt and invoice receipt — cleared when both sides match
- **Open Item Management:** Tracks unpaid items; cleared when payment applied

#### 6.1.3 Accounts Receivable (FI-AR)
- Manages amounts owed by customers
- Customer invoice posted after goods/services delivered
- Cash application: incoming payments matched to open invoices
- **Dunning:** Automated overdue payment reminders (escalating levels)
- **Credit Management:** Credit limit checks before sales order fulfillment

#### 6.1.4 Asset Accounting (FI-AA)
- Manages the entire lifecycle of fixed assets
- **Asset Master:** Describes the asset (description, capitalization date, useful life, depreciation method)
- **Depreciation Areas:** Each asset has multiple depreciation areas for different accounting purposes (book, tax, group reporting)
- **Depreciation Methods:** Straight-line (equal amounts), declining balance (higher early, lower later), units of production
- **New Asset Accounting in S/4HANA:** Posts depreciation directly to ACDOCA in real time — no more periodic batch depreciation runs that could cause FI-AA reconciliation issues

**Asset Lifecycle:**
1. **Acquisition:** Asset purchased → Asset Master created → capitalized to balance sheet
2. **Depreciation:** Periodic depreciation posted → reduces asset book value, records expense
3. **Transfer:** Move asset between company codes, cost centers, plants
4. **Retirement/Disposal:** Asset sold or scrapped → gain/loss on disposal posted

#### 6.1.5 Bank Accounting
- **House Banks:** Company's banks configured in SAP
- **Bank Reconciliation:** Match SAP bank GL to external bank statement
- **Electronic Bank Statements:** Automatically uploaded and processed
- **Cash Management:** Liquidity planning and cash position reporting

### 6.2 SAP CO (Controlling)

**Purpose:** Internal management accounting — track costs and profitability to support decisions. Not for external reporting.

#### 6.2.1 Cost Element Accounting
- **Primary Cost Elements:** Mirror P&L GL accounts (salary expense GL = salary cost element)
- **Secondary Cost Elements:** Only exist in CO — used for allocations, settlements, overhead absorption (no corresponding GL account)

#### 6.2.2 Cost Center Accounting (CCA)
- **Cost Centers:** Organizational units collecting costs (IT Dept, Marketing, Plant 1)
- Planned costs loaded via budgeting/planning
- Actual costs flow in from FI postings, payroll, depreciation
- **Variance Analysis:** Planned vs. Actual cost per cost center
- **Allocations:** Distribute shared costs to consuming cost centers/profit centers:
  - **Distribution:** Keeps original cost element visible in receiver
  - **Assessment:** Uses secondary cost element (summarizes sender's costs)

#### 6.2.3 Profit Center Accounting (PCA)
- **Profit Centers:** Business segments responsible for revenue and costs
- Revenues + Costs → P&L per profit center
- Enables internal profitability management by business unit
- Document splitting ensures every posting is assigned a profit center

#### 6.2.4 Internal Orders
- Temporary cost collectors for specific tasks (office renovation, trade show, R&D project)
- Budget can be set with availability control (block/warn if exceeded)
- At period-end: **settled** — costs transferred to final cost objects (cost center, fixed asset, project)

#### 6.2.5 Product Costing (CO-PC)
- Calculates **standard cost** of a manufactured product
- Components: Material cost + Labor (activity rates × hours) + Overhead (overhead rates)
- **Cost Estimate:** Run before production to set standard price on material master
- **Production Order Variances:** Actual production cost vs. standard cost = variance
- **Variance Categories:** Price variance, quantity variance, lot size variance

#### 6.2.6 Profitability Analysis (CO-PA)
- Analyzes profitability by market segment (customer, product, sales region, channel)
- **Account-Based CO-PA (S/4HANA):** Uses GL accounts — fully reconciled with FI (mandatory in S/4HANA)
- **Costing-Based CO-PA (legacy):** Uses value fields — more flexible but not reconciled
- Revenue → Cost of Goods Sold → Gross Margin → Sales deductions → Contribution Margin

#### 6.2.7 Material Ledger (ML) — Mandatory in S/4HANA
- Records all material movements with quantity and value
- **Actual Costing:** At period-end, revalues inventory from standard cost to actual weighted average cost
- Eliminates cost distortions from standard costing
- Multi-currency support: materials valued in transaction currency, local currency, group currency
- Supports multi-valuation (IFRS, local GAAP, group) simultaneously

#### 6.2.8 Budgeting and Planning in CO
- **Cost Center Planning:** Plan expected costs per cost center/period
- **Availability Control:** Blocks or warns when actual spending exceeds budget
- **Top-Down Distribution:** High-level plan distributed to lower levels
- Integration with SAP Analytics Cloud for integrated business planning

---

## 7. SAP MM — Materials Management

### 7.1 Procurement Process
**Purchase Requisition (PR):** Internal request — "I need to buy X"
- Created manually or automatically by MRP
- Requires approval workflow

**Purchase Order (PO):** Formal commitment to vendor
- Contains: vendor, material, quantity, price, delivery date, payment terms
- Types: Standard, Subcontracting, Consignment, Framework/Contract

**Goods Receipt (GR):**
- Records physical receipt of goods against PO
- Accounting entry: Dr. Stock account / Cr. GR/IR clearing account
- Transaction: MIGO

**Invoice Verification (MIRO):**
- Vendor invoice matched to PO and GR
- Three-way match validation
- Accounting entry: Dr. GR/IR clearing / Cr. Vendor (AP)

**Vendor Payment:** AP payment run settles vendor invoice

### 7.2 Material Master
Central data record for every material. Has multiple **views** (organizational levels):
- Basic Data: Description, material group, weight
- Purchasing: Lead time, purchasing group
- MRP: MRP type, lot size, safety stock
- Accounting: Price control (standard or moving average), valuation class
- Sales: Sales unit, delivery plant

**Price Control:**
- **Standard Price (S):** Fixed planned cost — variances posted separately. Used for finished/semi-finished goods.
- **Moving Average Price (V):** Recalculated on each goods receipt. Used for raw materials.

### 7.3 Inventory Management
- **Goods Issue:** Remove stock from inventory (e.g., issue materials to production)
- **Transfer Posting:** Move stock between storage locations, plants, or change stock type
- **Physical Inventory:** Count physical stock vs. system stock; post differences
- **Special Stock:** Consignment stock, stock provided to vendor (subcontracting), project stock

### 7.4 Valuation and Account Determination
- MM is tightly integrated with FI
- Every goods movement triggers automatic FI posting via **automatic account determination** (configured in OBYC transaction)
- Valuation class on material master → G/L account assignment

---

## 8. SAP SD — Sales & Distribution

### 8.1 O2C Process in SAP SD
**Inquiry → Quotation → Sales Order → Delivery → Goods Issue → Billing → AR Clearing**

**Sales Order:**
- Captures customer, material, quantity, pricing, delivery date
- Checks: credit limit, material availability (ATP — Available to Promise)
- Drives delivery and billing

**Delivery:**
- Outbound delivery document created from sales order
- Picking (warehouse picks items), packing, shipping
- **Post Goods Issue (PGI):** Stock reduced, COGS posted → triggers revenue recognition timing

**Billing:**
- Invoice created from delivery
- Revenue recognized (Dr. AR / Cr. Revenue)
- Transfers to FI-AR as open item

**Pricing:**
- Condition technique: prices, discounts, surcharges calculated automatically
- Condition types: PR00 (base price), K007 (customer discount), MWST (tax)
- Pricing procedure determines sequence of condition types

### 8.2 Customer Master (Business Partner in S/4HANA)
Contains: address, payment terms, credit limit, sales area assignment, partner functions (sold-to, ship-to, bill-to, payer)

---

## 9. SAP Business Technology Platform (BTP)

### 9.1 What is SAP BTP?
SAP BTP (Business Technology Platform) is SAP's unified cloud platform providing services for:
- **Application Development & Automation** — build apps, automate processes
- **Integration** — connect SAP and non-SAP systems
- **Data & Analytics** — data management and analytics
- **AI** — AI/ML model lifecycle management
- **Database** — HANA Cloud, Datasphere

Think of BTP as the "operating system" on top of which customers extend, integrate, and innovate beyond the standard SAP applications.

### 9.2 BTP Account Structure
**Global Account:** Top-level entity — a customer's overall BTP contract and entitlements

**Subaccount:** Isolated environment within global account — for Dev, Test, Prod separation or different teams

**Space (Cloud Foundry):** Deployment space within a subaccount

**Entitlements:** Which services are authorized for which subaccounts (from the contract)

**Service Instance:** A provisioned instance of a specific BTP service (e.g., one HANA Cloud database, one Integration Suite tenant)

**Service Binding:** Credentials/connection details linking an application to a service instance

### 9.3 BTP Runtime Environments

**Cloud Foundry (CF):**
- Open-source application runtime
- Deploy Java, Node.js, Python, and other language apps
- Managed: SAP handles OS, containers, load balancing
- Uses: CAP (Cloud Application Programming model) apps, custom extensions

**Kyma:**
- Kubernetes-based runtime for containerized microservices
- More control, more complexity than CF
- Uses: Event-driven integrations, microservice extensions

**ABAP Environment:**
- Cloud-native ABAP development without managing infrastructure
- For developers extending SAP with ABAP in the cloud (RESTful ABAP Programming — RAP model)
- Clean Core compatible extensions

**Serverless (Functions):**
- Event-triggered lightweight functions
- Similar to AWS Lambda

### 9.4 SAP Integration Suite
SAP's cloud integration platform connecting SAP and non-SAP systems.

**Components:**
- **Cloud Integration (iFlow):** Design and run message-based integration flows — transform, route, filter messages between systems. Graphical design tool for integration flows (iFlows).
- **API Management:** Publish, secure, monitor, and analyze APIs. Rate limiting, OAuth, analytics.
- **Open Connectors:** 170+ pre-built connectors to third-party apps (Salesforce, Workday, ServiceNow)
- **Event Mesh:** Asynchronous, event-driven messaging — publish/subscribe model for business events
- **Integration Advisor:** AI-assisted message mapping and B2B standards
- **Trading Partner Management:** EDI-based B2B integration

**iFlow (Integration Flow):**
A graphical artifact defining how a message moves from sender to receiver — including:
- Message transformations (map fields from one format to another)
- Routing (conditional branching)
- Error handling
- Adapters (HTTPS, SFTP, OData, SOAP, JDBC, etc.)

### 9.5 SAP Cloud Connector
An on-premise agent (installed in customer's network) that:
- Creates a reverse SSH tunnel to BTP
- Allows BTP cloud apps to securely call on-premise systems (S/4HANA, SAP ECC, databases)
- No inbound firewall ports needed — tunnel is outbound-initiated

### 9.6 SAP Build (Low-Code/No-Code)
**SAP Build Apps (formerly AppGyver):**
- Visual, no-code app builder for mobile and web apps
- Connect to SAP and non-SAP data sources
- Target audience: citizen developers, business users

**SAP Build Process Automation:**
- Combines workflow management and RPA
- Design approval workflows visually (drag-and-drop)
- Automate desktop tasks with bots (desktop RPA)
- Pre-built automation content for SAP processes

**SAP Build Work Zone:**
- Digital workplace — unified launchpad aggregating SAP Fiori apps, third-party apps, SharePoint content
- Standard Edition: Fiori launchpad with basic enhancements
- Advanced Edition: Full employee experience portal with mobile (SAP Mobile Start), team spaces, content management

### 9.7 SAP AI on BTP

**SAP AI Core:**
- Service for training, deploying, and serving AI/ML models on BTP
- Supports any ML framework (TensorFlow, PyTorch, scikit-learn)
- Kubernetes-based — scalable compute for ML workloads
- Provides: training pipelines, model deployment as REST endpoints, resource scheduling

**SAP AI Launchpad:**
- UI layer over AI Core
- Manage AI scenarios, register connections to AI Core, monitor runs and deployments
- "Control tower" for AI model lifecycle across multiple AI Core instances

**SAP Generative AI Hub:**
- Access to multiple LLMs through a unified API on BTP
- Available models: GPT-4 (Azure OpenAI), Claude (Anthropic), Gemini (Google), SAP's own FLAN-T5 based models
- Enables building AI-powered apps without managing LLM infrastructure
- Includes: prompt management, content filtering, usage tracking

**SAP Joule:**
- SAP's generative AI assistant embedded across SAP products (S/4HANA, SuccessFactors, Ariba, SAP Analytics Cloud)
- Natural language interaction: "Show me open purchase orders over $100K" → Joule queries, summarizes, acts
- Context-aware: understands SAP business data and processes
- Built on Generative AI Hub

**Document Information Extraction (DOX):**
- AI service extracting structured data from unstructured documents (PDF invoices, delivery notes, purchase orders)
- Uses ML trained on millions of business documents
- Output: structured JSON (vendor name, invoice number, line items, amounts)
- Use case: Automate invoice capture in AP processing

**SAP AI Business Services:**
- Pre-built AI microservices for specific business tasks:
  - Business Entity Recognition (extract company names, amounts from text)
  - Recommendation (product/item recommendations)
  - Data Attribute Recommendation (classify master data)
  - Translation (business document translation)

### 9.8 SAP Business Application Studio (BAS)
- Cloud-based IDE (Integrated Development Environment) on BTP
- Replaces SAP Web IDE
- Browser-based — no local installation
- Dev spaces for specific development types:
  - SAP Fiori (frontend apps)
  - CAP (full-stack cloud apps)
  - SAP HANA (database development)
  - Mobile (SAP Mobile Services)
- Integrated with Git for source control

### 9.9 Cloud Application Programming Model (CAP)
SAP's framework for building cloud-native business applications on BTP:
- Define **data models** using CDS (Core Data Services)
- Define **service APIs** (OData, REST) declaratively
- Add **business logic** in JavaScript (Node.js) or Java
- Automatically generates boilerplate code, OpenAPI specs
- Database-agnostic — works with HANA Cloud, SQLite (dev), PostgreSQL
- Widely used for building S/4HANA extensions and BTP applications

### 9.10 SAP BTP Security Services

**SAP Authorization and Trust Management (XSUAA):**
- OAuth 2.0 / OpenID Connect-based identity service for BTP applications
- Manages: application roles, scopes, user assignments, token issuance
- Every BTP app uses XSUAA for authentication/authorization

**Identity Authentication Service (IAS):**
- SAP's cloud identity provider — central SSO hub
- Federates with corporate IdP (Azure AD, OKTA, ADFS)

**Destination Service:**
- Centrally stores connection configurations (URLs, credentials, auth settings) for reaching backend systems
- Apps reference destinations by name — credentials not hardcoded in apps

---

## 10. SAP Analytics Cloud (SAC)

### 10.1 What is SAC?
SAP Analytics Cloud is SAP's **unified cloud-based analytics platform** combining three capabilities in one:
1. **Business Intelligence (BI)** — reports, dashboards, data exploration
2. **Planning** — financial and operational planning with collaboration
3. **Predictive Analytics** — automated ML for forecasting and classification

Replaces on-premise tools: BusinessObjects, BPC (Business Planning and Consolidation), Lumira

### 10.2 SAC Data Connectivity

**Live Connection:**
- SAC queries data directly from source in real time — no data copy
- Source does all computation
- Supported sources: SAP S/4HANA (via BW queries or CDS views), SAP HANA, SAP BW/4HANA
- Always shows latest data; no refresh lag
- Limitation: source performance affects SAC performance; limited modeling in SAC

**Import Connection:**
- Data physically copied into SAC's in-memory store
- SAC handles computation — fast self-service
- Scheduled refresh (hourly, daily)
- Supports more sources: SQL databases, files, third-party systems
- Limitation: data is a point-in-time snapshot, not real-time

### 10.3 SAC Stories
**Stories** are the primary content type — interactive dashboards and reports combining:
- Charts (bar, line, waterfall, scatter, geo map, etc.)
- Tables (crosstabs with drill-down)
- KPI tiles (value, trend indicator, comparison)
- Text elements, images, filters
- **Linked Analysis:** Select a data point in one chart → all related charts filter (cross-filtering)
- **Drill-down:** Click a summary → see detail
- Can embed input controls (sliders, dropdowns) for interactive filtering

### 10.4 SAC Planning
- Create **planning models** with account dimension, time dimension, version dimension
- **Versions:** Actuals (real data from ERP), Budget (authorized plan), Forecast (rolling update)
- **Input forms:** Finance/business users enter plan data directly in SAC
- **Data Actions:** Automated planning calculations (copy actuals to budget, spread annual to monthly)
- **Multi-Actions:** Chain multiple data actions into a workflow
- **Predictive Planning (Smart Predict):** ML model auto-forecasts within planning model

### 10.5 SAC Smart Features (AI/ML)

**Smart Insights:** Click a chart element → AI explains what's driving that value (top influencing factors, anomalies vs. expectation)

**Smart Predict:** Build predictive models directly in SAC:
- **Time Series Forecast:** Predict future values of a metric (revenue next 6 months)
- **Classification:** Predict a category (will this customer churn? Yes/No)
- **Regression:** Predict a continuous value (what price will maximize margin?)
- No data science expertise required — automated ML pipeline

**Just Ask (Natural Language Query):** Type a business question in plain English → SAC generates a chart. Example: "Show me revenue by region for Q3 2025 compared to Q3 2024"

**Search to Insight:** Similar to Just Ask — type in a search bar

### 10.6 SAC Administration
- **Tenant:** Each organization has its own SAC tenant (URL: companyname.analytics.sapcloud.cn)
- **Roles:** Admin, BI Creator, Planner Expert, Viewer — control what users can do
- **Teams:** Group users for content sharing
- **Content Network:** Pre-built story templates and packages from SAP/partners

### 10.7 Analytic Application (Analytics Designer)
- Scripted, highly customized analytics apps
- Uses JavaScript-like scripting language
- Dynamic behaviors: show/hide panels, conditional formatting based on data, custom navigation
- Used when standard Stories don't provide enough flexibility

---

## 11. SAP Datasphere

### 11.1 What is SAP Datasphere?
Previously called **SAP Data Warehouse Cloud (DWC)**. Renamed Datasphere in 2023.

SAP Datasphere is SAP's **cloud-native data management platform** providing a unified environment for:
- Data integration and transformation
- Data modeling with business semantics
- Data governance and access control
- Federation (query data in-place without moving it)
- Data marketplace

**Core Vision — Business Data Fabric:** Connect data from any source (S/4HANA, non-SAP, cloud, on-premise), apply business meaning (semantic layer), and make it available for analytics — without necessarily centralizing all data.

### 11.2 Datasphere Architecture Concepts

**Spaces:**
- Isolated workspaces within a Datasphere tenant
- Each space: own storage quota, compute quota, users, connections
- Used to separate domains: Finance Space, Supply Chain Space, HR Space
- Data sharing between spaces is controlled

**Data Layer (Technical Layer):**
- Remote Tables: Virtual tables pointing to source data — no data copied
- Local Tables: Physical data stored in Datasphere (imported/replicated)
- Views: SQL or graphical transformations combining/transforming data
- Data Flows: ETL-style pipelines loading data between sources and targets
- Replication Flows: High-performance real-time/batch replication from SAP sources

**Business Layer (Semantic Layer):**
- **Dimensions:** Descriptive entities (Products, Customers, Time) — business concepts
- **Facts:** Measurable events (Sales Transactions, Journal Entries)
- **Analytic Models:** Combine facts with dimensions, define measures, hierarchies, filters
- **Business Entities:** Reusable semantic objects shared across spaces
- **Perspectives:** Pre-configured slices of analytic models for specific user groups

### 11.3 Datasphere Key Features

**Federation:**
- Query external data sources in-place without copying data
- Supported sources: SAP HANA Cloud, SAP S/4HANA, SAP BW, Azure, Snowflake, databases
- Uses HANA Smart Data Integration / Smart Data Federation

**Data Marketplace:**
- Share and consume data products within the organization
- Internal data products: Finance publishes a sales dataset; Supply Chain consumes it
- Third-party data: Dun & Bradstreet company data, weather data, etc.

**SAP BW Bridge:**
- Managed BW/4HANA environment within Datasphere
- Run existing BW objects (InfoProviders, DSOs, BEx queries) without a separate BW system
- Migration path from legacy BW to Datasphere

**Open SQL Schemas:**
- Allow external tools (Python, third-party BI, data science) to access Datasphere data via standard SQL
- Enables ML engineers to query Datasphere from Jupyter notebooks, Databricks, etc.

**Data Access Control (DAC):**
- Row-level security based on user identity
- Example: User in "Europe" sales group sees only European data

**Transformation Flow:**
- ETL within Datasphere — read from source, apply SQL transformations, write to target table
- Similar to DBT or traditional ETL

### 11.4 SAC + Datasphere Integration
SAC connects to Datasphere via **live connection** — SAC queries Datasphere Analytic Models directly. This is SAP's strategic analytics architecture:
- **Datasphere** = governed, business-context-rich data layer
- **SAC** = analytics and planning front-end
- Together = "Business Data Fabric" end-to-end analytics

---

## 12. SAP BW/4HANA

### 12.1 What is SAP BW/4HANA?
SAP BW (Business Warehouse) is SAP's data warehouse product. BW/4HANA is the latest version — a simplified, HANA-optimized data warehouse.

**BW vs. Datasphere:** BW/4HANA is a traditional data warehouse (import data, model it, report from it). Datasphere is a modern data fabric (federation-first, business semantics, broader connectivity). SAP is positioning Datasphere as the future; BW/4HANA continues for existing customers.

### 12.2 Key BW Concepts
- **InfoObjects:** Master data objects (characteristics and key figures)
- **InfoProvider:** Data containers (DSO, InfoCube, Composite Provider, HANA view)
- **DSO (DataStore Object):** Stores detailed data; processes change log for delta updates
- **InfoCube / ADSO:** Multidimensional data models for reporting
- **BEx (Business Explorer):** Traditional SAP reporting tool for BW queries
- **ODP (Operational Data Provisioning):** Modern extraction framework from SAP sources to BW or Datasphere

---

## 13. SAP Embedded Analytics in S/4HANA

### 13.1 What is Embedded Analytics?
Real-time reporting built directly into S/4HANA — no separate BW/BO system needed for operational reporting.

### 13.2 CDS Views (Core Data Services)
- **CDS Views** are semantic data models defined in ABAP/SQL that expose S/4HANA data
- Defined directly on the database layer (pushed down to HANA)
- Types:
  - **Basic Interface Views:** Raw data access
  - **Composite Views:** Join multiple basic views
  - **Consumption Views:** Optimized for UI/analytics consumption
- Add metadata: field labels, aggregations, associations, access controls
- Exposed as: OData services (for Fiori), analytical queries (for embedded reporting), APIs

### 13.3 SAP Fiori Analytical Apps
- Fiori apps built on CDS analytical views
- Real-time operational KPIs — no BW needed
- Examples: Accounts Payable Overview, Overdue Receivables, Inventory Turnover

### 13.4 SAP Signavio (Process Intelligence)
- SAP's process mining and process management tool (acquired 2021)
- Analyzes S/4HANA event logs to discover actual process flows
- Identifies deviations, bottlenecks, compliance gaps
- Integration with SAP Activate — analyze as-is processes before transformation

---

## 14. SAP Master Data Governance (MDG)

### 14.1 What is MDG?
SAP Master Data Governance is an application for **centrally managing and governing master data** across an enterprise.

**Key Capabilities:**
- **Consolidation:** Detect and merge duplicates across systems
- **Mass Processing:** Bulk changes to master data records
- **Change Requests:** Workflow-driven approval process for creating/changing master data
- **Validation Rules:** Enforce data quality rules before master data is activated
- **Data Replication:** Distribute approved master data to target systems (ERP, MDM, legacy)
- **Edition-based staging:** Changes prepared, reviewed, and activated in a controlled way

**Domains:** Material, Customer/Supplier (Business Partner), Finance (GL accounts, cost centers), Custom domains

---

## 15. SAP Basis Administration

### 15.1 What is SAP Basis?
SAP Basis (now called SAP Technology) is the technical foundation and administration layer of SAP systems.

**Basis Administrator responsibilities:**
- System installation and upgrades
- Transport management (moving changes DEV → QAS → PRD)
- User and authorization management
- System performance monitoring and tuning
- Background job scheduling
- System landscape management
- Client management
- System refresh (copy PRD to QAS for testing)

### 15.2 Key Basis Concepts
- **Client:** A self-contained unit within an SAP system — has its own master data, transactional data, users. Multiple clients can exist in one SAP system (e.g., 100 = Production, 200 = Test, 300 = Training)
- **ABAP (Advanced Business Application Programming):** SAP's proprietary programming language used to develop custom programs, reports, enhancements
- **BAPI (Business Application Programming Interface):** Standard SAP function modules exposed for programmatic access — used in integrations and data migration
- **RFC (Remote Function Call):** SAP's protocol for calling function modules in remote SAP systems
- **Batch Jobs:** Background programs scheduled to run automatically (e.g., payment run, depreciation run, MRP run)

---

## 16. SAP Authorization Concept

### 16.1 How SAP Security Works
**Authorization Object:** Defines what you can do (e.g., display vs. change) on which data (e.g., which company code)

**Authorization Field:** A specific criterion within an authorization object (e.g., company code = 1000, activity = 03 Display)

**Profile:** A collection of authorizations

**Role (Activity Group):**
- Contains authorization profiles + menu entries
- Assigned to users
- **Single Role:** For a specific activity (e.g., Create Vendor Invoice)
- **Composite Role:** Bundles of single roles for a job function

**User Master:** SAP user account — has roles assigned, logon data, validity dates

### 16.2 Segregation of Duties (SoD)
Critical compliance requirement — no user should have conflicting authorizations:
- Cannot both create and approve a purchase order
- Cannot both create a vendor and process payment to that vendor
- Monitored using SAP GRC (Governance, Risk & Compliance) Access Control

### 16.3 SAP GRC (Governance, Risk and Compliance)
- **Access Control:** SoD conflict detection, emergency access (firefighter), user access review
- **Process Control:** Continuous monitoring of internal controls
- **Risk Management:** Enterprise risk tracking

---

## 17. SAP and AI — The Intelligent Enterprise

### 17.1 SAP's AI Strategy
SAP's vision is the **Intelligent Enterprise** — where AI and automation are embedded throughout business processes, enabling companies to sense, adapt, and act in real time.

**Three pillars:**
1. **Intelligent Suite:** AI embedded in S/4HANA, SuccessFactors, Ariba, Concur
2. **Business Technology Platform:** AI services (AI Core, Generative AI Hub) for custom AI
3. **Intelligent Spend Group:** AI in procurement, travel, HR

### 17.2 AI Use Cases in SAP

**Finance (S/4HANA Finance):**
- Invoice capture via Document Information Extraction
- Payment term optimization recommendations
- Cash flow prediction
- Automatic journal entry suggestions
- Anomaly detection in GL postings

**Procurement (SAP Ariba):**
- Supplier risk scoring
- Spend classification
- Contract deviation detection

**Supply Chain:**
- Demand forecasting (ML-based)
- Inventory optimization
- Quality prediction in production

**HR (SuccessFactors):**
- Job description recommendations
- Candidate matching
- Learning path recommendations

### 17.3 SAP Joule — Generative AI Assistant
- Natural language interface across all SAP products
- "What are my top 10 open purchase orders?"
- "Summarize this contract and identify key risks"
- Powered by SAP's Generative AI Hub (accesses multiple LLMs)
- Context-aware of SAP business objects and user's authorizations
- Can both surface information AND execute actions

### 17.4 Clean Core Strategy
A critical SAP architectural philosophy:

**Problem:** Traditional SAP implementations had heavy custom code modifying standard SAP programs. This made upgrades extremely expensive and time-consuming.

**Clean Core Principle:**
- Keep the S/4HANA core unmodified (no modifications to standard SAP code)
- Move all customizations to:
  - **BTP side-by-side extensions** (separate apps on BTP connecting via API)
  - **Key User Extensibility** (in-app customization tools that survive upgrades)
  - **ABAP Cloud (RAP model)** — custom ABAP using only released SAP APIs

**Benefits:**
- Faster, cheaper upgrades (no regression testing of custom code)
- Cloud-ready (public cloud requires clean core)
- Access to SAP innovations (AI, new Fiori apps) without conflict

**Key User Extensibility Tools:**
- Custom Fields and Logic (add fields to SAP objects)
- Custom Business Objects (create new objects)
- Business Rules Framework plus (BRFplus) — rule-based decision logic
- In-App Extensibility in Fiori apps

---

## 18. SAP RISE and SAP GROW

### 18.1 RISE with SAP
SAP's commercial offering for large enterprises migrating to S/4HANA Cloud.

**Includes:**
- S/4HANA Cloud (Private or Public Edition)
- SAP BTP entitlements
- SAP Business Process Intelligence (Signavio)
- SAP Business Network Starter Pack
- Managed infrastructure (SAP or hyperscaler)
- Technical migration support

**Value:** One contract, one vendor, end-to-end cloud transformation

### 18.2 GROW with SAP
SAP's offering for **new SAP customers** (mid-market) implementing S/4HANA Cloud Public Edition.
- Focuses on fast time-to-value
- Pre-configured best practices
- Strong partner ecosystem

---

## 19. SAP Terminology Quick Reference

| Term | Meaning |
|---|---|
| ABAP | Advanced Business Application Programming — SAP's programming language |
| ACDOCA | Universal Journal table in S/4HANA |
| BAPI | Business Application Programming Interface |
| BAS | Business Application Studio (cloud IDE on BTP) |
| BTP | Business Technology Platform |
| CAP | Cloud Application Programming model |
| CDS | Core Data Services — semantic data models |
| COA | Chart of Accounts |
| CO-PA | Controlling — Profitability Analysis |
| DAC | Data Access Control (row-level security in Datasphere) |
| ECC | SAP ERP Central Component (legacy ERP, predecessor to S/4HANA) |
| EOS | End of Standard support |
| FIORI | SAP's UX framework and design language |
| GRC | Governance, Risk & Compliance |
| HANA | High-performance ANalytic Appliance (SAP's in-memory database) |
| IBP | Integrated Business Planning (SAP's supply chain planning) |
| IFRS | International Financial Reporting Standards |
| ISSU | In-Service Software Upgrade |
| LSMW | Legacy System Migration Workbench (data migration tool) |
| MDG | Master Data Governance |
| MIGO | SAP transaction for goods movements |
| MIRO | SAP transaction for invoice verification |
| ML | Material Ledger |
| NETCONF | Network Configuration Protocol (also: Network Configuration in S/4HANA context) |
| NOC | Network Operations Center |
| ODATA | Open Data Protocol — REST-based API standard used in Fiori |
| RAP | RESTful ABAP Programming model |
| RFC | Remote Function Call |
| RISE | SAP's cloud transformation commercial offering |
| SAC | SAP Analytics Cloud |
| SID | System Identification (3-character SAP system identifier) |
| SoD | Segregation of Duties |
| SPF | Shortest Path First (OSPF algorithm) |
| STMS | SAP Transport Management System |
| XSUAA | Extended Services for User Account and Authentication (BTP security) |
| ZTP | Zero Touch Provisioning |

---

## 20. SAP OA Exam Strategy — Key Themes to Remember

### Things SAP OA Questions Always Test:

**1. Purpose differentiation:** Know exactly what each product/module does vs. what it doesn't do.
- SAC = analytics and planning
- Datasphere = data management and governance
- BTP = platform for extensions, integration, AI
- S/4HANA = core ERP transactions and processes
- FICO = finance; MM = procurement; SD = sales

**2. S/4HANA simplifications vs. ECC:**
- Universal Journal (ACDOCA) replaces multiple FI and CO tables
- Business Partner replaces separate Customer and Vendor masters
- Material Ledger mandatory
- No more FI-CO reconciliation needed
- Embedded analytics eliminate need for separate BW for operational reporting

**3. SAP Activate phases:** Discover → Prepare → Explore → Realize → Deploy → Run

**4. Clean Core:** Never modify standard SAP code; use BTP extensions, Key User Extensibility, or ABAP Cloud

**5. HANA architecture:** In-memory + columnar storage = no pre-aggregation needed = real-time analytics

**6. Three-way match in P2P:** PO + GR + Invoice must all match before payment approved

**7. RISE with SAP:** A commercial bundling, not a product. Includes S/4HANA Cloud + BTP + Signavio + managed infrastructure.

**8. Fiori principles:** Role-based, responsive, simple, coherent, delightful. Tiles on Launchpad. OData backend.

**9. Document principle:** Every transaction creates an immutable document. Errors are corrected by reversal, not deletion.

**10. Parallel ledgers:** Multiple accounting standards maintained simultaneously in ACDOCA (IFRS as leading ledger, local GAAP as non-leading).

---

*End of SAP Comprehensive Notes — approximately 6,500 words of in-depth SAP concepts.*
*Read alongside: ERP_Comprehensive_Notes.md for ERP foundations and business process context.*
