# Microsoft 365 & Power Platform — Comprehensive Study Notes
### For Corning OA Prep | JD4 (Collaboration Services), JD5 (Business Engagement)
> Covers: M365 suite, Teams, SharePoint, Exchange, OneDrive, Entra ID, Security, Power BI, Power Apps, Power Automate, Power Virtual Agents, Dataverse, Copilot.

---

## 1. Microsoft 365 — Overview

### 1.1 What is Microsoft 365?
Microsoft 365 (formerly Office 365) is Microsoft's **cloud-based subscription suite** combining:
- **Productivity apps:** Word, Excel, PowerPoint, OneNote, Outlook
- **Collaboration platform:** Microsoft Teams, SharePoint, OneDrive
- **Communication:** Exchange Online (email), Teams (chat/meetings/calls)
- **Security & compliance:** Microsoft Defender, Purview, Intune
- **Low-code platform:** Power Platform (Power BI, Power Apps, Power Automate)
- **AI assistant:** Microsoft 365 Copilot (GPT-4 powered)

**Deployment:** Pure cloud (SaaS) — Microsoft manages all infrastructure, updates, and availability. No servers to manage.

**Licensing Plans:**
- **Microsoft 365 Business Basic/Standard/Premium** — SMB (up to 300 users)
- **Microsoft 365 E3/E5** — Enterprise with advanced security and compliance
- **Microsoft 365 F1/F3** — Frontline workers (limited features, lower cost)

### 1.2 Microsoft 365 Tenant
A **tenant** is a dedicated, isolated instance of M365 services for one organization:
- Identified by a primary domain (e.g., `corning.com`)
- All users, data, and settings for that organization live within the tenant
- Tenants are completely isolated from each other — Corning's data is never visible to another company
- Each tenant has its own: Azure AD directory, Exchange mailboxes, SharePoint sites, Teams

### 1.3 Administration
**Microsoft 365 Admin Center** (admin.microsoft.com):
- Manage users, licenses, groups
- Configure services
- View service health
- Access specialized admin centers (Teams, Exchange, SharePoint, Security, Compliance)

**Role-based administration:** Global Admin (full control) → Service-specific admins (Teams Admin, Exchange Admin, SharePoint Admin, etc.) — least privilege principle

---

## 2. Microsoft Entra ID (Azure Active Directory)

### 2.1 What is Entra ID?
Microsoft Entra ID (formerly Azure Active Directory / Azure AD) is Microsoft's **cloud-based identity and access management (IAM) service** — the foundation of M365 security.

**What it does:**
- Authenticates users (verifies who you are)
- Authorizes access (determines what you can access)
- Manages: users, groups, devices, applications, policies

### 2.2 Hybrid Identity — Entra Connect
Most enterprises have **on-premise Active Directory** (Windows AD domain) alongside cloud M365. **Microsoft Entra Connect** (formerly Azure AD Connect) synchronizes:
- User accounts from on-premise AD → Entra ID
- Passwords (hash sync or pass-through auth)
- Groups and device objects

**Result:** Users log in once with corporate credentials (Single Sign-On) and access both on-premise and cloud resources.

**Three hybrid auth models:**
- **Password Hash Sync (PHS):** Hash of password synced to cloud — simplest, most resilient
- **Pass-Through Authentication (PTA):** Cloud validates by forwarding to on-prem agent — password never leaves on-prem
- **Federation (ADFS):** Authentication fully handled on-prem; cloud redirects to ADFS — most complex, highest control

### 2.3 Multi-Factor Authentication (MFA)
Requires two or more verification factors:
- **Something you know:** Password
- **Something you have:** Authenticator app, SMS code, hardware token
- **Something you are:** Biometric (fingerprint, face)

**Microsoft Authenticator app:** Push notification or TOTP code — most common in enterprise.

**MFA methods in M365:** Microsoft Authenticator, SMS, voice call, hardware OATH token, FIDO2 security key.

**Security Default / Conditional Access:** Microsoft recommends Conditional Access (see below) over blanket MFA for better control.

### 2.4 Conditional Access
A **policy engine** that grants/blocks access based on signals:
- **User/Group:** Who is trying to access?
- **Application:** What are they accessing (Teams, SharePoint, SAP)?
- **Device compliance:** Is the device managed/compliant (Intune)?
- **Location:** Corporate network vs. unknown country?
- **Risk level:** Is the sign-in risky (impossible travel, leaked credentials)?

