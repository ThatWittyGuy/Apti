# ERP — Comprehensive Study Notes
### For Corning OA Prep | All 6 JDs
> Covers: What is ERP, history, architecture, core modules, business processes, implementation, digital transformation, governance, and modern trends.

---

## 1. What is ERP?

**Enterprise Resource Planning (ERP)** is a category of integrated business management software that connects and manages an organization's core business processes through a **single, unified database and system**. Instead of separate, disconnected software for each department, ERP gives every function — finance, HR, procurement, manufacturing, sales, supply chain — visibility into the same real-time data.

**Core Idea:** One system, one database, one source of truth for the entire enterprise.

**Why ERP exists:** Before ERP, companies used siloed systems (a different tool for accounting, another for inventory, another for payroll). These systems couldn't talk to each other, leading to:
- Duplicate data entry
- Inconsistent numbers across departments
- Slow reporting (manual consolidation of spreadsheets)
- Poor decision-making due to lack of real-time data

ERP solves all of this by integrating everything.

---

## 2. Brief History of ERP

| Era | Development |
|---|---|
| 1960s | MRP (Material Requirements Planning) — inventory control for manufacturers |
| 1970s–80s | MRP II — added capacity planning, shop floor control |
| 1990s | ERP coined by Gartner — extended to finance, HR, CRM beyond manufacturing; SAP R/3 launches (1992) |
| 2000s | ERP goes web-enabled; best-of-breed vs. suite debate |
| 2010s | Cloud ERP (SaaS) emerges; mobile, analytics integration |
| 2020s | Intelligent ERP — AI, ML, automation, IoT embedded into ERP; SAP S/4HANA, Oracle Fusion, Microsoft Dynamics 365 |

---

## 3. ERP Architecture

### 3.1 Traditional (On-Premise) Architecture
- Software installed on company's own servers
- Company manages hardware, database, upgrades, security
- High upfront cost (CapEx), high control
- Example: SAP ECC installed on-premise

### 3.2 Cloud ERP Architecture
**Three deployment models:**

**SaaS (Software as a Service):**
- Fully managed by vendor (Microsoft Dynamics 365, SAP S/4HANA Cloud Public Edition)
- Subscription-based (OpEx), automatic upgrades
- Limited customization — "fit-to-standard" approach
- Fastest to implement, lowest TCO for standard processes

**Private Cloud:**
- ERP runs on dedicated cloud infrastructure (managed by vendor or partner)
- More customization than public cloud
- Example: SAP S/4HANA on AWS/Azure dedicated tenant

**Hybrid:**
- Mix of on-premise (core, sensitive data) + cloud (edge functions, subsidiaries)
- Most large enterprises during transition phase

### 3.3 Multi-tier Architecture
Modern ERP typically has:
- **Presentation layer** — UI (web browser, mobile, SAP Fiori)
- **Application layer** — business logic, processing
- **Database layer** — data storage (SAP HANA, Oracle, SQL Server)

### 3.4 Integration Architecture
ERP integrates with:
- **CRM** (Customer Relationship Management — Salesforce, SAP CX)
- **SCM** (Supply Chain Management — SAP IBP)
- **HCM** (Human Capital Management — SAP SuccessFactors, Workday)
- **BI/Analytics** (SAP Analytics Cloud, Power BI)
- **EDI** (Electronic Data Interchange — for vendor/customer transactions)
- **E-commerce platforms, banking systems, tax engines**

Integration happens via:
- **APIs** (REST, SOAP, OData)
- **Middleware** (SAP Integration Suite, MuleSoft, Dell Boomi)
- **File-based** (EDI, flat files — legacy)
- **Event-driven** (Kafka, SAP Event Mesh)

---

## 4. Core ERP Modules

Every ERP has modules covering core business domains. Here are the universal ones:

