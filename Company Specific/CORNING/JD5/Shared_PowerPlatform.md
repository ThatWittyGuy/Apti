# Shared — Microsoft Power Platform MCQs
### Applies to: JD5 (Business Engagement), JD4 (Collaboration Services — Power Automate)
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What is the Microsoft Power Platform?
- A) A hardware platform for high-performance computing
- B) A suite of low-code/no-code business application tools — Power BI (analytics), Power Apps (apps), Power Automate (automation), Power Virtual Agents (chatbots), and Power Pages (websites) — integrated with Microsoft 365 and Azure
- C) Microsoft's cloud computing platform (competing with AWS)
- D) A set of PowerShell tools for IT administrators

**Answer: B**

---

**Q2.** 🟢 What is Power BI primarily used for?
- A) Automating business processes using visual workflows
- B) A business analytics and data visualization tool for creating interactive reports and dashboards — connecting to data sources, transforming data, and sharing insights across the organization
- C) Building mobile apps for field workers
- D) Creating AI chatbots for customer service

**Answer: B**

---

**Q3.** 🟢 What is Power Apps used for?
- A) Managing Microsoft 365 app licenses
- B) A low-code platform for building custom business applications — enabling non-developers to create canvas apps (custom UI) or model-driven apps (data-driven) without extensive coding
- C) Automating approval workflows in SharePoint
- D) A tool for building Power BI dashboards

**Answer: B**

---

**Q4.** 🟢 What is Power Automate (formerly Microsoft Flow)?
- A) A tool for automating IT infrastructure provisioning
- B) A low-code workflow automation tool for automating repetitive processes — connecting apps and services with triggers and actions, supporting cloud flows, desktop flows (RPA), and business process flows
- C) A Power BI feature for auto-refreshing dashboards
- D) A scheduling tool for Microsoft 365 meetings

**Answer: B**

---

**Q5.** 🟢 What is "Dataverse" (formerly Common Data Service) in Power Platform?
- A) A data lake storage service within Power Platform
- B) A cloud-based, low-code data platform storing and managing data used by Power Apps and Power Automate — providing a structured, secure data store with built-in business logic, relationships, and role-based security
- C) A connector for linking Power Platform to external databases
- D) A data transformation tool within Power BI

**Answer: B**

---

**Q6.** 🟢 What is a "connector" in Power Automate and Power Apps?
- A) A physical network cable for connecting devices
- B) A pre-built integration component that enables Power Platform tools to communicate with external services and applications (e.g., SharePoint, Salesforce, SAP, Twitter) — abstracting API complexity for low-code builders
- C) A connector between Power BI and Power Apps
- D) A Power Platform pricing tier

**Answer: B**

---

**Q7.** 🟡 What is the difference between a "canvas app" and a "model-driven app" in Power Apps?
- A) Canvas apps are for mobile; model-driven are for desktop
- B) Canvas apps give full control over UI layout, screen design, and data sources — ideal for custom experiences; model-driven apps are generated automatically from Dataverse data model and business logic — ideal for complex, process-driven scenarios
- C) Model-driven apps require coding; canvas apps are fully no-code
- D) They are identical — just different terminology

**Answer: B**

---

**Q8.** 🟡 What is a "flow trigger" in Power Automate?
- A) A button that manually starts a flow from Power Apps
- B) The event or condition that initiates a Power Automate flow — e.g., when a new email arrives, a SharePoint item is created, a schedule fires, or an HTTP request is received
- C) A trigger warning added to automated email content
- D) A Power Automate setting that pauses flow execution

**Answer: B**

---

**Q9.** 🟡 What is the difference between "cloud flows" and "desktop flows" in Power Automate?
- A) Cloud flows run on servers; desktop flows run on mobile devices
- B) Cloud flows automate cloud-based services and APIs (triggered by events, run in the cloud); desktop flows (RPA) automate legacy desktop applications and UI interactions on a local Windows machine using robotic process automation
- C) Desktop flows require coding; cloud flows are no-code
- D) They are identical but for different user types

