# SDLC, CI/CD & DevOps — Comprehensive Study Notes
### For Corning OA Prep | JD5 (Business Engagement)
> Covers: SDLC models, Agile/Scrum, Git, CI/CD pipelines, Docker, Kubernetes, DevOps culture, testing, IaC, monitoring, security (DevSecOps).

---

## 1. Software Development Life Cycle (SDLC)

### 1.1 What is SDLC?
SDLC is a structured process for planning, creating, testing, deploying, and maintaining software. It provides a framework ensuring software is built systematically, meeting requirements within time and budget.

**Core phases (apply to all models):**
1. **Planning** — Define scope, feasibility, resources, timeline, budget
2. **Requirements Analysis** — Gather and document what the system must do
3. **System Design** — Architecture, database, UI, technology stack decisions
4. **Implementation (Coding)** — Developers write code
5. **Testing** — Verify software meets requirements and find defects
6. **Deployment** — Release to production environment
7. **Maintenance** — Bug fixes, enhancements, monitoring

### 1.2 Waterfall Model
**Characteristics:**
- Sequential — each phase fully complete before next begins
- Heavy documentation — requirements frozen before design begins
- No going back — changes are expensive once in later phases
- Single delivery — software delivered at the end of the project

**When to use:**
- Requirements are very clear, stable, and well-documented upfront
- Regulatory/compliance projects with mandatory documentation
- Fixed-scope government contracts
- Short projects with low complexity

**Weaknesses:**
- Inflexible to changing requirements (business evolves during development)
- Testing happens late — bugs found near the end are expensive to fix
- Customer sees nothing until final delivery — risk of building wrong thing
- High failure rate for complex or long projects

### 1.3 Agile Model
**Core philosophy:** Deliver working software frequently, embrace change, collaborate with customers.

**Agile Manifesto (2001) — four values:**
1. **Individuals and interactions** over processes and tools
2. **Working software** over comprehensive documentation
3. **Customer collaboration** over contract negotiation
4. **Responding to change** over following a plan

**12 Agile Principles (key ones):**
- Deliver working software frequently (weeks, not months)
- Welcome changing requirements, even late in development
- Business people and developers must work together daily
- Build projects around motivated individuals; give them trust
- Working software is the primary measure of progress
- Continuous attention to technical excellence and good design
- Simplicity — maximize the amount of work not done

**Agile vs. Waterfall:**
| Aspect | Waterfall | Agile |
|---|---|---|
| Requirements | Fixed upfront | Evolve continuously |
| Delivery | Single final release | Incremental releases (sprints) |
| Customer involvement | Beginning and end | Continuous |
| Change | Expensive, discouraged | Expected, welcome |
| Risk | High (late discovery) | Lower (early feedback) |
| Documentation | Heavy | Lightweight (just enough) |
| Best for | Stable, clear requirements | Complex, evolving requirements |

### 1.4 Scrum Framework
The most popular Agile framework. Scrum divides work into fixed-length **Sprints** (1–4 weeks, typically 2).

**Three Roles:**

**Product Owner (PO):**
- Represents business/customer interests
- Owns and prioritizes the **Product Backlog** (list of all desired features/stories)
- Defines acceptance criteria for user stories
- Makes final call on what gets built and in what order
- Single person (not a committee)

**Scrum Master:**
- Servant-leader for the team — not a manager
- Facilitates Scrum ceremonies (meetings)
- Removes impediments/blockers for the team
- Coaches team on Scrum practices
- Shields team from external distractions

**Development Team:**
- Self-organizing, cross-functional group (developers, testers, designers)
- 3–9 people (optimal size)
- Collectively responsible for delivering the Sprint increment
- No sub-teams; everyone shares ownership

**Five Scrum Events (Ceremonies):**

**1. Sprint Planning:**
- At the start of each sprint
- Team selects items from Product Backlog into Sprint Backlog
- Team estimates effort and commits to what they can deliver
- Output: Sprint Goal + Sprint Backlog

**2. Daily Scrum (Daily Standup):**
- 15 minutes, every day, same time
- Each person answers: What did I do yesterday? What will I do today? Any blockers?
- NOT a status report to management — it's team synchronization
- Helps identify impediments early

**3. Sprint Review:**
- End of sprint; team demonstrates working software to stakeholders
- Product Owner accepts or rejects completed work
- Stakeholders provide feedback
- Product Backlog updated based on feedback

**4. Sprint Retrospective:**
- After Sprint Review; team only (no stakeholders)
- What went well? What didn't? What do we improve next sprint?
- Output: 1–3 actionable improvement items for next sprint
- Continuous improvement mechanism