### 4.1 Financial Accounting (FI)
Records all financial transactions for external reporting.
- **General Ledger (GL):** Central accounting record; all transactions post here
- **Accounts Payable (AP):** Money owed to vendors; invoice processing, payments
- **Accounts Receivable (AR):** Money owed by customers; billing, collections
- **Asset Accounting (AA):** Fixed assets — acquisition, depreciation, retirement
- **Bank Accounting:** Bank reconciliation, cash management
- **Tax Management:** VAT, GST, withholding tax

**Key Output:** Balance Sheet, Income Statement (P&L), Cash Flow Statement

### 4.2 Controlling / Management Accounting (CO)
Internal cost management — not for external reporting.
- **Cost Center Accounting:** Track costs by department (IT, Marketing, HR)
- **Profit Center Accounting:** Track P&L by business segment
- **Internal Orders:** Collect costs for specific events/projects
- **Product Costing:** Calculate cost to manufacture a product
- **Profitability Analysis (CO-PA):** Margin analysis by customer, product, region
- **Budgeting & Planning:** Set cost budgets with availability control

### 4.3 Materials Management (MM)
Covers procurement and inventory.
- **Purchase Requisition (PR):** Internal request to buy something
- **Purchase Order (PO):** Formal order to vendor
- **Goods Receipt (GR):** Recording goods received from vendor
- **Invoice Verification:** Matching vendor invoice to PO and GR (3-way match)
- **Inventory Management:** Stock levels, movements, valuation
- **Vendor Master:** Centralized vendor data

**Key Process:** Procure-to-Pay (P2P)

### 4.4 Sales & Distribution (SD)
Covers the sales cycle.
- **Sales Order:** Customer's request to buy
- **Delivery / Shipping:** Outbound goods movement
- **Billing / Invoicing:** Customer invoice generation
- **Pricing:** Condition-based pricing (discounts, surcharges)
- **Credit Management:** Credit limit checks
- **Customer Master:** Centralized customer data

**Key Process:** Order-to-Cash (O2C)

### 4.5 Production Planning (PP)
Manufacturing execution.
- **Bills of Materials (BOM):** What components make up a product
- **Routing:** How to manufacture (steps, work centers)
- **Production Orders:** Execute manufacturing
- **MRP (Material Requirements Planning):** Calculate what materials needed, when
- **Capacity Planning:** Ensure machines/labor available

### 4.6 Human Resources / HCM
- **Organizational Management:** Org structure, reporting lines
- **Personnel Administration:** Employee master data
- **Payroll:** Salary calculation, tax deductions
- **Time Management:** Attendance, leave
- **Recruitment, Performance, Learning** (often in separate cloud HCM)

### 4.7 Project Systems (PS)
- Project creation, WBS (Work Breakdown Structure)
- Budget, cost collection, milestone billing
- Integrated with FI, CO, MM, SD

### 4.8 Quality Management (QM)
- Inspection plans, quality certificates
- Defect recording, notifications

### 4.9 Plant Maintenance (PM)
- Equipment master, maintenance plans
- Work orders for repairs

---

## 5. Core Business Processes in ERP

ERP organizes work around **end-to-end business processes** that cut across multiple modules:

### 5.1 Procure-to-Pay (P2P)
**What:** Everything from identifying a purchasing need to paying the vendor.

**Steps:**
1. **Purchase Requisition (PR)** — department requests goods/services
2. **Approval** — PR approved by manager
3. **Request for Quotation (RFQ)** — if needed, vendors submit bids
4. **Purchase Order (PO)** — formal order issued to vendor
5. **Goods Receipt (GR)** — warehouse records goods received (inventory ↑, GR/IR ↑)
6. **Invoice Receipt (IR)** — vendor invoice arrives; 3-way match (PO + GR + Invoice)
7. **Payment** — payment run processes invoice; bank payment made
8. **Accounting entries** — AP cleared, bank account reduced

**Key ERP tables/accounts:** GR/IR clearing account, vendor open items, AP subledger

### 5.2 Order-to-Cash (O2C)
**What:** Everything from receiving a customer order to collecting payment.