**Answer: B**

---

**Q10.** 🟡 What is "Power BI Desktop" vs. "Power BI Service"?
- A) Power BI Desktop is for viewing reports; Power BI Service is for creating them
- B) Power BI Desktop is a free Windows application for building reports and data models; Power BI Service (app.powerbi.com) is the cloud platform for publishing, sharing, scheduling refresh, and collaborating on reports
- C) They are identical applications with different interfaces
- D) Power BI Desktop runs on-premise; Power BI Service runs on Azure only

**Answer: B**

---

**Q11.** 🟡 What is "Power Query" in Power BI?
- A) A query language specific to Power BI
- B) A data transformation and preparation tool embedded in Power BI (and Excel) that connects to data sources, cleans and reshapes data using M language, and loads it into the data model — the ETL layer of Power BI
- C) A tool for querying Power BI dashboards via API
- D) A Power BI feature for natural language query (Q&A)

**Answer: B**

---

**Q12.** 🟡 What is "DAX" (Data Analysis Expressions) in Power BI?
- A) A deprecated Excel formula language
- B) A formula language for defining calculations in Power BI and Power Pivot — including calculated columns, measures, and KPIs using functions like `CALCULATE`, `SUMX`, `RELATED`, `DATEADD`, and time intelligence functions
- C) A data access extension for connecting Power BI to SAP
- D) A SQL dialect used in Power Query transformations

**Answer: B**

---

**Q13.** 🟡 What is a "measure" vs. a "calculated column" in Power BI DAX?
- A) Measures are for text; calculated columns are for numbers
- B) Measures are dynamic, aggregated calculations evaluated in query context at report runtime (stored in memory); calculated columns are computed row-by-row at data refresh time and stored in the data model — measures are preferred for aggregations, calculated columns for row-level attributes
- C) They are identical — just evaluated at different times
- D) Calculated columns are for relationships; measures are for visuals

**Answer: B**

---

**Q14.** 🟡 What is "Row-Level Security" (RLS) in Power BI?
- A) A security feature restricting access to specific rows of Power BI reports based on the user's screen resolution
- B) A feature that restricts data access at the row level based on the user's identity — ensuring users see only the data they're authorized to view (e.g., a sales manager sees only their region's data)
- C) A Power BI admin setting restricting row counts in exports
- D) A feature locking specific report rows from editing

**Answer: B**

---

**Q15.** 🟡 What is a "Power BI dataset" (now called "semantic model")?
- A) A Power BI file containing raw unprocessed data
- B) A reusable, centrally published data model containing tables, relationships, measures, and transformations — which multiple reports can connect to as a shared source of truth
- C) A sample dataset provided by Microsoft for learning Power BI
- D) A database backup stored in Power BI Service

**Answer: B**

---

**Q16.** 🟡 What is "DirectQuery" mode in Power BI and when is it used?
- A) A direct API query sent from Power BI to SharePoint
- B) A connection mode where Power BI queries the source database live for every report interaction (no data imported) — used for very large datasets that can't be imported or when real-time data freshness is required; trades performance for currency
- C) A feature for directly editing data in the source database from Power BI
- D) A connection mode only available for SQL Server sources

**Answer: B**

---

**Q17.** 🟡 What is a "Power BI Gateway"?
- A) A pricing gateway controlling Power BI license access
- B) An on-premise software bridge that enables Power BI Service (cloud) to securely connect to and refresh data from on-premise data sources (SQL Server, SharePoint on-premise, SAP, etc.) without exposing them to the internet
- C) A security gateway controlling who can publish Power BI reports
- D) A network router optimized for Power BI traffic

**Answer: B**

---