**5. Backlog Refinement (Grooming):**
- Ongoing (not just sprint boundaries)
- PO and team review, clarify, estimate, and reprioritize backlog items
- Ensures top items are ready and well-understood for upcoming sprints

**Scrum Artifacts:**

**Product Backlog:**
- Ordered list of everything that might be needed in the product
- Contains: User Stories, Features, Bug Fixes, Technical Debt, Research
- Only one Product Backlog per product
- PO is sole owner; team contributes

**Sprint Backlog:**
- Subset of Product Backlog selected for current sprint
- Plus the plan for delivering it (tasks broken down)
- Team-owned; updated daily

**Product Increment:**
- Sum of all completed Product Backlog Items in a sprint plus all previous sprints
- Must be usable and meet "Definition of Done"

**Definition of Done (DoD):**
- Shared agreement on when a story is truly "done" (not just coded)
- Typically includes: coded, code reviewed, unit tested, integration tested, documented, deployed to staging, accepted by PO
- Prevents "90% done" syndrome

**User Stories:**
Format: `As a [type of user], I want [goal] so that [benefit]`
Example: `As a finance manager, I want to export GL reports to Excel so that I can perform ad-hoc analysis`

**Acceptance Criteria:**
- Specific, testable conditions that define when a story is complete
- Format: Given [context], When [action], Then [expected result]
- Written by PO with team input

**Story Points:**
- Relative estimation units (not hours) representing complexity + effort + uncertainty
- Common scales: Fibonacci (1,2,3,5,8,13,21) or T-shirt sizes (S,M,L,XL)
- **Velocity:** Average story points completed per sprint — used for forecasting
- **Planning Poker:** Team estimation technique — everyone reveals estimate simultaneously, discuss disagreements

### 1.5 Other Agile Frameworks

**Kanban:**
- Visualize work on a board (To Do → In Progress → Done)
- Limit Work In Progress (WIP) — no more than X items per stage
- Pull system — items pulled when capacity available
- No sprints or fixed roles
- Best for: operations, support, continuous flow work (maintenance, bug fixes)

**SAFe (Scaled Agile Framework):**
- Agile for large enterprises with many teams
- Adds: Program Increment (PI) planning, Agile Release Trains (ART), Program/Portfolio levels
- Coordinates multiple Scrum teams working on the same product

**LeSS (Large-Scale Scrum):**
- Scaled Scrum with fewer additional roles/ceremonies than SAFe
- Multiple teams, one Product Backlog, one Product Owner

---

## 2. Version Control with Git

### 2.1 What is Git?
Git is a **distributed version control system (DVCS)** — every developer has a full copy of the repository including entire history. Created by Linus Torvalds (2005) for Linux kernel development.

**Key advantages:**
- Full history of all changes — who changed what, when, why
- Branching — parallel development without interfering with main code
- Rollback — revert to any previous state instantly
- Collaboration — merge work from multiple developers
- Distributed — work offline; push when ready

**Hosting platforms:** GitHub, GitLab, Bitbucket, Azure DevOps Repos

### 2.2 Core Git Concepts

**Repository (Repo):**
- Directory containing all project files + `.git` folder (all history and metadata)
- **Local repo:** On developer's machine
- **Remote repo:** On server (GitHub, GitLab) — shared with team

**Commit:**
- A snapshot of the repository at a point in time
- Immutable — once created, it doesn't change
- Has: unique SHA-1 hash, author, date, message, pointer to parent commit
- `git commit -m "Add user authentication feature"`

**Branch:**
- A lightweight, movable pointer to a commit
- Default branch: `main` (formerly `master`)
- `git branch feature/login` — create branch
- `git checkout feature/login` or `git switch feature/login` — switch to branch
- `git checkout -b feature/login` — create and switch in one command

**Staging Area (Index):**
- Intermediate area between working directory and repository
- Choose exactly which changes to include in next commit
- `git add file.py` — stage specific file
- `git add .` — stage all changes
- `git status` — see staged/unstaged/untracked files

**Working Directory:**
- Actual files on disk — your current working files
- Three states for files: Modified (changed), Staged (marked for commit), Committed (saved in repo)