**Steps:**
1. **Sales Inquiry / Quotation** — customer asks for quote
2. **Sales Order** — customer places order; credit check
3. **Delivery** — warehouse picks, packs, ships goods (inventory ↓)
4. **Goods Issue** — inventory movement records cost of goods sold
5. **Customer Invoice (Billing)** — invoice sent to customer (AR ↑, Revenue ↑)
6. **Payment Receipt** — customer pays; cash received
7. **Clearing** — customer open item cleared in AR

### 5.3 Record-to-Report (RTR)
**What:** Everything from recording transactions to producing financial statements.

**Steps:**
1. **Transaction Recording** — all business events posted to GL (via P2P, O2C, payroll, etc.)
2. **Sub-ledger Reconciliation** — AR, AP, AA reconciled to GL
3. **Accruals & Adjustments** — expenses/revenues not yet posted in cash recorded as accruals
4. **Intercompany Reconciliation** — eliminate IC transactions before consolidation
5. **Period-End Close** — depreciation runs, allocations, cost settlements, GR/IR clearing
6. **Trial Balance** — verify total debits = total credits
7. **Financial Reporting** — Balance Sheet, P&L, Cash Flow
8. **Consolidation** — combine all legal entities into group financials
9. **Period Lock** — close accounting period; no more postings allowed

**Key RTR metrics:**
- Close cycle time (days to close after period end)
- Number of manual journal entries (more = less automated)
- Reconciliation items outstanding

### 5.4 Hire-to-Retire (H2R)
Hire → Onboard → Manage → Pay → Offboard employee

### 5.5 Plan-to-Produce (P2P Manufacturing)
Demand planning → Production planning → MRP → Production order → Manufacturing → Quality check → Goods receipt to stock

### 5.6 Acquire-to-Retire (A2R)
Asset acquisition → Capitalization → Depreciation → Disposal/retirement

---

## 6. ERP Data Model Fundamentals

### 6.1 Master Data vs. Transactional Data
**Master Data:** Stable, reusable reference data that drives transactions.
- Vendor Master (name, bank details, payment terms)
- Customer Master (address, credit limit, payment terms)
- Material Master (description, weight, price, MRP settings)
- Employee Master (name, position, salary)
- Chart of Accounts / GL Account Master
- Cost Center, Profit Center, Business Partner

**Transactional Data:** Event-driven records generated when business activities occur.
- Purchase Orders, Sales Orders
- Journal Entries, Invoices
- Production Orders, Deliveries, Payments

**Why it matters:** Poor master data = poor ERP. "Garbage in, garbage out."

### 6.2 Organizational Structure in ERP
ERP mirrors the company's legal and operational structure:

| Level | Concept | Description |
|---|---|---|
| Global | Client | The entire ERP system instance |
| Legal | Company Code | A legal entity with own financial statements |
| Operational | Plant | Manufacturing/distribution location |
| Sales | Sales Organization | Sales entity (may cross company codes) |
| Finance | Controlling Area | Management accounting boundary |
| Warehouse | Storage Location | Physical storage area within plant |

**Example:** Corning Inc. (Client) → Corning Incorporated USA (Company Code) → Corning NY Plant (Plant) → Warehouse A (Storage Location)

### 6.3 Document Principle
Every business event in ERP creates a **document** with:
- Document number (unique)
- Document type (classifying what kind of transaction)
- Posting date, fiscal year
- Line items (debit/credit entries)
- Reference fields (linking to source documents)

Documents are **immutable** — you don't delete, you reverse.

---

## 7. ERP Implementation

### 7.1 Implementation Approaches

**Greenfield (New Implementation):**
- Start fresh — new system, new processes
- Opportunity to adopt best practices / standard processes
- No legacy customizations carried forward
- More effort for data migration and change management
- Best for: moving to cloud ERP, major business transformation

**Brownfield (System Conversion):**
- Convert existing ERP (e.g., SAP ECC) to new version (SAP S/4HANA)
- Historical data and configurations migrated
- Faster, lower risk, less change management
- Risk of carrying forward technical debt
- Best for: companies with stable processes wanting to upgrade

