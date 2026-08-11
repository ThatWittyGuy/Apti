# Shared — SDLC & CI/CD MCQs
### Applies to: JD5 (Business Engagement), any JD mentioning software development lifecycle, DevOps, CI/CD
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What does SDLC stand for?
- A) System Design and Launch Cycle
- B) Software Development Life Cycle — a structured process for planning, creating, testing, deploying, and maintaining software
- C) Software Delivery and Launch Checklist
- D) System Development and Licensing Contract

**Answer: B**

---

**Q2.** 🟢 What are the main phases of a traditional (Waterfall) SDLC?
- A) Plan, Code, Test, Ship
- B) Requirements → System Design → Implementation → Testing → Deployment → Maintenance
- C) Ideate → Prototype → Build → Launch → Iterate
- D) Analysis → Architecture → Development → QA → Release → Support

**Answer: B**

---

**Q3.** 🟢 What is "Agile" software development?
- A) A fast-paced methodology requiring no documentation
- B) An iterative, incremental approach delivering software in short cycles (sprints) — emphasizing collaboration, customer feedback, and adaptability to changing requirements over rigid upfront planning
- C) A project management methodology for large waterfall projects
- D) A software testing framework for automated testing

**Answer: B**

---

**Q4.** 🟢 What does CI/CD stand for?
- A) Code Integration / Code Delivery
- B) Continuous Integration / Continuous Delivery (or Deployment) — a set of DevOps practices automating code integration, testing, and delivery to production
- C) Central Infrastructure / Centralized Deployment
- D) Component Integration / Component Deployment

**Answer: B**

---

**Q5.** 🟢 What is "version control" (source control)?
- A) Controlling which version of software users can install
- B) A system (e.g., Git) tracking changes to code over time — enabling collaboration, history review, branching for parallel development, and rollback to previous versions
- C) A process for managing software license versions
- D) A tool for controlling deployment versions in production

**Answer: B**

---

**Q6.** 🟢 What is "Git"?
- A) A cloud-based deployment platform
- B) A distributed version control system tracking code changes — allowing developers to branch, merge, and collaborate on code; the most widely used VCS, with platforms like GitHub, GitLab, and Azure DevOps built on it
- C) A package manager for Python libraries
- D) A CI/CD pipeline automation tool

**Answer: B**

---

**Q7.** 🟢 What is a "repository" (repo) in Git?
- A) A physical server storing compiled software
- B) A directory containing a project's code, history of all changes (commits), branches, and configuration — either local (on developer's machine) or remote (on GitHub, GitLab, etc.)
- C) A package repository for software dependencies
- D) A cloud storage bucket for deployment artifacts

**Answer: B**

---

**Q8.** 🟡 What is "Continuous Integration" (CI)?
- A) Continuously integrating new features without testing
- B) A practice where developers frequently merge code changes into a shared repository — triggering automated builds and tests to detect integration errors early, reducing integration risk
- C) A process for continuously monitoring production systems
- D) Integrating continuous feedback from business stakeholders

**Answer: B**

---

**Q9.** 🟡 What is the difference between "Continuous Delivery" and "Continuous Deployment"?
- A) They are identical — the terms are interchangeable
- B) Continuous Delivery automates building and testing, making software always release-ready — but requires manual approval to deploy to production; Continuous Deployment goes further, automatically deploying every passing build to production without human approval
- C) Continuous Deployment is for cloud; Continuous Delivery is for on-premise
- D) Continuous Delivery deploys to staging; Continuous Deployment deploys to development

**Answer: B**

---

**Q10.** 🟡 What is a "pipeline" in CI/CD?
- A) A network data pipe for faster code transmission
- B) An automated sequence of stages (build → test → package → deploy) triggered by code changes — orchestrating the full path from developer commit to production deployment
- C) A project plan for software delivery
- D) A branching strategy in Git

**Answer: B**

---

**Q11.** 🟡 What is a "pull request" (PR) / "merge request" (MR)?
- A) A request to pull the latest code from a remote repository
- B) A mechanism for proposing, reviewing, and discussing code changes before merging them into a target branch — enabling peer code review, automated checks, and quality gates
- C) A deployment request for pushing code to production
- D) A request to pull data from an external API

**Answer: B**

---

**Q12.** 🟡 What is "branching strategy" in Git and why does it matter?
- A) A strategy for physically branching a software company into divisions
- B) A convention defining how and when branches are created for features, fixes, and releases — e.g., GitFlow (main/develop/feature branches) or trunk-based development — enabling parallel work without conflicts and structured releases
- C) A Git setting controlling who can create branches
- D) A deployment strategy for rolling out features to branch offices

**Answer: B**

---