### 2.3 Essential Git Commands
```bash
# Setup
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# Initialize / clone
git init                          # Create new repo in current directory
git clone https://github.com/...  # Copy remote repo locally

# Daily workflow
git status                        # See changes
git add .                         # Stage all changes
git add file.py                   # Stage specific file
git commit -m "Message"           # Commit staged changes
git push origin main              # Push to remote
git pull origin main              # Fetch + merge from remote

# Branching
git branch                        # List branches
git branch feature/new            # Create branch
git switch feature/new            # Switch branch
git merge feature/new             # Merge branch into current
git branch -d feature/new         # Delete branch (after merge)

# History and differences
git log --oneline                 # Compact history
git diff                          # Show unstaged changes
git diff --staged                 # Show staged changes

# Undoing
git restore file.py               # Discard unstaged changes
git restore --staged file.py      # Unstage a file
git revert <commit>               # Create new commit that undoes a commit (safe)
git reset --hard HEAD~1           # Remove last commit (destructive - use carefully)

# Remote operations
git remote -v                     # List remotes
git fetch origin                  # Download remote changes (don't merge)
git pull                          # fetch + merge

# Stashing
git stash                         # Temporarily save uncommitted changes
git stash pop                     # Restore stashed changes
```

### 2.4 Branching Strategies

**GitHub Flow (Simple, best for CI/CD):**
1. `main` branch is always deployable
2. Create descriptive branch for new feature/fix
3. Open Pull Request (PR) early for discussion
4. Review, discuss, make adjustments
5. Merge to `main` after approval + passing CI
6. Deploy immediately after merge

**GitFlow (More structured, for release-based software):**
- `main` — production-ready code only
- `develop` — integration branch for features
- `feature/*` — new features (branch from develop, merge back to develop)
- `release/*` — release preparation (branch from develop, merge to main + develop)
- `hotfix/*` — urgent production fixes (branch from main, merge to main + develop)

**Trunk-Based Development (for high-performing teams):**
- Developers commit directly to `main` (or very short-lived feature branches, < 1 day)
- Feature flags control what's active in production
- Requires excellent CI/CD and test coverage
- Enables continuous deployment

### 2.5 Pull Requests (PRs) / Merge Requests (MRs)
**Purpose:** Propose, discuss, and review code before merging into main branch

**PR workflow:**
1. Developer creates feature branch, makes changes, pushes
2. Opens PR: title, description, screenshots, linked ticket
3. **Automated checks run:** unit tests, integration tests, linting, security scan
4. **Code review:** Peers read code, leave comments, suggest improvements, approve/request changes
5. Once approved + all checks green → merge
6. Branch deleted after merge

**Good PR practices:**
- Small PRs (< 400 lines) — easier to review thoroughly
- Clear title and description explaining WHAT and WHY
- Link to related issue/ticket
- Include screenshots for UI changes
- Self-review before requesting review
- Respond to all comments before merge

---

## 3. CI/CD — Continuous Integration & Continuous Delivery

### 3.1 The Problem CI/CD Solves
**Without CI/CD:**
- Developers work in isolation for weeks/months → integration is painful ("integration hell")
- Manual testing is slow, inconsistent, skipped under pressure
- Deployments are rare, high-risk, nerve-wracking events
- Bugs found late (in QA or production) are expensive to fix
- Slow feedback loop — developers don't know if their code broke something

**With CI/CD:**
- Every commit automatically tested → find bugs immediately when context is fresh
- Deployments are automated, frequent, low-risk, and reversible
- Short feedback loops → higher quality, faster delivery

### 3.2 Continuous Integration (CI)

**What it is:** Practice where developers frequently merge code into a shared branch, triggering automated build and test execution.

**CI pipeline triggered by:** Every push to any branch, or every PR

**Typical CI pipeline stages:**
```
Code push → Trigger CI → Source checkout → Install dependencies → 
Lint/Format check → Unit tests → Build artifact → 
Integration tests → Security scan → Coverage report → Pass/Fail
```

**Key CI principles:**
- **Commit frequently:** Small commits merged often (at least daily) — don't let branches diverge
- **Don't break the build:** If CI fails, fix it immediately — never leave a broken main branch
- **Fast feedback:** CI should complete in < 10–15 minutes for quick feedback
- **Everyone sees results:** Build status visible to whole team

**What runs in CI:**
- **Linting:** Check code style/formatting (ESLint, flake8, pylint, black)
- **Unit tests:** Test individual functions/components in isolation
- **Integration tests:** Test components working together
- **Static analysis (SAST):** Security vulnerability scanning in code (SonarQube, Bandit, Semgrep)
- **Dependency scanning:** Check for vulnerable dependencies (Snyk, Dependabot, OWASP Dependency-Check)
- **Build:** Compile code, build Docker image, create artifact

### 3.3 Continuous Delivery vs. Continuous Deployment

**Continuous Delivery:**
- Every passing CI build is automatically deployed to a staging/pre-production environment
- Software is always in a releasable state
- **Deployment to production requires manual approval** (human decision)
- Why manual? Regulatory requirements, business timing, coordination with support teams