**Bluefield / Selective Data Transition:**
- Hybrid: selectively migrate chosen data and processes
- Combine the best of greenfield (clean new processes) with brownfield (retain needed history)

### 7.2 Implementation Methodology: SAP Activate
SAP's agile implementation methodology with 6 phases:

**1. Discover** — Understand scope, assess readiness, access SAP Best Practices content

**2. Prepare** — Project kick-off, team setup, infrastructure provisioning, initial configuration

**3. Explore** — Fit-to-Standard workshops: run business processes in standard SAP, identify gaps, document decisions (no customization yet)

**4. Realize** — Configure, build, test iteratively in sprints:
- System configuration based on workshop decisions
- Custom development (only for justified gaps)
- Unit testing, integration testing
- Data migration preparation

**5. Deploy** — Final preparation for go-live:
- User Acceptance Testing (UAT) — business users validate
- End-user training
- Data migration cutover
- Go/no-go decision

**6. Run** — Post-go-live support, hypercare, continuous improvement

### 7.3 System Landscape
A standard SAP landscape has three environments:

| System | Purpose |
|---|---|
| DEV (Development) | Developers configure and build |
| QAS (Quality/Test) | Testing, UAT, regression testing |
| PRD (Production) | Live business operations |

Changes flow: **DEV → QAS → PRD** via **Transport Requests** (packages of configuration changes moved through the landscape).

### 7.4 Key Implementation Roles
- **Project Sponsor:** Executive owning the project
- **Project Manager:** Manages scope, timeline, budget
- **Business Process Owner:** Accountable for a process area
- **Functional Consultant:** Configures ERP (e.g., FICO, MM specialist)
- **Technical Consultant/Developer:** Custom development, integrations
- **Basis Administrator:** System administration, transports, performance
- **Change Manager:** User adoption, training, communications
- **Data Migration Lead:** Cleansing and migrating legacy data
- **Business Users/Key Users:** Subject matter experts, UAT testers

### 7.5 Testing in ERP Implementation
- **Unit Testing:** Individual configuration items tested in isolation
- **Integration Testing:** End-to-end process testing (e.g., full P2P cycle)
- **User Acceptance Testing (UAT):** Business users validate against requirements
- **Regression Testing:** Ensure changes don't break existing functionality
- **Performance Testing:** System handles expected transaction volumes
- **Cutover Testing:** Rehearse data migration and go-live steps

### 7.6 Data Migration
Moving data from legacy systems to new ERP:
1. **Extract** data from legacy system
2. **Transform/cleanse** — fix quality issues, map to new format
3. **Load** into ERP using migration tools (SAP Migration Cockpit, LSMW, BAPI)
4. **Validate** — reconcile counts and values

Migration objects: Vendor masters, Customer masters, Open items (invoices, POs), GL balances, Asset masters, Material masters

---

## 8. ERP Governance & Controls

### 8.1 Segregation of Duties (SoD)
No single person should control all phases of a transaction:
- Person who creates a vendor ≠ person who approves a PO ≠ person who processes payment
- Enforced through ERP role-based access control (RBAC)
- Violations create fraud risk and audit findings

### 8.2 Role-Based Access Control (RBAC)
- Users are assigned roles (collections of authorizations)
- Roles define what transactions they can execute, what data they can see
- Principle of least privilege: only minimum access needed

### 8.3 Audit Trail
ERP maintains complete audit trail:
- Every document has created by/changed by/date
- Change logs for master data
- Document reversal history
- Critical for financial audits (SOX compliance at public companies)

### 8.4 SOX Compliance (Sarbanes-Oxley)
US law requiring public companies to:
- Maintain adequate internal controls over financial reporting
- Test and certify those controls annually
- ERP controls (SoD, authorization, audit trail) are central to SOX compliance

### 8.5 IT General Controls (ITGC)
- Access management (user provisioning, termination)
- Change management (transport controls)
- Backup and recovery
- System availability
- All audited annually for SOX

---

## 9. ERP KPIs and Reporting