**Actions:** Grant (with MFA, with compliant device), Block, Require password change.

**Examples:**
- Require MFA for all users accessing from outside corporate network
- Block access from countries company doesn't operate in
- Allow access to email only from Intune-managed devices

### 2.5 Single Sign-On (SSO)
Users authenticate once → access all integrated apps without re-entering credentials.
- Works for M365 apps + thousands of third-party SaaS apps registered in Entra ID (Salesforce, ServiceNow, SAP, Workday)
- Protocol: SAML 2.0 or OpenID Connect (OIDC)
- **Enterprise App Gallery:** Pre-configured SSO for 3,000+ apps

### 2.6 Entra ID Groups
- **Security Groups:** Assign app access, license, policies — no email
- **Microsoft 365 Groups:** Collaborative groups — come with shared mailbox, SharePoint site, Teams, Planner, OneNote
- **Distribution Groups:** Email distribution only (legacy)
- **Dynamic Groups:** Membership automatically managed by rules (e.g., all users with Department="Finance" automatically join Finance group)

---

## 3. Microsoft Teams

### 3.1 What is Microsoft Teams?
Teams is Microsoft's **unified collaboration hub** combining:
- Persistent chat (channels and direct messages)
- Video/audio meetings and webinars
- File sharing and co-authoring
- App integrations (tabs, bots, connectors)
- Voice calls (Teams Phone / PSTN)

Teams replaced Skype for Business and is the central communication tool in M365.

### 3.2 Teams Structure
**Team:** A group of people working together. Each Team automatically gets:
- Microsoft 365 Group (membership)
- SharePoint site (file storage)
- Exchange shared mailbox
- OneNote notebook
- Planner (tasks)

**Channel:** Organized conversation within a Team around a topic.
- **Standard Channel:** Visible to all Team members; files stored in SharePoint document library subfolder
- **Private Channel:** Visible only to selected members; has its own isolated SharePoint site collection
- **Shared Channel:** Can include people from outside the Team or even outside the organization (B2B collaboration without guest accounts)

**Direct Messages (DMs):** Private 1:1 or group conversations outside of Teams/Channels.

**Posts vs. Files vs. Apps:** Each channel has tabs — Posts (conversations), Files (SharePoint document library), Wiki, and custom app tabs.

### 3.3 Meetings in Teams
- **Scheduled Meeting:** Calendar invite → joins via Teams link
- **Channel Meeting:** Posted in a channel — visible to all channel members
- **Meet Now:** Instant unscheduled meeting
- **Webinars:** Structured events with registration and presenter/attendee roles
- **Live Events:** Large-scale broadcasts (up to 10,000 attendees)
- **Town Halls:** Large internal company broadcasts (newer feature)

**Meeting features:** Breakout rooms, polls, whiteboard (Whiteboard app), recording, live transcription, captions, noise suppression, background effects, Together Mode.

**Meeting Recordings:**
- Stored in **OneDrive** (for personal/ad-hoc meetings) or **SharePoint** (for channel meetings)
- Configurable retention policies
- Transcript available separately

### 3.4 Teams Phone (Cloud Voice)
- Enterprise telephony within Teams — replacing traditional PBX
- **Calling Plans:** Microsoft provides PSTN numbers directly
- **Direct Routing:** Connect own PSTN carrier via Session Border Controller (SBC)
- **Features:** Call queues, auto-attendants, voicemail, call forwarding, emergency calling

### 3.5 Teams Governance
Without governance, organizations end up with:
- Hundreds of abandoned Teams ("Teams sprawl")
- Inconsistent naming
- Sensitive data in guest-accessible Teams
- No visibility into inactive Teams

**Governance controls:**
- Who can create Teams (restrict to specific groups/roles)
- Naming policy (prefix/suffix rules, blocked words)
- Expiration policy (Teams expire after X days of inactivity unless renewed)
- Guest access policy (allow/block external users)
- External access policy (federation with other M365 tenants)
- Data retention and eDiscovery policies

### 3.6 Teams Admin Center
Specialized admin portal for Teams management:
- Manage policies (messaging, meeting, calling)
- Monitor usage and call quality
- Manage devices (Teams Rooms, phones, displays)
- Configure voice settings