**Continuous Deployment:**
- Every passing CI build is **automatically deployed to production** with zero manual steps
- Highest level of automation
- Requires: excellent test coverage, feature flags, fast rollback capability, strong monitoring
- Used by: Netflix, Amazon, Facebook, Google (deploy thousands of times per day)

**Most enterprises use Continuous Delivery** — automated deployment to staging, human approval for production.

### 3.4 Deployment Strategies

**Recreate:**
- Stop all old version → deploy new version
- Downtime during switch
- Simple but not suitable for zero-downtime requirements

**Rolling Deployment:**
- Gradually replace old instances with new ones (one at a time or in batches)
- No downtime; users hit both old and new versions during rollout
- Can be slow to complete

**Blue-Green Deployment:**
- Two identical environments: Blue (current production) and Green (new version)
- Deploy new version to Green; test it fully
- Switch traffic from Blue to Green (instant — DNS/load balancer switch)
- **Instant rollback:** Switch back to Blue if problems detected
- Cost: requires double infrastructure during switch

**Canary Deployment:**
- Route small % of traffic (e.g., 5%) to new version initially
- Monitor metrics — error rates, latency, business metrics
- Gradually increase percentage if metrics are healthy
- Full rollout or rollback based on results
- Named after "canary in a coal mine" — canary users detect problems first
- Tools: Argo Rollouts, Flagger, AWS CodeDeploy, Istio traffic splitting

**Feature Flags (Feature Toggles):**
- Deploy code to production but control activation via configuration
- Decouple deployment (when code is released) from release (when feature is enabled)
- Enable: gradual rollouts, A/B testing, instant kill switches, beta groups
- Tools: LaunchDarkly, Unleash, AWS AppConfig, Azure App Configuration

### 3.5 CI/CD Tools

**CI/CD Platforms:**
| Tool | Description |
|---|---|
| **GitHub Actions** | Native CI/CD in GitHub; workflow files in `.github/workflows/`; free for public repos |
| **GitLab CI/CD** | Built into GitLab; `.gitlab-ci.yml` file; strong built-in features |
| **Jenkins** | Open-source; most flexible; requires self-hosting and maintenance; Jenkinsfile (Groovy) |
| **Azure DevOps Pipelines** | Microsoft's CI/CD; YAML or classic UI; deep Azure integration |
| **CircleCI** | Cloud-native; fast; `.circleci/config.yml` |
| **Travis CI** | Early cloud CI/CD; `.travis.yml` |
| **Tekton** | Kubernetes-native CI/CD pipelines |
| **Argo CD** | GitOps CD for Kubernetes |

**GitHub Actions — Structure:**
```yaml
name: CI Pipeline

on:                              # Trigger
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest       # Runner
    steps:
      - uses: actions/checkout@v4               # Check out code
      - uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - run: pip install -r requirements.txt    # Install deps
      - run: flake8 .                          # Lint
      - run: pytest --cov=./src tests/         # Test
      - run: bandit -r src/                    # Security scan

  build:
    needs: test                  # Run after test job
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: docker build -t myapp:${{ github.sha }} .
      - run: docker push myapp:${{ github.sha }}
```

---

## 4. Containerization — Docker

### 4.1 The Problem Docker Solves
"Works on my machine" — code runs fine locally but fails in production due to different:
- OS version, Python version, library versions, environment variables, file paths

**Docker solution:** Package the application AND all its dependencies (OS libraries, runtime, config) into a **container image** — runs identically everywhere.

### 4.2 Key Docker Concepts

**Image:**
- Read-only template for creating containers
- Built from a `Dockerfile` (series of instructions)
- Layered — each instruction adds a layer; layers are cached
- Stored in a container registry (Docker Hub, AWS ECR, Azure ACR, GitHub Container Registry)

**Container:**
- A running instance of an image
- Isolated process with own filesystem, network, process space
- Lightweight — shares host OS kernel (unlike VMs that include full OS)
- Stateless by default — data gone when container stops (use volumes for persistence)

**Dockerfile:**
```dockerfile
FROM python:3.11-slim              # Base image
WORKDIR /app                       # Set working directory
COPY requirements.txt .            # Copy dependency list
RUN pip install -r requirements.txt # Install dependencies (creates a layer)
COPY . .                           # Copy application code
EXPOSE 8080                        # Document port (informational)
CMD ["python", "app.py"]           # Default command when container starts
```