**Q18.** 🟡 What is "Power Virtual Agents" (now Microsoft Copilot Studio)?
- A) A Power Platform tool for creating virtual reality experiences
- B) A low-code platform for building AI-powered chatbots and virtual agents that can answer user questions, automate workflows, and integrate with Power Automate — deployed in Teams, websites, and other channels
- C) An AI agent for automating Power BI report creation
- D) A tool for building robotic process automation bots

**Answer: B**

---

**Q19.** 🟡 What is "AI Builder" in Power Platform?
- A) A tool for building AI-powered Power BI dashboards
- B) A low-code AI capability in Power Platform enabling users to add AI features (form processing, object detection, text classification, prediction) to Power Apps and Power Automate flows without data science expertise
- C) A marketplace for purchasing pre-built AI models
- D) A Power Platform tool for training large language models

**Answer: B**

---

**Q20.** 🟡 What is a "Power Pages" (formerly Power Apps Portals) site?
- A) A Power BI feature for creating multi-page reports
- B) A low-code tool for building external-facing websites connected to Dataverse data — enabling organizations to create secure portals for customers, partners, or suppliers to interact with business data
- C) A SharePoint feature for creating internal team pages
- D) A Power Automate feature for creating process documentation pages

**Answer: B**

---

**Q21.** 🟡 What is "Power Platform Center of Excellence (CoE) Starter Kit"?
- A) A physical center for Power Platform training
- B) A set of templates, tools, and guidance provided by Microsoft to help organizations establish governance, gain visibility into Power Platform adoption, nurture makers, and manage their Power Platform environment at scale
- C) A starter kit of pre-built Power Apps templates
- D) A Power BI report showing Power Platform licensing costs

**Answer: B**

---

**Q22.** 🟡 What is "Power Automate Desktop" used for?
- A) Automating cloud-based Microsoft 365 processes
- B) A free Windows application for recording and automating desktop application interactions using RPA — automating legacy apps, web browsers, Excel, and other desktop tools that lack APIs
- C) A Power BI Desktop replacement for data transformation
- D) A tool for automating desktop publishing workflows

**Answer: B**

---

**Q23.** 🔴 What is the "CALCULATE" function in DAX and why is it central to Power BI?
- A) A basic arithmetic function for summing calculated values
- B) The most important DAX function — it evaluates an expression in a modified filter context, enabling complex calculations like year-over-year comparisons, running totals, and conditional aggregations: `CALCULATE(expression, filter1, filter2, ...)`
- C) A function for calculating model-driven app logic
- D) A Power Query function for calculating derived columns

**Answer: B**

---

**Q24.** 🔴 What is "composite model" in Power BI?
- A) A Power BI model combining data from multiple tenants
- B) A Power BI data model combining Import mode tables (fast, cached) and DirectQuery tables (live source) in the same model — enabling flexibility to import frequently used small tables while keeping large/real-time tables in DirectQuery
- C) A model combining Power BI and Power Apps data
- D) A multi-page Power BI report with composite visuals

**Answer: B**

---

**Q25.** 🔴 What is "incremental refresh" in Power BI?
- A) Refreshing only the Power BI app interface incrementally
- B) A feature that partitions large datasets by date and refreshes only recent data partitions on each scheduled refresh — avoiding full data reloads for historical data, dramatically reducing refresh time and resource consumption
- C) Incremental updates to DAX measures during report interaction
- D) A Power BI feature for streaming incremental data changes

**Answer: B**

---

**Q26.** 🔴 What is "Power BI Embedded"?
- A) An embedded Power BI visual within a Power Apps form
- B) An Azure service that allows developers to embed Power BI reports and dashboards into custom applications or websites for external users — without requiring those users to have Power BI licenses
- C) A version of Power BI embedded in Microsoft Teams only
- D) A Power BI feature for embedding Python visuals in reports

**Answer: B**

---