### 9.1 Finance KPIs
- **Days Payable Outstanding (DPO):** Avg days to pay vendors — longer = better cash position
- **Days Sales Outstanding (DSO):** Avg days to collect from customers — shorter = better
- **Days Inventory Outstanding (DIO):** Avg days inventory held
- **Cash Conversion Cycle (CCC):** DIO + DSO − DPO — measures working capital efficiency
- **Close Cycle Time:** Days to complete month-end close
- **Cost per Invoice Processed:** Efficiency of AP

### 9.2 Procurement KPIs
- PO cycle time
- Supplier on-time delivery
- Maverick spending (off-contract purchases)
- 3-way match rate

### 9.3 Supply Chain KPIs
- Inventory turnover
- Fill rate
- Perfect order rate

---

## 10. ERP Digital Transformation

### 10.1 Why ERP Transformation?
Companies undertake ERP transformation to:
- Modernize aging legacy systems (SAP ECC end-of-maintenance 2027, extended to 2030)
- Adopt cloud and reduce infrastructure costs
- Enable real-time analytics and AI
- Standardize processes across global operations
- Enable new business models

### 10.2 Digital Finance Transformation
Within finance specifically:
- **Continuous Accounting:** Distribute close activities throughout the month rather than a month-end rush
- **Financial Close Automation:** RPA + AI automating journal entries, reconciliations, report generation
- **Real-time Reporting:** Eliminate batch runs — finance has live data at any moment
- **Predictive Analytics:** ML forecasting cash flow, sales, expenses
- **Process Mining:** Analyze actual process execution from ERP event logs

### 10.3 Intelligent ERP / AI in ERP
Modern ERP embeds AI for:
- **Intelligent invoice processing:** OCR + ML extracting data from vendor invoices
- **Anomaly detection:** Flagging unusual transactions for fraud/error review
- **Demand forecasting:** ML predicting future sales/inventory needs
- **Predictive maintenance:** IoT + ML predicting equipment failures
- **Chatbots/virtual assistants:** Natural language queries for business data
- **Robotic Process Automation (RPA):** Bots handling repetitive transactional tasks

### 10.4 RPA in ERP Context
RPA bots automate tasks like:
- Entering vendor invoices from email into ERP
- Running standard reports and emailing them
- Reconciling GL accounts
- Creating purchase orders from requisitions
- Account statement downloads from banking portals

RPA tools: UiPath, Automation Anywhere, SAP Build Process Automation

### 10.5 Process Mining
- Extracts event logs from ERP (SAP, Oracle)
- Visualizes actual process flows (not the intended flowchart)
- Quantifies: average cycle time, rework loops, bottlenecks, compliance deviations
- Tools: Celonis, SAP Signavio, IBM Process Mining

### 10.6 Cloud ERP Benefits vs. Risks
**Benefits:**
- No infrastructure management
- Automatic updates (always current)
- Scalability (scale up/down as needed)
- Lower upfront CapEx → OpEx subscription
- Built-in best practices

**Risks:**
- Less customization flexibility
- Data residency/privacy concerns
- Vendor lock-in
- Internet dependency
- Integration complexity with on-premise systems

---

## 11. ERP Vendor Landscape

| Vendor | Product | Target |
|---|---|---|
| SAP | S/4HANA (on-premise/cloud), ECC (legacy) | Large enterprise |
| Oracle | Oracle Fusion Cloud ERP, Oracle EBS | Large/mid enterprise |
| Microsoft | Dynamics 365 Finance & Operations | Mid/large enterprise |
| Workday | Workday Finance & HCM | Mid/large enterprise (finance + HR) |
| Infor | CloudSuite | Industry-specific large enterprise |
| NetSuite (Oracle) | NetSuite ERP | SMB and growing companies |
| Epicor | Epicor Kinetic | Manufacturing mid-market |

**Market reality:** SAP dominates large enterprise globally (~22% market share). Corning uses SAP.

---

## 12. ERP Implementation Challenges