**Key Docker commands:**
```bash
docker build -t myapp:1.0 .       # Build image from Dockerfile in current dir
docker run -p 8080:8080 myapp:1.0 # Run container; map host:container ports
docker run -d myapp:1.0           # Run detached (background)
docker ps                          # List running containers
docker ps -a                       # List all containers (including stopped)
docker stop <container_id>         # Stop running container
docker rm <container_id>           # Remove container
docker images                      # List images
docker rmi myapp:1.0               # Remove image
docker logs <container_id>         # View container logs
docker exec -it <id> /bin/bash     # Shell into running container
docker pull python:3.11            # Pull image from registry
docker push myrepo/myapp:1.0      # Push image to registry
```

**Docker Compose (multi-container applications):**
```yaml
# docker-compose.yml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=postgresql://db:5432/mydb
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_PASSWORD=secret
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```
`docker compose up` — starts all services; `docker compose down` — stops all

### 4.3 Container vs VM
| Aspect | Container | Virtual Machine |
|---|---|---|
| OS | Shares host kernel | Full OS per VM |
| Size | MBs | GBs |
| Startup | Seconds | Minutes |
| Isolation | Process-level | Hardware-level (stronger) |
| Resource overhead | Minimal | Significant |
| Use case | Microservices, CI/CD | Full OS isolation needed |

---

## 5. Container Orchestration — Kubernetes

### 5.1 Why Kubernetes?
Running one container is easy. Running hundreds of containers across many servers is hard:
- Which server has capacity? (Scheduling)
- What if a container crashes? (Self-healing)
- How to handle traffic spikes? (Auto-scaling)
- How to update without downtime? (Rolling updates)
- How do containers discover each other? (Service discovery)

**Kubernetes (K8s)** solves all of this — it's a container orchestration platform.

### 5.2 Kubernetes Architecture

**Control Plane (manages the cluster):**
- **API Server:** Front-end for the Kubernetes API; all communication goes through it
- **etcd:** Distributed key-value store; stores all cluster state/config
- **Scheduler:** Assigns pods to nodes based on resource availability and constraints
- **Controller Manager:** Runs controllers that watch state and make corrections (e.g., ensure desired replica count)

**Worker Nodes (run workloads):**
- **kubelet:** Agent on each node; ensures containers in pods are running
- **kube-proxy:** Network proxy; implements Service networking
- **Container Runtime:** Docker, containerd, CRI-O — actually runs containers

### 5.3 Core Kubernetes Objects

**Pod:**
- Smallest deployable unit in Kubernetes
- One or more tightly-coupled containers sharing network and storage
- Ephemeral — if pod dies, it's replaced (don't store state in pods)
- Gets its own IP address within the cluster

**Deployment:**
- Manages a set of identical pods (replicas)
- Ensures desired number of pods always running
- Handles rolling updates and rollbacks
- `kubectl create deployment myapp --image=myapp:1.0 --replicas=3`

**Service:**
- Stable network endpoint for accessing a set of pods
- Types:
  - **ClusterIP** (default): Internal only — pods communicate within cluster
  - **NodePort**: Expose on static port on each node — external access (dev/test)
  - **LoadBalancer**: Provision cloud load balancer — production external access
  - **ExternalName**: Map to external DNS name

**ConfigMap:**
- Store non-sensitive configuration as key-value pairs
- Inject into pods as environment variables or files

**Secret:**
- Store sensitive data (passwords, API keys, certificates) — base64 encoded
- Inject into pods like ConfigMaps
- Best practice: use external secrets manager (AWS Secrets Manager + external-secrets operator)

**Namespace:**
- Virtual cluster within a cluster — isolate resources by team/environment
- Default namespace: `default`
- Common namespaces: `kube-system` (K8s components), `monitoring`, `production`, `staging`

**PersistentVolume (PV) / PersistentVolumeClaim (PVC):**
- Provide persistent storage that survives pod restarts
- PV: Actual storage resource (cloud disk, NFS)
- PVC: Request for storage by a pod

**Ingress:**
- HTTP/HTTPS routing rules to expose services externally
- Path-based routing: `/api` → api service, `/app` → frontend service
- Requires an Ingress Controller (nginx-ingress, Traefik, AWS ALB Ingress Controller)

### 5.4 Key kubectl Commands
```bash
kubectl get pods                          # List pods
kubectl get pods -n production            # List pods in namespace
kubectl describe pod <pod-name>           # Detailed pod info + events
kubectl logs <pod-name>                   # View pod logs
kubectl exec -it <pod> -- /bin/bash      # Shell into pod
kubectl apply -f deployment.yaml         # Create/update from manifest
kubectl delete -f deployment.yaml        # Delete resources from manifest
kubectl scale deployment myapp --replicas=5  # Scale
kubectl rollout status deployment/myapp  # Check rollout
kubectl rollout undo deployment/myapp    # Rollback to previous version
kubectl get nodes                        # List cluster nodes
kubectl top pods                         # Pod resource usage
kubectl get events                       # Cluster events (troubleshooting)
```