---

## 4. SharePoint Online

### 4.1 What is SharePoint?
SharePoint is Microsoft's **intranet and document management platform** — providing:
- Team sites (collaboration spaces for groups)
- Communication sites (broadcast information to broader audiences)
- Document libraries (centralized file storage with versioning, metadata, permissions)
- Lists (structured data — like a lightweight database table or spreadsheet)
- Pages and news (intranet content)

### 4.2 SharePoint Site Types
**Team Site:**
- Connected to a Microsoft 365 Group (and therefore a Team)
- For collaboration within a defined group
- Private by default
- Has: document library, lists, pages, notebook

**Communication Site:**
- For broadcasting information to a wide audience
- Like an internal website or department homepage
- Not connected to a group — standalone
- Full page design control (sections, web parts, mega menu)

**Hub Site:**
- Connects related sites together under one navigation and branding
- Shared navigation across all associated sites
- Rollup news and search across associated sites

### 4.3 Document Libraries
- Primary storage for files in SharePoint
- Each file/folder has: name, created by, modified date, size
- **Metadata columns:** Add custom columns (status, document type, project code) — enables filtering, grouping without folders
- **Versioning:** Major and minor versions tracked — view/restore previous versions, see who changed what
- **Check-in/Check-out:** Prevent simultaneous edits on a file (optional — usually not needed with co-authoring)
- **Content Types:** Define what metadata columns are required for different document types
- **Views:** Custom filtered/grouped displays of library contents (doesn't change underlying data)

### 4.4 SharePoint Lists
Structured tabular data (like a database table or spreadsheet):
- **List types:** Custom list, task list, issue tracker, calendar, contact list, announcement
- **Columns:** Single line text, multiple lines, choice (dropdown), number, currency, date, person/group, lookup, yes/no, hyperlink
- **Views:** Filter, sort, group by different columns; Kanban view (for tasks), Gallery view (tiles)
- **Validation:** Column-level and item-level validation rules
- **Alerts:** Notify users when items change

### 4.5 Permissions Model
SharePoint has hierarchical permissions:
- **Site collection** → **Site** → **Library/List** → **Folder** → **Item**
- Permissions inherit from parent by default; break inheritance to set unique permissions at lower levels
- **Permission levels:** Full Control, Design, Edit, Contribute (add/edit/delete), Read, View Only
- **Groups:** Owners (full control), Members (contribute), Visitors (read)
- **Best practice:** Manage permissions at Group level, not individual user level

### 4.6 SharePoint Search
- Crawls all SharePoint content automatically
- Search across all content user has access to
- **Refiners:** Filter results by site, file type, author, modified date
- **Promoted Results (Bookmarks):** Pin specific results for key queries
- **Microsoft Search:** Unified search across M365 (SharePoint, Teams, Exchange, OneDrive, Bing)

### 4.7 Co-Authoring
Multiple users edit the same Word, Excel, or PowerPoint file simultaneously:
- Changes visible to all in real time
- Works in browser (Office Online) or desktop Office apps
- Auto-save with each edit (stored in SharePoint/OneDrive)
- Conflict resolution: if same cell/paragraph edited simultaneously, user prompted to choose

### 4.8 SharePoint Versioning
- **Major versions:** Published — visible to all readers (v1.0, v2.0)
- **Minor versions (drafts):** Only visible to contributors/approvers (v1.1, v1.2)
- **Restore:** Right-click → Version History → Restore any previous version
- **Version limits:** Configure max number of versions to control storage

---

## 5. Exchange Online & Outlook

### 5.1 Exchange Online
Microsoft's hosted email and calendaring service — the cloud version of Exchange Server.
- Every M365 user gets a mailbox (50GB by default, 100GB with E3/E5)
- Includes: email, calendar, contacts, tasks
- **Archiving:** In-Place Archive — additional mailbox for old emails
- **Litigation Hold / eDiscovery Hold:** Preserve mailbox content for legal purposes

### 5.2 Key Exchange Concepts
**Shared Mailbox:**
- A mailbox not associated with a single user
- Multiple users can send from and read (e.g., support@corning.com)
- No per-user license required (up to 50GB)

**Room/Resource Mailboxes:**
- Calendar resources for booking meeting rooms, equipment
- Can be auto-accepted or require approval

**Distribution Groups:** Email alias that sends to multiple users (DL@corning.com → all members)

**Dynamic Distribution Groups:** Membership automatically determined by attributes (all users in Finance department)

**Mail-Enabled Security Groups:** Can receive email AND be used for permissions

### 5.3 Exchange Online Protection (EOP)
- Anti-spam, anti-malware filtering for all inbound/outbound email
- Built into every M365 plan
- **Microsoft Defender for Office 365 (MDO):** Additional protection — Safe Links (URL detonation), Safe Attachments (sandbox), Anti-phishing (impersonation detection)

### 5.4 Outlook Features (Desktop & Web)
- **Focused Inbox:** AI sorts important emails from newsletters/low-priority
- **@mentions:** Type @Name in email body → person's name highlighted and added to To field
- **Search:** Full-text search across all mailbox content
- **Rules:** Auto-process incoming email (move, categorize, forward, delete)
- **Quick Steps:** One-click multi-action macros (move + mark read + categorize)
- **Add-ins:** Third-party integrations (SAP Concur, Salesforce, Zoom, DocuSign)
- **Viva Insights (MyAnalytics):** Personal productivity analytics (focus time, meeting time, email habits)

---

## 6. OneDrive for Business

### 6.1 What is OneDrive for Business?
Each M365 user gets **1TB+ of personal cloud storage** in OneDrive for Business (distinct from consumer OneDrive).

**Use cases:**
- Personal work files (drafts, personal notes)
- Files shared with specific individuals
- Sync files to local machine for offline access

**Not for:** Team files that multiple people own (use SharePoint for those)

### 6.2 Sync Client
- OneDrive sync app (installed on Windows/Mac) keeps local folder in sync with cloud
- **Files On-Demand:** Files show in Explorer but only download when opened (saves disk space)
- **Known Folder Move:** Automatically backs up Desktop, Documents, Pictures to OneDrive

### 6.3 Sharing from OneDrive
- Share with: specific people, people with link, anyone (anonymous — if allowed by policy)
- Set permissions: Can view, Can edit, Can review (comment only)
- Expiry dates and passwords on links
- **Sharing governance:** Admin can restrict sharing to internal only or block anonymous links

---

## 7. Microsoft 365 Security & Compliance

### 7.1 Microsoft Purview (Compliance)
Formerly Microsoft Compliance Center — umbrella for all data governance and compliance tools.

**Sensitivity Labels:**
- Classify documents/emails by sensitivity level (Public, Internal, Confidential, Highly Confidential)
- Actions: encryption (restrict who can open), watermarking, access restrictions
- Auto-labeling: ML identifies sensitive content and applies labels automatically
- Integrated in Office apps, Teams, SharePoint, Exchange

**Retention Policies:**
- Define how long content is retained before deletion
- Or: retain for minimum period (hold) regardless of user deletion
- Applied to: Exchange mailboxes, SharePoint sites, Teams messages, OneDrive
- **Litigation Hold:** Preserve all content indefinitely for legal purposes

**Data Loss Prevention (DLP):**
- Detect and prevent sharing of sensitive information
- Detects: credit card numbers, SSNs, passport numbers, health records (pre-built or custom patterns)
- Actions: block send/share, show policy tip to user, allow override with justification, log incident
- Applied to: Exchange, Teams, SharePoint, OneDrive, Endpoints (devices)

**eDiscovery:**
- Legal investigation tool — search, hold, export content across M365
- Content Search: search mailboxes, Teams, SharePoint for keywords/date ranges
- Core eDiscovery: add holds to preserve content, export for legal review
- Premium eDiscovery: advanced analytics, review sets, custodian management

**Audit Log:**
- Records all user and admin activity across M365
- Searchable: who did what, when, from where
- Retention: 90 days (standard), 1 year with E5 or add-on

**Insider Risk Management:**
- Detects patterns suggesting insider threats (data exfiltration before employee resignation, policy violations)
- Uses ML on activity signals — privacy-preserving (anonymized until confirmed risk)

### 7.2 Microsoft Defender for Office 365
Layer of protection beyond basic EOP:
- **Safe Attachments:** Opens attachments in sandbox before delivery — catches zero-day malware
- **Safe Links:** Rewrites URLs, checks at click-time — blocks malicious links even if domain becomes malicious after delivery
- **Anti-Phishing:** AI detects impersonation of executives, domains, brands
- **Attack Simulator:** Send simulated phishing to train users

### 7.3 Microsoft Intune
**Mobile Device Management (MDM) and Mobile Application Management (MAM):**
- **MDM:** Full device enrollment — IT controls the device; can wipe, configure policies, deploy apps
- **MAM:** App-level management — for personal (BYOD) devices; only manage work apps and data without controlling the personal device
- **Compliance Policies:** Define required device state (OS version, encryption, PIN, not jailbroken)
- **Conditional Access Integration:** Block M365 access from non-compliant devices
- **App Deployment:** Push apps to managed devices automatically
- Supports: Windows, macOS, iOS, Android

### 7.4 Zero Trust in M365
**Zero Trust principles** applied to M365:
- **Verify explicitly:** Every access request authenticated with MFA, device compliance, risk score
- **Least privilege:** Users get minimum access needed (RBAC, Conditional Access)
- **Assume breach:** Segment access, monitor continuously, encrypt data

M365 zero trust tools: Entra ID (identity), Intune (devices), Purview (data), Defender (threat protection)

---

## 8. Microsoft Power Platform

### 8.1 What is Power Platform?
Microsoft's **low-code/no-code suite** for business users to build apps, automate workflows, analyze data, and create chatbots — without heavy development. Fully integrated with M365 and Azure.

**Four core products:**
1. **Power BI** — Business analytics and data visualization
2. **Power Apps** — Custom application development
3. **Power Automate** — Workflow and process automation
4. **Microsoft Copilot Studio** (formerly Power Virtual Agents) — AI chatbot builder

**Shared infrastructure:**
- **Dataverse:** Common data platform (relational database) underlying all Power Platform apps
- **Connectors:** Pre-built integrations to 1,000+ data sources
- **AI Builder:** Pre-built AI models for non-data scientists
- **Power Fx:** Low-code formula language (Excel-like syntax)

### 8.2 Power BI — In Depth

**Architecture:**
- **Power BI Desktop:** Free Windows app for building reports and data models
- **Power BI Service:** Cloud platform (app.powerbi.com) for publishing, sharing, and collaborating
- **Power BI Mobile:** iOS and Android apps for consuming reports on mobile
- **Power BI Report Server:** On-premise report server (for organizations not ready for cloud)
- **Power BI Embedded:** Embed reports into custom applications (Azure service)

**Data Connectivity Modes:**
| Mode | Description | When to Use |
|---|---|---|
| **Import** | Data copied into Power BI's in-memory model | Fastest performance; not real-time; up to 1GB (Premium: larger) |
| **DirectQuery** | Queries sent to source on every interaction | Real-time data; large datasets; source must handle load |
| **Composite** | Mix of Import and DirectQuery tables | Balance speed and currency |
| **Live Connection** | Connect to existing SSAS/Power BI dataset | Reuse existing models |
| **Streaming** | Real-time data (IoT, monitoring dashboards) | Live operational dashboards |

**Power BI Data Model:**
- Built in Power BI Desktop using Power Query (for ETL) and DAX (for calculations)
- Multiple tables connected via relationships (like a relational database)
- Star schema: Fact tables (sales transactions) + Dimension tables (products, customers, dates)
- Measures (DAX): calculated KPIs evaluated in filter context

**DAX Key Concepts (see also Excel section):**
- `CALCULATE()` — modify filter context
- `ALL()` / `ALLEXCEPT()` — remove filters
- Time Intelligence: `TOTALYTD()`, `SAMEPERIODLASTYEAR()`, `DATEADD()`
- `DIVIDE()` — safe division
- `RANKX()` — ranking within context
- `VAR ... RETURN` — define variables in measures for cleaner code

**Power BI Reports vs. Dashboards:**
- **Report:** Multi-page, interactive, built in Power BI Desktop — full analytical experience
- **Dashboard:** Single-page, pinned tiles from multiple reports/datasets — executive monitoring view; less interactive than reports; real-time via streaming

**Row-Level Security (RLS):**
- Define roles with DAX filter rules restricting which rows each role sees
- Assign users to roles in Power BI Service
- Example: Europe Sales role → `[Region] = "Europe"` — users in this role only see European data
- Dynamic RLS: `[SalesRep] = USERPRINCIPALNAME()` — each user sees only their own rows

**Power BI Premium / Fabric:**
- Premium capacity: dedicated resources — larger models, faster refresh, paginated reports, AI features
- **Microsoft Fabric:** Microsoft's new unified analytics platform (announced 2023) — integrates Power BI with Synapse Analytics, Data Factory, Data Activator — all in one platform. Power BI is one "workload" within Fabric.

**Power BI Gateway:**
- On-premise software bridge allowing Power BI Service to refresh data from on-premise sources
- Standard mode: For scheduled refresh and live connections
- Personal mode: For one user only; doesn't support live connections

**Sharing and Distribution:**
- **Workspace:** Collaboration area in Power BI Service for publishing/sharing reports within a team
- **App:** Curated collection of reports/dashboards published to end users (with one-click access)
- **Share:** Direct link sharing with specific users or M365 groups
- **Embed in SharePoint/Teams:** Embed Power BI reports directly in SharePoint pages or Teams tabs

### 8.3 Power Apps — In Depth

**Two App Types:**

**Canvas App:**
- Start from a blank canvas — full control over UI layout
- Drag-and-drop controls (buttons, forms, galleries, dropdowns)
- Connect to data sources via connectors
- Formula-based logic using **Power Fx** (Excel-like)
- Best for: custom mobile apps, forms, field worker apps, simple CRUD interfaces

**Model-Driven App:**
- Built on **Dataverse** data model — UI generated automatically from data structure
- Configure views, forms, dashboards — not pixel-level layout control
- Includes business process flows, business rules, and workflows
- Best for: complex business process apps (CRM-like, case management)

**Power Pages (formerly Power Apps Portals):**
- Low-code tool for building external-facing websites connected to Dataverse
- Customer/partner portals where external users submit forms, view their data
- Requires licensing for external anonymous/authenticated users

**Power Fx:**
- Excel-inspired formula language used in Power Apps
- Formulas respond to user actions: `=Filter(Products, Category = Dropdown1.Selected.Value)`
- Imperative logic via `OnSelect` behavior formulas
- Supports: functions, variables (global and context), collections (in-memory tables)

**Connectors in Power Apps:**
- Standard connectors: SharePoint, SQL Server, Excel Online, Dataverse, Common Data Service
- Premium connectors: Salesforce, SAP, ServiceNow, Dynamics 365, Azure SQL (require premium license)
- Custom connectors: Define your own API connector

### 8.4 Power Automate — In Depth

**Flow Types:**

**Cloud Flows (cloud-based):**
- **Automated:** Triggered by an event (new email, SharePoint item, Teams message)
- **Instant:** Manually triggered (button in Teams/Power Apps, or HTTP request)
- **Scheduled:** Run on a timetable (daily, every hour, every Monday)

**Desktop Flows (RPA — Robotic Process Automation):**
- Automate Windows desktop applications via UI recording/scripting
- Record clicks, keystrokes on desktop/web apps
- No API needed — automates the UI directly
- Requires Power Automate Desktop (free) + attended/unattended RPA license
- **Attended:** Bot runs with user present (user triggers)
- **Unattended:** Bot runs on its own (no user needed) — requires premium license

**Business Process Flows:**
- Guided, stage-by-stage process within Dataverse/model-driven apps
- User follows prescribed steps (like a wizard) — ensures process compliance

**Flow Structure:**
```
Trigger → Actions → Conditions → Loops → Error Handling
```

**Key built-in features:**
- **Conditions:** If/else branching
- **Apply to Each:** Loop through a list/array
- **Do Until:** Loop until condition met
- **Variables:** Initialize, increment, set, append to array
- **Compose / Select:** Transform data
- **Parse JSON:** Extract data from API responses
- **HTTP action:** Call any REST API (premium)
- **Expressions:** `formatDateTime()`, `concat()`, `length()`, `first()`, `body()`, `triggerOutputs()`

**Approval Workflows:**
- Built-in Approvals connector — send approval requests, wait for response, branch based on outcome
- Responses stored in Approvals center in Teams
- Supports: single approver, sequential, parallel, everyone must approve

**Common use cases:**
- New hire notification → HR creates record in SharePoint → Automate notifies manager in Teams → Creates AD account request
- Invoice approval → Email arrives → Parse → Route for approval → Update SAP via API → Notify requestor
- Weekly report → Scheduled flow → Query SQL → Format data → Email as HTML table

### 8.5 Microsoft Copilot Studio (Power Virtual Agents)

**What it is:** Low-code platform for building AI-powered chatbots and virtual agents.

**Key concepts:**
- **Topics:** Conversation flows triggered by user phrases (intents)
- **Trigger phrases:** Example phrases that activate a topic
- **Nodes:** Conversation steps — ask a question, show message, call an action, condition
- **Actions:** Call Power Automate flows from within conversation
- **Entities:** Extract structured data from user input (name, date, number)
- **Variables:** Store conversation data (user name, selected option)
- **Escalation:** Hand off to human agent (integrates with Dynamics 365 Customer Service)

**Deployment channels:** Teams, SharePoint, websites, mobile apps, Facebook, custom channels

**AI capabilities:** GPT-based generative answers from knowledge sources (documents, URLs) — bot answers questions from uploaded content without manually authoring all topics.

### 8.6 Dataverse

**What it is:** Microsoft's cloud-based relational data platform — the common data store for Power Platform.

- Tables (like database tables) with typed columns
- Relationships between tables (1:1, 1:N, N:N)
- Built-in security: row-level and column-level security
- Business rules: validate data at the data layer (not just UI)
- Business events: triggers on record create/update/delete
- Auditing: track all changes to records
- **Standard tables:** Account, Contact, Lead, Case, Opportunity — pre-built for common business entities

**Dataverse vs. SharePoint Lists:**
| Feature | Dataverse | SharePoint List |
|---|---|---|
| Relationships | Full relational model | Lookup columns only |
| Data volume | Millions of rows | ~30 million items max |
| Business logic | Server-side rules | Limited |
| Licensing | Requires Power Apps license | Included with M365 |
| Best for | Complex business apps | Simple collaboration lists |

**Dataverse for Teams:** Lightweight version embedded in Teams — free with M365, limited to 2GB per team, accessible only within Teams.

### 8.7 AI Builder
Pre-built AI models usable in Power Apps and Power Automate without data science knowledge:
- **Form Processing:** Extract fields from invoices, receipts, forms (like SAP's Document Information Extraction)
- **Object Detection:** Detect items in images (products on shelf, damaged parts)
- **Text Classification:** Classify incoming text (customer feedback sentiment, ticket categorization)
- **Entity Extraction:** Pull structured info from text (dates, amounts, names)
- **Prediction:** Classify records as likely outcomes (will this deal close? is this customer at risk?)
- **Business Card Reader:** Extract contact info from business card photos

### 8.8 Power Platform Governance

**Environment Strategy:**
- **Default environment:** Every M365 user can create apps — ungoverned, risky
- **Production/Dev/Test environments:** Controlled, licensed environments managed by IT
- Best practice: restrict app creation to dedicated environments

**DLP Policies (Data Loss Prevention):**
- Define which connectors can be used together
- **Business data group:** Internal business connectors (Dataverse, SharePoint)
- **Non-business data group:** Consumer connectors (Twitter, Gmail)
- **Blocked group:** Connectors not allowed at all (personal storage)
- Example policy: Block combining Dataverse (sensitive data) with Gmail (external)

**Center of Excellence (CoE) Starter Kit:**
- Free Microsoft-provided toolkit for governing Power Platform
- Inventory of all apps/flows in the tenant
- Usage analytics (who uses what)
- Maker activity monitoring
- Compliance processes (review and archive unused apps)

**License requirements:**
- Power Apps: Included in M365 (limited) or Per App (£5/app/month) or Per User (£16/user/month)
- Power Automate: Included in M365 (cloud flows only) or Premium (desktop flows, premium connectors)
- Power BI: Free (limited) or Pro (£8.40/user) or Premium Per User (£16.90/user)

---

## 9. Microsoft Viva — Employee Experience

Microsoft Viva is an **employee experience platform** built on top of M365 and Teams, delivered directly within the Teams interface.

| Module | Description |
|---|---|
| **Viva Connections** | Company intranet experience within Teams — news, announcements, dashboard |
| **Viva Insights** | Personal productivity analytics (focus time, meeting load, wellbeing nudges) and manager/leader analytics |
| **Viva Learning** | Centralized learning hub — LinkedIn Learning, third-party LMS, custom content in Teams |
| **Viva Topics** | AI-powered knowledge management — automatically identifies topics and expertise from M365 content, creates topic pages |
| **Viva Goals** | OKR (Objectives and Key Results) goal management integrated with Teams |
| **Viva Engage** | Enterprise social network (Yammer) — communities, leadership connection, story posts |

---

## 10. M365 Administration Operations (Relevant to JD4)

### 10.1 Service Health
- **M365 Admin Center → Service Health:** Real-time status of all M365 services
- Incident: A service degradation affecting users (Microsoft's fault)
- Advisory: Something to be aware of (upcoming change, reduced functionality)
- **Before escalating to Microsoft:** Always check Service Health first — may already be a known issue

### 10.2 Message Center
- Microsoft announces upcoming changes, new features, required admin actions
- Filter by service, tag (action required, new feature, updated feature)
- Plan change management based on announced changes

### 10.3 User Lifecycle Management
- **Provision:** Create user account, assign license, add to groups
- **Modify:** Change department, license, manager, add roles
- **Offboard:** Disable account, convert mailbox to shared (retain email), remove licenses, transfer OneDrive files to manager, remove from groups

### 10.4 Support Tiers
- **L1 (Help Desk):** Common issues — password resets, MFA setup, basic Office help, guided by knowledge base
- **L2 (Collaboration Services team like JD4):** M365 service configuration, policy issues, Teams/SharePoint administration, escalations from L1
- **L3 (Microsoft Support):** Platform bugs, service incidents, complex configuration, cases opened via admin portal

### 10.5 Common Troubleshooting Scenarios
| Issue | First Steps |
|---|---|
| User can't sign in | Check account status, password expiry, MFA, Conditional Access block, Sign-in Logs in Entra ID |
| Teams meeting quality issues | Check network connectivity, check M365 Service Health, use Teams Admin Center → Call Quality Dashboard |
| Can't access SharePoint site | Check site permissions, check sharing policies, check if user's license includes SharePoint |
| Email not delivering | Check spam/junk, check message trace in Exchange Admin Center, check DLP policies |
| OneDrive sync issues | Check sync client status, check storage quota, check Conditional Access requiring compliant device |

---

## 11. Key Terminology Quick Reference

| Term | Meaning |
|---|---|
| Entra ID | Microsoft's cloud identity platform (formerly Azure AD) |
| Conditional Access | Policy-based access control (signal → decision → enforcement) |
| MFA | Multi-Factor Authentication |
| SSO | Single Sign-On |
| Tenant | Organization's isolated M365 environment |
| Dataverse | Power Platform's relational data storage |
| Power Fx | Low-code formula language for Power Platform |
| DLP | Data Loss Prevention — prevents sensitive data sharing |
| RLS | Row-Level Security — restrict data rows per user/role |
| DAX | Data Analysis Expressions — Power BI/Power Pivot formula language |
| DirectQuery | Power BI mode querying source in real-time (no data import) |
| CoE | Center of Excellence — governance framework for Power Platform |
| MDM | Mobile Device Management (via Intune) |
| MAM | Mobile Application Management (app-level, for BYOD) |
| eDiscovery | Legal investigation tool to search/hold/export M365 content |
| Purview | Microsoft's unified compliance and data governance platform |
| Safe Links | Defender feature that checks URLs at click-time |
| Safe Attachments | Defender feature that sandboxes email attachments |
| iFlow | Integration Flow — message routing artifact in SAP Integration Suite |
| Teams Sprawl | Uncontrolled proliferation of unused Teams due to lack of governance |
| Hub Site | SharePoint site connecting related sites under shared navigation/branding |
| Retention Policy | Rules defining how long content is kept before deletion |
| Sensitivity Label | Classification tag applied to documents/emails enforcing protection |

---
*End of Microsoft 365 & Power Platform Comprehensive Notes — ~680 lines.*
*Read alongside: Shared_Microsoft365.md and Shared_PowerPlatform.md for MCQ practice.*