**Q13.** 🟡 What is "unit testing"?
- A) Testing a complete software system end-to-end
- B) Testing individual, isolated units of code (functions, methods, classes) to verify each behaves correctly in isolation — fast, automated, and run frequently in CI pipelines
- C) Testing one user scenario at a time
- D) Testing a single deployment environment

**Answer: B**

---

**Q14.** 🟡 What is "integration testing"?
- A) Testing that all team members can integrate into the project
- B) Testing that multiple components or services work correctly when combined — verifying data flow, API contracts, and interactions between units that passed unit testing in isolation
- C) Testing code after integrating it into the main branch
- D) Automated testing of the CI pipeline itself

**Answer: B**

---

**Q15.** 🟡 What is "code review" and why is it a best practice?
- A) A management review of developer productivity metrics
- B) A systematic examination of source code by peers before merging — catching bugs, enforcing coding standards, sharing knowledge, and improving code quality and maintainability
- C) A security audit of production code for vulnerabilities
- D) An automated process that reviews code for syntax errors

**Answer: B**

---

**Q16.** 🟡 What is "DevOps"?
- A) A developer operations department separate from IT
- B) A cultural and technical movement combining software development (Dev) and IT operations (Ops) — breaking silos to enable faster, more reliable software delivery through automation, collaboration, and feedback
- C) A project management methodology for IT operations teams
- D) A cloud service for managing development environments

**Answer: B**

---

**Q17.** 🟡 What is "Infrastructure as Code" (IaC) in a DevOps context?
- A) Writing code that runs on infrastructure
- B) Managing and provisioning infrastructure (servers, networks, databases) through machine-readable code files rather than manual processes — enabling versioning, repeatability, peer review, and automation of environment setup
- C) A programming language for network engineers
- D) A cloud provider's API for managing resources

**Answer: B**

---

**Q18.** 🟡 What is "Docker" and why is it used in CI/CD?
- A) A project management tool used by DevOps teams
- B) A platform for building, shipping, and running applications in containers — standardizing the runtime environment so code runs consistently across development, testing, and production; containers are built and tested in CI pipelines
- C) A version control alternative to Git
- D) A cloud provider specializing in container hosting

**Answer: B**

---

**Q19.** 🟡 What is a "container" and how does it differ from a virtual machine (VM)?
- A) Containers are larger and slower than VMs
- B) A container packages an application and its dependencies into a lightweight, isolated runtime sharing the host OS kernel — much lighter and faster to start than VMs, which run a full OS per instance
- C) VMs are for cloud; containers are for on-premise only
- D) Containers run one process each; VMs can run multiple

**Answer: B**

---

**Q20.** 🟡 What is "Kubernetes" (K8s) and its role in DevOps?
- A) A cloud provider for container hosting
- B) An open-source container orchestration platform automating deployment, scaling, load balancing, self-healing, and rolling updates of containerized applications across a cluster
- C) A CI/CD pipeline tool for Kubernetes applications
- D) A monitoring tool for containerized applications

**Answer: B**

---

**Q21.** 🟡 What is "automated testing" and why is it central to CI/CD?
- A) Testing performed automatically by QA engineers without manual effort
- B) Tests written as code and executed automatically by the CI pipeline on every commit — providing fast, consistent feedback on code quality and preventing regressions from reaching production
- C) A tool that automatically generates test cases from code
- D) Automating the deployment of test environments

**Answer: B**

---

**Q22.** 🟡 What is "test coverage" in software testing?
- A) The percentage of users covered by the testing team
- B) A metric measuring the proportion of source code exercised by automated tests — higher coverage reduces the risk of undetected bugs, though 100% coverage doesn't guarantee bug-free code
- C) The number of test cases in a test suite
- D) The percentage of requirements covered by test scenarios

**Answer: B**

---

**Q23.** 🟡 What is "blue-green deployment"?
- A) A deployment strategy for cloud environments using blue (Azure) and green (AWS)
- B) A release strategy running two identical production environments (blue=current, green=new) — traffic switches to green after validation, with instant rollback by switching back to blue if issues arise
- C) A deployment strategy named after the two co-founders of DevOps
- D) A color-coded deployment pipeline stage (build=blue, test=green)

**Answer: B**

---

**Q24.** 🟡 What is "canary deployment"?
- A) A deployment to production servers in the Canary Islands region
- B) Gradually rolling out a new version to a small subset of users (the "canaries") first — monitoring for issues before progressively expanding to all users, minimizing blast radius of problems
- C) A deployment triggered by a monitoring alert (like a canary in a coal mine)
- D) A testing strategy using synthetic users to simulate production traffic

**Answer: B**

---