---

## 6. DevOps Culture & Practices

### 6.1 What is DevOps?
DevOps is a **cultural and technical movement** combining software Development (Dev) and IT Operations (Ops) into a unified, collaborative practice.

**Core idea:** Break down silos between dev (builds it) and ops (runs it) so teams can deliver software faster, more reliably, and continuously.

**Before DevOps:** Dev team builds → throws code "over the wall" → Ops team struggles to deploy → blame when things break → slow, adversarial, infrequent releases

**With DevOps:** Dev and Ops share responsibility for the full lifecycle — "you build it, you run it" (Amazon's model)

### 6.2 DevOps Principles

**CALMS Framework:**
- **Culture:** Shared responsibility, collaboration, blameless post-mortems, psychological safety
- **Automation:** Automate everything possible (CI/CD, testing, infrastructure, security)
- **Lean:** Eliminate waste, small batch sizes, limit WIP, continuous flow
- **Measurement:** Measure everything; data-driven decisions; DORA metrics
- **Sharing:** Share knowledge, tools, processes; no hoarding information

### 6.3 DORA Metrics (DevOps Research & Assessment)
Four metrics that distinguish high-performing software teams:

| Metric | Elite | High | Medium | Low |
|---|---|---|---|---|
| **Deployment Frequency** | On-demand (multiple/day) | 1/week–1/month | 1/month–6months | < once/6 months |
| **Lead Time for Changes** | < 1 hour | 1 day – 1 week | 1 week – 1 month | > 6 months |
| **Change Failure Rate** | 0–15% | 16–30% | 16–30% | 16–30% |
| **Time to Restore Service** | < 1 hour | < 1 day | 1 day – 1 week | > 6 months |

High performers: Deploy frequently, recover fast, have low failure rates. The data shows frequent deployment correlates with FEWER incidents (not more) because changes are smaller.

### 6.4 Infrastructure as Code (IaC)

**What:** Manage infrastructure (servers, networks, databases) through code files rather than manual configuration.

**Benefits:**
- **Reproducible:** Same code = same infrastructure every time
- **Version controlled:** Infrastructure history in Git; review changes; rollback
- **Collaborative:** Teams review infrastructure changes like code reviews
- **Fast:** Provision complex environments in minutes
- **Documentation:** Code IS the documentation

**Terraform (HashiCorp — most popular IaC tool):**
- Declarative: Define desired state; Terraform figures out how to achieve it
- Works with AWS, Azure, GCP, Kubernetes, and 1,000+ providers
- **State file:** Tracks what Terraform has created (store remotely in S3/Azure Blob)
- Commands: `terraform init` → `terraform plan` (preview) → `terraform apply` (create) → `terraform destroy`

```hcl
# main.tf - Example AWS EC2 instance
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  tags = {
    Name        = "WebServer"
    Environment = "Production"
    Owner       = "platform-team"
  }
}
```

**AWS CloudFormation:** AWS-native IaC using JSON/YAML templates
**Azure Bicep / ARM Templates:** Azure-native IaC
**Pulumi:** IaC using general-purpose languages (Python, TypeScript, Go)
**Ansible:** Configuration management + IaC (YAML playbooks; agentless)

### 6.5 Site Reliability Engineering (SRE)

**SRE** (Google's approach to operations) applies software engineering practices to operations:

**Key SRE concepts:**

**SLI (Service Level Indicator):** A quantitative metric measuring service behavior
- Example: "Percentage of requests completing in < 200ms"

**SLO (Service Level Objective):** Target for an SLI
- Example: "99.9% of requests complete in < 200ms"

**SLA (Service Level Agreement):** Legal/contractual commitment to customers (usually looser than internal SLO)
- Example: "We guarantee 99.5% uptime in our contract" (internal SLO may be 99.9%)

**Error Budget:**
- `Error Budget = 100% - SLO`
- If SLO = 99.9%, error budget = 0.1% = 43.8 minutes/month of allowed downtime
- If error budget is depleted → freeze new feature deployments until it recovers
- Aligns dev and ops: both want to preserve error budget

**Toil:** Repetitive, manual, automatable operational work. SRE targets toil reduction through automation. Cap toil at < 50% of SRE time.

**Blameless Post-Mortems:**
- When an incident occurs, focus on WHAT happened and HOW to prevent it
- Not WHO caused it — individual blame discourages transparency and learning
- Output: timeline, root causes, action items, lessons learned

---

## 7. Testing in Software Development

### 7.1 Testing Types

**Unit Tests:**
- Test individual functions/methods/classes in isolation
- No external dependencies (mock databases, APIs, file systems)
- Fast: thousands run in seconds
- Written by developers; run on every commit

**Integration Tests:**
- Test multiple components working together
- May use real databases, APIs, message queues
- Slower than unit tests; catch interface/contract bugs
- Run in CI after unit tests pass

**End-to-End (E2E) Tests:**
- Test complete user journeys through the system
- Simulate real user behavior (browser automation with Selenium, Playwright, Cypress)
- Slowest; most brittle; run less frequently
- Catch issues that unit/integration tests miss

**Contract Tests:**
- Verify that APIs/services fulfill their contracts (agreed interface)
- Catch breaking changes in microservice APIs before deployment
- Tools: Pact, Spring Cloud Contract

**Performance/Load Tests:**
- Verify system handles expected (and peak) load
- Measure: response time, throughput, error rate under load
- Tools: JMeter, Locust, k6, Gatling

**Security Tests (SAST/DAST):**
- **SAST (Static Application Security Testing):** Scan source code for vulnerabilities (SonarQube, Snyk Code, Bandit)
- **DAST (Dynamic Application Security Testing):** Test running application for vulnerabilities (OWASP ZAP, Burp Suite)

### 7.2 Test Pyramid
Guideline for balancing test types:
```
        /\
       /  \  E2E Tests (few — slow, expensive)
      /----\
     /      \ Integration Tests (moderate)
    /--------\
   /          \ Unit Tests (many — fast, cheap)
  /____________\
```
- Many fast cheap unit tests form the base
- Fewer integration tests in the middle
- Very few slow E2E tests at the top
- Violations: "ice cream cone" (too many E2E) or "cupcake" (no unit tests)

### 7.3 Test-Driven Development (TDD)
**Red → Green → Refactor cycle:**
1. **Red:** Write a failing test for new functionality (test first, code doesn't exist yet)
2. **Green:** Write minimum code to make the test pass
3. **Refactor:** Clean up code while keeping tests green
4. Repeat

**Benefits:** Forces clear thinking about requirements; results in highly testable code; creates comprehensive test suite as a side effect.

### 7.4 Code Quality

**Code Review:** Peer inspection of code before merging — catch bugs, enforce standards, share knowledge

**Static Analysis:** Automated tools analyzing code without running it:
- Style/formatting: `black` (Python), `prettier` (JS), `gofmt` (Go)
- Linting: `flake8`, `pylint` (Python), `ESLint` (JS)
- Security: `bandit` (Python), `semgrep`, SonarQube
- Complexity: Cyclomatic complexity, code duplication

**Code Coverage:** % of code executed by tests
- 80%+ is a common target
- 100% coverage doesn't mean bug-free — tests can exist without meaningful assertions
- Track: line coverage, branch coverage, function coverage

**Technical Debt:** The implied cost of rework accumulated by choosing fast/easy solutions over better ones. Like financial debt — must be paid eventually with "interest" (slowing down future development).

---

## 8. Monitoring & Observability

### 8.1 Three Pillars of Observability

**Metrics:** Quantitative measurements collected over time
- CPU usage, memory, request rate, error rate, latency percentiles
- Tools: Prometheus, CloudWatch, Datadog, New Relic

**Logs:** Timestamped records of discrete events
- Application errors, user actions, system events
- Tools: ELK Stack (Elasticsearch + Logstash + Kibana), Splunk, Loki + Grafana, CloudWatch Logs

**Traces (Distributed Tracing):** Track a request as it flows through multiple services
- Shows: which services called, time spent in each, where errors occurred
- Tools: Jaeger, Zipkin, AWS X-Ray, Datadog APM, OpenTelemetry

### 8.2 Alerting Best Practices
- Alert on symptoms (user-visible impact), not just causes (CPU high)
- **SLO-based alerting:** Alert when error budget burn rate is high
- Avoid alert fatigue — too many alerts → ignored alerts
- Every alert must be actionable — if you can't do anything, don't alert
- Runbooks: Document how to respond to each alert

### 8.3 Key Tools
| Category | Tools |
|---|---|
| Metrics | Prometheus, Grafana, CloudWatch, Datadog |
| Logs | ELK Stack, Splunk, Loki, Papertrail |
| Tracing | Jaeger, Zipkin, AWS X-Ray, OpenTelemetry |
| APM | Datadog, New Relic, Dynatrace |
| Incident Management | PagerDuty, OpsGenie, VictorOps |
| Status Pages | Statuspage.io, Atlassian Status |

---

## 9. DevSecOps — Security in CI/CD

### 9.1 Shift-Left Security
Traditional: Security tested at the end (before release) — expensive to fix late bugs.

Shift-left: Integrate security throughout the development lifecycle:
- **Development:** IDE plugins flagging security issues as code is written
- **Commit:** Pre-commit hooks (secret scanning — don't commit API keys to Git)
- **CI:** SAST scans, dependency vulnerability checks
- **CD:** Container image scanning, DAST on staging environment
- **Production:** Runtime security monitoring, WAF, SIEM

### 9.2 Key DevSecOps Practices

**Dependency Scanning:** Check libraries for known CVEs (Common Vulnerabilities and Exposures)
- Tools: Snyk, Dependabot (GitHub), OWASP Dependency-Check, npm audit, pip audit

**Secret Detection:** Prevent API keys, passwords, certificates from being committed to Git
- Tools: GitGuardian, truffleHog, detect-secrets, GitHub Secret Scanning

**Container Image Scanning:** Scan Docker images for OS/library vulnerabilities before deployment
- Tools: Trivy, Snyk Container, Clair, AWS ECR scanning

**Infrastructure Security Scanning:** Check IaC for security misconfigurations
- Tools: Checkov, tfsec, KICS (Terraform/CloudFormation/K8s manifests)

**SBOM (Software Bill of Materials):** Inventory of all components in software — required for compliance and vulnerability management

---

## 10. Key Terms Quick Reference

| Term | Meaning |
|---|---|
| SDLC | Software Development Life Cycle — structured phases for building software |
| Agile | Iterative, flexible development methodology; welcomes change |
| Scrum | Agile framework with sprints, PO, Scrum Master, dev team |
| Sprint | Fixed time-box (1–4 weeks) in Scrum to deliver working software |
| Kanban | Visual workflow management; WIP limits; no fixed sprints |
| User Story | Feature described from user perspective: "As a [user], I want [goal]" |
| Story Points | Relative effort estimate (complexity × uncertainty) |
| Velocity | Average story points completed per sprint; used for forecasting |
| Product Backlog | Ordered list of all desired product features/work |
| DoD | Definition of Done — team agreement on when work is complete |
| CI | Continuous Integration — automated build + test on every commit |
| CD | Continuous Delivery/Deployment — automated path from code to production |
| Pipeline | Automated sequence of stages from code to deployment |
| Git | Distributed version control system |
| Branch | Independent line of development in a Git repository |
| Pull Request | Proposal to merge code changes + code review mechanism |
| Docker | Container platform — package app + dependencies for consistent execution |
| Container | Isolated process running an application; shares host OS kernel |
| Image | Read-only template for creating containers |
| Dockerfile | Instructions for building a Docker image |
| Kubernetes | Container orchestration system — deploys, scales, manages containers |
| Pod | Smallest Kubernetes unit — one or more containers |
| Deployment | Kubernetes object managing a set of identical pod replicas |
| Service | Stable network endpoint for accessing pods in Kubernetes |
| Helm | Package manager for Kubernetes — templates for deploying apps |
| DevOps | Cultural movement uniting development and operations for faster delivery |
| SRE | Site Reliability Engineering — applying software engineering to ops |
| SLO | Service Level Objective — internal performance target |
| SLI | Service Level Indicator — metric measuring service behavior |
| Error Budget | Allowed failure time = 100% - SLO; governs deployment velocity |
| IaC | Infrastructure as Code — manage infrastructure through code/version control |
| Terraform | Most popular IaC tool — declarative, multi-cloud |
| Blue-Green | Deployment: two identical environments; instant traffic switch |
| Canary | Deployment: route small % to new version; monitor; gradually expand |
| Feature Flag | Toggle feature on/off in production via configuration (no deployment) |
| TDD | Test-Driven Development — write test first, then code |
| DORA | DevOps Research & Assessment — four key metrics for delivery performance |
| SAST | Static Application Security Testing — scan code for vulnerabilities |
| DAST | Dynamic Application Security Testing — test running app for vulnerabilities |
| Shift-Left | Moving testing/security earlier in development lifecycle |
| Toil | Repetitive manual ops work; target for automation in SRE |
| Blameless PM | Post-mortem focused on systemic causes, not individual blame |
| CALMS | Culture, Automation, Lean, Measurement, Sharing — DevOps principles |

---
*End of SDLC, CI/CD & DevOps Comprehensive Notes — ~660 lines.*
*Read alongside: Shared_SDLC_CICD.md for MCQ practice.*
