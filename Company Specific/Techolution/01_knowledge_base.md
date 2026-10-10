# 📘 01_Knowledge Base — Project/Product Management Intern Interview Prep

> **How to use this document:** Read it top to bottom once, then use the Q&A sections to self-test. Every major topic is explained from scratch with definitions, real examples, and sample interview answers. This single document covers everything you need to clear a PM/Coordinator intern interview.

---

## TABLE OF CONTENTS

1. [Project Management Fundamentals](#1-project-management-fundamentals)
2. [Project Lifecycle & SDLC](#2-project-lifecycle--sdlc)
3. [Agile Methodology — Deep Dive](#3-agile-methodology--deep-dive)
4. [Scrum Framework — Ceremonies, Roles & Artifacts](#4-scrum-framework--ceremonies-roles--artifacts)
5. [Kanban](#5-kanban)
6. [Project Planning — Schedules, Milestones & Dependencies](#6-project-planning--schedules-milestones--dependencies)
7. [Risk Management](#7-risk-management)
8. [Stakeholder Management & Communication](#8-stakeholder-management--communication)
9. [Documentation — Plans, Reports & Meeting Minutes](#9-documentation--plans-reports--meeting-minutes)
10. [Project Management Tools — Jira, Trello, Asana, Notion](#10-project-management-tools--jira-trello-asana-notion)
11. [Technical Concepts Every PM Intern Should Know](#11-technical-concepts-every-pm-intern-should-know)
12. [Product Management Basics](#12-product-management-basics)
13. [Resource Allocation & Task Prioritization](#13-resource-allocation--task-prioritization)
14. [Cross-Functional Team Coordination](#14-cross-functional-team-coordination)
15. [Soft Skills — Communication, Ownership & Emotional Intelligence](#15-soft-skills--communication-ownership--emotional-intelligence)
16. [AI Product Context](#16-ai-product-context)
17. [Commonly Asked Interview Questions & Model Answers](#17-commonly-asked-interview-questions--model-answers)

---

## 1. Project Management Fundamentals

### What is a Project?

A **project** is a temporary endeavor with a defined start and end, undertaken to create a unique product, service, or result. It is NOT ongoing operations (like customer support running every day). Examples:
- Building a mobile app → Project
- Running the IT helpdesk daily → Operations
- Launching a new AI feature → Project

### What is Project Management?

**Project management** is the discipline of planning, organizing, executing, controlling, and closing work to achieve specific goals within a given time, budget, and scope.

The classic "Iron Triangle" or "Triple Constraint" of project management:

```
         SCOPE
          /\
         /  \
        /    \
       /______\
    TIME      COST
```

- **Scope** — What exactly are we building or delivering?
- **Time** — By when must it be done?
- **Cost** — What is the budget (money, people, resources)?

> **Key Insight:** If you change one side of the triangle, it affects the others. Example: If a stakeholder expands the scope (more features), you either need more time, more budget, or you compromise quality.

A fourth element often added is **Quality** — the standard to which the deliverable must meet.

### Key PM Terminology (Glossary)

| Term | Simple Definition |
|------|-----------------|
| **Deliverable** | A tangible output — e.g., a working login feature, a status report |
| **Milestone** | A significant checkpoint — e.g., "Beta Launch: June 15" |
| **Dependency** | Task B cannot start until Task A finishes |
| **Blocker** | Something stopping progress right now |
| **Scope Creep** | Uncontrolled expansion of project scope without adjusting time/cost |
| **Stakeholder** | Anyone impacted by or having an interest in the project |
| **Escalation** | Raising an issue to someone with higher authority to resolve it |
| **Action Item** | A specific task assigned to a person with a due date |
| **KPI** | Key Performance Indicator — a measurable value showing progress |
| **Baseline** | The original approved plan (schedule, cost, scope) |
| **Change Request** | A formal proposal to modify scope, timeline, or budget |
| **Status Report** | A periodic update on project progress, risks, and issues |

### Project Manager vs. Project Coordinator vs. Product Manager

| Role | Responsibility |
|------|---------------|
| **Project Manager** | Owns the entire project. Accountable for delivery. Makes decisions. |
| **Project Coordinator (Intern)** | Supports the PM — tracks tasks, schedules meetings, maintains documentation, follows up |
| **Product Manager** | Defines WHAT to build and WHY. Owns the product vision and roadmap. Works with business + users. |
| **Program Manager** | Manages multiple related projects together (a "program") |

> **As an intern, you will be in the Coordinator role** — assisting, tracking, documenting, following up, and coordinating — not making final decisions.

---

## 2. Project Lifecycle & SDLC

### The 5 Phases of a Project (PMI Standard)

Every project, regardless of methodology, goes through these phases:

#### Phase 1: Initiation
- Define the project at a high level
- Identify stakeholders
- Create a **Project Charter** (a document authorizing the project)
- Answer: *Why are we doing this? Is it worth doing?*

#### Phase 2: Planning
- Break down work (Work Breakdown Structure)
- Create a schedule (Gantt chart or sprint plan)
- Identify risks
- Define communication plan
- Answer: *How are we going to do this? Who does what by when?*

#### Phase 3: Execution
- Teams do the actual work
- PM coordinates, communicates, removes blockers
- Answer: *Let's build it!*

#### Phase 4: Monitoring & Controlling
- Track progress against the plan
- Manage changes, risks, issues
- Hold status meetings, update dashboards
- Answer: *Are we on track? What's slipping?*

> Note: Phase 3 and Phase 4 happen **simultaneously**. While the team executes, the PM monitors.

#### Phase 5: Closure
- Final deliverable handed over
- Lessons learned documented
- Project formally closed
- Answer: *Did we achieve what we set out to do? What did we learn?*

---

### Software Development Life Cycle (SDLC)

The **SDLC** is specifically how a software product gets built — from idea to deployment. It is a subset of the project lifecycle applied to software.

#### Classic SDLC Phases:

```
[1. Requirements] → [2. System Design] → [3. Implementation/Coding] 
→ [4. Testing/QA] → [5. Deployment] → [6. Maintenance]
```

**1. Requirements Gathering**
- Understand what the users/business need
- Outputs: Business Requirements Document (BRD), User Stories
- Who's involved: Business Analyst, PM, Product Manager, Stakeholders

**2. System Design**
- Translate requirements into technical architecture
- Outputs: High-Level Design (HLD), Low-Level Design (LLD), Database schema, API contracts
- Who's involved: Architects, Senior Engineers, Tech Leads

**3. Implementation (Coding)**
- Developers write code
- Frontend, Backend, Database layers are built
- Who's involved: Software Engineers

**4. Testing / QA**
- Code is tested for bugs, performance issues, security vulnerabilities
- Types: Unit Testing, Integration Testing, System Testing, User Acceptance Testing (UAT)
- Who's involved: QA Engineers, Test Leads

**5. Deployment**
- Code is released to production (real users see it)
- May involve staging environments → UAT → Production
- Who's involved: DevOps, Release Managers, Engineers

**6. Maintenance**
- Bug fixes, updates, performance improvements post-launch
- Ongoing phase

---

### Waterfall vs. Agile SDLC

| Aspect | Waterfall | Agile |
|--------|-----------|-------|
| **Approach** | Sequential — one phase finishes before next starts | Iterative — work in short cycles |
| **Flexibility** | Rigid — changes are hard and expensive | Flexible — changes are welcomed |
| **Customer Involvement** | At start (requirements) and end (delivery) | Continuous throughout |
| **Delivery** | One big release at the end | Small, frequent releases (each sprint) |
| **Best For** | Fixed-scope projects (construction, defense) | Software, products with evolving requirements |
| **Risk** | High — problems found late | Low — problems found early each sprint |

**Real example:** Building a bridge → Waterfall (you can't change the design midway). Building a mobile app → Agile (user feedback shapes the product continuously).

---

## 3. Agile Methodology — Deep Dive

### What is Agile?

**Agile** is not a specific process — it is a **mindset and set of values** for software development, defined in the **Agile Manifesto (2001)**.

The 4 Agile Values:
1. **Individuals and interactions** over processes and tools
2. **Working software** over comprehensive documentation
3. **Customer collaboration** over contract negotiation
4. **Responding to change** over following a plan

The keyword is "over" — the right side still matters, but the left side is valued more.

### The 12 Agile Principles (Simplified)

You don't need to memorize all 12, but understand the core ideas:
- Deliver working software frequently (every few weeks)
- Welcome changing requirements even late in development
- Business people and developers work together daily
- Build projects around motivated individuals and trust them
- Working software is the primary measure of progress
- Sustainable pace — teams should be able to maintain the pace indefinitely
- Simplicity — maximize the amount of work NOT done (don't over-build)
- Teams self-organize
- Regularly reflect and adjust behavior

### Agile Frameworks

Agile is the philosophy. The **frameworks** are specific ways to implement it:

| Framework | Best For | Key Feature |
|-----------|----------|-------------|
| **Scrum** | Most software teams | Fixed-length sprints, defined roles |
| **Kanban** | Continuous flow work (support, ops) | Visual board, WIP limits |
| **SAFe** | Large enterprises scaling Agile | Multiple teams, program increments |
| **XP (Extreme Programming)** | Engineering-focused teams | TDD, pair programming |

For your interview, focus on **Scrum** and **Kanban** — these are the most commonly used.

---

## 4. Scrum Framework — Ceremonies, Roles & Artifacts

### What is Scrum?

**Scrum** is the most popular Agile framework. It organizes work into fixed-length iterations called **Sprints** (usually 1–4 weeks, most commonly 2 weeks).

Think of it like this: instead of planning everything for 6 months and then building it all at once, Scrum says "let's plan 2 weeks of work, build it, review it, and then plan the next 2 weeks."

---

### Scrum Roles (3 Roles)

#### 1. Product Owner (PO)
- Represents the customer/business
- Owns the **Product Backlog** (the prioritized list of everything to build)
- Decides WHAT gets built next (priority)
- Says "yes" or "no" to what goes into a sprint
- **Single point of accountability** for the product

#### 2. Scrum Master (SM)
- A servant-leader and coach for the team
- Facilitates Scrum ceremonies
- Removes impediments (blockers) for the team
- Ensures the team follows Scrum principles
- Does NOT manage people — they enable the team

#### 3. Development Team (Developers)
- Cross-functional — includes engineers, QA, designers
- Self-organizing — they decide HOW to build the work
- Typically 3–9 people
- Commits to sprint work and delivers it

> **As a PM intern**, you work closest to the Project Manager or Program Manager who may play a role similar to the Scrum Master in coordinating and tracking.

---

### Scrum Artifacts (3 Artifacts)

#### 1. Product Backlog
- The **master list** of all features, bug fixes, enhancements, and work for the product
- Owned and prioritized by the Product Owner
- Items are called **User Stories** or **Backlog Items**
- Never complete — always evolving
- Example items:
  - "As a user, I want to log in with Google so that I don't have to remember a password"
  - "Fix the crash on the checkout page"
  - "Add dark mode to the app"

#### 2. Sprint Backlog
- The subset of Product Backlog items the team commits to completing **in the current sprint**
- Created during Sprint Planning
- Owned by the Development Team
- Should not change mid-sprint (ideally)

#### 3. Increment
- The **working, potentially shippable product** at the end of each sprint
- It must be "Done" according to the **Definition of Done (DoD)**
- Example: After Sprint 3, the user registration feature is complete, tested, and ready to ship

---

### Scrum Ceremonies (5 Events)

#### 1. Sprint Planning
- **When:** At the start of each sprint
- **Duration:** ~4 hours for a 2-week sprint
- **Who:** Entire Scrum team
- **Purpose:** Decide what work will be done in this sprint and how
- **Output:** Sprint Backlog (the plan for the sprint)
- **What happens:**
  - PO presents top-priority backlog items
  - Team discusses, asks questions, estimates effort
  - Team selects items they can complete in the sprint
  - Team breaks items into tasks

#### 2. Daily Scrum (Stand-up)
- **When:** Every day, same time
- **Duration:** 15 minutes maximum
- **Who:** Development Team (Scrum Master facilitates, PO optional)
- **Purpose:** Synchronize work, identify blockers
- **The classic 3 questions each person answers:**
  1. What did I do yesterday?
  2. What will I do today?
  3. Is there anything blocking me?
- **Key Rule:** It is NOT a status report to the manager. It's the team talking to each other.

#### 3. Sprint Review
- **When:** At the end of each sprint
- **Duration:** ~2 hours for a 2-week sprint
- **Who:** Scrum team + stakeholders
- **Purpose:** Demonstrate completed work, gather feedback
- **What happens:**
  - Team demos what was built
  - Stakeholders give feedback
  - PO decides what to accept/reject
  - Product Backlog may be updated based on feedback

#### 4. Sprint Retrospective
- **When:** After Sprint Review, end of each sprint
- **Duration:** ~1.5 hours
- **Who:** Scrum team only (no external stakeholders)
- **Purpose:** Reflect on the PROCESS, not the product. How can we work better?
- **Classic format — "What went well / What didn't / What to improve":**
  - ✅ What went well? (Keep doing)
  - ❌ What didn't go well? (Stop doing)
  - 💡 What should we try next sprint? (Start doing / Improve)
- **Output:** Action items to improve team process

#### 5. Backlog Refinement (Grooming)
- **When:** Mid-sprint, ongoing
- **Duration:** ~1–2 hours per sprint
- **Who:** PO + Development Team (Scrum Master facilitates)
- **Purpose:** Clarify, estimate, and prioritize upcoming backlog items
- **What happens:**
  - PO presents upcoming items
  - Team asks clarifying questions
  - Stories are estimated (story points)
  - Items are broken down or merged

---

### Story Points & Velocity

**Story Points** are a relative unit of effort estimation — they account for complexity, risk, and effort combined (NOT just time).

Common scale: Fibonacci sequence → 1, 2, 3, 5, 8, 13, 21

- 1 point = trivial task (e.g., change button color)
- 3 points = small but requires some thought
- 5 points = medium complexity
- 8 points = complex, some unknowns
- 13+ points = too big, should be broken down

**Velocity** = the average number of story points a team completes per sprint. If a team consistently completes 30 points per sprint, that's their velocity, used to forecast future sprints.

---

### Definition of Done (DoD)

A shared checklist a team uses to confirm a story is truly "done." Examples:
- Code written and peer-reviewed
- Unit tests written and passing
- QA testing passed
- Documentation updated
- Deployed to staging environment
- Product Owner accepted the story

---

### User Story Format

```
As a [type of user],
I want [a goal/feature],
So that [a reason/benefit].
```

**Example:**
```
As a registered user,
I want to reset my password via email,
So that I can regain access to my account if I forget my password.
```

**Acceptance Criteria** (how you know the story is done):
- Given: I am on the login page and click "Forgot Password"
- When: I enter my registered email and submit
- Then: I receive a password reset email within 2 minutes

---

## 5. Kanban

### What is Kanban?

**Kanban** (Japanese for "visual signal") is an Agile framework that focuses on **visualizing work** and **limiting work in progress (WIP)** to improve flow and efficiency.

Unlike Scrum, Kanban has no sprints, no fixed roles, no time-boxes. Work flows continuously.

### Kanban Board

A Kanban board is a visual tool showing work status:

```
| TO DO | IN PROGRESS | IN REVIEW | DONE |
|-------|-------------|-----------|------|
| Task A| Task C      | Task E    | Task G|
| Task B| Task D      |           | Task H|
```

Each card (task/story) moves left to right as it progresses.

### WIP Limits

**WIP (Work in Progress) Limits** are maximum numbers of tasks allowed in a column at one time.

Example: "In Progress" column has a WIP limit of 3 — only 3 tasks can be in progress at once. If a 4th needs to be started, the team must first finish something in progress.

**Why WIP limits?** They prevent multitasking overload and encourage finishing tasks before starting new ones.

### Scrum vs. Kanban — Quick Comparison

| Feature | Scrum | Kanban |
|---------|-------|--------|
| Time-boxed | Yes (Sprints) | No (Continuous flow) |
| Roles | PO, SM, Dev Team | No prescribed roles |
| Ceremonies | Sprint Planning, Stand-up, Review, Retro | Stand-up (optional) |
| Change mid-cycle | Not recommended | Anytime |
| Best for | Feature development teams | Support, ops, maintenance teams |
| Metrics | Velocity, Burndown | Lead time, Cycle time, Throughput |

---

## 6. Project Planning — Schedules, Milestones & Dependencies

### Work Breakdown Structure (WBS)

A **WBS** breaks a project into smaller, manageable pieces hierarchically.

```
PROJECT: Launch Mobile App
├── 1. Discovery & Planning
│   ├── 1.1 Requirements gathering
│   ├── 1.2 Competitive analysis
│   └── 1.3 Project plan creation
├── 2. Design
│   ├── 2.1 Wireframes
│   ├── 2.2 UI mockups
│   └── 2.3 Design review & approval
├── 3. Development
│   ├── 3.1 Backend APIs
│   ├── 3.2 Frontend development
│   └── 3.3 Integration
├── 4. Testing
│   ├── 4.1 Unit testing
│   ├── 4.2 QA testing
│   └── 4.3 UAT
└── 5. Launch
    ├── 5.1 App store submission
    └── 5.2 Marketing announcement
```

### Gantt Chart

A **Gantt chart** is a horizontal bar chart showing:
- Tasks on the Y-axis
- Timeline on the X-axis
- Bars showing task duration
- Dependencies between tasks shown by arrows

Example:
```
Task               Week1  Week2  Week3  Week4  Week5
Requirements       [====]
Design                    [========]
Development                      [===========]
Testing                                  [====]
Launch                                        [=]
```

### Milestones

A **milestone** is a significant event or checkpoint in a project — it has zero duration (it's a point in time, not a task).

Examples:
- "Requirements Approved" — May 1
- "Beta Version Ready" — June 15
- "Product Launch" — July 30

Milestones are used to track whether the project is on schedule at a high level.

### Dependencies

A **dependency** defines the relationship between tasks — which task must happen before another can start.

Types of dependencies:
- **Finish-to-Start (FS):** Task B starts only after Task A finishes *(most common)*
  - "Coding can start only after design is approved"
- **Start-to-Start (SS):** Task B starts when Task A starts
  - "Testing can start as soon as development starts (on completed portions)"
- **Finish-to-Finish (FF):** Task B finishes when Task A finishes
  - "Documentation finishes when coding finishes"

### Critical Path

The **Critical Path** is the longest sequence of dependent tasks that determines the minimum project duration. Any delay on the critical path delays the entire project.

Non-critical tasks have **float/slack** — extra time without delaying the project.

> **Interview tip:** You don't need to calculate critical paths, but understand the concept — delays on the critical path are the most dangerous and must be escalated immediately.

---

## 7. Risk Management

### What is a Risk?

A **risk** is an uncertain event that, IF it occurs, has a positive or negative effect on project objectives. Most commonly we focus on **negative risks (threats)**.

Difference:
- **Risk** = Something that MIGHT happen (uncertain)
- **Issue** = Something that HAS happened (certain, needs to be managed now)

### Risk Components

**Risk = Probability × Impact**

- **Probability:** How likely is the risk to occur? (Low / Medium / High)
- **Impact:** How bad would it be if it occurs? (Low / Medium / High)

### Risk Register

A **Risk Register** is a document/tracker listing all identified risks.

| Risk ID | Risk Description | Probability | Impact | Risk Score | Owner | Mitigation Plan | Status |
|---------|-----------------|-------------|--------|------------|-------|-----------------|--------|
| R-001 | Key developer may leave | Medium | High | High | PM | Cross-train team members | Open |
| R-002 | API from 3rd party delayed | High | High | Critical | Tech Lead | Build mock API for dev | Open |
| R-003 | Budget cut by 20% | Low | High | Medium | PM | Identify low-priority features to cut | Open |

### Risk Response Strategies

| Strategy | Meaning | Example |
|----------|---------|---------|
| **Avoid** | Change the plan to eliminate the risk | Don't use an unstable 3rd-party API — build it yourself |
| **Mitigate** | Reduce probability or impact | Add buffer time; cross-train team members |
| **Transfer** | Shift risk to another party | Buy insurance; use a vendor SLA |
| **Accept** | Acknowledge and monitor (for low risks) | "If the UI takes an extra day, we'll adjust" |

### Escalation

**Escalation** means raising an issue or risk to someone with higher authority or more resources to resolve it.

**When to escalate:**
- Risk or issue is beyond your authority to resolve
- It will impact timeline, scope, or budget significantly
- You've tried to resolve it at your level and can't

**How to escalate (as an intern):**
1. Don't just dump the problem — come with context
2. State: What is the issue, what's the impact, what have you tried, what you recommend
3. Escalate to your direct manager first
4. Do it in a timely manner — don't wait until it's too late

> **Example:** "The design deliverable was due Friday and the designer hasn't submitted it. This blocks the dev team starting Monday. I've followed up twice with no response. I wanted to flag this to you so we can decide whether to get a backup resource or adjust the sprint plan."

---

## 8. Stakeholder Management & Communication

### Who is a Stakeholder?

A **stakeholder** is anyone who:
- Is affected by the project's outcome, OR
- Has interest or influence over the project

Types of stakeholders:
- **Internal:** Project team, engineering, design, QA, marketing, leadership
- **External:** Customers, vendors, partners, regulators

### Stakeholder Analysis (Power/Interest Grid)

Categorize stakeholders based on power (ability to influence) and interest (how affected they are):

```
HIGH POWER │ Manage Closely │   Keep Satisfied
           │ (Eng Manager)  │   (CEO)
           ├────────────────┼───────────────────
LOW POWER  │ Monitor        │   Keep Informed
           │ (Junior Dev)   │   (End User)
           └────────────────┴───────────────────
              LOW INTEREST      HIGH INTEREST
```

- **Manage Closely:** High power, High interest — engage frequently, involve in decisions
- **Keep Satisfied:** High power, Low interest — update regularly but don't overload
- **Keep Informed:** Low power, High interest — send regular updates
- **Monitor:** Low power, Low interest — minimal engagement

### Communication Plan

A **Communication Plan** defines who gets what information, how often, and through which channel.

| Stakeholder | Information | Frequency | Channel | Owner |
|-------------|-------------|-----------|---------|-------|
| Engineering Team | Sprint updates, blockers | Daily | Slack stand-up | Scrum Master |
| Product Manager | Feature status, risks | Weekly | Status report | PM Intern |
| CEO / Leadership | Project health, milestones | Bi-weekly | Dashboard/deck | Project Manager |
| Client | Progress, deliverables | Monthly | Email report | PM |

### Meeting Best Practices

As a PM intern, you'll often **prepare agendas and document minutes**:

**Meeting Agenda Template:**
```
Meeting: Sprint Review — Sprint 5
Date: [Date] | Time: [Time] | Location: [Zoom link]
Attendees: [List]

Agenda:
1. Review Sprint 5 goals vs. actuals (10 min)
2. Demo: Feature A (15 min)
3. Demo: Feature B (10 min)
4. Stakeholder feedback (10 min)
5. Preview of Sprint 6 goals (5 min)
6. Q&A (10 min)
```

**Meeting Minutes Template:**
```
Meeting Minutes — Sprint Review, Sprint 5
Date: [Date] | Attendees: [Names]

Summary: [2-3 sentence overview]

Key Decisions:
- [Decision 1]
- [Decision 2]

Action Items:
| # | Action Item | Owner | Due Date | Status |
|---|-------------|-------|----------|--------|
| 1 | Fix login bug from demo | Rahul (Dev) | June 20 | Open |
| 2 | Update UI mockup for feedback | Priya (Design) | June 22 | Open |

Next Meeting: [Date and agenda preview]
```

---

## 9. Documentation — Plans, Reports & Meeting Minutes

### Status Report

A **status report** is a regular update given to stakeholders about project health.

**Common Format — RAG Status:**
- 🟢 **Green:** On track, no issues
- 🟡 **Amber/Yellow:** Minor issues, manageable with attention
- 🔴 **Red:** Significant issue, needs immediate action

**Status Report Template:**
```
Project: AI Chatbot MVP
Report Date: June 20, 2025
Overall Status: 🟡 AMBER

Summary:
Sprint 4 completed 8/10 stories. 2 stories carried over to Sprint 5.
Development is on track. QA phase is at risk due to environment issues.

Progress:
- Completed: Login, User Profile, API integration
- In Progress: Payment module (80%), Notification system (40%)
- Not Started: Admin dashboard, Analytics

Risks & Issues:
- 🔴 QA environment is down — blocking testing of payment module
  Action: DevOps to fix by June 22 (Escalated to Arun)

Key Milestones:
- Beta Release: July 10 (🟡 At Risk — 5-day delay possible)
- Production Launch: Aug 1 (🟢 On track)

Next Steps:
- Resolve QA environment issue
- Begin Admin dashboard design
- Sprint 5 Planning — June 23
```

### Project Charter

A **Project Charter** formally authorizes a project. It includes:
- Project name and description
- Objectives and success criteria
- Scope (what's in, what's out)
- Key stakeholders
- High-level timeline and budget
- Project Manager name and authority

### RACI Matrix

A **RACI Matrix** clarifies roles for each task or deliverable:
- **R — Responsible:** Does the work
- **A — Accountable:** Ultimately answerable; approves the work (only ONE person)
- **C — Consulted:** Provides input, expertise
- **I — Informed:** Kept updated but no active role

| Task | PM Intern | Project Manager | Tech Lead | Design Lead |
|------|-----------|----------------|-----------|-------------|
| Requirements doc | R | A | C | C |
| Sprint plan | C | A | R | I |
| UI mockups | I | A | C | R |
| Status report | R | A | I | I |

---

## 10. Project Management Tools — Jira, Trello, Asana, Notion

### Jira

**Jira** (by Atlassian) is the most widely used project management tool in software development. It is built around Agile concepts.

**Key Concepts in Jira:**
- **Project:** Top-level container (e.g., "Mobile App Project")
- **Issue:** Any unit of work — can be an Epic, Story, Task, Sub-task, or Bug
- **Epic:** A large body of work spanning multiple sprints (e.g., "User Authentication System")
- **Story:** A user-facing feature described as a user story (lives inside an Epic)
- **Task:** A piece of work not necessarily user-facing (e.g., "Set up CI/CD pipeline")
- **Sub-task:** A smaller piece of a Story or Task
- **Bug:** A defect found during testing
- **Sprint:** A time-boxed iteration in Jira's board
- **Board:** A visual Kanban/Scrum board showing issue status
- **Backlog:** The list of all unstarted issues in the project

**Issue Hierarchy:**
```
Epic → Story → Sub-task
              → Task → Sub-task
              → Bug
```

**Common Jira Fields:**
- **Summary:** Short description of the issue
- **Assignee:** Who is responsible
- **Reporter:** Who created the issue
- **Priority:** Critical / High / Medium / Low
- **Status:** To Do / In Progress / In Review / Done
- **Story Points:** Effort estimate
- **Sprint:** Which sprint it belongs to
- **Labels/Components:** Categorization tags

**As a PM intern in Jira, you'll likely:**
- Create and update issues
- Move issues between statuses
- Add comments and tag team members
- Monitor sprint progress on the board
- Generate sprint reports

---

### Trello

**Trello** is a simpler, visual Kanban-style tool. Great for smaller teams or less complex projects.

**Key Concepts in Trello:**
- **Board:** The project workspace
- **List:** A column representing a stage (e.g., To Do, In Progress, Done)
- **Card:** A task/item that moves between lists
- **Members:** People assigned to a card
- **Labels:** Color-coded tags (e.g., Red = Urgent, Blue = Frontend)
- **Due Dates:** Deadlines on cards
- **Checklist:** Sub-tasks within a card
- **Power-Ups:** Integrations (e.g., calendar view, Slack notifications)

Trello is intuitive and visual — great for getting started quickly.

---

### Asana

**Asana** is a work management tool with both list and board views.

**Key Concepts in Asana:**
- **Workspace/Organization:** Top-level account
- **Project:** A collection of tasks (can be shown as list, board, calendar, timeline)
- **Task:** A unit of work with assignee, due date, description
- **Subtask:** Nested tasks within a task
- **Section:** Group tasks into categories within a project
- **Timeline View:** Gantt-like view showing task schedules
- **Portfolio:** Manage multiple projects in one view (for managers)
- **Rules:** Automation (e.g., "When task is moved to Done, notify the PM")

---

### Notion

**Notion** is an all-in-one workspace for documentation, databases, and project tracking.

**Key Concepts in Notion:**
- **Page:** A document that can contain anything — text, tables, databases, embeds
- **Database:** A collection of pages (can be viewed as table, board, calendar, gallery, list)
- **Properties:** Fields on database items (like columns) — text, date, select, person
- **Templates:** Pre-built page/database structures
- **Views:** Different ways to look at the same data (board = Kanban, calendar, table)

**Common PM use cases in Notion:**
- Meeting notes wiki
- Project status tracker database
- Sprint planning board
- Requirement documentation
- Team handbook / onboarding docs

---

### Tool Comparison Summary

| Feature | Jira | Trello | Asana | Notion |
|---------|------|--------|-------|--------|
| **Best For** | Software/Agile teams | Simple projects, small teams | Mid-size teams, task-heavy | Documentation + light PM |
| **Complexity** | High | Low | Medium | Medium |
| **Agile Support** | Native (Scrum + Kanban) | Kanban only | Basic | Via templates |
| **Reporting** | Robust (burndowns, velocity) | Minimal | Moderate | Manual |
| **Free Tier** | Yes (limited) | Yes | Yes | Yes |

---

## 11. Technical Concepts Every PM Intern Should Know

You don't need to code, but you need to understand these concepts to talk to engineers and understand project discussions.

### Frontend vs. Backend vs. Full Stack

- **Frontend:** The part users see and interact with — the UI/UX. Built with HTML, CSS, JavaScript, React, etc.
- **Backend:** Server-side logic, databases, APIs — what powers the frontend. Built with Node.js, Python, Java, etc.
- **Database:** Where data is stored. Types: relational (SQL — MySQL, PostgreSQL) and non-relational (NoSQL — MongoDB)
- **Full Stack:** A developer who works on both frontend and backend

**Example:** An e-commerce website:
- Frontend = Product pages, cart UI, checkout form (what you see)
- Backend = Order processing logic, inventory management, payment processing
- Database = User accounts, product catalog, order history

---

### API (Application Programming Interface)

An **API** is a set of rules that allows one software application to talk to another.

**Simple analogy:** A restaurant menu is like an API. You (the frontend/client) look at the menu and order items. The kitchen (the backend/server) prepares them. You don't need to know how the food is made — you just send a request and get a response.

**REST API:** The most common type. Uses HTTP methods:
- **GET** — Retrieve data (e.g., "Get me user profile #123")
- **POST** — Create data (e.g., "Create a new user account")
- **PUT/PATCH** — Update data (e.g., "Update this user's email")
- **DELETE** — Delete data (e.g., "Delete this user account")

**Why PM cares:** When you hear "the API is not ready," it means one team can't start work because the interface between their system and another isn't defined or built yet. This is a **dependency** and a potential **blocker**.

---

### Database Basics

- **SQL Database (Relational):** Data in structured tables with rows and columns. Uses SQL language. Examples: MySQL, PostgreSQL, SQLite.
- **NoSQL Database (Non-relational):** Flexible schema, stores data as documents, key-value, or graphs. Examples: MongoDB, Firebase.

**When a developer says:** "We need to finalize the database schema" — they mean they need to define what data will be stored and how it's organized. This is a design decision made early in the project.

---

### Testing Types (QA)

| Type | What It Tests | Who Does It |
|------|--------------|-------------|
| **Unit Testing** | Individual functions/components in isolation | Developer |
| **Integration Testing** | Multiple components working together | Developer / QA |
| **System Testing** | End-to-end testing of the whole system | QA Team |
| **Regression Testing** | Ensures new changes don't break existing features | QA Team |
| **UAT (User Acceptance Testing)** | Real users validate the product meets requirements | Business/Client |
| **Performance Testing** | Speed, load handling under stress | QA / DevOps |
| **Security Testing** | Vulnerabilities, penetration testing | Security Team / QA |

---

### Deployment & Environments

**Environments** are separate setups of the application at different stages:

```
Development (Dev) → Staging (UAT) → Production (Prod)
```

- **Dev:** Where developers build and test their code locally or on a shared dev server
- **Staging:** A mirror of production where QA and UAT testing happens before release
- **Production (Prod):** The live environment that real users use

**Deployment** = releasing code from one environment to the next. When engineers say "we pushed to prod," they released new code to real users.

---

### CI/CD (Continuous Integration / Continuous Delivery)

- **Continuous Integration (CI):** Every time a developer pushes code, automated tests run immediately to catch bugs early.
- **Continuous Delivery (CD):** Code that passes CI can be automatically deployed to staging or production.

**Why PM cares:** CI/CD reduces manual deployment delays. If CI is broken, the team can't deploy — this is a blocker to escalate.

---

### Version Control (Git)

**Git** is a version control system that tracks code changes.
- **Repository (Repo):** Where the code lives (GitHub, GitLab, Bitbucket)
- **Branch:** A separate copy of the code for a feature or fix
- **Commit:** Saving changes to the branch
- **Pull Request (PR) / Merge Request (MR):** Request to merge a branch's changes into the main codebase (requires code review)
- **Merge:** Combining code branches

**Why PM cares:** PRs need to be reviewed and approved. If PRs are piling up unreviewed, it's a bottleneck. The team's "code review" process is a project dependency.

---

### Cloud Basics

- **AWS (Amazon Web Services), GCP (Google Cloud), Azure (Microsoft):** Major cloud providers — they host servers, databases, storage
- **Serverless:** Computing where you don't manage servers — functions run on demand
- **Container / Docker:** A lightweight way to package and run applications consistently across environments
- **Kubernetes (K8s):** A system to manage large numbers of containers

**For a PM intern:** Know these exist. When engineers discuss "cloud costs," it affects the project budget. When they say "we're deploying to AWS," that's where the product lives.

---

## 12. Product Management Basics

### Product vs. Project Management

| Aspect | Product Management | Project Management |
|--------|-------------------|-------------------|
| **Focus** | WHAT to build & WHY (strategy) | HOW to build it & by WHEN (execution) |
| **Time Horizon** | Long-term (product life) | Temporary (project end) |
| **Success Metric** | Business outcomes, user adoption | On-time, on-budget delivery |
| **Key Document** | Product Roadmap, PRD | Project Plan, Gantt Chart |
| **Role** | Product Manager | Project Manager |

In practice, for this role you'll do both — planning execution AND understanding what needs to be built.

---

### Product Roadmap

A **product roadmap** is a high-level visual plan showing what the product will do over time. It communicates the product direction to stakeholders.

Example:
```
Q1 2025: User Authentication + Onboarding
Q2 2025: Core Feature (AI Recommendations)
Q3 2025: Analytics Dashboard + Reporting
Q4 2025: Integrations (Slack, Zapier) + Mobile App
```

---

### Product Requirements Document (PRD)

A **PRD** describes what a product feature or product should do. It bridges the gap between business needs and engineering.

Typical PRD sections:
- **Overview:** What we're building and why
- **Problem Statement:** What user pain are we solving?
- **Goals & Success Metrics:** How do we know it worked?
- **User Stories:** Who wants what and why
- **Functional Requirements:** What the system must do
- **Non-Functional Requirements:** Performance, security, scalability
- **Out of Scope:** What we are NOT building
- **Dependencies:** What other teams/systems are involved
- **Open Questions:** Things still to be decided

---

### MoSCoW Prioritization

A framework to prioritize requirements:
- **M — Must Have:** Non-negotiable, core to launch
- **S — Should Have:** Important but not critical for launch
- **C — Could Have:** Nice to have if time permits
- **W — Won't Have (this time):** Out of scope for now

**Example (Login Feature):**
- Must: Email/password login
- Should: Google/Facebook OAuth
- Could: Biometric login
- Won't: QR code login (v2 feature)

---

## 13. Resource Allocation & Task Prioritization

### Resource Allocation

**Resource allocation** means assigning the right people (and tools/budget) to the right tasks at the right time.

Key considerations:
- **Capacity:** How many hours can each team member contribute?
- **Availability:** Are they on other projects or on leave?
- **Skills:** Does the task require specific expertise?
- **Dependencies:** Can this task even start if another isn't done?

**Over-allocation** = A person is assigned more work than they can handle → leads to burnout and delays.
**Under-allocation** = Resource is idle → inefficiency.

As a PM intern, you track this by:
- Maintaining a resource/capacity tracker in Excel or Sheets
- Noting in standup if someone seems overloaded
- Flagging to the PM if resource constraints may impact the timeline

---

### Task Prioritization Frameworks

#### Eisenhower Matrix (Urgent vs. Important)

```
             URGENT     |   NOT URGENT
IMPORTANT  | Do Now     | Schedule It
           | (Crisis,   | (Planning,
           | deadlines) | strategy)
-----------+------------+-------------
NOT        | Delegate   | Eliminate
IMPORTANT  | (Some      | (Busywork,
           | meetings)  | distractions)
```

#### Priority Levels (Typical in PM tools)

- **P0 / Critical:** Drop everything. Production is down, major bug, launch blocker.
- **P1 / High:** Must be done this sprint/week. Significant impact.
- **P2 / Medium:** Important but not urgent. Next sprint.
- **P3 / Low:** Nice to have. Backlog.

---

## 14. Cross-Functional Team Coordination

### Who Is on a Tech/AI Product Team?

| Role | What They Do | PM Interacts By... |
|------|--------------|--------------------|
| **Product Manager** | Defines what to build | Aligning on priorities, scope |
| **Engineering / Developers** | Write code | Tracking sprint progress, removing blockers |
| **QA Engineers** | Test the product | Coordinating test cycles, tracking bug fixes |
| **UX/UI Designers** | Design user interface | Reviewing design timelines, approvals |
| **Data Scientists / ML Engineers** | Build AI models | Understanding model development timelines |
| **DevOps** | Infrastructure, deployment | Tracking environment readiness, deployment schedules |
| **Marketing** | Promote the product | Aligning on launch dates |
| **Business Analyst (BA)** | Gathers & documents requirements | Collaborating on requirements docs |

### Tips for Cross-Functional Coordination (as an intern)

1. **Know your audience:** Technical vs. non-technical — adjust your language
2. **Be a connector:** If Design needs input from Engineering, facilitate that conversation
3. **Document everything:** If a decision is made verbally, write it down and share it
4. **Follow up proactively:** Don't wait for things to come to you
5. **Respect different working styles:** Engineers may prefer async; designers may want real-time feedback sessions

---

## 15. Soft Skills — Communication, Ownership & Emotional Intelligence

### Communication Skills for a PM Intern

**Written Communication:**
- Emails should be clear, concise, and action-oriented
- Use bullet points for updates; paragraphs for context
- Always state: What happened → What it means → What's needed next

**Verbal Communication:**
- In meetings, speak clearly and summarize decisions before closing
- Ask clarifying questions — never assume
- Practice active listening: don't just wait to speak

**Status Updates (The "So What" Rule):**
Bad: "Sprint 4 completed 8 stories and 2 are carried over."
Good: "Sprint 4 completed 8/10 stories. The 2 carried-over stories are on the critical path for the beta launch. We're tracking a 3-day risk to the timeline and I've flagged this with the PM."

---

### Ownership & Accountability

**Ownership** means treating the project as if it's your own, even as an intern:
- Don't wait to be told — anticipate what's needed
- If you notice a risk, flag it. Don't wait for someone else to.
- If you made a mistake, own it, explain what you'll do to fix it, and fix it
- Follow through on action items — if you said you'll do it by Friday, do it

---

### Emotional Intelligence (EQ) in a Team

**EQ** = The ability to recognize, understand, and manage your own emotions and recognize others' emotions.

Why it matters in PM:
- Teams have pressure, disagreements, and stress
- A PM coordinates diverse personalities
- You must stay calm when things go wrong
- You need to empathize with teammates who are blocked or overworked

Key EQ behaviors:
- **Self-awareness:** Know when you're stressed and manage it
- **Empathy:** "I understand this QA environment issue is frustrating — let's work on getting it resolved."
- **Conflict resolution:** Address disagreements calmly and focus on solutions, not blame
- **Positive framing:** "We have a blocker, but here's what we can do..."

---

## 16. AI Product Context

Since this role is specifically for **AI product initiatives**, understand these basics:

### What is an AI Product?

An AI product uses machine learning or artificial intelligence to deliver value to users. Examples:
- AI chatbot (ChatGPT, customer support bots)
- Recommendation systems (Netflix, Spotify)
- AI-powered search (Google)
- Computer vision products (facial recognition, medical imaging)
- Predictive analytics (forecasting sales, churn prediction)

### AI Development — Key Terms for PMs

| Term | Simple Definition |
|------|-----------------|
| **Machine Learning (ML)** | A type of AI where computers learn from data without explicit programming |
| **Model** | The trained AI/ML algorithm that makes predictions or decisions |
| **Training Data** | The dataset used to teach the ML model |
| **Model Training** | The process of feeding data to the algorithm to build the model |
| **Inference** | Using a trained model to make predictions on new data |
| **Accuracy / Precision / Recall** | Metrics to evaluate how well an ML model performs |
| **Feature** | An input variable used to train the model (e.g., age, location, browsing history) |
| **LLM** | Large Language Model — AI trained on vast text data (e.g., GPT, Gemini, Claude) |
| **Prompt Engineering** | Crafting inputs to get desired outputs from an LLM |
| **Hallucination** | When an AI generates incorrect information confidently |
| **RAG (Retrieval Augmented Generation)** | Technique to ground LLM responses in real documents/data |
| **Fine-tuning** | Training a pre-existing AI model further on specific data |

### AI Project Challenges (PM Perspective)

- **Data dependencies:** AI needs high-quality training data — if data isn't ready, nothing else can move forward
- **Model iteration:** Unlike traditional software, AI development is experimental — models may need many iterations before they're good enough
- **Non-deterministic outputs:** AI behavior can be unpredictable, making QA harder
- **Ethical/Bias concerns:** AI can reflect biases in training data — compliance and ethical review may be required
- **Longer development cycles:** Training and evaluating models takes time — timelines are harder to estimate

### PM's Role in an AI Product Project

- Track data collection/labeling milestones (often a dependency)
- Coordinate between ML engineers, product, and business
- Monitor model performance metrics as part of project tracking
- Document model evaluation criteria as part of requirements
- Manage stakeholder expectations around AI output quality
- Track compute costs (GPU/cloud usage) as part of budget

---

## 17. Commonly Asked Interview Questions & Model Answers

### About You / General

---

**Q1: Tell me about yourself.**

> *Framework: Past → Present → Future. Keep it to ~90 seconds.*

"I'm currently pursuing [your degree] at [your college], where I've been focusing on [relevant areas]. I've had the opportunity to [mention 1-2 internships/projects briefly]. For example, during my internship at [Company], I supported project tracking, coordinated cross-functional meetings, and maintained our Jira boards — which gave me hands-on exposure to Agile workflows. I'm passionate about working at the intersection of technology and project execution, especially in the AI space, and this role feels like the perfect next step because [specific reason about the company/role]."

---

**Q2: Why do you want to be a Project/Product Management intern?**

"I'm drawn to PM because I enjoy working across functions — connecting engineering, design, and business to deliver outcomes. I like the mix of structured thinking (planning, tracking, documentation) and interpersonal coordination (stakeholder management, facilitation). I've seen from my previous experience how important a strong coordinator role is to keeping teams aligned and projects on track. I want to deepen these skills in a real AI product environment where the challenges are complex and the impact is significant."

---

### Project Management Knowledge

---

**Q3: What is Agile? How is it different from Waterfall?**

"Agile is a software development mindset that values iterative delivery, customer collaboration, and adaptability to change. In Agile, work is broken into short cycles called sprints — typically 2 weeks — and working software is delivered at the end of each cycle. This allows teams to incorporate feedback continuously.

Waterfall, by contrast, is a sequential approach where each phase — requirements, design, development, testing, deployment — happens one after another. Once a phase is complete, you typically don't go back. It works well for projects with fixed, well-understood requirements but is inflexible for evolving products.

For a software product with changing user needs, Agile is typically far more effective because it catches issues early and allows the team to adapt."

---

**Q4: Explain the Scrum ceremonies.**

"Scrum has five key ceremonies:

**Sprint Planning** happens at the start of each sprint. The team selects items from the product backlog and commits to completing them within the sprint.

**Daily Stand-up** is a 15-minute daily sync where each team member answers: what did I do yesterday, what will I do today, and are there any blockers?

**Sprint Review** happens at the end of the sprint. The team demonstrates what was built to stakeholders, and feedback is gathered.

**Sprint Retrospective** also happens at sprint end, but it focuses on the process — what went well, what didn't, and what we'll improve next sprint.

**Backlog Refinement** is an ongoing process where the team clarifies, estimates, and prioritizes upcoming backlog items with the Product Owner."

---

**Q5: What is a risk and how would you manage it?**

"A risk is an uncertain event that could negatively impact the project if it occurs. Managing risks involves four steps: identify, analyze, plan a response, and monitor.

For example, in a previous project, we identified a risk that a third-party API we depended on might not be ready in time. We analyzed it as high probability and high impact. Our mitigation plan was to build a mock API so development could continue. We also tracked it weekly and escalated when the vendor's timeline slipped.

As a PM intern, my role would be to maintain the risk register, flag new risks identified in standup or team discussions, and bring them to the PM's attention with context and a suggested response option."

---

**Q6: What would you do if a team member missed a deadline?**

"First, I wouldn't immediately escalate — I'd follow up directly and calmly to understand why. Was it a capacity issue? A blocker they hadn't raised? A misunderstood priority? Once I understand the root cause, I'd assess the impact: does this delay anything else? Is it on the critical path?

I'd document the situation and update the tracker. If it impacts the sprint goal or milestone, I'd bring it to the PM with the context: what was missed, why, what the impact is, and what options we have — like adjusting scope, shifting timelines, or getting more resources.

I wouldn't be accusatory with the team member — my role is to keep things moving and support the team, not police them."

---

**Q7: Walk me through how you'd track a project's progress.**

"I'd use a combination of tools and practices:

- **Jira/Trello/Asana board:** Check daily that issues are moving through statuses correctly, sprint burndown is healthy, and nothing is stuck in 'In Progress' too long.
- **Standup notes:** Capture blockers and action items daily.
- **Weekly status report:** Summarize RAG status, milestone progress, risks, and key actions.
- **Milestone tracker:** A spreadsheet or Notion database showing each milestone, expected date, actual date, owner, and status.
- **Sprint velocity:** Track story points completed per sprint to forecast if we'll meet the release date.

If I see the burndown chart going flat (work isn't being completed), I'd investigate immediately — are people blocked? Were stories underestimated? This lets us course-correct before the end of the sprint."

---

### Behavioral / Situational Questions

---

**Q8: Tell me about a time you managed multiple tasks with competing deadlines.**

> *Use STAR: Situation, Task, Action, Result*

"During my internship at [Company], I was simultaneously supporting two project workstreams. One week, both required status reports on the same day, and a new client deliverable was also due. 

I used a priority matrix to assess urgency and impact. I blocked dedicated time for each report in my calendar, started with the one with the most external dependency, and communicated proactively to my manager that I was managing the load and would deliver both on time.

I delivered all three on time. After that, I started maintaining a weekly planning sheet every Monday to map out my tasks, deadlines, and time blocks — which significantly reduced similar crunches."

---

**Q9: Describe a time you identified a risk or problem before it became an issue.**

"During a project coordination internship, I was tracking our testing timeline and noticed that the QA environment hadn't been set up yet, even though coding was nearly complete. Testing was scheduled to begin in 4 days.

I flagged this in the next standup — it hadn't been on anyone's radar. The DevOps team was able to provision the environment in time because we caught it early. If we'd waited until testing day, we would have lost at least 3 days, which would have directly delayed the release.

This taught me the value of proactively cross-checking dependencies, not just tracking individual tasks in isolation."

---

**Q10: Tell me about a time you worked with a cross-functional team.**

"In my internship, I coordinated between the design team, the backend engineers, and the QA team for a new feature launch. These teams had different working styles and communication preferences — engineers preferred Slack threads, designers liked weekly syncs, and QA needed detailed documentation.

I adapted my communication approach for each group: shared technical specs in Slack for engineers, created a shared Figma annotation guide for designers to communicate requirements, and maintained a test plan doc for QA.

The feature launched on time, and the post-launch retrospective highlighted that cross-team communication was a strength of that sprint. I learned that effective coordination means meeting people where they are."

---

**Q11: Tell me about a time you made a mistake. How did you handle it?**

"During one project, I miscommunicated a deadline to a team member — I said a deliverable was due Thursday when the PM had actually said Wednesday. This caused a one-day delay that pushed back a client review.

As soon as I realized, I immediately informed my manager, owned the mistake, and didn't make excuses. I also apologized to the team member who was in an uncomfortable position. We rescheduled the review without significant impact.

After that, I created a habit of always documenting deadlines in writing — confirming them via Slack or email after verbal discussions — and double-checking my notes before communicating timelines. I haven't made that mistake since."

---

### Tool & Technical Knowledge

---

**Q12: Are you familiar with Jira? How have you used it?**

"Yes. I've used Jira in [internship/project] to manage sprint backlogs and track issue statuses. I created and updated user stories, moved issues through the board, logged comments to document blockers and decisions, and used sprint reports to monitor velocity and burndown.

I'm comfortable with Jira's hierarchy — Epics, Stories, Sub-tasks, and Bugs — and I understand how to configure a sprint board, use labels and components for categorization, and generate basic reports. I'm always eager to go deeper on any tool, and I pick up new tools quickly."

---

**Q13: What is an API and why does it matter in a software project?**

"An API, or Application Programming Interface, is a defined contract that allows two software systems to communicate with each other. For example, when a mobile app displays weather data, it's calling a weather API to fetch that information from an external service.

In a software project, APIs are often critical dependencies. If Team A is building the backend API and Team B's frontend work depends on it, then Team B is blocked until the API is available. As a PM, I'd track this as a dependency in the project plan and monitor it in standups. I'd also encourage teams to define the API contract early (even before the backend is built) so the frontend team can work against a mock."

---

**Q14: What is the difference between a bug and a feature?**

"A feature is new functionality intentionally designed and planned for the product. A bug is an unintended defect — something that is supposed to work but doesn't, due to an error in the code.

In a Jira context, they're separate issue types treated differently: features go through the normal backlog and sprint planning process, while bugs — especially production bugs — may be prioritized as P0 or P1 and pulled into the current sprint outside the normal process, depending on severity."

---

### Role-Specific Scenarios

---

**Q15: How would you prepare for and facilitate a sprint planning meeting as a PM intern?**

"Before the meeting, I'd:
- Work with the Product Owner to ensure the backlog is refined — stories are written, estimated, and prioritized
- Confirm all team members can attend
- Set up the Jira sprint board and prepare a shared screen or board view
- Send a calendar invite with the agenda: review sprint goal, walk through top backlog items, team commits to sprint

During the meeting, I'd:
- Take notes on what's committed, any concerns raised, and decisions made
- Document action items with owners and due dates
- Capture any stories broken down into sub-tasks

After the meeting, I'd:
- Send meeting minutes to the team within 24 hours
- Update Jira to reflect the sprint backlog accurately
- Confirm any outstanding questions are addressed"

---

**Q16: You're tracking a project and you notice a milestone is at risk of slipping. What do you do?**

"First, I'd quantify the risk: how many days is it likely to slip, and what's the downstream impact? Does it affect other milestones or external stakeholder commitments?

Then I'd immediately flag it to the Project Manager with full context: the milestone in question, current status, estimated delay, root cause, and any options I see — such as de-scoping lower-priority items to protect the date, or getting additional resources.

I would NOT sit on it or wait to see if it self-corrects. In project management, early visibility is everything. I'd also update the status report to Amber with a note about the risk, so stakeholders aren't surprised."

---

**Q17: What makes a good status report?**

"A good status report is clear, concise, and actionable. It should give a reader an accurate picture of project health in under 2 minutes.

It should include:
- **Overall RAG status** (Green/Amber/Red) at a glance
- **Progress summary** — what was completed, what's in progress, what's next
- **Milestone status** — are we on track for key dates?
- **Risks and issues** — with owners and actions, not just a list of problems
- **Action items** from the previous report — were they completed?
- **Next steps** for the coming week

A status report should tell a story: things are going well except for X, here's what we're doing about it, and here's what we need from leadership. It should never be a surprise."

---

**Q18: How would you handle a situation where two stakeholders have conflicting priorities?**

"This is common in cross-functional work. My approach would be:

First, I'd make sure I fully understand both perspectives — what does each stakeholder need and why? Sometimes what looks like a conflict is actually complementary when you dig deeper.

If it's a genuine conflict, I'd bring both priorities to the Project Manager or Product Manager, not try to resolve it myself. My job as an intern is to surface these conflicts with clear context — not to make the call.

I'd present the options: if we prioritize X, Y gets delayed by Z days. If we prioritize Y, X moves to the next sprint. This gives decision-makers the information they need to make the right trade-off. I'd then document and communicate the decision clearly to both parties."

---

### Quick-Fire Knowledge Checks

**Q: What is the difference between a Sprint Review and a Sprint Retrospective?**
> Sprint Review = Did we build the right thing? (Product/outcome focused, stakeholders attend). Sprint Retrospective = Did we work the right way? (Process focused, team only).

**Q: What does "Definition of Done" mean?**
> A shared checklist that a user story must meet before the team considers it complete. Ensures consistent quality standards.

**Q: What is scope creep?**
> Uncontrolled expansion of a project's scope — adding features or requirements without adjusting timeline, budget, or resources accordingly.

**Q: What is a blocker?**
> Anything that is actively preventing a team member from making progress on their task right now.

**Q: What is velocity?**
> The average number of story points a Scrum team completes per sprint. Used to forecast future sprint capacity.

**Q: What does RAG status mean?**
> Red/Amber/Green — a color-coded system to communicate project health quickly: Green = on track, Amber = at risk, Red = significant issue.

**Q: What is the difference between a risk and an issue?**
> Risk = might happen (uncertain, future). Issue = has happened (certain, present, needs action now).

**Q: What is backlog grooming/refinement?**
> A collaborative activity where the team clarifies, estimates, and prioritizes upcoming backlog items to prepare them for future sprints.

**Q: What is a user story?**
> A requirement described from the user's perspective: "As a [user type], I want [goal], so that [benefit]."

**Q: What is a RACI matrix?**
> A chart defining who is Responsible, Accountable, Consulted, and Informed for each task/deliverable. Clarifies ownership.

---

## BONUS: 30 Terms to Know Cold

| Term | One-liner Definition |
|------|---------------------|
| Sprint | Fixed time-box (usually 2 weeks) for delivering work in Scrum |
| Epic | Large feature spanning multiple sprints |
| User Story | Requirement from the user's perspective |
| Product Backlog | Prioritized master list of all work for the product |
| Sprint Backlog | Work selected for the current sprint |
| Burndown Chart | Graph showing remaining work vs. time in a sprint |
| Velocity | Average story points completed per sprint |
| Stand-up | Daily 15-min sync on progress and blockers |
| Retrospective | End-of-sprint team process reflection |
| Product Owner | Defines WHAT to build; owns the backlog |
| Scrum Master | Facilitates Scrum; removes blockers |
| Kanban | Visual flow-based Agile framework |
| WIP Limit | Max tasks allowed in a Kanban column |
| Dependency | Task B requires Task A to be done first |
| Milestone | Significant checkpoint with a date but no duration |
| Risk Register | Document tracking all identified risks |
| Escalation | Raising an issue to higher authority for resolution |
| Scope Creep | Uncontrolled expansion of project scope |
| RACI | Responsible, Accountable, Consulted, Informed |
| MoSCoW | Must, Should, Could, Won't — prioritization framework |
| PRD | Product Requirements Document |
| API | Interface allowing software systems to communicate |
| CI/CD | Automated code testing and deployment pipeline |
| UAT | User Acceptance Testing — final validation by users |
| Gantt Chart | Bar chart showing project timeline and task durations |
| Critical Path | Longest sequence of tasks determining project duration |
| DoD | Definition of Done — completion checklist for stories |
| Stakeholder | Anyone affected by or interested in the project |
| Action Item | Specific task with an owner and due date from a meeting |
| RAG Status | Red/Amber/Green project health indicator |

---

*End of 01_Knowledge_Base.md*

> **What to do next:** Read this document once fully. Then cover the answers and test yourself on every Q&A. Focus extra time on Scrum ceremonies, risk management, and the behavioral STAR answers — these come up in almost every PM intern interview.