### Common Failure Reasons:
1. **Poor change management** — users resist new system
2. **Scope creep** — adding requirements mid-project
3. **Bad data migration** — dirty data corrupts new system
4. **Over-customization** — deviating too far from standard, making upgrades painful
5. **Under-resourced** — insufficient budget, wrong team, inadequate testing
6. **Unrealistic timeline** — underestimating complexity
7. **Lack of executive sponsorship** — no champion driving change

### Best Practices:
- Adopt standard processes where possible (fit-to-standard)
- Invest heavily in data quality before go-live
- Strong change management and training
- Phased/agile implementation (not big-bang where possible)
- Dedicated, empowered project team

---

## 13. Key ERP Concepts for OA

### Double-Entry Accounting (Foundation of ERP Finance)
Every financial transaction has equal debits and credits.
- **Assets = Liabilities + Equity** (must always balance)
- Debit: increases assets/expenses; decreases liabilities/equity/revenue
- Credit: increases liabilities/equity/revenue; decreases assets/expenses

### Chart of Accounts
Structured list of GL accounts. Types:
- Balance Sheet accounts (Assets, Liabilities, Equity) — carry forward
- P&L accounts (Revenue, Expenses) — reset to zero each year

### Fiscal Year
The accounting year. May be:
- Calendar year (Jan–Dec) — most common globally
- Non-calendar (Apr–Mar used in India/UK)
- 4-4-5 week structure (retail)

### Period-End Close Checklist (Common exam topic)
1. Post all transactions
2. Run depreciation
3. Reconcile sub-ledgers to GL
4. Clear GR/IR accounts
5. Post accruals
6. Run cost allocations/settlements
7. Reconcile intercompany
8. Run reporting
9. Lock period

### Three-Way Match
Vendor invoice approved for payment only when:
- PO quantity/price matches GR quantity matches Invoice quantity/price

### CAPEX vs. OPEX
- **CAPEX (Capital Expenditure):** Buying long-term assets (building, machine) — capitalized on balance sheet, depreciated over useful life
- **OPEX (Operating Expenditure):** Day-to-day costs (utilities, salaries) — expensed immediately on P&L

### Accrual Accounting
Record revenue when **earned** and expenses when **incurred** — not when cash moves.
Example: December services delivered → record December revenue even if invoice paid in January.

### Reconciliation
Comparing two sets of records and resolving differences.
Types in ERP:
- Bank reconciliation (ERP cash account vs. bank statement)
- Subledger to GL (AR/AP subledger vs. GL)
- Intercompany (entity A's receivable = entity B's payable)
- GR/IR clearing account

---

## 14. ERP Terminology Quick Reference

| Term | Meaning |
|---|---|
| GL | General Ledger — central accounting record |
| AP | Accounts Payable — money owed to vendors |
| AR | Accounts Receivable — money owed by customers |
| AA | Asset Accounting — fixed assets |
| CO | Controlling — internal management accounting |
| MM | Materials Management — procurement/inventory |
| SD | Sales & Distribution — sales cycle |
| PP | Production Planning — manufacturing |
| HR/HCM | Human Resources / Human Capital Management |
| PR | Purchase Requisition |
| PO | Purchase Order |
| GR | Goods Receipt |
| IR | Invoice Receipt |
| SLA | Service Level Agreement |
| KPI | Key Performance Indicator |
| SoD | Segregation of Duties |
| RBAC | Role-Based Access Control |
| UAT | User Acceptance Testing |
| RTR | Record-to-Report |
| P2P | Procure-to-Pay |
| O2C | Order-to-Cash |
| TCO | Total Cost of Ownership |
| CapEx | Capital Expenditure |
| OpEx | Operating Expenditure |
| RPA | Robotic Process Automation |
| BOM | Bill of Materials |
| MRP | Material Requirements Planning |
| IFRS | International Financial Reporting Standards |
| GAAP | Generally Accepted Accounting Principles |
| COA | Chart of Accounts |
| ECC | SAP's legacy ERP (predecessor to S/4HANA) |

---

*End of ERP Comprehensive Notes — approximately 3,500 words of core concepts.*
*Read alongside: SAP_Comprehensive_Notes.md for SAP-specific technology details.*