**Q25.** 🔴 What is "GitOps"?
- A) IT operations managed through GitHub issues
- B) An operational framework using Git as the single source of truth for both application code and infrastructure configuration — changes to production are made via Git commits and pull requests, with automated reconciliation ensuring the live system matches the Git state
- C) A Git plugin for automating DevOps workflows
- D) A branch strategy for operations teams

**Answer: B**

---

**Q26.** 🔴 What is "shift-left testing" in CI/CD?
- A) Moving testing resources from right to left geographically
- B) Moving testing earlier in the development lifecycle — performing more testing during development (unit, integration, static analysis) rather than waiting for late-stage QA, reducing the cost and time of finding defects
- C) Shifting test execution to the left side of the CI pipeline
- D) A testing methodology moving slower tests earlier in the pipeline

**Answer: B**

---

**Q27.** 🔴 What is "DORA metrics" in DevOps?
- A) A software for monitoring DevOps resource allocation
- B) Four key metrics from the DevOps Research and Assessment group measuring software delivery performance: Deployment Frequency, Lead Time for Changes, Change Failure Rate, and Time to Restore Service
- C) A set of Docker orchestration resource allocation metrics
- D) A DevOps maturity assessment framework by Gartner

**Answer: B**

---

**Q28.** 🔴 What is "static code analysis" (SAST) in a CI pipeline?
- A) Analyzing code performance on static (non-cloud) infrastructure
- B) Automated analysis of source code without executing it — detecting security vulnerabilities, code quality issues, style violations, and potential bugs early in the pipeline using tools like SonarQube, Bandit (Python), or ESLint
- C) A code review process performed by senior engineers
- D) Analyzing code that is not being actively developed (static/archived)

**Answer: B**

---

**Q29.** 🔴 What is "feature flag" (feature toggle)?
- A) A Git flag marking a feature branch for review
- B) A software technique enabling teams to enable/disable features in production without deploying new code — allowing gradual rollouts, A/B testing, instant kill switches, and decoupling deployment from release
- C) A flag in a CI pipeline marking a feature as complete
- D) A product backlog item flagging high-priority features

**Answer: B**

---

**Q30.** 🔴 What is "Scrum" and how does it relate to SDLC?
- A) A software for managing software development projects
- B) An agile framework structuring work into fixed-length sprints (1–4 weeks) with defined roles (Product Owner, Scrum Master, Development Team), events (Sprint Planning, Daily Standup, Sprint Review, Retrospective), and artifacts (Product Backlog, Sprint Backlog, Increment)
- C) A project management methodology for waterfall SDLC phases
- D) A testing methodology for agile teams

**Answer: B**

---

**Q31.** 🟡 What is "technical debt"?
- A) Financial debt incurred purchasing technical infrastructure
- B) The implied cost of future rework from choosing a quick/easy solution now instead of a better approach — accumulates when shortcuts are taken under time pressure, slowing future development and increasing defect risk
- C) A backlog of unresolved technical support tickets
- D) The cost of maintaining legacy systems alongside new ones

**Answer: B**

---

**Q32.** 🟡 What is "refactoring" in software development?
- A) Rebuilding a software system from scratch
- B) Restructuring existing code without changing its external behavior — improving readability, reducing complexity, and paying down technical debt while keeping tests passing
- C) Updating software to use the latest framework version
- D) A code review process for identifying security vulnerabilities

**Answer: B**

---

**Q33.** 🔴 What is "semantic versioning" (SemVer)?
- A) A versioning system based on feature names
- B) A versioning convention using three numbers MAJOR.MINOR.PATCH — MAJOR increments for breaking changes, MINOR for backward-compatible features, PATCH for backward-compatible bug fixes; enabling dependency management and compatibility communication
- C) A versioning system using dates (YYYY.MM.DD)
- D) A versioning standard for API documentation

**Answer: B**

---

**Q34.** 🔴 What is "observability" in a DevOps context?
- A) The ability of IT management to observe developer activity
- B) The capability to understand a system's internal state from its external outputs — achieved through three pillars: logs (event records), metrics (quantitative measurements), and traces (request flows across services) — enabling effective debugging and performance management
- C) A monitoring tool for observing network traffic
- D) The visibility of CI/CD pipeline execution to stakeholders

**Answer: B**

---

**Q35.** 🔴 What is "chaos engineering"?
- A) A chaotic, unstructured approach to software development
- B) The practice of intentionally introducing failures into a system in a controlled way — killing servers, injecting latency, dropping network packets — to proactively discover weaknesses before they cause real outages; popularized by Netflix's "Simian Army"
- C) A testing methodology for highly complex distributed systems
- D) A project management style embracing uncertainty and change

**Answer: B**

---
*35 Questions | Shared — SDLC & CI/CD.*