**Q27.** 🔴 What is "environment" in Power Platform governance?
- A) The physical data center environment hosting Power Platform
- B) A container in Power Platform isolating apps, flows, Dataverse data, and connections — used to separate Development, Test, and Production workloads, and apply different security policies and data loss prevention rules
- C) A Power Platform setting for selecting the UI theme
- D) A geographic region selection for Power Platform data residency

**Answer: B**

---

**Q28.** 🔴 What is "DLP policy" (Data Loss Prevention) in Power Platform?
- A) A policy deleting data after a defined retention period
- B) An admin policy defining which connectors can be used together in Power Automate flows and Power Apps — preventing users from accidentally exfiltrating sensitive business data to unauthorized external services
- C) A policy limiting the number of flows a user can create
- D) A Power Platform policy enforcing data encryption

**Answer: B**

---

**Q29.** 🔴 What is "Copilot in Power Platform" and what capabilities does it add?
- A) A Power Platform add-in for Microsoft Copilot 365
- B) AI-powered assistance embedded across Power Platform tools — generating Power Apps screens from natural language descriptions, creating Power Automate flows from prompts, writing DAX measures in Power BI, and building chatbots in Copilot Studio from plain-language instructions
- C) A Copilot chatbot built using Power Virtual Agents
- D) An AI tool for reviewing and approving Power Platform governance policies

**Answer: B**

---

**Q30.** 🔴 What is "solution" in Power Platform application lifecycle management (ALM)?
- A) A workaround for a Power Platform issue
- B) A container for Power Platform components (apps, flows, tables, connections) that can be exported and imported between environments — enabling ALM practices like source control, deployment pipelines, and environment promotion
- C) A Microsoft support solution for Power Platform issues
- D) A package of pre-built Power Platform templates

**Answer: B**

---

**Q31.** 🟡 What is "Power BI Q&A" feature?
- A) A Q&A forum embedded in Power BI for user questions
- B) A natural language query feature allowing users to type questions in plain English (e.g., "total sales by region last quarter") and Power BI automatically generates an appropriate visualization from the data model
- C) A Power BI feature for creating quiz-style reports
- D) A customer support channel embedded in Power BI Service

**Answer: B**

---

**Q32.** 🟡 What is the "Power Platform admin center"?
- A) A help center for Power Platform administrators
- B) A centralized web portal for managing Power Platform environments, governance policies, capacity, licensing, DLP policies, and monitoring usage and health across an organization's Power Platform deployment
- C) A Power BI dashboard showing admin metrics
- D) A Microsoft support portal for Power Platform issues

**Answer: B**

---

**Q33.** 🔴 What is "Dataverse for Teams" and how does it differ from full Dataverse?
- A) A version of Dataverse only accessible from Microsoft Teams chats
- B) A lightweight version of Dataverse embedded within Microsoft Teams — offering limited storage, fewer features, and Teams-scoped access; full Dataverse supports larger data volumes, more complex data models, advanced business logic, and broader access
- C) A Dataverse connector for Microsoft Teams notifications
- D) A Teams app providing Dataverse administration features

**Answer: B**

---

**Q34.** 🔴 What is "Power Fx" in Power Apps?
- A) A Power Platform billing and licensing framework
- B) An open-source, Excel-inspired low-code programming language used across Power Platform (Power Apps, Power Automate, Dataverse) — enabling formula-based logic familiar to Excel users while supporting professional extensibility
- C) A visual effects library for Power Apps UI design
- D) A Power BI expression language similar to DAX

**Answer: B**

---

**Q35.** 🔴 What is "managed environments" in Power Platform?
- A) Power Platform environments managed by Microsoft employees
- B) A premium governance feature enabling admins to apply enhanced controls — including maker onboarding, pipeline enforcement, solution checker requirements, usage insights, and Copilot restrictions — to specific Power Platform environments
- C) Environments with automated scaling managed by Azure
- D) A feature for managing Power Platform across multiple tenants

**Answer: B**

---
*35 Questions | Shared — Microsoft Power Platform.*
