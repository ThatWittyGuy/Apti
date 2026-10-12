# 2. HR INTERVIEW PREP: Barclays Edition (Resume + RISES Values)

> **How to use this document**
> - Answers are in first person, built from your resume. **[Brackets] = fill in your own real detail.** HR interviewers detect rehearsed or inflated answers quickly, so keep the *structure*, change the *wording*, and stay truthful.
> - HR cares about: **communication, attitude, honesty, teamwork, leadership, learnability, stability and culture fit.** Technical depth is in the other document.
> - **Barclays target:** Barclays says its five values (Respect, Integrity, Service, Excellence and Stewardship, i.e. **RISES**) are its moral compass. Many answers below carry a **RISES link** line or a **[RISES: ...]** tag showing which value your answer demonstrates. Say the value naturally once per answer, never as a slogan.
> - Use **STAR** (Situation, Task, Action, Result) for any "Tell me about a time..." question. Keep each STAR answer to about 90 seconds, and always end with the **learning** or **impact**.
> - **Explicit STAR answers** are labelled **(STAR)** and written as **S (Situation), T (Task), A (Action), R (Result)** plus a one-line **Learning**. Don't read the labels aloud; speak them as a flowing story of about 90 seconds, with roughly 15% Situation, 10% Task, 60% Action and 15% Result. The Action is what the interviewer is scoring: say **I**, not **we**, for the parts you personally did.
> - A good HR answer has three parts: **a direct answer → a short proof from your experience → what it means for the company.**

**Sections**
★ Your 6 Signature STAR Stories (start here)
1. Introduction and Core HR Questions (40)
2. Globant Internship: Behavioral (40)
3. Gagangiri Foods Internship: Behavioral (40)
4. Projects: Behavioral, AI-SQL Assistant and URL Shortener (40)
5. Achievements: Leadership and Teamwork, SIH, Citi, OneEarth, Cummins (40)
6. Skills, Learning and Growth (40)
7. Strengths, Weaknesses, Failure and Conflict (40)
8. Barclays, Role and Career Goals (40)
9. Situational and Stress Questions (40)
10. Salary, Relocation, Joining and Wrap-up (40)
11. Barclays Values Deep-Dive: RISES (60)
12. Final Tips and Checklist

> **Facts to verify on home.barclays before you go:** the exact wording of the Purpose and Values page, the current business divisions, and the India/Pune technology and operations presence. Use only what you have checked.

---

# ★ YOUR 6 SIGNATURE STORIES: POLISHED 2-MINUTE STAR ANSWERS (START HERE)

> **Why these six:** Each one comes straight from a line on your resume, so you can defend it under follow-up questions. Together they cover all five Barclays values (RISES) and almost every "tell me about a time..." question.
> **How to use:** (1) Read the **STAR outline** so you know the structure. (2) Practice the **spoken version** aloud until it takes about two minutes (roughly 280 words). (3) Fill every **[bracket]** with your true detail, or cut that sentence. (4) Before the interview, write one line next to each story: *which question types it answers* (matrix at the end of this block).
> **Rule:** Use "I" for what you personally did and "we" for team results. Never add a number you can't back up.

---

## STORY 1: The N+1 Query Fix at Globant
**RISES covered:** Excellence (primary), Integrity, Service
**Use for:** "greatest technical achievement", "problem you solved", "going beyond the task", "using data to decide", "improving something that already worked".

**STAR outline**
- **S (Situation):** At Globant, an API behind a dashboard worked correctly but got slower as data grew. [Name the dashboard/endpoint type.]
- **T (Task):** Find the real cause and bring response time down without changing behaviour.
- **A (Action):** I measured first instead of guessing: query logs and execution plans showed the same query repeating inside a loop (an N+1 pattern). I replaced it with an optimized SQL JOIN, added indexes on the join and filter columns and re-measured.
- **R (Result):** Queries dropped from N+1 to one or two, and response time fell from [X ms] to [Y ms] (or "noticeably faster and stable as data grew").
- **Learning:** Measure, fix, re-measure. Done means "will it hold up in real use?", not just "does it work?".

**Spoken version (about 2 minutes)**
"At Globant I worked on a dashboard whose API returned correct data but slowed down as the data grew. Nobody had flagged it as broken, but I didn't want to leave it that way, so I decided to find out why. Instead of guessing, I looked at the query logs and the execution plans, and I noticed the same query running again and again inside a loop. That's a classic N+1 problem: one query for the list, then one more for every row. With a hundred rows, that's a hundred and one round trips to the database. I rewrote the data fetching as a single SQL JOIN so the database matched the rows in one pass, and I added indexes on the columns used for joining and filtering. Then I measured again to confirm it helped. The number of queries dropped from N plus one to one or two, and response time improved from [X] to [Y] milliseconds. What I took from this is a habit: measure first, fix the real cause, then measure again. And I try to look one step beyond 'it works' to 'will it still work with real data?'. That's the standard of excellence I'd like to bring to Barclays."

---

## STORY 2: Gagangiri Foods: Honest Results for a Real Client
**RISES covered:** Integrity (primary), Service, Respect, Excellence, Stewardship
**Use for:** "working with stakeholders", "ambiguity", "being honest when it's hard", "teamwork", "handling a requirement that wasn't clear", "making a difference".

**STAR outline**
- **S (Situation):** In a team of four, I worked on an industry project with Gagangiri Foods to automate microplastic detection in water. The client's request was broad.
- **T (Task):** Turn it into clear, measurable deliverables and build a working prototype for production monitoring.
- **A (Action):** I coordinated with stakeholders to define what they needed. The team built a CNN classifier on Digital Holographic Microscopy data and applied optimization techniques, and we built a dashboard around what operators needed to see. When we reached 80.9% binary accuracy, I reported it as a prototype result on limited data, with its limits and a plan to improve.
- **R (Result):** A working end-to-end prototype and dashboard, 80.9% binary accuracy, and stakeholders who trusted our numbers.
- **Learning:** Clients care about outcomes and honesty more than model architecture, and a prototype is only valuable when others can use and continue it.

**Spoken version (about 2 minutes)**
"At Gagangiri Foods, my team of four built a prototype to detect microplastics in water automatically. The request started very broad: 'detect microplastics'. So first I worked with the stakeholders to understand what decision the tool would support. It turned out they needed fast, trustworthy screening for production, not a research model. We combined CNN-based classification with Digital Holographic Microscopy data, tuned the model and built a dashboard for real-time monitoring, designed around what operators would actually look at. When the model reached 80.9% accuracy on the binary task, there was a temptation to present it as a finished solution. Instead, I presented it as a prototype result on limited data, explained where it would fail and showed our plan to improve it, such as more labeled data and transfer learning. Because we were honest about limits, the stakeholders trusted the work and were more open with feedback. We also documented the preprocessing, settings and results so the company could continue without us. For me that experience showed that integrity and service go together: tell people the truth about what the tool can do, and build it around the people who will use it."

---

## STORY 3: Leading Six People to the Smart India Hackathon Finals
**RISES covered:** Respect (primary), Excellence, Integrity
**Use for:** "leadership", "handling conflict", "working under pressure", "your biggest achievement", "managing a team with different skills", "tough decisions".

**STAR outline**
- **S (Situation):** I led a six-member team at the Smart India Hackathon 2024 Grand Finale, one of the Top 500 teams out of 88,221 nationwide, with a real problem statement and a continuous, time-limited build.
- **T (Task):** Deliver a working, convincing solution and keep a team of different skills aligned.
- **A (Action):** I defined an MVP, assigned owners by strengths, set integration checkpoints and handled final integration. When two teammates disagreed on architecture, I let each explain for two minutes, compared options on time and risk, chose the simpler one with reasons and gave the other person ownership of a critical component. I froze features early to protect demo stability.
- **R (Result):** A working demo and 2nd place (runner-up) nationally.
- **Learning:** A leader's job is clarity and fairness, not doing everything.

**Spoken version (about 2 minutes)**
"In December 2024 I led a team of six in the Grand Finale of the Smart India Hackathon. We were among the top 500 teams out of more than 88,000 nationwide, and we had a limited time to build a working solution to a real problem statement. My first step was to define the MVP: the smallest end-to-end flow that would show the value. Then I assigned owners by strengths and set checkpoints so integration didn't happen at the very end. At one point two teammates strongly disagreed about the architecture. I let each of them explain their approach for two minutes, compared the options on time and risk, picked the simpler one and explained why, and I gave the other person ownership of a critical component so their contribution still mattered. I also made the unpopular decision to freeze features hours before the end, because a stable demo mattered more than one more feature. We finished as the national runners-up. What I learned is that leading is about clarity and fairness: making sure every person's skills are used and that decisions come with reasons. That's how I'd like to work in a team at Barclays: respecting everyone's contribution and still delivering."

---

## STORY 4: The URL Shortener: Let the Database Enforce the Rules
**RISES covered:** Excellence (primary), Integrity, Stewardship
**Use for:** "technical challenge", "choosing correctness over speed", "refusing a shortcut", "learning from a mistake or near-mistake", "attention to detail".

**STAR outline**
- **S (Situation):** For my URL shortener (Java, React, SQLite), users can claim custom aliases. Two people could request the same alias at the same moment.
- **T (Task):** Guarantee that every alias is unique and consistent, even under concurrent requests.
- **A (Action):** The quick option was to check in application code whether an alias existed and then insert it, but two requests can both pass that check. I enforced uniqueness with a database UNIQUE constraint, handled the violation by returning HTTP 409 and tested by sending parallel requests with the same alias.
- **R (Result):** Only one request succeeds and the rest receive a clear conflict response; no duplicates in my tests.
- **Learning:** Critical rules belong where they can't be bypassed; correctness under real conditions beats a faster build.

**Spoken version (about 2 minutes)**
"One project I'm proud of is a URL shortener I built with Java and React. It lets users choose a custom alias, which created an interesting problem: what if two people ask for the same alias at the same moment? The quick approach is to check in code whether the alias exists and, if not, insert it. But that's a race condition, because two requests can both pass the check before either one inserts. I knew that would pass casual testing and fail under real traffic. So I made the database enforce uniqueness with a UNIQUE constraint, caught the violation and returned an HTTP 409 Conflict with a clear message. Then I tested it by firing parallel requests with the same alias to confirm that exactly one succeeded and the others were rejected cleanly. It took longer than the shortcut, but the result was correct under real conditions. The lesson I carry is that important rules should be enforced by the system itself, where they can't be skipped or raced, and that applies even more in banking, where a small inconsistency can affect real customers' money."

---

## STORY 5: The AI-SQL Assistant: Building Safety Into an AI Tool
**RISES covered:** Integrity (primary), Service, Respect, Stewardship
**Use for:** "innovation", "responsible use of AI", "security thinking", "designing for users", "a project you built end to end", "why banking".

**STAR outline**
- **S (Situation):** Many people who need answers from databases can't write SQL, and letting an LLM generate SQL creates risks: wrong queries, SQL injection and abuse.
- **T (Task):** Build an NL2SQL assistant (Python, LangChain, SQLite) that non-technical users, including regional-language speakers, could use safely.
- **A (Action):** I built the question-to-SQL pipeline with LangChain, added multilingual voice input for accessibility, and layered in safety: a salted SHA-256 hashing auth layer, input validation against SQL injection and a rate limiter. I treated the model's output as untrusted and validated it before running it.
- **R (Result):** A working assistant that turns natural-language questions into executable SQL while blocking unsafe input and abuse.
- **Learning:** Never trust model output blindly; safety and usability must be designed together. (I'd upgrade hashing to bcrypt or Argon2 for production, since SHA-256 is fast and therefore weaker for passwords.)

**Spoken version (about 2 minutes)**
"I noticed that many people who need answers from data depend on engineers because they can't write SQL. I wanted to build an assistant that lets them ask in plain language, even by voice and in different languages, and get answers. But letting an AI write SQL raises a serious question: what if it's wrong, or if someone misuses it? So I built it with safety from the start. Using Python and LangChain, I created the pipeline from question to SQL, and I treated everything the model produced as untrusted: it's validated before it runs. I added input validation to prevent SQL injection, an authentication layer with salted password hashing and a rate limiter to stop abuse. I also added multilingual voice input so that people more comfortable in their own language can use it. Looking back, I know salted SHA-256 isn't ideal for passwords, because it's fast; for production I'd use bcrypt or Argon2. That's what I'd like to bring to a bank: AI features that are genuinely helpful to people, built with security, honesty about limitations and respect for the users who rely on them."

---

## STORY 6: From Runner-Up to Winning: Learning Across Three Hackathons
**RISES covered:** Excellence (primary), Respect, Stewardship
**Use for:** "resilience", "learning from setbacks", "failure", "continuous improvement", "teamwork", "what motivates you".

**STAR outline**
- **S (Situation):** After finishing as runners-up at Smart India Hackathon (Dec 2024), I wanted to turn the experience into better results. [Confirm what you felt you could do better, e.g., scope, polish, presentation.]
- **T (Task):** Apply those lessons as team leader in the next competitions.
- **A (Action):** I held a short review of what we'd improve: [tighter scope, early integration, a rehearsed pitch and a backup demo]. I applied them at the Citi Ada Lovelace Hackathon (Oct 2025) and the OneEarth International Hackathon (Nov 2025), and made sure every member's skills and credit were visible.
- **R (Result):** 1st place out of 198 teams from 5 colleges at Citi Ada Lovelace, and 1st runner-up with a £500 prize at OneEarth.
- **Learning:** Treat each result as feedback, review it honestly and change specific behaviours.

**Spoken version (about 2 minutes)**
"After our team finished as runners-up at the Smart India Hackathon in December 2024, I could have simply been proud of the result. Instead I asked myself and the team what we could do better. [Say your real lessons, for example: scope the MVP smaller, integrate earlier, and rehearse the pitch with a backup demo.] I made those lessons our working rules in the next events, where I led the team again. At the Citi Ada Lovelace Hackathon in October 2025, we placed first out of 198 teams from five colleges, and a month later at the OneEarth International Hackathon, we were first runners-up and received a £500 prize and industry recognition for sustainable innovation. In each event I also made sure everyone's skills were used and that credit went to the whole team, because strong teams stay strong only if people feel valued. What I take from this is that results are feedback: you review them honestly, change specific habits and then apply them again. That's the attitude I'd bring to Barclays: aim for high standards, learn from every outcome and bring the team with me."

---

## STORY-TO-QUESTION MATRIX (pin this on your wall)

| If they ask about... | Lead with | Back-up story |
|---|---|---|
| Greatest technical achievement | Story 1 (N+1) | Story 4 (URL shortener) |
| Teamwork or leadership | Story 3 (SIH) | Story 6 (hackathon streak) |
| Handling conflict or disagreement | Story 3 (architecture disagreement) | Story 2 (stakeholders) |
| Integrity or honesty (RISES) | Story 2 (honest 80.9%) | Story 5 (security design) |
| Customer/Service focus (RISES) | Story 2 (dashboard for operators) | Story 5 (accessible AI assistant) |
| Excellence or high standards (RISES) | Story 1 (N+1) | Story 4 (correctness over speed) |
| Respect or inclusion (RISES) | Story 3 (leading six people) | Story 6 (shared credit) |
| Stewardship or long-term thinking (RISES) | Story 2 (documentation, water quality) | Story 5 (responsible AI) |
| Handling ambiguity | Story 2 (broad requirement) | Story 5 (building safely) |
| Working under pressure or deadlines | Story 3 (finals) | Story 6 (three events) |
| Learning from failure or setbacks | Story 6 (runner-up to winner) | Story 5 (SHA-256 limitation) |
| Innovation or creativity | Story 5 (AI-SQL assistant) | Story 2 (CNN + DHM) |
| Taking a risk or doing the right thing | Story 4 (rejecting the shortcut) | Story 2 (honest reporting) |

**Practice plan (3 days)**
1. **Day 1:** Fill every bracket with your true details. Read each spoken version aloud once, then say it from the STAR outline without reading.
2. **Day 2:** Record yourself (phone voice memo) for each story, aiming for 90 to 120 seconds. Cut filler and long setup. Check that the **Action** is the longest part.
3. **Day 3:** Ask a friend to shuffle the question types from the matrix and choose which story you'd use. Practice the first sentence that bridges a question to your story: *"A good example of that is when I..."*.

**One gap to fill yourself:** these six are all achievements. Interviewers (especially at Barclays, where Integrity and Stewardship carry weight) also ask for **a mistake or failure** and **a real conflict with a person**. Write one true story for each in the same S/T/A/R shape. If it's small, that's fine: a real, honestly owned mistake, such as a missed estimate or a bug you shipped and fixed, is more convincing than a polished one that sounds invented.

---

# SECTION 1: INTRODUCTION AND CORE HR QUESTIONS

### Q1. Tell me about yourself.
Good morning. I'm [Name], a final-year [branch] student at [college]. I enjoy building software that solves real problems, and over the last year I've worked on that in different settings. I interned at Globant in Pune as a Software Developer, where I built MERN-stack features, implemented secure JWT authentication and fixed a database performance bottleneck. I also worked on an AI/ML industry project with Gagangiri Foods, where my team of four prototyped microplastic detection using CNNs and built a monitoring dashboard. Alongside this, I built projects like an AI-based SQL assistant and a URL shortener, and I've led teams in hackathons: runner-up at a Smart India Hackathon grand finale, first place at the Citi Ada Lovelace Hackathon, and first runner-up at the OneEarth International Hackathon. I'm also a Cummins Nurturing Brilliance Scholar. I'm now looking for a role where I can contribute from day one, learn from strong engineers and take on growing responsibility.
**Tip:** 60-90 seconds. Don't recite your whole resume; leave hooks for the interviewer to ask about.

**RISES link:** Close with what you value: honesty, serving users, leaving things better than you found them. It signals RISES without sounding scripted.

### Q2. Walk me through your resume.
I'll go section by section. **Experience:** two internships. Globant gave me a professional engineering environment (code reviews, sprints, security and performance concerns), and Gagangiri gave me client-facing AI work with ambiguous requirements. **Achievements:** hackathons where I mostly led teams, which show I can deliver under pressure and coordinate people. **Projects:** two end-to-end builds, one around AI and security and one around backend fundamentals. **Skills:** Java, Python and JavaScript are my core languages, with SQL and NoSQL databases and the React/Node ecosystem. The common thread is that I take a vague problem, learn what's needed, and deliver something that works.

### Q3. Why should we hire you?
Three reasons. First, **proven experience**: I've already worked in a professional engineering team and on a client-driven AI project, so I understand real workflows, not just classroom ones. Second, **ability to deliver under pressure and lead**: my hackathon results came from planning scope, dividing work and finishing demos on tight deadlines. Third, **learning speed and ownership**: when I met an unfamiliar domain (holographic microscopy) or a hard bug (N+1 queries), I researched, measured and fixed it. I would be productive quickly, and I'm eager to keep improving with the team.

**RISES link:** Excellence (you checked beyond 'done') and Integrity (you measured instead of guessing).

### Q4. What are your strengths?
My top three. **(1) Ownership and leadership:** I naturally take responsibility, for example leading six-member and smaller teams in hackathons and coordinating stakeholders at Gagangiri. **(2) Fast learning:** I picked up a new domain and a new framework (LangChain) within weeks and applied them in working systems. **(3) Evidence-based problem solving:** at Globant I diagnosed a slow API by measuring queries rather than guessing, which led to a real improvement. These help a team because they make me reliable and low-maintenance.

**RISES link:** Integrity: an honest self-rating is credible and shows self-awareness.

### Q5. What are your weaknesses?
I tend to take on too much myself instead of delegating, particularly under deadline pressure. During a hackathon I found myself debugging teammates' modules late at night instead of trusting them with it. I recognized that it doesn't scale, and I now assign clear ownership at the start, hold short check-ins and step in only when someone is blocked. It's improving, but it's something I keep watching. I'm also still building experience in cloud and DevOps, which I'm addressing by learning Docker and deploying my projects.
**Tip:** Choose a *real* weakness that's not critical to the job, and always show the fix.

**RISES link:** Integrity (an honest weakness) and Respect (delegating means trusting teammates).

### Q6. Where do you see yourself in five years?
I see myself as a strong engineer who owns significant parts of a product, understands system design and mentors newer team members. Technically, I want depth in backend systems and real experience with AI-enabled features in production. In terms of growth, I'd like to move toward technical leadership, and I hope to build that path here as I grow with the company.

**RISES link:** Stewardship: growth that also benefits the team and the systems you work on.

### Q7. Why do you want to join Barclays?
Three reasons, each tied to something real. **(1) Values:** Barclays describes Respect, Integrity, Service, Excellence and Stewardship as its moral compass, and those match how I already work: I credit teammates, I report results honestly, I design for users, I hold myself to high standards and I document so others can continue my work. **(2) The work:** technology in a bank has to be secure, correct and reliable at scale, which is exactly the kind of engineering I enjoy (database performance, authentication, data integrity). **(3) Learning:** I want strong mentors, exposure to real customer-facing systems and global teams. [Add one verified, specific fact from Barclays' website or recent news, and the team or business area you'd like to join.]

### Q8. Why did you choose your branch and college?
[Truthful.] I chose [branch] because I enjoyed problem-solving and building things, and I wanted a field where I could keep learning for years. My college gave me the platform: labs, hackathon culture, placements and mentors, and I used those opportunities through internships and competitions.

### Q9. What are your hobbies and interests?
[Truthful, 1-2.] For example: I enjoy hackathons and building side projects because I like rapid problem-solving, and in my free time I [read tech blogs/play sport/music]. These help me stay curious and balanced. Pick hobbies that show something positive: discipline, teamwork or creativity.

### Q10. What motivates you?
Seeing something I built actually work and be useful. A clear problem, a deadline and a team to build with energize me. I'm also motivated by learning, because every project I've done has taught me something I didn't know before.

**RISES link:** Service: say the best part is seeing people actually use and benefit from what you built.

### Q11. How would your friends and professors describe you?
Friends: dependable and calm under pressure, the person who organizes the team. Professors and mentors: curious, hardworking and responsible. [Add a real anecdote, e.g., a mentor trusting you with a task.] Specific examples make it believable.

### Q12. What's your biggest achievement?
Leading my team to the Grand Finale of the Smart India Hackathon 2024 and finishing as runners-up, among the top 500 teams out of 88,221 nationwide. It combined technical delivery with leadership and handling stress. [STAR version in Section 5, Q3.]

**RISES link:** Respect (credit the team) and Excellence (high standards under pressure).

### Q13. What does success mean to you?
Delivering work that has real impact, continuously growing as an engineer, and helping my team succeed. I don't define it only by titles or marks; I define it by whether I'm learning, contributing and trusted.

### Q14. How do you handle pressure and deadlines?
I break work into milestones, prioritize must-haves, communicate early if there's a risk and stay calm. Hackathons trained me well for this: with 24-36 hours, you quickly learn to cut scope, keep a stable demo and use the last hours for testing rather than new features. In the internship, I applied the same habit with sprint tasks.

**RISES link:** Integrity: flag risks early instead of hiding delays.

### Q15. Are you a team player or do you prefer working alone?
Mostly a team player: every major result on my resume came from a team. I'm also comfortable working independently, as in my solo projects. What matters is picking the right mode: collaborating when problems are ambiguous and working independently when I own a clear task.

**RISES link:** Respect: value teammates' strengths and give credit.

### Q16. How do you prioritize multiple tasks?
I list all tasks, rank them by urgency and impact, tackle the critical ones first, and clarify trade-offs with whoever owns priorities. I review the list daily because priorities change. When I was handling college work alongside two internships, this habit was essential.

### Q17. How do you handle feedback?
I welcome it and treat it as free learning. At Globant, code-review feedback on naming, error handling and query structure improved my code, and I applied the same patterns afterwards so the same comment didn't repeat. I listen without defending, ask clarifying questions if needed, and follow up with the change.

**RISES link:** Respect and Excellence: take feedback gracefully and act on it.

### Q18. How do you stay updated with technology?
Official documentation, tech blogs and YouTube, plus building small projects with new tools, because I learn best by doing. For instance, I learned LangChain by building the AI-SQL assistant, and I learned query optimization by working on a real performance problem.

### Q19. What do you do when you don't know something?
I research first: documentation, examples, experiments. If I'm still stuck after a reasonable time, I ask a teammate or mentor with a specific question that shows what I've already tried. I'd rather say "I don't know yet, I'll find out" than guess and create a mistake.

**RISES link:** Integrity: admitting what you don't know yet is a strength in a bank.

### Q20. What are your career goals?
Short-term: become productive quickly, learn the company's stack and quality standards. Medium-term: own features end to end and contribute to design decisions. Long-term: grow into a technical leadership role while continuing to build.

### Q21. Are you a leader or a follower?
Both, depending on the situation. I've led teams in four competitions, so I'm comfortable taking charge, but I'm equally comfortable following when someone has more expertise or when a mentor sets the direction. Good teams need people who can do both.

**RISES link:** Respect: following when others know better is as important as leading.

### Q22. Describe your ideal work environment.
Collaborative and supportive, with clear goals, honest feedback, room to learn and ownership of meaningful work. I do well where people share knowledge and where mistakes are treated as learning, not blame.

**RISES link:** Respect: describe an inclusive, honest, supportive environment.

### Q23. What is your learning style?
Learn by doing: read the minimum to get started, build something small, then go deeper when I hit a problem. I also like to write down what I learn, which helps retention and creates notes for others.

### Q24. How do you manage studies along with internships and hackathons?
Time-blocking and weekly planning. In January to April 2026 I had overlapping commitments, so I fixed schedules for each, finished academic tasks in advance, and communicated my constraints early to everyone involved. [Add real specifics.] It taught me discipline and honesty about my capacity.

### Q25. Tell me something that's not on your resume.
[Choose a genuine one: a side interest, a community role, a small project, a personal habit.] Pick something that shows character, e.g., "I mentor juniors informally" or "I keep a learning journal of bugs I've fixed."

### Q26. What is your biggest regret?
[Truthful and minor.] For example: "Not starting hackathons and internships earlier, because I learned more through doing than through classes. Since realizing that, I've tried to build and compete regularly." Turn the regret into action.

### Q27. Where did you grow most in the last year?
From someone who builds projects to someone who delivers for stakeholders. Two internships and three hackathons taught me planning, communication, ownership and handling ambiguity. Technically, I moved from "making it work" to "making it secure, fast and maintainable."

**RISES link:** Excellence: growth from 'making it work' to 'making it secure, fast and maintainable'.

### Q28. What do you do in your free time?
[Truthful.] Mix of building side projects/learning, and [sports/music/reading] to recharge. Keep it genuine and brief.

### Q29. Describe yourself in three words.
Driven, adaptable, accountable. *Driven:* I push through hard deadlines like hackathons. *Adaptable:* I moved between MERN web work and ML research. *Accountable:* I own my tasks and communicate early when something slips. Always back each word with an example.

### Q30. How do you define teamwork?
Shared goals, clear roles, open communication and helping each other when someone is stuck. In hackathons, the teams that win aren't the ones with the smartest individual, but the ones that integrate smoothly and trust each other.

**RISES link:** Respect: shared goals and valuing every contribution.

### Q31. What would you do if you didn't get this job?
I'd ask for feedback if possible, work on the gaps, keep building projects and apply to similar roles. But I'm confident that I'd add value here, and I'd also treat rejection as data, not as a verdict.

### Q32. What do you know about Barclays?
Barclays is a long-established global bank (its own materials say over 330 years old). It describes its purpose as working together for a better financial future, and it says its five values (Respect, Integrity, Service, Excellence, Stewardship, or RISES) are its moral compass. It also has a code of conduct, The Barclays Way, which applies to colleagues everywhere. [Before the interview, check the current business divisions, the technology and operations presence in India/Pune, and one recent announcement on the Barclays website, and mention only what you've verified.]

### Q33. What role do you expect to play in our company?
A software engineer role where I can build and ship features, learn the codebase, and contribute to team goals, taking on more responsibility as I grow. I'm open to where the team needs me most.

### Q34. How do you handle criticism from seniors?
Listen fully, ask clarifying questions, separate the idea from my ego and act on it. If I disagree, I explain my reasoning respectfully with data, but I accept the final decision and focus on making it work.

**RISES link:** Respect and Integrity: accept criticism; disagree with evidence, not ego.

### Q35. How do you make decisions?
Gather facts, list options with trade-offs, consult people affected when time allows, decide, and review the outcome. In hackathons, time pressure forced quick decisions, so I learned to decide with 70% information and adjust.

### Q36. Which quality do you most want to improve?
Delegation and confidence in public speaking. I'm working on both through leading meetings, presenting in hackathons and trusting teammates with ownership.

### Q37. What is your communication style?
Clear, concise and proactive. I give updates before I'm asked, confirm my understanding of requirements, and summarize decisions in writing. With non-technical people I use simple language and examples.

**RISES link:** Respect and Service: clear, kind, proactive communication.

### Q38. How do you deal with ambiguity?
Clarify what I can with stakeholders, state assumptions explicitly, start with a small version and iterate. This was the key lesson at Gagangiri, where "detect microplastics" had to be turned into measurable deliverables.

**RISES link:** Excellence: clarify, state assumptions, iterate.

### Q39. What's one thing you'd like to change about yourself?
A tendency toward perfectionism: I sometimes over-polish. I've learned to ship a working MVP first, get feedback and then improve, which hackathons reinforced.

**RISES link:** Excellence: ship a good MVP, but keep high standards for what must be right.

### Q40. Do you have any questions for us?
Yes. (1) What does onboarding and mentoring look like for freshers? (2) What learning and growth opportunities exist in the first year? (3) What would success look like in this role after six months? (4) What does the team's engineering culture look like (code reviews, testing, releases)?

---

# SECTION 2: GLOBANT INTERNSHIP: BEHAVIORAL (Software Developer Intern, Jan–Apr 2026)

### Q1. Tell me about your internship at Globant.
For four months I worked as a Software Developer Intern on full-stack web applications built with the MERN stack. I built REST APIs and dashboards, implemented JWT authentication using access and refresh tokens, assisted with PostgreSQL schema design, and diagnosed a slow API caused by an N+1 query problem, which I fixed using SQL JOINs and indexes. It was my first time working inside a professional engineering team with sprints, code reviews and quality expectations, and it changed how I think about writing code: not just "does it work?" but "is it secure, fast and maintainable?"

**RISES link:** Excellence and Integrity: not just 'does it work' but 'is it secure, fast and maintainable'.

### Q2. Why did you choose this internship?
Globant is a well-known global digital engineering company, and I wanted exposure to real client-grade work, not just college projects. I specifically wanted to learn how production full-stack systems handle security and performance, and the role offered both.

### Q3. How was your first day or first week? How did you adapt?
At first, the codebase and processes felt big. I spent the first week reading code, setting up the environment, understanding the architecture and asking my mentor targeted questions. I picked a small bug to fix to learn the workflow (branching, PR, review). By the second week I could take on tasks independently. My approach was to *start small, learn the flow, then scale responsibility.*

**RISES link:** Respect: asking questions and learning from others is how new joiners earn trust.

### Q4. What was your biggest achievement at Globant? (STAR)
- **S (Situation):** An API powering a dashboard was slow, and it got worse as data grew.
- **T (Task):** I was asked to investigate and improve its performance.
- **A (Action):** I examined the query logs and execution plans, noticed the same query running in a loop (an N+1 pattern), replaced it with an optimized JOIN and added indexes on the key columns, then re-measured.
- **R (Result):** API response time dropped [X%/from X ms to Y ms], the dashboard became smoother, and my mentor [appreciated/merged the change]. I learned to measure first and fix second.

**RISES link:** Excellence (measured improvement) and Service (a faster dashboard for real users).

### Q5. Tell me about a time you faced a technical challenge. (STAR)
- **S (Situation):** While implementing the JWT refresh flow, users were randomly logged out.
- **T (Task):** Find the cause and ensure a smooth session.
- **A (Action):** I reproduced the issue, checked the network calls and found that parallel requests were triggering several refresh calls that invalidated each other's tokens. I introduced a single queued refresh so other requests waited for it.
- **R (Result):** The random logouts stopped, and I documented the fix for the team. [Adapt to your real issue.] The lesson: reproduce reliably, then change one thing at a time.

**RISES link:** Excellence: reproduce, isolate, fix, document. Stewardship: you documented the fix for the team.

### Q6. How did you handle tasks you had no knowledge of?
I broke the task into smaller parts, read official docs, tried tiny experiments and then asked my mentor with a clear description of what I'd tried. For PostgreSQL schema design, I studied normalization and indexing and applied them to a real schema within days, instead of waiting to feel "ready."

### Q7. How did you receive and apply feedback?
Code-review comments covered naming, error handling and query structure. I made the requested changes, asked "why" where I didn't understand, and noted recurring comments in a personal checklist so the same issue didn't appear again. Over the internship, the number of review comments per PR dropped, which told me the feedback was working.

**RISES link:** Respect: you took feedback seriously and acted on it.

### Q8. Tell me about a mistake you made at work. (STAR)
- **S (Situation):** [E.g., I pushed a change that broke a dependent feature.]
- **T (Task):** Fix it quickly and prevent recurrence.
- **A (Action):** I informed my mentor immediately instead of hiding it, rolled back/fixed the change, and added a check/test for that scenario.
- **R (Result):** Resolved within [time]. Lesson: test dependent flows, and communicate early. Managers value honesty and speed in recovery over perfection.

**RISES link:** Integrity: the strongest part is that you told your mentor immediately.

### Q9. How did you work with your team?
Daily stand-ups to share progress and blockers, sprint tasks tracked on a board, pull requests with review, and active communication in team channels. I tried to be reliable: do what I said I'd do, flag blockers early and help others when I could.

**RISES link:** Respect: reliable, open communication.

### Q10. How did you manage your time between college and the internship?
I planned weekly, blocked fixed hours for work, finished college tasks ahead of time and communicated my academic schedule to the team early. [Specifics.] The result was that I met my sprint commitments and kept up academically. It taught me realistic estimation.

### Q11. What did you learn about professional software development?
Code is read far more than it's written, so clarity matters. Communication is as important as coding. Performance problems come from real data, not assumptions. And "done" includes testing, review and documentation, not just a working feature.

**RISES link:** Excellence: quality includes testing, review and documentation.

### Q12. What did you like most and least about the internship?
Most: ownership of real features and seeing a measurable improvement from my optimization. Least: the initial learning curve of a large codebase, but that became a strength because it taught me how to read and navigate unfamiliar code quickly.

### Q13. Did you get any recognition or feedback from your manager?
[Truthful: positive feedback on [X], expanded responsibilities, a PPO/offer if any.] If there was constructive feedback, mention it honestly with what you did about it, e.g., "My mentor suggested writing more tests, and I've started doing that in my later projects."

### Q14. Did you ever disagree with a teammate or mentor?
[STAR.] I once suggested a different query approach. Instead of arguing, I showed the execution plan and timing for both options. My mentor raised concerns about maintainability, and we agreed on a hybrid. I learned to disagree with data and stay open to constraints I couldn't see.

**RISES link:** Respect and Integrity: disagree with evidence, then stay open.

### Q15. Tell me about a time you took initiative. (STAR)
- **S (Situation):** The Globant APIs I worked on had no clear written reference, so new teammates and I kept asking each other how endpoints behaved. [Adapt to your real case.]
- **T (Task):** Nobody had assigned it, but I saw it slowing the team and decided to fix it.
- **A (Action):** I wrote a short API guide listing endpoints, request/response examples, auth requirements and error codes, and checked it against the code with my mentor.
- **R (Result):** Teammates used it instead of asking, my mentor [appreciated it/asked me to keep it updated] and onboarding questions dropped.
- **Learning:** Small proactive improvements build trust, and documenting is a form of stewardship.

**RISES link:** Excellence and Stewardship: improvements nobody asked for, made for the team's benefit.

### Q16. How do you ensure the quality of your code?
Readable naming, modular structure, input validation and error handling, testing with Postman and [unit tests], self-review before raising a PR and acting on peer review. I also check edge cases: empty data, invalid tokens and large datasets.

**RISES link:** Excellence: quality habits are the proof.

### Q17. Did you work with deadlines? How did you handle them?
Yes, sprint deadlines. I estimated tasks, broke them into smaller pieces, communicated early if something would slip and prioritized core functionality first. If a task was growing beyond the estimate, I raised it right away rather than at the deadline.

### Q18. Were you ever stuck? What did you do?
Yes, several times. I timeboxed myself (say 30-45 minutes) to try on my own, then asked for help with a clear explanation of the problem and what I had already tried. This respects teammates' time and speeds up my learning.

### Q19. How did you handle security responsibilities (JWT)?
I took them seriously: short-lived access tokens, refresh token handling with revocation, secrets kept out of code, and guidance from my mentor on safe storage. I learned that security isn't a feature you add at the end; it influences design decisions from the start.

**RISES link:** Integrity and Stewardship: security from the start protects customers' data.

### Q20. What would you do differently if you repeated the internship?
Ask more questions earlier, learn the testing setup sooner, document my learnings as I go and volunteer for one more cross-team task. The first weeks are when asking is cheapest.

### Q21. How did you contribute beyond your assigned tasks?
[Examples: documentation, helping peers debug, sharing a short learning session, improving a small utility.] Choose something real, and say its impact.

**RISES link:** Stewardship: documentation and leaving things better.

### Q22. How did you handle working with senior engineers?
Respectfully and prepared: I came with context and specific questions, took notes so I didn't repeat questions, and acted on their advice quickly. I also tried to make their review easier with clear PR descriptions.

**RISES link:** Respect: preparation shows you value seniors' time.

### Q23. What did you learn about Agile?
Work is broken into sprints with a prioritized backlog; daily stand-ups keep everyone aligned; estimation is approximate; retrospectives improve the process; and delivering small increments reduces risk.

### Q24. Why didn't you continue or convert at Globant? (if asked)
[Truthful.] "It was a fixed-term college traineeship. I'm grateful for the experience, and now I'm looking for a full-time role where I can [grow in X/work on Y]." Never speak negatively about them.

### Q25. How did the internship change your career plans?
It confirmed my interest in backend and full-stack engineering, especially performance and secure design, and it showed me the value of working with experienced engineers, which is why I want to join a team with strong mentorship.

### Q26. Describe a time you helped a teammate. (STAR)
- **S (Situation):** A teammate was stuck for hours on an API response that didn't render in React. [Adapt.]
- **T (Task):** Help them unblock without just taking over, since the sprint deadline was close.
- **A (Action):** We paired for about 30 minutes: I asked what they'd already tried, we checked the network tab, found the data shape didn't match what the component expected and fixed the mapping together.
- **R (Result):** The feature was finished that day and they could handle similar issues afterwards.
- **Learning:** Asking questions beats giving answers, and I learned something from the debugging too.

**RISES link:** Respect and Service: helping teammates improves team outcomes.

### Q27. Tell me about a time you had to learn something quickly. (STAR)
- **S (Situation):** At Globant I was asked to help with PostgreSQL schema design and query tuning, areas I had only touched in coursework.
- **T (Task):** Become useful quickly, since the team needed the work for the sprint.
- **A (Action):** I studied normalization, indexing and `EXPLAIN ANALYZE`, practiced on the real schema in a dev environment and asked my mentor to review my first design.
- **R (Result):** Within about two weeks I contributed a reviewed schema and fixed a slow query using JOINs and indexes.
- **Learning:** Learn by applying immediately to a real problem and get feedback early.

### Q28. How do you handle repetitive tasks?
Automate where possible (scripts, templates), batch similar tasks and keep a quality bar through a checklist. If a task repeats often, it's a sign something should be automated.

### Q29. What are you proud of from the internship, beyond the code?
Becoming comfortable in a professional environment: communicating status clearly, asking for help at the right time and accepting reviews constructively. Those habits will carry across any job.

**RISES link:** Excellence: professional habits that transfer to any team.

### Q30. What advice would you give a future intern there?
Be curious, ask early, learn the codebase by fixing small bugs, write things down and don't wait to be perfect before contributing.

**RISES link:** Respect and Stewardship: helping the next intern is leaving things better.

### Q31. Tell me about a time you protected user or data security in your work. (STAR) **[RISES: Integrity, Stewardship]**
- **S (Situation):** While building login for a Globant application, I had to issue and verify tokens for real users.
- **T (Task):** Implement authentication that was secure, not just functional.
- **A (Action):** I used short-lived JWT access tokens with refresh tokens, kept secrets in environment variables, validated inputs, and followed my mentor's guidance on storing and revoking refresh tokens. When unsure, I asked instead of guessing.
- **R (Result):** The auth flow passed review and worked reliably, and I could explain each security choice.
- **Learning:** In systems that handle people's data, 'it works' is not enough; it must be safe.

### Q32. How did you handle sensitive or client data during the internship? **[RISES: Integrity, Respect]**
I used only the data and environments I was given access to, never copied client or project data to personal tools or public repositories, and kept credentials out of Git using `.env` files and `.gitignore`. If I needed more access than I had, I asked the owner instead of working around the restriction. In a bank, trust in how data is handled is the foundation of the business, and these small habits are exactly what I'd carry into Barclays.

### Q33. Tell me about a time you went the extra mile for quality. (STAR) **[RISES: Excellence]**
- **S (Situation):** An API endpoint returned correct data in testing, so it could have been marked done.
- **T (Task):** Make sure it would also hold up with real data volumes, which nobody had asked me to check.
- **A (Action):** I tested with larger data, saw response times grow, traced it to an N+1 query pattern, replaced it with a JOIN and added indexes, then re-measured.
- **R (Result):** Queries dropped from N+1 to a couple and response time improved from [X ms] to [Y ms].
- **Learning:** Excellence is looking one step past 'done' to 'will this still work in real use?'

### Q34. How did you keep your code easy for the next developer to work with? **[RISES: Stewardship, Excellence]**
I wrote readable names, kept functions small, separated routes, controllers and data access, added comments where the reason wasn't obvious and wrote clear PR descriptions. I also noted what I'd learned about the database indexes I added so others knew why they existed. Stewardship, to me, is leaving the codebase better than I found it, because someone else will inherit it.

### Q35. How did you work with colleagues who were senior to you or from different backgrounds? **[RISES: Respect]**
I treated every opinion as useful: I listened fully before responding, asked questions instead of assuming, and credited others' ideas in standups and reviews. Seniors often had context I didn't, and peers had different strengths, such as testing or UI polish, that I learned from. I also made sure to thank reviewers for their time. Respect means valuing each person's contribution regardless of title.

### Q36. Tell me about a time you raised a concern or pushed back honestly. (STAR) **[RISES: Integrity]**
- **S (Situation):** A planned shortcut would have worked for a demo but could slow down badly with large data. [Adapt.]
- **T (Task):** Raise the concern without sounding negative or blocking the sprint.
- **A (Action):** I spoke to my mentor privately, showed query timing as evidence and proposed a better approach with a similar time cost.
- **R (Result):** We adopted a middle path, avoided the performance problem and my mentor appreciated that I came with data and a solution.
- **Learning:** Honesty early is cheaper than fixing a known problem after it reaches users.

### Q37. Describe a time you thought about the end user's experience, not just the code. (STAR) **[RISES: Service]**
- **S (Situation):** The dashboards I built showed blank screens while data loaded and gave vague errors when something failed.
- **T (Task):** Make them usable for people who rely on them every day.
- **A (Action):** I added clear loading states, helpful error messages, pagination and better form validation, asking myself what would frustrate me as a daily user.
- **R (Result):** The dashboards felt faster and clearer, and [teammates/mentor] noticed fewer confused questions.
- **Learning:** Service means putting the person using the product at the centre of technical decisions.

### Q38. What would your mentor at Globant say about you? **[RISES: Respect, Excellence]**
[Truthful.] I'd expect them to say I'm eager to learn, ask good questions, take feedback well and follow through on tasks. They might also say I should speak up sooner when I'm blocked, which I've been working on. Being honest about both sides shows self-awareness.

### Q39. Was there a time your performance wasn't up to your standard? How did you recover? (STAR) **[RISES: Integrity, Excellence]**
- **S (Situation):** Early in the internship I underestimated how long a task would take and delivered later than promised.
- **T (Task):** Recover trust and avoid repeating the problem.
- **A (Action):** As soon as I realized, I told my mentor, explained the cause, gave a new realistic estimate and delivered it. After that I added buffers and gave short daily updates.
- **R (Result):** The task was finished by the new date, and later tasks were on time.
- **Learning:** Owning a miss early and fixing the process matters more than avoiding discomfort.

### Q40. How did you contribute to the team's goals, not just your own tasks? **[RISES: Service, Respect]**
I treated the sprint goal as shared: when I finished my work early, I picked up small bugs or helped teammates debug rather than waiting for new tasks. I also shared things I'd learned, such as how I diagnosed the slow query, so others could apply it. A team succeeds when people look beyond their own tickets.

---

# SECTION 3: GAGANGIRI FOODS INTERNSHIP: BEHAVIORAL (AI/ML Industry Project, Feb–May 2026)

### Q1. Tell me about this project.
In a team of four, I worked on an industry project for Gagangiri Foods to automate microplastic detection in water. We combined CNN-based classification with Digital Holographic Microscopy data, applied optimization techniques to reach about 80.9% binary classification accuracy, and built an interactive dashboard for real-time water-quality monitoring in commercial production. Beyond the model, a big part was coordinating with company stakeholders to make sure what we built matched their real business needs.

**RISES link:** Service (built for operators), Integrity (honest accuracy) and Stewardship (water quality).

### Q2. Why did you choose an AI/ML industry project?
I wanted to see how AI works with real, messy data and a real client, not just clean textbook datasets. It also fit my interest in building products where AI is part of a complete solution, including interface, data flow and business context.

**RISES link:** Stewardship: using technology for water safety.

### Q3. How did you get started in an unfamiliar domain?
I started with fundamentals: what microplastics are, why they matter for food production, and what DHM measures. Then I met stakeholders to understand what they actually needed, set a plan (data → baseline model → improvement → dashboard), and began with a simple baseline to learn from before optimizing.

### Q4. How did you coordinate with stakeholders? (STAR)
- **S (Situation):** The company's needs were broad ("detect microplastics").
- **T (Task):** Turn them into specific, measurable deliverables.
- **A (Action):** We held discussions to clarify acceptable accuracy for a prototype, output format, data availability and timeline, then shared regular progress updates with demos so feedback came early.
- **R (Result):** The deliverables matched their needs with no big surprises at the end, and we avoided wasting time on features they didn't value.

**RISES link:** Service and Integrity: clarify real needs, report progress honestly.

### Q5. What was your role in the team of four?
[Be specific.] I was responsible for [model development/dashboard/data preprocessing/stakeholder communication] and supported others where needed. Name one concrete task you owned and what you delivered.

### Q6. How did you divide responsibilities?
By strengths and interest, with a shared tracker and weekly reviews. Roughly: data and preprocessing, modelling and tuning, dashboard/backend integration, and documentation/stakeholder updates. We reviewed each other's work to avoid silos, so if someone was unavailable, others could continue.

**RISES link:** Respect: shared ownership and mutual review.

### Q7. Tell me about a conflict or disagreement in this team. (STAR)
- **S (Situation):** We disagreed on whether to start with a complex model or a simpler baseline.
- **T (Task):** Decide objectively without hurting team harmony.
- **A (Action):** We ran both quickly, compared validation results and chose according to the data.
- **R (Result):** The decision was made fast, the relationship stayed positive and the final model improved. Lesson: experiments settle debates better than opinions.

**RISES link:** Respect: experiments settled it fairly, with no ego.

### Q8. What was the hardest part of this project? (STAR)
- **S (Situation):** Limited and noisy data caused overfitting.
- **T (Task):** Improve accuracy without much new data.
- **A (Action):** Applied augmentation, regularization and hyperparameter tuning, and analyzed errors to see what the model got wrong.
- **R (Result):** We reached 80.9% accuracy. I learned that data quality often matters more than model complexity.

**RISES link:** Excellence: disciplined experiments. Integrity: honest limits.

### Q9. How did you handle a requirement change?
I re-scoped with the team, discussed the impact on timeline with the stakeholder, and prioritized the changes with the most value. I think the key is to treat a change as a trade-off conversation (what do we drop or delay?) rather than silently absorbing it.

### Q10. Are you satisfied with 80.9% accuracy?
It's a solid baseline for a prototype built with limited, real-world data, but I'm not satisfied that it's the end. I know what would raise it: more labeled data, transfer learning, better preprocessing and thorough error analysis. For a safety context, I'd also look at recall, not accuracy alone.

**RISES link:** Integrity: be upfront that 80.9% is a baseline, not production-ready.

### Q11. How did you communicate technical results to non-technical stakeholders?
I used simple language, visuals on the dashboard, and framed results in business terms (speed, reliability, early alerts) rather than only metrics. I avoided jargon, or I explained it with analogies: e.g., "the model is like a trained inspector who looks at each sample image."

**RISES link:** Respect and Service: speak in the listener's language.

### Q12. How did you manage deadlines in this project?
Milestones with small buffers, weekly goals, early demos to catch problems and clear ownership of tasks. When the model work took longer than planned, we prioritized a working dashboard integration so the end-to-end prototype was always demonstrable.

### Q13. What did you do when the model wasn't improving?
I stopped changing random things and analyzed errors: which samples were misclassified and why. That pointed to data variation and noise as the root causes, so we addressed those (augmentation, preprocessing) instead of just making the model bigger.

### Q14. How did you ensure accountability in the team?
Clear task ownership, short regular status updates, shared documentation and an honest culture of saying "I'm blocked." Accountability is easier when people are not afraid to raise problems early.

### Q15. What did you learn about working with a client?
Clarify expectations early, under-promise and over-deliver, share progress frequently and translate business needs into technical goals. Clients care about outcomes, not model architectures.

**RISES link:** Service: clients care about outcomes, not model architecture.

### Q16. How did you handle disagreements with the client or stakeholder?
Listen first to understand their concern, explain technical constraints with evidence, and offer alternatives rather than a flat "no." For example, if a feature wasn't feasible with our data, I'd propose a smaller version that delivers most of the value.

**RISES link:** Respect and Service: offer alternatives instead of a flat 'no'.

### Q17. What was your proudest moment in this project?
The first time the dashboard displayed live predictions on real data and the stakeholders could actually *see* the value. It turned an abstract model into something tangible.

### Q18. What would you do differently?
Start data collection and cleaning earlier, define evaluation metrics up front, and document experiments systematically so results are reproducible and easy to explain.

### Q19. How did you motivate the team when progress was slow?
Celebrated small wins, broke tasks into short goals, offered help to whoever was stuck, and reminded everyone of the real-world purpose of the project, which is to make water safer to produce and consume.

### Q20. Did you consider ethics or responsibility in this AI project?
Yes. For water safety, false negatives are costly, so we discussed recall, keeping a human in the loop, and not overclaiming accuracy. Being honest about limitations is part of responsible AI.

**RISES link:** Integrity and Stewardship: honest limits and safety-first thinking.

### Q21. How do you explain AI limitations to others?
Honestly and simply: models learn from data, can fail in new conditions, and need monitoring and human oversight. I give concrete examples of where it's reliable and where it isn't, so people use it appropriately.

### Q22. How did this project differ from your Globant internship?
Globant had established processes, a large codebase and clear engineering standards; Gagangiri was open-ended R&D with a client, a new domain and ambiguous requirements. One taught discipline and quality, the other taught problem framing and stakeholder communication. Together they made me a more complete engineer.

### Q23. Tell me about something creative you did here.
[E.g., a custom augmentation strategy, a dashboard feature for trend alerts, or a way to present model confidence to operators.] Describe the idea, why you tried it and its result.

### Q24. What would you tell the company about next steps?
Collect more labeled data, move to detection and counting of particles, validate against lab results, and deploy with monitoring and a retraining plan. I'd also suggest measuring time and cost saved to build the business case.

**RISES link:** Stewardship: plan beyond the prototype.

### Q25. How did you balance this with your Globant internship (the dates overlap)?
[Be honest and clear.] I split my time with a fixed schedule, kept both teams informed of my availability and prioritized by deadlines. [Explain the arrangement: e.g., one was part-time or integrated with college hours.] The key is transparency and reliable delivery on both.

### Q26. Did you face any ethical or confidentiality issues?
I respected the company's data confidentiality: used the data only for the project, shared it only within the team and avoided uploading to public tools. In professional settings, protecting client data is a basic trust requirement.

**RISES link:** Integrity: confidentiality is basic trust in banking.

### Q27. What feedback did the company give you?
[Truthful: appreciated the dashboard/communication; suggested improving accuracy or documentation.] Share one positive and one improvement area, with what you did about it.

### Q28. How did you demonstrate leadership here?
By taking initiative to clarify requirements with stakeholders and keeping the team aligned on priorities, even without a formal title. [Give a real example.] Leadership is often about initiative and clarity, not authority.

**RISES link:** Respect: leadership through initiative, not title.

### Q29. What soft skills did it build?
Stakeholder communication, collaboration, documentation, managing expectations and presenting technical work to a non-technical audience.

### Q30. Why should this experience matter to us?
It shows I can work with real data and clients, handle ambiguity, and deliver a usable product, not just code. Those skills transfer directly to any engineering team that builds for users and customers.

**RISES link:** Service and Excellence: usable products for real users.

### Q31. How did you make sure the accuracy you reported to the company was honest? **[RISES: Integrity]**
We reported the 80.9% figure on a held-out test set, said clearly it was a prototype result on limited data, and discussed limitations like dataset size and noise. We avoided cherry-picking the best run or reporting training accuracy as if it were real performance. In a domain related to water safety, overstating results could mislead decisions, so transparency was non-negotiable.

### Q32. How did you keep the end users, the operators who'd use the dashboard, in mind? **[RISES: Service]**
We asked what operators needed to see at a glance: clear status (OK/alert), recent history and simple controls. We avoided showing raw model outputs without context. I wanted the dashboard to help someone make a decision quickly, rather than impress a data scientist. That is what putting the user at the centre looks like.

### Q33. Tell me about a time you made sure a quieter teammate's ideas were heard. (STAR) **[RISES: Respect]**
- **S (Situation):** In a Gagangiri team meeting, one teammate had a useful preprocessing idea but wasn't sharing it. [Adapt.]
- **T (Task):** Get everyone's input before we decided how to improve the model.
- **A (Action):** I asked them directly for their view, gave them space to finish, and we tested the idea on a validation split.
- **R (Result):** It improved our results and the teammate started contributing more in later meetings.
- **Learning:** Inclusive teams make better decisions; I now ask every person for input.

### Q34. How does your project connect to sustainability? **[RISES: Stewardship]**
Detecting microplastics helps protect water quality, which affects public health and the environment. Automating detection can make monitoring more frequent and cheaper, so problems are caught earlier. It's a concrete example of technology being used to leave things better than we found them, which is the heart of stewardship.

### Q35. How did you document your work so others could continue it? **[RISES: Stewardship, Excellence]**
We recorded the data preprocessing steps, model settings, results and known limitations, and described how to run and update the dashboard. If the team or company staff changed, someone else could reproduce our work. A prototype that can't be continued has limited value, so documentation was part of delivery, not an afterthought.

### Q36. How did you maintain high standards when time was limited? **[RISES: Excellence]**
We prioritized what mattered most for the client: a working end-to-end prototype with honest metrics, over adding extra models. We tracked experiments, validated on a fixed split, and reviewed each other's work before demos. High standards under time pressure come from clear priorities and consistent habits.

### Q37. How did you handle the pressure of presenting to the company? **[RISES: Excellence, Respect]**
We rehearsed, divided who explained which part, prepared for likely questions and kept the demo simple and reliable. I focused on explaining in plain language so everyone in the room could follow. During questions, I listened fully and answered honestly, including saying "we don't know yet" when that was true.

### Q38. What would you do if the client wanted you to report a higher accuracy than you actually achieved? **[RISES: Integrity]**
I'd politely refuse to misstate the number and explain why: inflated results can lead to wrong decisions and harm trust later. I'd offer constructive alternatives, like showing the improvement path, reporting recall for the most critical class, or presenting the result as a baseline with next steps. Integrity means doing the right thing even when it's uncomfortable.

### Q39. How did you handle negative feedback from the stakeholders? **[RISES: Respect, Service]**
I listened without defending, asked clarifying questions to understand the underlying concern, and turned it into specific action items. For example, if they said the dashboard was hard to read, I asked which decision they were trying to make and redesigned the view around it. Negative feedback is a gift when it points to what the user needs.

### Q40. How did you make sure work in the team was shared fairly? **[RISES: Respect, Integrity]**
We tracked tasks openly, checked each person's workload in weekly syncs and adjusted when someone was overloaded. We credited contributions accurately in our updates. Fairness reduces resentment and keeps the team motivated, and it ensures nobody takes credit for another person's work.

---

# SECTION 4: PROJECTS: BEHAVIORAL

## 4A. Multilingual AI-SQL Assistant & Analysis Pipeline

### Q1. Tell me about this project in simple words.
It lets non-technical users ask questions about their data in plain language, even by voice and in different languages, and get answers, because the system converts the question into a safe SQL query. Since letting AI write SQL is risky, I also added authentication, protection against SQL injection and a rate limiter, so it's both useful and safe.

**RISES link:** Service (non-technical users) and Integrity (safety layer).

### Q2. What inspired this project?
I noticed that many people who need insights from data can't write SQL and depend on engineers for every question. AI can bridge that gap, but only if it's secure and reliable. That combination of accessibility and safety was the challenge I wanted to solve.

**RISES link:** Service: lowering the barrier so more people can use data.

### Q3. Why multilingual and voice input?
Many users in India are more comfortable in regional languages than in English, and voice makes the tool accessible to people who are not comfortable typing queries. I wanted to design for real users, not just developers. It also pushed me to handle messy input like transcription errors.

**RISES link:** Respect: inclusion through language and voice.

### Q4. How did you plan and execute it in about a month (Feb–Mar 2026)?
I defined an MVP first: text question → SQL → result. Then I added layers incrementally: authentication, validation, voice input and rate limiting, with a working version at each stage. Weekly milestones and a task list kept me on schedule, and I tested after each layer so problems were caught early.

### Q5. What challenge did you face and how did you solve it? (STAR)
- **S (Situation):** The LLM sometimes generated invalid or unsafe SQL.
- **T (Task):** Make results reliable and secure.
- **A (Action):** Improved the prompt with schema context and examples, added a validation layer, restricted queries to read-only SELECT statements and added a retry with error feedback.
- **R (Result):** Errors dropped significantly and dangerous queries were blocked. Lesson: never trust model output blindly.

**RISES link:** Integrity and Excellence: never trust model output blindly.

### Q6. What did you learn from it?
LLM application design and prompt engineering, security thinking (injection, password hashing, rate limiting) and the difference between a demo and something that is safe to use. I also learned that evaluation matters: you need test questions to know whether changes help.

### Q7. How is it useful in the real world?
Business analysts, small business owners and managers could get answers from databases without waiting for engineers, saving time and improving decisions. It could be embedded into dashboards or internal tools as a self-service analytics assistant.

### Q8. What was the hardest decision?
Balancing flexibility and security: allowing powerful queries makes the tool more useful but increases risk. I chose a restrictive design (read-only, validated) and accepted that some complex questions might not work, because safety matters more than coverage.

**RISES link:** Integrity: safety over coverage.

### Q9. How did you test it with real users?
[Truthful: classmates/friends/mentors tried it and gave feedback, e.g., confusion with ambiguous questions or voice errors.] I used their feedback to show the generated SQL, confirm the transcribed text and improve error messages.

### Q10. What would you improve?
Switch to bcrypt/Argon2 for password hashing, add role-based access control, generate charts automatically, and deploy with Docker and tests. I'd also build an evaluation set to track accuracy over time.

### Q11. Did you work alone? How did you manage it?
[Solo or team.] If solo: I handled planning, building and testing myself, using a task list and milestones, and I asked peers for feedback to avoid blind spots. Working solo made me responsible for the whole stack, which is valuable learning.

### Q12. What if a user misuses it?
Several layers limit the damage: rate limiting to stop abuse, input and SQL validation to block malicious queries, read-only permissions so data can't be changed and logging so misuse can be traced. Security should assume that some user will try to break it.

**RISES link:** Integrity and Stewardship: assume misuse and defend against it.

### Q13. What was your proudest part?
The security layer. It's what moved the project from a demo to something I could defend in a design discussion: I could explain the threats (SQL injection, prompt injection, brute force) and the controls I put in place.

**RISES link:** Integrity: security you can defend in a design discussion.

### Q14. How did you stay motivated?
By getting small visible wins: the first successful query, the first voice query, the first blocked injection attempt. Each win made the next layer exciting.

### Q15. Why will this project matter to our company?
It shows I can build AI features responsibly, thinking about security, usability and reliability, not just "calling an API." Any modern product team adding LLM features needs that mindset.

**RISES link:** Excellence: AI built responsibly is highly relevant to a bank.

### Q16. How did you think about user data privacy and trust in this assistant? **[RISES: Integrity, Stewardship]**
Users ask questions that reveal what they care about, and the data underneath may be sensitive. I limited what the system could access (read-only SELECT), planned logging only of what is necessary, kept API keys and credentials out of code and recognized that sending schema or rows to an external LLM has privacy implications. For real deployments, I'd mask sensitive columns, use approved model endpoints and add audit logs. Trust must be designed in from the beginning.

### Q17. How did you design the experience for non-technical users? **[RISES: Service]**
I made the interface accept plain questions (including voice), show results in a readable form, display the generated SQL for transparency and give friendly messages when something can't be answered. I tested with people who don't write SQL to see where they got confused. The aim was to make users feel supported, not blamed, when the system fails.

### Q18. How would you prevent the AI from giving wrong answers that lead to bad business decisions? **[RISES: Integrity, Excellence]**
By validating generated SQL against the schema, showing the query and result so users can verify, handling ambiguity through clarifying questions, testing on a set of known question-answer pairs and being clear about the assistant's limitations. For critical decisions, I'd recommend a human review step. A tool that is confidently wrong is more dangerous than one that admits uncertainty.

### Q19. How did multilingual and voice support relate to inclusion? **[RISES: Respect, Service]**
Many users are more comfortable in their regional language, and some find voice easier than typing. Supporting these means more people can use data without depending on someone else's technical or language skills. Inclusion in technology means designing for the users you have, not just the ones that are easiest to build for.

### Q20. How did you make the project maintainable? **[RISES: Stewardship]**
Modular code (auth, validation, LLM pipeline, DB access in separate parts), a clear README, configuration through environment variables and a small set of tests for the validator and auth. I thought about the person who would open this project six months later, which might be me.

## 4B. URL Shortener with Custom Aliases

### Q21. Tell me about this project.
It's a Bitly-like service built with Java and React. It generates short Base62 codes, supports custom aliases, expiry, QR codes, and handles redirects correctly with HTTP status codes. It taught me backend fundamentals deeply: HTTP, concurrency, databases and design trade-offs.

**RISES link:** Excellence: fundamentals done properly.

### Q22. Why did you choose this project?
It's simple to explain but rich in engineering problems: uniqueness, concurrency, HTTP semantics and scalability. It's a good way to demonstrate fundamentals, and every question in a design discussion (caching, analytics, scaling) can be anchored to something I actually built.

### Q23. Why Java HttpServer rather than a framework?
I wanted to learn what frameworks do under the hood, so I understood routing, threading and HTTP myself. That makes me better at using frameworks like Spring later, because I know what they abstract.

### Q24. What was your biggest challenge? (STAR)
- **S (Situation):** Two users could claim the same alias at the same moment.
- **T (Task):** Guarantee uniqueness and consistency.
- **A (Action):** Added a database UNIQUE constraint and handled the conflict (returning 409) instead of relying on a check-then-insert in code.
- **R (Result):** Concurrent tests showed no duplicates, and I learned that the database should enforce critical rules.

**RISES link:** Excellence and Integrity: let the database enforce the rules.

### Q25. What did you learn?
HTTP status codes (302/410/409), concurrency pitfalls, schema constraints and designing for read-heavy workloads. Beyond the technical, I learned how to scope a project: start with the core flow, then add features.

**RISES link:** Excellence: the lesson is correctness under real conditions.

### Q26. How did you plan the work (Dec 2025–Jan 2026)?
Backend core first (shorten and redirect), then expiry and QR code, then the React UI, with testing at each step. I wrote down the API design before coding, which reduced rework.

### Q27. How did you decide features?
I prioritized the core flow, then added features that gave the most real-world value (expiry, aliases, QR). I deliberately postponed extras like analytics and accounts, noting them as future work.

### Q28. How did you test it?
Postman for API cases (invalid URL, duplicate alias, expired link), manual testing of the frontend and concurrency checks by sending parallel requests. I learned that edge cases, not happy paths, find the bugs.

### Q29. What would you add if given more time?
Analytics, user accounts with JWT, caching with Redis, malicious URL detection, rate limiting and Docker deployment. These are the steps from a working prototype to a service people could rely on.

**RISES link:** Stewardship: the path from prototype to reliable service.

### Q30. How do these projects show your growth?
The first (Dec–Jan) is solid backend engineering; the second (Feb–Mar) combines AI with security thinking. They show a progression from fundamentals to applied AI, and from "making it work" to "making it safe."

### Q31. What if the company uses a different tech stack?
I'll adapt. My projects taught me concepts, not just tools, and I've already worked across Java, Python and JavaScript. Fundamentals like HTTP, databases, concurrency and security transfer to any stack.

### Q32. How do you document your projects?
READMEs with the problem, architecture, setup steps, API endpoints and screenshots on GitHub, plus clear commit messages. Good documentation helps others and my future self.

**RISES link:** Stewardship: documentation for others and your future self.

### Q33. Why do side projects matter to you?
They are my lab: nobody gives me the problem, so I learn to define it, build it and decide trade-offs end to end. They also keep me learning between internships.

**RISES link:** Excellence: you learn by defining and solving problems end to end.

### Q34. What do you do with failed experiments?
Note what I learned and apply it. For example, early code-generation logic produced collisions, which led to the constraint-based design that's now core to the project.

### Q35. How do you handle project scope creep?
Maintain an MVP list, park extra ideas in a backlog, and add them only after the core works. When working with a team or client, I discuss the trade-off (time vs. features) openly.

### Q36. How did you think about misuse of a URL shortener? **[RISES: Integrity, Stewardship]**
Shorteners can hide malicious links. In my prototype I validated URLs, restricted dangerous schemes and kept expiry options, but I recognize more is needed in production: rate limiting, malicious URL scanning, reporting mechanisms and accountability through accounts. Building a tool means taking responsibility for how it can be abused, not just how it's meant to be used.

### Q37. How did you design the user experience of the app? **[RISES: Service]**
A simple form with clear fields, instant feedback, a copy button, a QR code and specific error messages (for instance, "alias already taken, try another"). I wanted a first-time user to succeed without instructions. Good service is often invisible: nothing confusing, nothing wasted.

### Q38. Why did you implement expiry, and how does it relate to responsible design? **[RISES: Stewardship]**
Expiry lets users create temporary links and prevents the database from growing with useless entries forever. It also reduces the risk of old links being misused. Responsible systems manage the full lifecycle of data: creation, use and removal.

### Q39. What standards did you hold yourself to in this project? **[RISES: Excellence]**
Correctness first (uniqueness and consistency enforced by the database), then clarity of code, proper HTTP semantics (302/410/409), tested edge cases and documentation. I deliberately didn't call it done when the happy path worked.

### Q40. Describe a decision where you chose correctness over speed. **[RISES: Excellence, Integrity]**
I could have checked alias availability in application code and moved on, which is faster to write. But I knew concurrent requests could break it, so I used a database UNIQUE constraint and handled conflicts properly. It took longer but meant the system was right under real conditions. In banking, this principle matters even more.

---

# SECTION 5: ACHIEVEMENTS: LEADERSHIP AND TEAMWORK

## 5A. Smart India Hackathon 2024: Grand Finalist, Team Leader (Runner-up)

### Q1. Tell me about this achievement.
I led a six-member team at the Smart India Hackathon 2024 Grand Finale, where we were among the top 500 teams out of 88,221 nationwide, and we secured 2nd place (runner-up). It was my biggest test of leadership and technical delivery, because we had a real problem statement, a continuous time-limited build and a panel of evaluators.

**RISES link:** Respect (team) and Excellence (delivery under pressure).

### Q2. How did you get selected for the finale?
We cleared the college-level round, then the national screening based on our idea and proposal, and then built a working prototype for the final. [Add specifics.] Selection requires both a strong idea and an execution plan, and I led the team in preparing both.

### Q3. How did you lead the team? (STAR)
- **S (Situation):** Six members with different skills, about 36 hours and a real problem statement.
- **T (Task):** Deliver a working, convincing solution.
- **A (Action):** Defined an MVP, assigned owners based on strengths, set integration checkpoints, handled the final integration myself and rehearsed the pitch.
- **R (Result):** A working demo and a runner-up position nationally. I learned that a leader's job is clarity, not doing everything.

**RISES link:** Respect and Excellence: clarity, ownership, shared credit.

### Q4. How do you handle a team member who isn't performing?
First understand why: personal issue, skill gap or unclear task. Then talk privately, offer help or pair them with someone, adjust the task to their strengths and set a clear check-in. If it persists, I'd inform the right person with facts, not blame.

**RISES link:** Respect: help first, then escalate with facts.

### Q5. Tell me about a conflict during the hackathon. (STAR)
- **S (Situation):** Two members disagreed on the architecture.
- **T (Task):** Decide quickly without hurting morale.
- **A (Action):** Listened to both, compared feasibility and time, chose the simpler approach and explained why, then gave the other member ownership of a key part.
- **R (Result):** Resolved in minutes and the team stayed aligned and motivated.

**RISES link:** Respect and Integrity: decisions with reasons.

### Q6. How did you handle stress and lack of sleep?
We planned shifts and short breaks, kept communication calm and focused on the next small goal. As a leader, my tone sets the team's tone, so staying calm mattered. We also avoided last-minute big changes.

### Q7. What did you learn from not winning 1st place?
That the last 10%, polish, presentation and edge cases, makes the difference, and that we should budget time for them. That insight guided me when we later won first place at the Citi Ada Lovelace Hackathon.

**RISES link:** Excellence: learning from a near miss is a growth story.

### Q8. What was your toughest decision as a leader?
Cutting a feature the team liked to ensure the core demo was stable. It was unpopular for a moment, but it protected the result. I explained the reasoning, and the team supported it.

**RISES link:** Integrity and Excellence: protecting quality over popularity.

### Q9. How do you motivate people?
Give clear ownership, recognize contributions, keep the mission visible ("why this matters") and share the credit. People work harder for something they own.

**RISES link:** Respect: ownership and recognition.

### Q10. How did the experience prepare you for the industry?
It gave me practice with deadlines, communication, delegation, decision-making under uncertainty and presenting to evaluators, all of which are workplace skills.

### Q11. Tell me about a teammate in the hackathon whose different background helped the team. (STAR) **[RISES: Respect]**
- **S (Situation):** In a hackathon, one teammate came from a [design/research/non-coding] background and was initially unsure how to contribute. [Adapt.]
- **T (Task):** Use every member's strengths within a tight timeline.
- **A (Action):** I involved them in framing the problem and the story, asked them to own the user flow and presentation, and made sure their input was part of key decisions.
- **R (Result):** Our pitch was clearer and judges commented on the user focus.
- **Learning:** Diverse skills strengthen the solution when you actively include them.

### Q12. Were you ever tempted to exaggerate in a presentation? What did you do? **[RISES: Integrity]**
In pressure situations it's tempting to present "future features" as if they exist. I made a rule in my team: demo only what works, and clearly label what's planned. Judges and users respect honesty, and an over-claimed demo that breaks does more harm than a smaller, truthful one.

### Q13. How did you make sure the solution served real users and not just judges? **[RISES: Service]**
We started from the problem statement's real users, asked what they'd need to adopt it and designed the core flow around that, rather than around flashy features. We also asked ourselves "who benefits and how will we know?" which shaped how we explained impact.

### Q14. How did you consider the long-term use of your SIH solution? **[RISES: Stewardship]**
We considered what it would take to maintain: documentation, a scalable architecture, data sources and who would own updates. A hackathon solution that can't outlive the event wastes the idea. Thinking about sustainability early helped us pitch a realistic path to adoption.

## 5B. Citi Ada Lovelace Hackathon 2025: 1st Place (198 teams, 5 colleges)

### Q15. Tell me about this win.
I led a team to first place out of 198 teams from five colleges. We built [solution] to address [problem], and won on innovation, feasibility and a clear demo. It was a result of applying lessons from earlier hackathons: tighter scope, early integration and a rehearsed presentation.

### Q16. What made your team win? (STAR)
- **S (Situation):** A competitive field of 198 teams.
- **T (Task):** Stand out with a practical, well-presented solution.
- **A (Action):** We focused on a clear user problem, built a working demo of the core flow and told a coherent story with impact and next steps.
- **R (Result):** 1st place. Lesson: clarity and execution beat complexity.

**RISES link:** Excellence: clarity and execution beat complexity.

### Q17. How did you apply lessons from earlier hackathons?
Better scoping (a smaller MVP), earlier integration, a rehearsed pitch with timing and a backup demo plan. I also asked the team for a short retrospective after each event to capture what worked.

### Q18. How did you ensure everyone's ideas were heard?
A quick brainstorming round where each person pitched an idea in two minutes, then we evaluated them with common criteria (impact, feasibility, time). That made the decision feel fair and kept everyone engaged.

**RISES link:** Respect: every voice heard.

### Q19. How did you feel after winning and what did you do next?
Proud, but I ran a retrospective on what worked and what could be improved, so the win didn't make us complacent. Then I thanked the team and shared what each person contributed.

### Q20. How does this relate to working in a company?
Companies need people who can align a team to a goal, make trade-offs and deliver on time. This was practice for exactly that, with the extra requirement of presenting the result convincingly.

### Q21. How did you ensure credit was shared fairly after winning? **[RISES: Respect, Integrity]**
I named each person's contribution when speaking to judges and others, avoided taking sole credit as team leader and made sure everyone got recognition in our write-ups. Leaders succeed when the team feels valued.

### Q22. How did you maintain high standards under the hackathon time pressure? **[RISES: Excellence]**
We set a "demo-ready" checklist: core flow working, edge cases tested, UI clean and pitch rehearsed. We froze features early to avoid last-minute bugs. High standards come from deliberate choices about what not to build.

## 5C. OneEarth International Hackathon: 1st Runner-Up (£500)

### Q23. Tell me about it.
We competed in an international hackathon focused on sustainability and finished as first runner-up, securing a £500 prize and industry recognition for sustainable innovation. It showed that our approach could stand up internationally, not just locally.

### Q24. Why sustainability?
It's an area where technology can create meaningful impact beyond the code. I like building things that matter, and sustainability problems benefit from software, data and clear thinking.

### Q25. What was it like competing internationally?
More diverse competition and judges, a stronger emphasis on global relevance and a higher bar for clear communication. It taught me to explain ideas without assuming local context, and I got valuable feedback from industry professionals.

### Q26. What did you do with the prize money?
[Truthful: shared among the team, reinvested in learning resources, saved.] A transparent answer shows fairness and integrity.

### Q27. What's your takeaway from the three hackathons?
A pattern: leadership, rapid learning, MVP thinking and communication, which improved with each event. The first taught me scope, the second taught me execution and story, and the third taught me to stand out on an international stage.

### Q28. Which was the toughest and why?
[Choose one with a reason.] For example: "SIH, because of the scale of competition and the continuous time limit, which tested stamina and team coordination."

### Q29. Do hackathon projects represent production work?
No. They're rapid prototypes. Production work requires testing, security, scalability, maintainability and monitoring, which I saw during my internships. Hackathons train speed and creativity; internships train discipline. I value both.

**RISES link:** Integrity: be honest about prototype versus production.

### Q30. How did you keep improving across events?
Retrospectives after each event, applying lessons to the next, and building side projects to strengthen weak areas (like backend fundamentals).

### Q31. Who benefits from your OneEarth solution, and how? **[RISES: Service, Stewardship]**
[Fill in.] I'd describe the specific users (communities, businesses, individuals), the problem they face and how the solution improves their situation in a measurable way. Linking the technology to real people is key.

### Q32. How did you think about the long-term sustainability of your solution? **[RISES: Stewardship]**
Beyond the environmental goal, I considered whether it could keep running: costs, users who would adopt it, data availability and maintenance. A solution that only works for the length of a competition doesn't really leave things better.

## 5D. Cummins Nurturing Brilliance Scholarship

### Q33. Tell me about the scholarship.
It's a scholarship (2026–present) given to top engineering students across India based on academic merit and technical aptitude, and I'm a recipient. It recognizes both consistent academic performance and technical ability.

**RISES link:** Respect and Integrity: describe only what you really went through.

### Q34. What did it take to get it?
Consistent academics, technical aptitude and a selection process [test/interview/application]. I prepared by strengthening fundamentals and showcasing projects and internships. [Describe real steps.]

### Q35. What does this mean to you?
It's recognition that motivates me to keep my standards high, and it provides support that lets me focus on learning and building. I see it as a responsibility to keep growing.

### Q36. How will you give back?
By mentoring juniors, sharing what I learn about hackathons and internships, and eventually supporting others through my work. [Give a real example if you already do.]

**RISES link:** Stewardship: giving back through mentoring.

### Q37. How do you balance academics with competitions and internships?
Planning, prioritizing and discipline: weekly schedules, early preparation for exams and clear communication with teams about my availability. [Add your CGPA story if strong.]

### Q38. What would you tell juniors who want to win hackathons or scholarships?
Build real projects, join hackathons early, learn from losses, collaborate with people who complement your skills, and keep fundamentals strong. Consistency beats one-time effort.

**RISES link:** Respect and Stewardship: help others up.

### Q39. How will you uphold the standards expected of a Cummins Nurturing Brilliance Scholar? **[RISES: Integrity, Excellence]**
By maintaining academic honesty, consistent performance and continuing to build technical skills. I see the scholarship as trust placed in me, so I want to keep being someone who earns that trust through actions.

### Q40. How do you treat peers who perform better than you in academics or hackathons? **[RISES: Respect]**
I congratulate them and learn from them. Seeing someone stronger in a topic is a chance to ask how they study or build. Healthy competition makes everyone better, and I care more about my growth than about ranking myself against others.

---

# SECTION 6: SKILLS, LEARNING AND GROWTH

### Q1. Which skills are you strongest at?
Java, Python, JavaScript/React and SQL, and I'm comfortable with full-stack development. I've used them in real deliverables, such as the URL shortener, the Globant APIs and the ML work, so I can talk about them with examples.

**RISES link:** Excellence: skills tied to real deliverables.

### Q2. How did you learn them?
A mix of courses and documentation, but mostly by building projects and applying them in internships. Building something real exposes the gaps that tutorials hide.

### Q3. Are you comfortable with new technologies we use that you haven't worked on?
Yes. My track record shows it: I learned LangChain for a project, picked up DHM and CNN concepts for an industry project, and learned PostgreSQL optimization on the job. I follow a repeatable method: official docs, a small prototype, then a real task.

**RISES link:** Excellence and Stewardship: learning methods that transfer.

### Q4. How do you learn a new framework in a short time?
Read the official quickstart, build a small project, read examples and source for deeper understanding, then apply it in a real task with a mentor's review. I focus on the concepts first and then the syntax.

### Q5. Which skill would you like to improve?
Cloud/DevOps and system design. I'm learning Docker and deployment, and I'm studying system design topics like caching, queues and scaling.

### Q6. Do you prefer frontend, backend or full-stack?
I enjoy full-stack, but I lean toward backend and data-driven work, because I like performance, data and architecture problems. I'm flexible and willing to work wherever the team needs me most.

### Q7. How do you prove that your resume skills are real?
Through my GitHub projects and my internship deliverables, and I'm happy to do a live technical discussion or coding on any listed skill. Every skill on my resume is attached to something I built.

### Q8. What if the company's stack is different?
Fundamentals transfer: HTTP, databases, OOP, security and design patterns. I've already moved between Java, Python and JavaScript, so I'm confident I can ramp up on a different stack quickly.

**RISES link:** Excellence: fundamentals over tools.

### Q9. How would you handle a task in a language you don't know?
Learn the syntax quickly by mapping it to languages I know, read the idioms, pair with a teammate for the first task and deliver in small increments with frequent reviews.

**RISES link:** Respect: collaboration across stacks.

### Q10. How do you decide which technology to use for a project?
Based on requirements, team skills, ecosystem maturity, performance needs and trade-offs, not on popularity. I prefer the simplest tool that meets the need.

### Q11. What are you learning currently?
[Truthful: Docker, system design, DSA, cloud basics, LLM applications.] Name one resource and one thing you built or practiced with it.

### Q12. How do you balance depth vs breadth?
I aim to be T-shaped: broad familiarity across the stack, with depth in backend/databases and applied AI. Breadth helps me collaborate; depth helps me solve hard problems.

### Q13. What was the most valuable thing you learned that wasn't taught in class?
Debugging and performance profiling with real data, and working with other people's code. These shaped me more than any single subject.

### Q14. Do you do DSA/competitive programming practice?
[Truthful: platform, level, frequency.] It strengthens problem-solving and is useful for coding rounds, and I connect it to real work, like choosing the right data structure for a cache or lookup.

### Q15. What is your approach to code quality?
Readable, modular code with clear naming, validation and error handling, tests where possible, and reviews. I treat code as something others will read and maintain.

**RISES link:** Excellence and Respect: clean code respects the next reader.

### Q16. Which of your skills are most relevant to a bank's technology teams? **[RISES: Excellence, Integrity]**
Backend development (Java, Node, REST APIs), strong SQL and database performance skills, secure authentication (JWT, hashing, validation) and an understanding of testing and reliability. Banks run on correctness, security and availability, and my experience with N+1 optimization, constraints for data integrity and SQL-injection defences speaks to those needs. I also know I must learn the regulatory and domain side.

### Q17. How do you make sure the code you write is secure? **[RISES: Integrity, Stewardship]**
I validate all input, use parameterized queries, hash passwords properly, keep secrets out of code, apply least-privilege access and review dependencies. I also think about abuse cases (what would an attacker try?) at design time. Security is a habit built into each change, not a final step.

### Q18. What do you know about secure coding practices like the OWASP Top 10? **[RISES: Integrity, Excellence]**
OWASP lists common web risks: injection, broken authentication, sensitive data exposure, broken access control, security misconfiguration, vulnerable components and more. I've dealt directly with injection (parameterized queries and SQL validation in my AI-SQL project) and authentication (JWT with refresh tokens). I'm continuing to learn about the rest, because in a bank, these aren't optional.

### Q19. How do you approach testing your work? **[RISES: Excellence]**
I test happy paths, edge cases and failure cases: empty inputs, invalid tokens, concurrency and large data. I use Postman and tests where possible, and I'm moving toward automated tests as a default. A feature isn't finished until I've tried to break it.

### Q20. How do you explain a technical concept to a non-technical person? **[RISES: Respect, Service]**
I start with their goal, use an analogy and avoid jargon. For example, I explain an index as the index at the back of a book that avoids reading every page. Then I check understanding by asking them to say it back. Explaining well is a form of respect for the listener.

### Q21. How do you handle technical debt? **[RISES: Stewardship, Excellence]**
I make it visible (note it in the backlog), prioritize the debt that slows the team or risks failures, fix small pieces regularly instead of waiting for a "big cleanup" and avoid creating new debt with quick hacks. I'd rather be honest about trade-offs than hide shortcuts.

### Q22. What's your approach to debugging under pressure? **[RISES: Excellence]**
Stay calm, reproduce the issue, check logs, isolate the layer, form a hypothesis and test one change at a time. If it's critical, communicate status early. Panic leads to random changes that make things worse.

### Q23. How do you decide when to stop optimizing? **[RISES: Excellence]**
When the requirement is met and the next improvement costs more than it's worth. I set a target (e.g., a response-time goal), measure, optimize the biggest bottleneck and stop once the target is reached. Over-optimizing adds complexity without value.

### Q24. How do you stay disciplined with DSA practice? **[RISES: Excellence]**
A consistent schedule (a few problems a few times a week), focusing on patterns rather than memorizing, reviewing mistakes and revisiting problems later. I also connect DSA to real use cases, like hash maps for caching.

### Q25. How would you learn the banking domain quickly? **[RISES: Service, Excellence]**
Start with the products and customer journeys (accounts, payments, cards, loans), read internal documentation, shadow or ask experienced colleagues and learn core terms like KYC, AML, risk and compliance. I'd treat domain knowledge as equally important as technical skills because it determines whether we build the right thing.

### Q26. What's your experience with Agile and team collaboration tools? **[RISES: Respect, Service]**
At Globant I worked with sprint tasks, stand-ups, pull requests and reviews using Git workflows, and issue tracking boards. I learned that Agile works when people communicate openly, keep tasks small and respond to change.

### Q27. How do you approach documentation? **[RISES: Stewardship]**
I document the why, not just the what: setup steps, architecture, API contracts and decisions. I update it with the code. Good documentation reduces onboarding time and mistakes, and it's one of the most respectful things you can do for future teammates.

### Q28. How do you use AI tools like coding assistants responsibly? **[RISES: Integrity, Excellence]**
I use them to speed up boilerplate, explain concepts and review ideas, but I read, test and understand everything I use, and I never paste sensitive or confidential data into them. I'm accountable for the code I submit. In a regulated environment, I'd follow the organization's approved tools and policies.

### Q29. Tell me about a skill you're building outside your syllabus. (STAR) **[RISES: Excellence]**
- **S (Situation):** My projects ran only on my laptop, and I had limited deployment experience. [Adapt to your real skill, e.g., Docker.]
- **T (Task):** Learn to package and run an application reliably.
- **A (Action):** I followed official docs, wrote a Dockerfile for one of my projects, ran it in a container and documented the steps in the README.
- **R (Result):** The project ran the same way on another machine, and I could explain what I had done.
- **Learning:** Pick a skill that closes a real gap and prove it with something built.

### Q30. How do you share knowledge with others? **[RISES: Respect, Stewardship]**
I write short notes after solving hard problems, explain my approach in team discussions and help juniors during hackathons. Sharing multiplies the value of what one person learns.

### Q31. How do you handle code review feedback you disagree with? **[RISES: Respect, Integrity]**
I first make sure I understand the reasoning, then explain my view with evidence if I still disagree and stay open to a better approach. If the reviewer insists after discussion, I follow the decision unless there's a serious risk. The aim is the best outcome, not winning.

### Q32. What does "production-ready" mean to you? **[RISES: Excellence, Stewardship]**
Tested, secure, observable (logs and monitoring), documented, handles failures gracefully, performs under expected load and can be deployed and rolled back safely. It's a much higher bar than "works on my machine."

### Q33. How would you handle a dependency with a known vulnerability? **[RISES: Integrity, Stewardship]**
Check severity and whether we're exposed, update or patch, test for regressions, inform the team and add automated dependency scanning to catch future issues. I'd never hide or ignore a vulnerability because fixing it is inconvenient.

### Q34. What does "clean code" mean to you? **[RISES: Excellence, Respect]**
Code that is easy to read, easy to change and hard to misuse: meaningful names, small functions, minimal duplication, clear structure and good error handling. Clean code respects the next reader.

### Q35. How do you balance speed with quality? **[RISES: Excellence, Integrity]**
I scope what must be right (security, data integrity, core flow) and move fast on the rest. I'm transparent about trade-offs: if a shortcut is needed, I note it and plan to fix it. Speed that creates hidden risk is not real speed.

### Q36. How do you approach learning a large unfamiliar codebase? **[RISES: Excellence]**
Start with the README and architecture, run the application, trace a single request through the layers, read tests, then fix a small bug. I keep notes and ask targeted questions. This approach worked at Globant.

### Q37. How do you think about accessibility in the interfaces you build? **[RISES: Respect, Service]**
Semantic HTML, labels for inputs, keyboard navigation, readable contrast, responsive layouts and, as in my AI-SQL project, voice input and multilingual support. Accessible design means more people can use what we build.

### Q38. What does reliability mean for banking systems? **[RISES: Service, Stewardship]**
Customers depend on being able to pay, transfer and view balances at any time, so systems must be available, correct and resilient. That means redundancy, monitoring, graceful degradation, careful releases and clear incident handling. A small error can affect many people's finances.

### Q39. How do you measure whether your work was successful? **[RISES: Excellence, Service]**
By outcome, not effort: Did the API get faster? Did users complete their task? Did the model meet its target on unseen data? I define a measurable success criterion before starting when I can.

### Q40. What would you do to continue improving as an engineer at Barclays? **[RISES: Excellence, Stewardship]**
Learn from mentors and reviews, take on increasingly complex tasks, study the bank's systems and domain, pursue relevant certifications or training and share what I learn. I'd also seek feedback regularly.

---

# SECTION 7: STRENGTHS, WEAKNESSES, FAILURE AND CONFLICT

### Q1. Tell me about a time you failed. (STAR)
- **S (Situation):** In [a hackathon/project], we aimed for too many features.
- **T (Task):** Deliver a working demo.
- **A (Action):** We ended up with an unstable demo, so afterward we reviewed what happened and decided to lock scope early and test continuously.
- **R (Result):** In later hackathons (a 1st place and a runner-up), we scoped smaller and performed better. Lesson: failure is useful when you analyze and change behavior.

**RISES link:** Integrity and Excellence: own the failure and change behaviour.

### Q2. Tell me about a time you disagreed with your team. (STAR)
- **S (Situation):** In the Smart India Hackathon finale, two teammates disagreed about the architecture with only hours left.
- **T (Task):** Decide quickly without hurting morale.
- **A (Action):** I gave each two minutes to explain, compared the options on time and risk, picked the simpler one and explained why, then gave the other person ownership of a critical component.
- **R (Result):** We decided in minutes, built on time and ended as national runners-up.
- **Learning:** Disagreements are resolved best with shared criteria and visible ownership.

### Q3. Tell me about a time you handled tight deadlines. (STAR)
- **S (Situation):** In the hackathon finale (about 36 hours) and in the semester when my internships overlapped, deadlines were tight.
- **T (Task):** Deliver working results without sacrificing quality.
- **A (Action):** I defined an MVP, ranked tasks must/should/nice, timeboxed work, froze features early, communicated risks and used the last hours for testing and the pitch.
- **R (Result):** We delivered stable demos and met internship commitments.
- **Learning:** Cut scope early rather than rush quality at the end.

**RISES link:** Excellence: communicate risk early.

### Q4. Tell me about a time you led without authority. (STAR)
- **S (Situation):** At Gagangiri, nobody was the formal lead and requirements from the company were broad.
- **T (Task):** Turn them into clear deliverables for the team of four.
- **A (Action):** I set up discussions with stakeholders, wrote down agreed outputs and metrics, shared them with the team and suggested who should take which part.
- **R (Result):** The team stayed aligned, and the stakeholders knew what to expect.
- **Learning:** Leadership is often initiative and clarity, not a title.

**RISES link:** Respect: initiative over authority.

### Q5. Describe a time you had to persuade someone. (STAR)
- **S (Situation):** Teammates wanted to keep adding features in a hackathon with limited time.
- **T (Task):** Convince them to simplify scope.
- **A (Action):** I showed a time estimate per feature, pointed out the risk of an unstable demo and proposed a must-have list, with the extras kept as 'future scope'.
- **R (Result):** They agreed, we shipped a stable demo and it placed well.
- **Learning:** Persuade with evidence and show how the change helps their goal.

**RISES link:** Respect and Integrity: persuade with evidence.

### Q6. Tell me about a time you worked with a difficult person. (STAR)
- **S (Situation):** A teammate [often missed updates / resisted feedback] and it slowed our progress. [Adapt to your real case.]
- **T (Task):** Keep the project moving without damaging the relationship.
- **A (Action):** I spoke privately, described the impact calmly, asked what was blocking them, gave them a task that matched their strengths and set short check-ins.
- **R (Result):** Their contribution improved and the team delivered on time.
- **Learning:** Focus on the shared outcome and seek to understand before judging.

**RISES link:** Respect: focus on shared goals.

### Q7. Tell me about a time you went above and beyond. (STAR)
- **S (Situation):** On the dashboard project, the requirements covered functionality but not how operators would handle errors. [Adapt.]
- **T (Task):** Make the tool genuinely useful, beyond the minimum.
- **A (Action):** I added clear error messages, basic monitoring and a short usage guide in my own time.
- **R (Result):** [Mentor/stakeholders] found it easier to adopt, and it saved support questions.
- **Learning:** Small extras that prevent future problems are worth the effort.

### Q8. Describe your biggest academic challenge.
[Truthful.] Describe the subject or semester, what you did (study plan, help from seniors) and the result. Focus on recovery and discipline.

### Q9. How do you respond when your idea is rejected?
I ask why, learn from the reasoning and support the final decision. Often the other person knows constraints I don't, and sometimes my idea can be revisited later with better evidence.

### Q10. How do you handle being wrong?
Admit it quickly, fix it, and share the lesson so the team doesn't repeat it. Defending a wrong position wastes more time than admitting a mistake.

**RISES link:** Integrity: admit the mistake and fix it.

### Q11. How do you handle a critical bug found by someone else?
Thank them, prioritize the fix, communicate status clearly, then add a test or check to prevent recurrence. I treat bug reports as help, not as criticism.

### Q12. Have you ever missed a deadline? (STAR)
- **S (Situation):** On an internship task I gave an estimate without accounting for testing and a dependency, and I ran late. [Adapt; if you truly haven't, say so and use a near-miss.]
- **T (Task):** Limit the impact and recover.
- **A (Action):** I informed my mentor as soon as I saw the risk, explained the cause, proposed a revised date and delivered the most important part first.
- **R (Result):** The final delivery landed on the new date with no surprises.
- **Learning:** Communicate early and build realistic estimates.

### Q13. How do you give feedback to peers?
Specific, kind and focused on the work, not the person, with a suggestion for improvement. I also try to mention what's going well so feedback feels balanced.

**RISES link:** Respect: kind, specific feedback.

### Q14. Describe a time you had to adapt quickly. (STAR)
- **S (Situation):** I was doing a MERN internship at Globant when the Gagangiri project began and required a new domain (DHM, microplastics, CNNs).
- **T (Task):** Get productive in the new domain without dropping the first commitment.
- **A (Action):** I split my week into fixed blocks, studied the basics, started with a simple baseline model and asked teammates and stakeholders for context.
- **R (Result):** Both commitments were met, and the prototype reached 80.9% accuracy.
- **Learning:** Structure learning time and start small.

### Q15. What do you do when you disagree with a manager's decision?
Share my view respectfully with data or reasons, ask what constraints led to the decision, and then commit to the final decision if it stands, unless it is unethical or unsafe.

**RISES link:** Respect and Integrity: disagree openly, commit afterward.

### Q16. Tell me about a time you received tough feedback. How did you respond? (STAR) **[RISES: Respect, Excellence]**
- **S (Situation):** A mentor told me my pull requests lacked clear explanations and tests.
- **T (Task):** Improve how I submit work.
- **A (Action):** I asked for examples of strong PRs, rewrote mine with clear descriptions, added tests and kept a checklist.
- **R (Result):** Later review comments dropped noticeably.
- **Learning:** Feedback is a shortcut to improving.

### Q17. Tell me about a time you made a decision with incomplete information. (STAR) **[RISES: Excellence, Integrity]**
- **S (Situation):** In a hackathon we weren't sure whether a third-party API would work reliably, with little time to spare.
- **T (Task):** Choose a direction without waiting for certainty.
- **A (Action):** I proposed a one-hour test with a fallback: if it failed, we'd switch to a simpler approach.
- **R (Result):** It failed, we switched quickly and kept our schedule.
- **Learning:** Decide with the best information available, set a checkpoint and stay ready to adjust.

### Q18. Tell me about a time you worked with someone with a very different working style. (STAR) **[RISES: Respect]**
- **S (Situation):** A teammate liked detailed planning, whereas I preferred quick prototyping. [Adapt.]
- **T (Task):** Work together without friction.
- **A (Action):** We discussed our styles openly and agreed on a short planning session followed by quick builds with check-ins.
- **R (Result):** We moved faster and had fewer rework cycles.
- **Learning:** Different styles complement each other when discussed openly.

### Q19. Tell me about a time you had to say no. (STAR) **[RISES: Integrity, Service]**
- **S (Situation):** Close to a deadline, someone asked for a large new feature. [Adapt.]
- **T (Task):** Protect stability without being unhelpful.
- **A (Action):** I explained the risk to quality, offered a smaller version that fit the time and proposed the rest for the next iteration.
- **R (Result):** They accepted and we shipped on time.
- **Learning:** Say no with an alternative and the reason.

### Q20. Tell me about a time you helped someone else succeed. (STAR) **[RISES: Respect, Service]**
- **S (Situation):** A junior teammate was stuck on an API integration in a hackathon.
- **T (Task):** Help them complete it while building their confidence.
- **A (Action):** I paired with them, explained the approach, then let them write it and review it.
- **R (Result):** They finished the feature and handled similar work on their own afterwards.
- **Learning:** Helping others succeed strengthens the whole team.

### Q21. Tell me about a time when your plan changed unexpectedly. (STAR) **[RISES: Excellence]**
- **S (Situation):** At Gagangiri, our dataset was smaller and noisier than expected, so the planned complex model overfit.
- **T (Task):** Keep the project on track under a new reality.
- **A (Action):** I reorganized tasks, switched to a simpler baseline with augmentation and regularization, and told stakeholders about the revised plan.
- **R (Result):** We kept the prototype on schedule and reached 80.9% accuracy.
- **Learning:** Flexibility with clear communication is key.

### Q22. What's the most difficult decision you've made? **[RISES: Integrity, Excellence]**
[Truthful.] For example, cutting a feature the team liked to protect the stability of our hackathon demo. It was hard because it disappointed teammates, but it was right for the goal. I explained my reasoning, and they understood.

### Q23. Tell me about a time you showed resilience. (STAR) **[RISES: Excellence]**
- **S (Situation):** We finished as runners-up, not winners, at the Smart India Hackathon after investing heavily.
- **T (Task):** Respond in a way that improved future results.
- **A (Action):** We reviewed what we'd do better (scope, polish, presentation), then applied those lessons in the next events.
- **R (Result):** We won first place at Citi Ada Lovelace and finished first runner-up at OneEarth.
- **Learning:** Setbacks become useful when you analyze them.

### Q24. Tell me about a time you took a calculated risk. (STAR) **[RISES: Excellence]**
- **S (Situation):** For the URL shortener I chose raw Java HttpServer instead of a framework, which risked slower delivery.
- **T (Task):** Learn the fundamentals without missing the deadline.
- **A (Action):** I scoped the core features first, set a time limit, kept a fallback plan to move to a framework if needed and tracked progress weekly.
- **R (Result):** I finished on time and understood HTTP, concurrency and routing much more deeply.
- **Learning:** Take risks where the downside is limited and recoverable.

### Q25. Tell me about a time you improved a process. (STAR) **[RISES: Excellence, Stewardship]**
- **S (Situation):** Our hackathon team kept integrating everyone's work only at the end, which caused last-minute failures.
- **T (Task):** Reduce integration surprises.
- **A (Action):** I introduced short integration checkpoints every few hours and a shared task board with clear owners.
- **R (Result):** Fewer last-minute breakages and a smoother final demo.
- **Learning:** Small process changes can have large benefits.

### Q26. Tell me about a time you noticed something wrong and spoke up. (STAR) **[RISES: Integrity]**
- **S (Situation):** A teammate's presentation slide claimed a feature that wasn't built yet.
- **T (Task):** Prevent us from misleading the judges.
- **A (Action):** I told them privately about the risk of being caught, and we relabeled it as 'planned'.
- **R (Result):** We presented honestly and the demo stayed credible.
- **Learning:** Speak up early and privately, with a solution.

### Q27. Describe an ethical dilemma you faced and how you handled it. (STAR) **[RISES: Integrity]**
- **S (Situation):** While reporting results for the ML project, a best-case run looked more impressive than typical performance.
- **T (Task):** Decide how to present the numbers to stakeholders.
- **A (Action):** I reported the held-out test accuracy and its limits instead of the best run.
- **R (Result):** Stakeholders trusted the results more and asked sensible follow-up questions.
- **Learning:** Ethical choices are often small daily decisions about honesty.

### Q28. Tell me about a time you built trust with a stakeholder. (STAR) **[RISES: Integrity, Service]**
- **S (Situation):** Gagangiri's stakeholders were unsure whether an AI prototype could deliver value.
- **T (Task):** Earn their confidence.
- **A (Action):** I gave regular demos, shared problems as well as progress, and explained limits plainly.
- **R (Result):** They became more open with feedback and data access.
- **Learning:** Trust grows from honest, regular communication.

### Q29. Tell me about a time you worked with limited resources. (STAR) **[RISES: Excellence]**
- **S (Situation):** The ML project had limited data and modest compute.
- **T (Task):** Get the best possible results anyway.
- **A (Action):** We used augmentation, a lightweight CNN, careful experiment tracking and early stopping.
- **R (Result):** We reached 80.9% accuracy and a working dashboard.
- **Learning:** Constraints encourage discipline.

### Q30. How do you handle stress and avoid burnout? **[RISES: Respect, Excellence]**
I plan work realistically, take short breaks, sleep properly even in deadlines, talk to teammates when overloaded and use exercise or hobbies to reset. Sustained performance requires looking after yourself.

### Q31. Tell me about a time you led a change. (STAR) **[RISES: Excellence, Respect]**
- **S (Situation):** A project team I was in had no shared way of tracking tasks. [Adapt.]
- **T (Task):** Introduce coordination without creating overhead.
- **A (Action):** I proposed a simple task board and short daily check-ins, started with a trial and adjusted from feedback.
- **R (Result):** People adopted it and missed fewer handoffs.
- **Learning:** People adopt change that solves their own problem.

### Q32. Tell me about a time you resolved a misunderstanding. (STAR) **[RISES: Respect, Service]**
- **S (Situation):** Stakeholders and our team interpreted 'real-time' differently (instant vs. periodic updates).
- **T (Task):** Align on expectations before building the wrong thing.
- **A (Action):** I clarified with examples and agreed on a specific refresh interval in writing.
- **R (Result):** No rework and the stakeholders were satisfied.
- **Learning:** Many conflicts come from unspoken assumptions.

### Q33. What do you do when you disagree with a peer's technical approach? **[RISES: Respect, Integrity]**
Ask them to explain their reasoning, share mine, compare options on facts (time, risk, maintainability) and test if possible. If we still disagree, I defer to the decision-maker and support it fully.

### Q34. Tell me about a time you disappointed someone. (STAR) **[RISES: Integrity, Respect]**
- **S (Situation):** I once promised a delivery date I didn't meet. [Adapt.]
- **T (Task):** Repair trust.
- **A (Action):** I apologized without excuses, explained the cause, committed to a new date and updated them along the way.
- **R (Result):** I delivered on the new date and the relationship recovered.
- **Learning:** Promise only what you can deliver.

### Q35. Tell me about criticism that was hard to hear. **[RISES: Respect, Excellence]**
[Truthful.] Being told I was taking on too much myself. It was true. I started delegating and trusting my teammates. Criticism that stings often points to real growth areas.

### Q36. Tell me about an unpopular decision you made. (STAR) **[RISES: Integrity, Excellence]**
- **S (Situation):** In a hackathon, I decided to freeze features hours before the end, though some teammates wanted more.
- **T (Task):** Protect demo stability.
- **A (Action):** I explained the risk, asked for any critical fix to be raised and set the cut-off time.
- **R (Result):** The demo ran smoothly and teammates later agreed it was right.
- **Learning:** Explain the reason and stay open to critical exceptions.

### Q37. Tell me about a goal you set and achieved. (STAR) **[RISES: Excellence]**
- **S (Situation):** I wanted two end-to-end projects built during my final year alongside my internships.
- **T (Task):** Finish them without hurting academics.
- **A (Action):** I broke each into milestones, scheduled weekly blocks and tracked progress.
- **R (Result):** I completed the URL shortener and the AI-SQL assistant.
- **Learning:** Milestones and weekly reviews make big goals manageable.

### Q38. How do you handle competition among peers? **[RISES: Respect]**
Healthy competition motivates me, but I avoid undermining others. I share learnings, congratulate wins and focus on improving my own work. A strong peer group lifts everyone.

### Q39. What does professionalism mean to you? **[RISES: Respect, Integrity, Excellence]**
Being reliable, honest, respectful in communication, prepared and accountable for results and mistakes. It also means keeping confidentiality and keeping your word.

### Q40. What would you do if you saw a colleague breaking a rule or policy? **[RISES: Integrity, Respect]**
If it's minor and unintentional, I'd talk with them privately and remind them. If it's serious or repeated, or it risks customers or compliance, I'd report it through the proper channel. Doing the right thing sometimes takes courage.

---

# SECTION 8: BARCLAYS, ROLE AND CAREER GOALS

### Q1. Why this role at Barclays?
It matches my strengths in full-stack and backend development, database performance and secure design, and I want to build systems where reliability and trust really matter. At Barclays, the values also fit how I work: respect for colleagues, honesty about results, service to the people who use the systems, high standards and care for long-term quality. [Mention the specific role or team from the job description.]

### Q2. What do you know about our products and culture?
On culture: Barclays says its values (RISES) guide decisions and behaviour, and The Barclays Way sets out the expected conduct, which tells me behaviour is assessed as seriously as results. On the business: it serves consumers, businesses and institutions, with technology and operations teams that keep those services running. [Add two or three facts you have verified: divisions, locations, a recent initiative, and one technology or business area that interests you.]

### Q3. What are you looking for in your first job?
Strong mentorship, real projects with meaningful responsibility, a learning culture and a team that treats people well. At a bank I also want to learn how security, risk and compliance shape engineering, because it will make me a better engineer. In return, I'll bring ownership, curiosity and a commitment to doing things the right way.

### Q4. How long do you plan to stay with us?
I'm looking for a long-term career where I can grow and contribute. If the learning and growth opportunities are good, I'd like to stay and take on increasing responsibility.

### Q5. Will you do higher studies?
[Truthful.] For now, my focus is gaining industry experience and building depth. I may revisit higher studies later if it strongly aligns with my career and the company's growth plans.

**RISES link:** Integrity: be honest; don't invent plans you can't back up.

### Q6. Where else have you applied?
I'm exploring a few companies, as one should in placements, but I'm interested in this one because [specific reason]. Be honest, and don't speak negatively about others.

### Q7. If you get two offers, which will you choose?
I'd compare learning opportunities, role, team, culture and growth, along with compensation as one factor. [Be honest and respectful, and don't commit to a hypothetical you can't back up.]

### Q8. What would you do in your first 90 days?
Month 1: learn the codebase, tools and processes, and ask questions. Month 2: take small tasks and deliver reliably with reviews. Month 3: own a small feature, suggest an improvement and build relationships across the team.

**RISES link:** Excellence: concrete steps in the first 90 days.

### Q9. How would you contribute to the company's growth?
Deliver quality work on time, share knowledge, bring ideas from hackathons and projects, and keep learning so my skills grow along with the company's needs.

**RISES link:** Stewardship: contribute to long-term growth.

### Q10. What type of manager do you work best with?
One who gives clear goals, regular feedback and trust to own tasks, while being available when I'm blocked. I also appreciate managers who explain the "why" behind decisions.

**RISES link:** Respect: say what you need from a manager.

### Q11. What do you know about Barclays' purpose and values? **[RISES: all]**
Barclays' stated purpose is about working together for a better financial future, and its five values (Respect, Integrity, Service, Excellence and Stewardship, together RISES) are described as its moral compass. In simple terms: value everyone's contribution, act with honesty and courage, put the people and businesses you serve first, set high standards and leave things better than you found them. [Re-read the Purpose and Values page on home.barclays the night before and use one phrase in your own words.]

### Q12. Which of the five values resonates most with you, and why? **[RISES: pick one + link]**
Pick one and prove it with a story. Example: "Integrity, because in my AI project I deliberately chose to block risky queries and in the ML project I reported the real 80.9% accuracy instead of a flattering number. In banking, trust is the product." Then add: "I also see the other four in my work: Service in how I designed for users, Excellence in how I optimized performance, Respect in how I led teams, Stewardship in how I documented and handed over."

### Q13. What do you know about The Barclays Way? **[RISES: Integrity, Respect]**
It is Barclays' code of conduct, which sets out the purpose, values and expected behaviours for colleagues across the organization, and I understand it's taken seriously, including the idea that behaviour matters as much as results. I'd read and complete any required training carefully because, in a bank, how you work is as important as what you deliver.

### Q14. Why financial services and not a product or startup company? **[RISES: Service, Excellence]**
Banks run systems that people's financial lives depend on, so engineering is held to high standards of correctness, security and reliability. That matches the work I enjoy most: performance, data integrity and secure design. I also like that technology at a bank can have a big impact on real customers at scale.

### Q15. What do you know about Barclays as an organization? **[RISES: Excellence]**
It's a long-established British bank (more than 330 years old according to its own materials) with consumer, corporate and investment banking activities and a global footprint, including technology and operations teams in India. [Check the current divisions and the Pune/India presence on the Barclays website and mention one recent fact or announcement.] I'd rather be accurate than long-winded, so I'll stick to what I've verified.

### Q16. How would you contribute to Barclays' culture? **[RISES: Respect, Stewardship]**
By being reliable and inclusive, sharing knowledge, taking feedback well and speaking up honestly when something isn't right. In hackathons I was the person who made sure everyone's skills were used and that credit was shared, and I'd bring that attitude to the team.

### Q17. What challenges do you think banks face in technology today? **[RISES: Excellence, Integrity]**
Cybersecurity and fraud, legacy systems that must be modernized without disrupting customers, strict regulation and data privacy, high customer expectations for digital experience, and responsible use of AI. Balancing innovation with control is the central challenge.

### Q18. How do you see AI changing banking? **[RISES: Excellence, Integrity]**
AI can improve fraud detection, customer support, document processing and risk analysis. But in banking, models must be explainable, fair, secure and monitored, with humans in the loop for important decisions. My projects have given me a view of both sides: what AI can do and why validation and guardrails matter.

### Q19. How would you feel working in a heavily regulated environment? **[RISES: Integrity, Excellence]**
Comfortable. Regulations protect customers and the system's stability, so I see controls as part of quality, not obstacles. I'd learn the rules for my area, follow approval processes and ask when unsure rather than guess.

### Q20. What if a control or process feels slow or unnecessary? **[RISES: Integrity, Respect]**
First understand why it exists, because there's often a risk it prevents that I can't see. If it still seems inefficient, I'd raise the suggestion through the proper channel with data or an example. I wouldn't bypass it.

### Q21. How would you explain to someone why customers should trust a bank? **[RISES: Integrity, Service]**
Because the bank keeps their money and data safe, is transparent, treats them fairly and delivers dependable service, and because it's accountable to regulators. Trust is earned through every transaction and every interaction.

### Q22. What is your view on sustainability in finance? **[RISES: Stewardship]**
Finance influences which businesses and projects get funded, so it can support a transition to more sustainable economies. I think banks should balance profit with responsible impact. Stewardship, as I understand it, means leaving things better than you found them, and that applies to the environment and the systems we build.

### Q23. How would you make a difference as a graduate joining Barclays? **[RISES: Excellence, Respect]**
By learning fast, doing the basics with high quality, asking good questions and bringing fresh energy and ideas, including from projects and hackathons. Making a difference early is mostly about reliability and attitude.

### Q24. How do you work with global teams across time zones? **[RISES: Respect, Excellence]**
Clear written communication, concise updates, shared documentation, respecting each other's working hours and confirming decisions in writing. I've practiced with distributed teammates and know that over-communication beats assumptions.

### Q25. What do you think a day in the life of a developer here looks like? **[RISES: Excellence]**
Stand-ups, working on user stories, code reviews, testing, coordinating with product, risk and operations colleagues, handling incidents or releases occasionally, and continuous learning. [Adjust after you've read the job description.]

### Q26. What do you expect from your manager at Barclays? **[RISES: Respect]**
Clear goals, regular feedback, the chance to own meaningful work and support when I'm stuck. In return, I'd give honest updates, take ownership and respond quickly to feedback.

### Q27. What would you do if your team's task seemed to conflict with a policy? **[RISES: Integrity]**
Pause, check the policy, raise it with my lead or compliance contact and propose an alternative that meets the goal within the rules. I wouldn't proceed on assumption. Doing the right thing includes asking.

### Q28. What kind of projects would you like to work on? **[RISES: Excellence, Service]**
Backend and data-driven systems: APIs, performance, secure payments or customer-facing platforms, and eventually applied-AI features like fraud detection or intelligent assistants. I'm flexible and care most about learning and real impact.

### Q29. What are the biggest risks banks face? **[RISES: Integrity, Stewardship]**
Credit risk, market risk, operational risk (including technology failures and cyber attacks), conduct risk and regulatory risk. As an engineer I'd contribute most to operational and conduct risk by building reliable, secure and auditable systems.

### Q30. How would you feel about working on legacy systems? **[RISES: Stewardship, Excellence]**
Positive. Legacy systems keep the business running, and improving them carefully is valuable work. I'd learn the history, respect the constraints and make incremental, well-tested improvements instead of rewriting recklessly.

### Q31. We might ask you to learn a stack different from what you know. How do you feel? **[RISES: Excellence]**
Fine: I've already moved between Java, JavaScript and Python and learned LangChain, PostgreSQL optimization and DHM concepts quickly. I'll start with the fundamentals, pair with a mentor and deliver small pieces early.

### Q32. How would you handle being a small part of a large organization? **[RISES: Respect, Service]**
Focus on doing my part exceptionally well, understand how it connects to the whole, communicate clearly across teams and find mentors. Large organizations reward people who collaborate and own their piece.

### Q33. What would your first six months at Barclays look like? **[RISES: Excellence, Respect]**
Months 1-2: onboarding, training, learning systems and processes, small tasks with reviews. Months 3-4: own small features, learn the domain and release process. Months 5-6: contribute consistently, suggest an improvement and ask for feedback on my growth.

### Q34. What would you do if you accidentally made a mistake involving customer data? **[RISES: Integrity, Service]**
Stop, report it immediately through the correct channel, describe what happened accurately, help contain and fix it and take part in learning what to change. Never hide it, because speed of disclosure limits harm.

### Q35. What does "customer first" mean in a technology role? **[RISES: Service]**
It means thinking about the person at the other end of every decision: reliability, security, clear error messages, accessibility and performance. Even if I never speak to a customer, my code affects their experience.

### Q36. Where do you see yourself at Barclays in three years? **[RISES: Excellence, Stewardship]**
A confident engineer who owns features end to end, understands the domain and contributes to design decisions and mentoring of newer joiners, with exposure to areas like security or applied AI. I'd like to be known as someone trustworthy and dependable.

### Q37. What would make you want to stay at Barclays long term? **[RISES: Respect, Excellence]**
Meaningful work, strong mentors, fair treatment, learning opportunities and a culture that lives its values. I'd stay where I can keep growing and feel my contribution is valued.

### Q38. What are your thoughts on diversity and inclusion at work? **[RISES: Respect]**
Teams with diverse backgrounds make better decisions and build better products for diverse customers. In practice it means listening to everyone, avoiding assumptions, sharing credit and making sure people can contribute fully.

### Q39. How do you feel about continuous mandatory training and compliance requirements? **[RISES: Integrity, Excellence]**
Positive: they protect customers and colleagues, and I'd take them seriously instead of clicking through. In a bank, a missed control can have a real impact.

### Q40. What questions do you have for Barclays about its culture? **[RISES: Respect, Stewardship]**
(1) How do teams here put the RISES values into practice day to day? (2) What does mentoring look like for graduates? (3) How do technology teams balance innovation with risk and control? (4) What opportunities exist to learn about other areas of the bank?

---

# SECTION 9: SITUATIONAL AND STRESS QUESTIONS

### Q1. You are given a task with unclear requirements. What do you do?
Clarify with the task owner (who is it for? what outcome? what constraints?), write down my assumptions, build a small version quickly and confirm it before going deeper. This is what I did at Gagangiri with stakeholder discussions.

**RISES link:** Excellence and Integrity: clarify before you build.

### Q2. Your teammate has not completed their part and the deadline is tomorrow.
Check in privately to understand the blocker, offer help or pair up, redistribute tasks if needed, and tell the manager about the risk early with a plan. Blaming doesn't help; recovering does.

**RISES link:** Respect and Integrity: help first, escalate with a plan.

### Q3. You find a major bug right before a release.
Assess severity and impact, inform stakeholders, decide whether to fix or roll back/delay, fix it with a test, and verify. I'd document the root cause afterward. Transparency matters more than hiding it to meet the date.

**RISES link:** Integrity: transparency over hiding the problem.

### Q4. A senior asks you to do something you think is wrong.
Respectfully raise my concern with evidence, ask for their reasoning, and listen. If they still decide to go ahead and it's not unethical or unsafe, follow the decision while documenting the risk. If it is unethical or unsafe, escalate appropriately.

**RISES link:** Integrity: raise it respectfully with evidence.

### Q5. You have two urgent tasks from two managers.
Be transparent about my workload, ask them to prioritize together, or negotiate timelines. Quietly doing one and neglecting the other creates surprises, so I'd communicate early.

**RISES link:** Service: clear communication on priorities.

### Q6. You are asked to work on a technology you've never used.
Accept, study the basics quickly, ask mentors for pointers and deliver incrementally with early review. I have done this multiple times, for example with LangChain and DHM concepts.

### Q7. The client changes requirements mid-sprint.
Assess the impact on scope and time, discuss trade-offs with the team/PM (what to drop or delay), and re-plan with the client's agreement. Changes are normal; unmanaged changes are the problem.

### Q8. You realize you made a serious mistake that nobody has noticed.
Report it immediately and propose a fix. Honesty builds trust, and early disclosure limits the damage.

**RISES link:** Integrity: report mistakes immediately.

### Q9. You are overloaded with work.
Prioritize by impact and urgency, communicate my capacity and ask for help or re-prioritization early. Silent overload leads to missed deadlines and burnout.

### Q10. A teammate takes credit for your work.
Address it privately and directly first, assuming it may be unintentional, and document my contributions going forward (PRs, notes, updates) so credit is visible naturally. Escalate only if it continues.

### Q11. (Stress) "Your resume seems exaggerated. Prove it."
Stay calm: "I'm happy to walk you through any item in detail, show code, explain my decisions and also discuss what I didn't do." Everything on my resume is something I can defend. Then give a specific example with details.

**RISES link:** Integrity: be ready with specifics, not defensiveness.

### Q12. (Stress) "Why shouldn't we hire you?"
The honest answer: I have limited full-time experience at production scale. But I offset that with internships, fast learning and a strong drive to improve, and I'd rely on mentorship to ramp up quickly.

### Q13. (Stress) "You're just a hackathon person; can you do real work?"
Hackathons showed my speed and leadership, but my internships showed real engineering: JWT security, query optimization and delivering to stakeholders. I know prototypes and production are different, and I've worked on both.

### Q14. (Stress) "Someone with a higher CGPA applied. Why you?"
Grades matter, and I respect them, but I bring applied experience, leadership and a demonstrated ability to deliver. I'm happy to show it through discussion or a technical task.

**RISES link:** Integrity and Respect: stay factual, never combative.

### Q15. (Stress) "Sell me this pen."
Ask about their needs first ("How often do you write? What matters: comfort, durability, style?"), then connect features to those needs, and close with the benefit. Keep it short and confident.

### Q16. A colleague asks to borrow your login to finish urgent work. What do you do? **[RISES: Integrity]**
Refuse politely and explain that sharing credentials breaks security policy and removes accountability. I'd help them request proper access quickly, or do the task myself under my own responsibility if I'm authorized. Urgency doesn't override controls.

### Q17. You find customer or sensitive data in an application log. What do you do? **[RISES: Integrity, Service, Stewardship]**
Stop sharing it, report it to my lead and the relevant security/privacy contact immediately, limit access if I can and help fix the logging so it can't happen again. Customer data exposure must be escalated, not quietly cleaned.

### Q18. Your manager asks you to skip testing to meet a deadline. **[RISES: Excellence, Integrity, Respect]**
I'd explain the risk with specifics (what could break and its impact), propose a reduced but risk-based test plan for the critical paths and ask for a decision on scope. If the release still goes ahead, I'd document the known risk. I'd be respectful but clear.

### Q19. A teammate makes an offensive joke in a meeting. **[RISES: Respect]**
If appropriate, speak to them privately afterward, explaining that it made others uncomfortable. If it's serious or repeated, escalate it through the right channel. Everyone should feel safe and respected at work.

### Q20. A customer is angry about a problem caused by a system bug. **[RISES: Service, Integrity]**
Listen, acknowledge the impact, apologize without excuses, explain what we're doing and give a realistic timeline. Then fix the cause and follow up. Empathy and honesty rebuild trust.

### Q21. A vendor offers you a gift. **[RISES: Integrity]**
Decline or follow the company's gifts and hospitality policy, and report it if required. Gifts can create conflicts of interest, and the perception of influence matters even if you feel unaffected.

### Q22. Two teams give you conflicting priorities. **[RISES: Respect, Service]**
Be transparent with both about the conflict, ask them to agree on priority with my manager's help and communicate trade-offs and timelines. I'd avoid choosing silently.

### Q23. You see a senior colleague taking a shortcut that violates a process. **[RISES: Integrity, Respect]**
Check that I understand the situation first, then raise it respectfully with them. If it's a significant risk or continues, escalate to my manager or the appropriate channel. Seniority doesn't exempt anyone from the rules.

### Q24. You disagree with your manager's decision on a requirement. **[RISES: Respect, Integrity]**
Share my concerns with data in a private, respectful conversation, listen to their constraints and, if the decision stands, support it unless it's unethical or unsafe.

### Q25. You're asked to work over a weekend for a critical release. **[RISES: Service, Respect]**
Understand the reason and the scope, agree if it's truly critical, and discuss time off afterward. I'd also ask whether the problem could be prevented in future through better planning.

### Q26. You realize your estimate was badly wrong. **[RISES: Integrity, Excellence]**
Tell my manager and team right away, explain the cause, propose a revised plan or reduced scope and learn from it for future estimates. Early honesty gives the team options.

### Q27. A teammate seems overworked and stressed. **[RISES: Respect, Stewardship]**
Check in privately, listen, offer help or redistribute tasks if possible and suggest speaking to the manager or support resources if needed. A healthy team looks after its people.

### Q28. A production issue occurs late at night. **[RISES: Service, Excellence]**
Follow the incident process: acknowledge, assess impact, notify stakeholders, mitigate first (rollback/workaround), then find the root cause and write a post-incident review. Customers come first.

### Q29. A client asks for a feature that conflicts with policy or regulation. **[RISES: Integrity, Service]**
Explain that it can't be done as requested and why, then offer compliant alternatives that meet the underlying need. Saying no with a solution preserves trust.

### Q30. You notice a process that wastes team time. **[RISES: Excellence, Stewardship]**
Measure the waste, propose a simple improvement, test it on a small scale, share results and adopt it if it works. Continuous improvement is part of the job.

### Q31. You find that code copied from the internet could violate a license. **[RISES: Integrity, Stewardship]**
Flag it, replace or properly license it, and document the learning for the team. Ignoring it could create legal and reputational risk.

### Q32. A friend asks you to help them cheat in an online assessment. **[RISES: Integrity, Respect]**
Say no clearly and offer to help them study instead. Cheating harms fairness and damages trust.

### Q33. You're asked to present data in a way that looks favorable but is misleading. **[RISES: Integrity]**
I'd present the data accurately, with context and limits, and suggest an honest framing that still shows progress. Misleading stakeholders erodes trust in the long run.

### Q34. You receive a confidential email by mistake. **[RISES: Integrity, Respect]**
Don't read further than necessary, don't forward it, notify the sender and follow the company's procedure for misdirected information.

### Q35. A new team member is being ignored in meetings. **[RISES: Respect]**
Invite their input directly, make sure they're included in discussions and check in afterwards. Inclusion is everyone's job.

### Q36. You're blamed for a mistake that wasn't yours. **[RISES: Integrity, Respect]**
Stay calm, gather facts (logs, commits, emails), explain objectively without attacking anyone and focus on fixing the issue. I'd avoid blaming back.

### Q37. You're given a deadline that is impossible. **[RISES: Integrity, Excellence]**
Explain why with evidence, propose options (reduced scope, more resources, later date) and let the stakeholder decide. Over-promising and failing damages trust more than negotiating early.

### Q38. You're asked to do a task outside your role. **[RISES: Service, Respect]**
Ask why and for how long; if it's reasonable and helps the team, I'd do it while learning. If it becomes a pattern, I'd discuss role clarity with my manager.

### Q39. Two senior colleagues disagree in a meeting, and you're caught in the middle. **[RISES: Respect]**
Stay neutral, summarize both views fairly, ask clarifying questions that move toward facts and propose that the decision-maker decides based on data.

### Q40. You have an idea to reduce infrastructure cost and waste. **[RISES: Stewardship, Excellence]**
Gather evidence (usage data), estimate savings and risks, discuss with the team and propose a pilot. Efficient use of resources is part of stewardship.

---

# SECTION 10: SALARY, RELOCATION, JOINING AND WRAP-UP

### Q1. What are your salary expectations?
I'm focused on the opportunity and the learning, and I trust the company to offer a fair package as per its standards for freshers. If pressed: "Based on my research, I'm looking at a range of [X to Y], but I'm flexible depending on the overall role and growth." Research market data before the interview.

**RISES link:** Integrity: be honest and flexible, not vague.

### Q2. Are you willing to relocate?
Yes, I'm flexible. [Answer honestly, and mention any real constraints calmly.] Relocation also gives exposure to different teams and cities.

**RISES link:** Respect: willingness to relocate shows commitment.

### Q3. Are you comfortable with shifts or travel?
[Truthful.] If yes: "Yes, I'm comfortable with whatever the role requires." If you have limits, state them politely and briefly.

### Q4. When can you join?
After my final-year completion, [date], or as per the company's schedule. Be clear about exam dates or notice obligations if any.

### Q5. Are you ready to sign a service agreement?
[Truthful.] "I'd like to understand the terms, but I'm committed to a long-term career where I can grow." Asking about terms is reasonable.

### Q6. Do you have any other commitments (higher studies, etc.)?
[Truthful.] If none, say so clearly: "No, my full focus is on starting my career."

### Q7. Any questions for us?
Use the questions from Section 1, Q40, and add: "What are the next steps in the process?" Asking thoughtful questions shows genuine interest.

**RISES link:** Respect and Excellence: thoughtful questions show interest.

### Q8. Anything else you'd like to add?
I'd like to thank you for your time and for the opportunity. I'm eager to contribute, learn and grow, and I believe my experience, mindset and ability to deliver make me a strong fit for the team.

**RISES link:** Respect: end with sincere thanks.

### Q9. Are you comfortable with a background verification process? **[RISES: Integrity]**
Yes. Banks typically verify education, employment and identity, and I'm comfortable with that because everything on my resume is true. I'll keep my documents ready and respond quickly.

### Q10. Are you comfortable with a hybrid or in-office working model? **[RISES: Respect, Service]**
Yes, I'm flexible. [Truthful.] I understand in-person work helps collaboration and learning, especially early in a career.

### Q11. Do you have any backlogs or academic gaps to explain? **[RISES: Integrity]**
[Truthful.] If yes: state the facts, the reason and how you recovered, without excuses. If no: "No, I've maintained a consistent record." Hiding something is far worse than explaining it.

### Q12. Do you have any conflict of interest, such as relatives working at a competitor or at Barclays? **[RISES: Integrity]**
[Truthful.] Declare anything relevant openly. Transparency about conflicts is fundamental in banking.

### Q13. Why should we offer you a particular package? **[RISES: Excellence, Integrity]**
I'm not asking for more than the standard fresher package for the role; I'm confident I'll deliver value through my experience and learning speed. I'm more interested in the right environment to grow than in negotiating the number.

### Q14. Would you accept a role or team different from your first preference? **[RISES: Service, Respect]**
Yes, as long as there's learning and good mentoring. I'd discuss my interests but I'm flexible about where the business needs me.

### Q15. How do you feel about probation or a service agreement? **[RISES: Integrity]**
Reasonable, as organizations invest in training new hires. I'd read the terms carefully, and I'm committed to a long-term career.

### Q16. What happens if you receive another offer after accepting ours? **[RISES: Integrity]**
I would honor my commitment. Once I accept an offer, I treat it as final, since integrity in commitments matters to me.

### Q17. What matters more to you: compensation or growth? **[RISES: Excellence]**
Growth first, especially at the start of my career, because skills and good mentors compound. But fair compensation matters too, and I expect it to follow performance.

### Q18. What would make you leave a company? **[RISES: Integrity, Respect]**
A lack of learning and growth for a long time, an environment that disrespects people or compromises on ethics. I'd first try to address issues with my manager before thinking about leaving.

### Q19. What are your top three priorities in a job? **[RISES: Excellence, Respect]**
(1) Learning and growth, (2) meaningful work with impact, (3) a respectful, collaborative team. These shape how long I'd want to stay.

### Q20. What does work-life balance mean to you? **[RISES: Respect, Stewardship]**
Being able to deliver strong results during working hours and having enough time to rest and pursue personal interests so I can sustain performance. I'm happy to put in extra effort during critical periods.

### Q21. What will you do between now and joining? **[RISES: Excellence]**
Complete my academics, strengthen core areas (system design, Docker, DSA), build a small project and read about the bank's domain and culture so I can contribute sooner.

### Q22. How did you hear about this role? **[RISES: Integrity]**
[Truthful: campus placement drive, career site, referral.] Keep it simple.

### Q23. Are you available for further assessments or interview rounds? **[RISES: Respect, Service]**
Yes, please share the schedule, and I'll plan accordingly. [Mention any exam clashes in advance.]

### Q24. How will you handle joining formalities and documents? **[RISES: Integrity, Excellence]**
I'll prepare documents in advance, submit accurate information on time and respond promptly to HR communication.

### Q25. How do you feel about signing a confidentiality agreement? **[RISES: Integrity]**
Completely fine; protecting customer and company information is part of professional responsibility, and I've already practiced it during my internships.

### Q26. How do you feel about relocating to another city for the first few months of training? **[RISES: Service, Respect]**
Open to it. [Truthful.] I see it as a chance to learn and meet colleagues from different teams.

### Q27. What expectations do you have from the employer in your first year? **[RISES: Respect]**
Structured onboarding, a mentor, clear goals, regular feedback and the chance to work on real tasks. In return I'd bring effort, curiosity and accountability.

### Q28. What would you do if the role turned out different from what you expected? **[RISES: Integrity, Excellence]**
Give it a fair try, find what I can learn, talk to my manager with specific feedback and ask about alternatives. I'd avoid deciding too early.

### Q29. How do you handle long working hours during critical releases? **[RISES: Service, Stewardship]**
Plan ahead, communicate, pace myself and recover afterwards. I'd also look for ways to prevent last-minute crunches in the future.

### Q30. How do you feel about being asked to take on more responsibility early? **[RISES: Excellence]**
Excited, and I'd ask for support and clarity on expectations. I've led teams in competitions, so I'm comfortable with responsibility.

### Q31. What would you like us to know that isn't on your resume or covered today? **[RISES: Integrity, Respect]**
[Truthful and positive.] Perhaps a personal trait or story: for example, how you learned from losing a hackathon, or helped a junior teammate succeed.

### Q32. How would you describe your interview experience today? **[RISES: Respect]**
Positive, thoughtful and informative. [Mention one specific thing you appreciated, e.g., a question that made you think.]

### Q33. How will you follow up after the interview? **[RISES: Respect, Service]**
Send a short thank-you note (if possible) summarizing my interest, and respond promptly to anything needed. Being courteous and prompt matters.

### Q34. What is the one thing you would like to be remembered for from this interview? **[RISES: Integrity, Excellence]**
That I'm honest about what I know and don't know, I deliver results and I work well with people.

### Q35. Can you summarize why you are the right fit in 30 seconds? **[RISES: all]**
"I bring real experience from two internships and several projects, a record of leading teams under pressure, and values that match RISES: I act with integrity, I put users first, I hold high standards, I respect teammates and I try to leave things better than I found them. I'm eager to contribute and grow at Barclays."

### Q36. Would you recommend your college or program to others? **[RISES: Respect, Integrity]**
[Truthful.] Yes, for the opportunities it gave me, such as hackathons, internships and mentors, while also being honest about areas I'd improve. Balanced answers sound credible.

### Q37. How would you handle a delay in your joining date or onboarding? **[RISES: Respect, Excellence]**
Stay patient, keep in touch with HR politely and use the time productively to learn and prepare.

### Q38. Is there anything that might prevent you from joining or performing at your best? **[RISES: Integrity]**
[Truthful.] If not: "No, I'm fully committed." If yes (health, family, exams), explain briefly and constructively.

### Q39. What would be the best way for us to support your growth? **[RISES: Respect, Excellence]**
Assigning a mentor, giving feedback regularly, offering varied learning projects and training and being clear on expectations. I'll take ownership of my own learning too.

### Q40. Final question: any last thought you'd like to share? **[RISES: Respect]**
Thank them for their time, reiterate your interest in the role and the values, and finish confidently. Short and sincere.

---

# SECTION 11: BARCLAYS VALUES DEEP-DIVE (RISES): Respect, Integrity, Service, Excellence, Stewardship

> **Why this section exists.** Barclays describes its five values as its moral compass, and it expects behaviour that matches them, so expect value-based questions throughout the interview and not only in this part. Third-party prep guides also report that interviewers map answers to RISES when scoring, though that is not confirmed by Barclays itself. **The method:** for each value, keep **two ready STAR stories** from your resume (N+1 fix, JWT bug, ML overfitting and honest reporting, hackathon leadership, URL shortener concurrency, AI-SQL security) and say the value's name naturally once in the answer.
>
> **Quick map of your stories to values**
> | Value | Official idea (paraphrased) | Your best stories |
> |---|---|---|
> | **Respect** | Value diversity and inclusion, trust colleagues, value every contribution | Hackathon team leadership; quiet teammate; fair credit; code review feedback |
> | **Integrity** | Honesty, courage, transparency and fairness | Honest 80.9% reporting; blocking unsafe SQL; secure JWT handling; speaking up on an inflated slide |
> | **Service** | Empathy and humility; the people and businesses you serve come first | Dashboard for operators; user-friendly shortener; voice and multilingual input; stakeholder communication |
> | **Excellence** | High standards, innovation, using skills to make a positive difference | N+1 optimization; DB-enforced uniqueness; tuning the CNN; winning hackathons |
> | **Stewardship** | Sustainability; leaving things better than you found them | Documentation and handover; microplastic detection; maintainable code; mentoring juniors |

## 11A. RESPECT

### Q1. What does Respect mean to you in the workplace? **[RISES: Respect]**
It means valuing every person's contribution regardless of role, background or seniority, listening before judging and trusting colleagues to do their part. Practically: I invite quieter people to speak, give credit by name, give feedback privately and kindly and respect others' time by being prepared and on time. In my hackathon teams, respect was what made six different personalities work as one.

### Q2. Tell me about a time you valued a colleague's contribution that others overlooked. **[RISES: Respect]**
- **S (Situation):** In a hackathon, one teammate's work on testing and documentation looked "invisible" next to flashy features.
- **T (Task):** Make sure the team valued it.
- **A (Action):** I highlighted their work in our presentation, explained how it prevented demo failures and made sure it was part of our story.
- **R (Result):** Judges noticed the reliability, and the teammate felt recognized. [Adapt.]

### Q3. Tell me about a time you handled a disagreement respectfully. **[RISES: Respect, Integrity]**
- **S (Situation):** Two teammates disagreed on architecture in the SIH finale.
- **T (Task):** Decide quickly without conflict.
- **A (Action):** I gave each two minutes to explain, compared options on time and risk, chose one and gave the other person ownership of a critical component.
- **R (Result):** Decision made in minutes, and no resentment.

### Q4. How do you make sure everyone is included in team discussions? **[RISES: Respect]**
I ask for input from each person directly, use round-robin during brainstorming, share notes afterwards so no one misses out and watch for people who are being talked over. Different perspectives often reveal the best solution.

### Q5. Tell me about a time you adapted your communication to a different audience. (STAR) **[RISES: Respect, Service]**
- **S (Situation):** At Gagangiri, I had to present results to company stakeholders and, separately, explain them to my engineering teammates.
- **T (Task):** Make both groups understand and act on the results.
- **A (Action):** For stakeholders I used plain language, visuals and outcomes like speed and reliability; for engineers I included metrics, data splits and limits.
- **R (Result):** Stakeholders approved the next steps and engineers could reproduce the work.
- **Learning:** Match the explanation to the listener.

### Q6. Tell me about a time you gave constructive feedback. (STAR) **[RISES: Respect, Integrity]**
- **S (Situation):** A teammate's hackathon slides were overloaded with text. [Adapt.]
- **T (Task):** Help them improve before the presentation without discouraging them.
- **A (Action):** I spoke privately, praised what worked, gave one specific suggestion and offered to rehearse with them.
- **R (Result):** The slides became clear and they thanked me.
- **Learning:** Be specific, kind and focused on the work.

### Q7. How do you respond if someone is disrespectful toward you? **[RISES: Respect, Integrity]**
Stay calm, address it directly and privately by describing the behaviour and its effect, and ask for a better way of working together. If it continues or is serious, I'd escalate through the right channel. I won't respond with disrespect.

### Q8. How would you work with someone from a different culture or language background? **[RISES: Respect]**
Be patient, speak clearly, avoid slang, confirm understanding in writing, learn about their working style and be curious. Our hackathon teams included people from different regions and skill sets, and differences became strengths.

### Q9. Tell me about a time you trusted a teammate with an important responsibility. (STAR) **[RISES: Respect, Excellence]**
- **S (Situation):** In a hackathon, a key integration piece needed an owner while I was coordinating.
- **T (Task):** Delegate it rather than do everything myself.
- **A (Action):** I handed it to a junior teammate, explained the goal, set a check-in and stayed available for help.
- **R (Result):** They delivered on time and gained confidence.
- **Learning:** Trust is how teams scale.

### Q10. How do you show respect for people's time? **[RISES: Respect, Excellence]**
Prepare for meetings, keep updates concise, share written notes, respond in a reasonable time and ask for help with specific questions. At Globant, I came to mentors with context and what I'd already tried.

## 11B. INTEGRITY

### Q11. What does Integrity mean to you? **[RISES: Integrity]**
Honesty, courage and fairness: telling the truth even when it's uncomfortable, owning mistakes, following rules and doing the right thing when nobody is watching. In a bank, it also means protecting customers' trust in everything you do.

### Q12. Tell me about a time you did the right thing when no one was watching. (STAR) **[RISES: Integrity]**
- **S (Situation):** While testing, I noticed an edge case that could expose more data than intended. [Adapt.]
- **T (Task):** Decide whether to ignore it, patch it quietly or report it.
- **A (Action):** I fixed it, added a test and told my mentor what I'd found.
- **R (Result):** The issue was closed before it reached users, and my mentor appreciated the transparency.
- **Learning:** Doing the right thing includes being transparent about it.

### Q13. Tell me about a time you admitted a mistake. **[RISES: Integrity, Respect]**
- **S (Situation):** I pushed a change that broke a dependent feature [adapt].
- **T (Task):** Fix it and prevent a repeat.
- **A (Action):** I told my mentor immediately, fixed it and added a test.
- **R (Result):** Minimal impact, and my mentor appreciated the honesty. Lesson: early disclosure builds trust.

### Q14. Tell me about a time you refused to take a shortcut. (STAR) **[RISES: Integrity, Excellence]**
- **S (Situation):** While building the URL shortener, the quick way to enforce alias uniqueness was a check in application code.
- **T (Task):** Make uniqueness correct under concurrent requests.
- **A (Action):** I used a database UNIQUE constraint, handled the conflict with a 409 and tested with parallel requests.
- **R (Result):** No duplicates under concurrency.
- **Learning:** A shortcut that hides risk isn't a shortcut.

### Q15. Tell me about a time you were honest when it was difficult. (STAR) **[RISES: Integrity]**
- **S (Situation):** At Gagangiri, our model reached 80.9% accuracy, lower than the stakeholders hoped.
- **T (Task):** Report it truthfully.
- **A (Action):** I presented the test-set result, explained the data limits and showed the plan to improve it.
- **R (Result):** Stakeholders trusted the work and supported the next steps.
- **Learning:** Honesty builds long-term credibility.

### Q16. Have you ever had to stand up to someone senior? (STAR) **[RISES: Integrity, Respect]**
- **S (Situation):** A mentor suggested an approach that would likely slow down with large data. [Adapt.]
- **T (Task):** Raise my concern respectfully.
- **A (Action):** I came with query timings and an alternative, asked for their view and listened to their constraints.
- **R (Result):** We agreed on an improved approach.
- **Learning:** Courage is honesty combined with respect.

### Q17. How would you handle pressure to hit a target by bending a rule? **[RISES: Integrity]**
I'd decline to bend it, explain the risk and offer a compliant way to get close to the goal. If pressure persisted, I'd escalate to a senior manager or compliance. A missed target can be recovered, but a breach of trust often can't.

### Q18. How do you handle confidential information? **[RISES: Integrity, Stewardship]**
Access only what I need, don't share it outside approved channels, don't discuss it in public places, keep credentials secure and report any accidental exposure at once. My internships taught me to treat data as someone else's trust.

### Q19. What would you do if you discovered a colleague lying about completed work? **[RISES: Integrity, Respect]**
Speak with them first to clarify and encourage correcting it. If it's serious or not corrected, report it to my manager, because inaccurate status can affect customers and decisions.

### Q20. What do transparency and fairness mean in teamwork? **[RISES: Integrity, Respect]**
Transparency: sharing progress and problems openly and being clear about decisions. Fairness: distributing work and credit equitably and judging people on contributions, not favoritism. These made our hackathon teams trust each other.

## 11C. SERVICE

### Q21. What does Service mean in a technology role? **[RISES: Service]**
Remembering that every system serves people: customers, colleagues and communities. It means designing for the user's needs, acting with empathy and humility, making things reliable and easy to use and listening to feedback. Even backend engineers serve customers through uptime, speed and security.

### Q22. Tell me about a time you delivered excellent service to a user or stakeholder. **[RISES: Service, Excellence]**
- **S (Situation):** Gagangiri operators needed to monitor water quality quickly.
- **T (Task):** Build something they could use without ML knowledge.
- **A (Action):** I designed the dashboard around their decisions (clear OK/alert status, trends) and gathered feedback after each demo.
- **R (Result):** They found it understandable and useful. [Adapt.]

### Q23. Tell me about a time you understood someone's underlying need, not just their request. (STAR) **[RISES: Service]**
- **S (Situation):** Gagangiri asked for 'detection of microplastics'.
- **T (Task):** Understand what decision the tool should support.
- **A (Action):** I asked what they would do with the result and learned they needed fast, trustworthy screening for production.
- **R (Result):** We focused on recall, clear alerts and honest limitations.
- **Learning:** Ask 'what decision will this help you make?'

### Q24. How do you handle a difficult or frustrated stakeholder? **[RISES: Service, Respect]**
Listen without interrupting, acknowledge their concern, clarify facts, propose next steps with dates and follow up. Frustration usually points to an unmet need.

### Q25. Tell me about a time you went beyond what was asked to help someone. (STAR) **[RISES: Service]**
- **S (Situation):** Operators would use our dashboard, but they weren't technical.
- **T (Task):** Make adoption easy.
- **A (Action):** I wrote a short usage guide and added tooltips for key indicators in my own time.
- **R (Result):** They could use it without calling us.
- **Learning:** Small extras can decide whether a tool is actually used.

### Q26. Give an example of acting with empathy and humility. (STAR) **[RISES: Service, Respect]**
- **S (Situation):** A teammate struggled with a concept and felt embarrassed to ask.
- **T (Task):** Help without making them feel small.
- **A (Action):** I asked what they'd tried, explained from their starting point and admitted I had also struggled with the same thing at first.
- **R (Result):** They understood it and felt comfortable asking again.
- **Learning:** Humility keeps you learning and makes others comfortable.

### Q27. How do you gather and use user feedback? **[RISES: Service, Excellence]**
Short demos, asking specific questions ("what was confusing?"), watching people use the product and acting on patterns. In the AI-SQL project, feedback led me to show the generated SQL and confirm voice transcriptions.

### Q28. Tell me about a time you put someone else's needs before your convenience. (STAR) **[RISES: Service, Respect]**
- **S (Situation):** In a hackathon, a teammate was struggling with a critical integration late at night while I had my own tasks.
- **T (Task):** Protect the team's outcome.
- **A (Action):** I stayed late to help them finish it and moved my tasks to the morning.
- **R (Result):** The integration worked and the demo succeeded.
- **Learning:** The team's success mattered more than my schedule.

### Q29. How do you balance customer needs with deadlines? **[RISES: Service, Integrity]**
Understand what is truly critical for the customer, deliver that first with quality, communicate clearly about the rest and avoid promises I can't keep. Honest expectation-setting is a service in itself.

### Q30. Have you done any community or social service work? **[RISES: Service, Stewardship]**
[Truthful.] If yes, describe it and what you learned. If limited: "My main community contribution has been mentoring juniors for hackathons and my sustainability-focused project work; I'd like to do more." Don't invent.

## 11D. EXCELLENCE

### Q31. What does Excellence mean to you? **[RISES: Excellence]**
Setting high standards, delivering reliably and continuously improving, while using my skills to make a positive difference. It's not perfectionism: it's choosing what must be right (security, data integrity, user experience) and doing it very well.

### Q32. Tell me about a time you set a high standard for yourself. (STAR) **[RISES: Excellence]**
- **S (Situation):** The URL shortener's happy path worked, but I wasn't satisfied.
- **T (Task):** Make it reliable in real conditions.
- **A (Action):** I tested duplicate aliases, expired links, invalid URLs and concurrent requests, then fixed each failure.
- **R (Result):** It behaved correctly in all those cases.
- **Learning:** Set the standard as 'works in real conditions'.

### Q33. Tell me about a time you improved something that was already working. (STAR) **[RISES: Excellence, Stewardship]**
- **S (Situation):** A Globant API gave correct data but was slow with growing data.
- **T (Task):** Improve performance without changing behavior.
- **A (Action):** I found an N+1 pattern, replaced it with a JOIN and added indexes, then re-measured.
- **R (Result):** Response time dropped from [X ms] to [Y ms].
- **Learning:** Measure first, then fix.

### Q34. Describe a time you were innovative. (STAR) **[RISES: Excellence]**
- **S (Situation):** Most NL2SQL demos accept only English text and do little to prevent unsafe queries.
- **T (Task):** Build an assistant that is more inclusive and safe.
- **A (Action):** I added multilingual voice input, a salted-hash auth layer, input validation and a rate limiter.
- **R (Result):** A working assistant that handled different languages while blocking unsafe SQL.
- **Learning:** Innovation is solving a real problem in a new way and making it safe.

### Q35. How do you balance perfectionism with deadlines? **[RISES: Excellence, Integrity]**
Define "good enough" for each part: critical components must be excellent; others can be simple and improved later. I communicate trade-offs and avoid hidden shortcuts.

### Q36. Tell me about a time you used your skills to make a positive difference. (STAR) **[RISES: Excellence, Stewardship]**
- **S (Situation):** Gagangiri needed faster screening of water for microplastics.
- **T (Task):** Build a prototype that could help protect water quality.
- **A (Action):** We trained a CNN on DHM data, tuned it and built a monitoring dashboard.
- **R (Result):** A prototype with 80.9% binary accuracy and a dashboard for production monitoring.
- **Learning:** Technical skill can serve a real-world cause.

### Q37. Tell me about a time you didn't meet your own standard and what you did. (STAR) **[RISES: Excellence, Integrity]**
- **S (Situation):** In an early hackathon, we added too many features and the demo was unstable. [Adapt.]
- **T (Task):** Learn from it and avoid repeating it.
- **A (Action):** We reviewed what went wrong and then set scope earlier, tested continuously and rehearsed more.
- **R (Result):** Later events produced a 1st place and a runner-up finish.
- **Learning:** Failure is useful only if you change behaviour.

### Q38. How do you keep learning to maintain high standards? **[RISES: Excellence]**
I seek feedback, read documentation and good code, build side projects and practice fundamentals like DSA and system design. I treat each internship or project as a chance to raise my baseline.

### Q39. How do you hold teammates to high standards without creating pressure? **[RISES: Excellence, Respect]**
Set clear expectations together, define what "done" means, give kind and specific feedback and help when someone is behind. People rise to high standards when they feel supported.

### Q40. Which project are you most proud of for its quality? **[RISES: Excellence]**
The URL shortener, for its correctness under concurrency and proper HTTP semantics, or the N+1 optimization for measurable improvement. [Choose one and explain concretely.]

## 11E. STEWARDSHIP

### Q41. What does Stewardship mean to you? **[RISES: Stewardship]**
Caring about the long term: leaving codebases, teams, communities and the environment better than I found them. It includes sustainability, responsible use of resources and handing over work in a state others can continue.

### Q42. Tell me about a time you left a project better than you found it. (STAR) **[RISES: Stewardship]**
- **S (Situation):** At the end of the Gagangiri project, the work could have been left as a few notebooks and a dashboard.
- **T (Task):** Make it possible for others to continue.
- **A (Action):** We documented preprocessing, model settings, results and limitations and described how to run and update the dashboard.
- **R (Result):** The company could reproduce and extend the work.
- **Learning:** Handover quality is part of delivery.

### Q43. Tell me about a time you considered sustainability in a decision. (STAR) **[RISES: Stewardship]**
- **S (Situation):** In the ML project, a heavier model would have needed more compute for small gains. [Adapt.]
- **T (Task):** Choose an approach that was effective and efficient.
- **A (Action):** We compared a lightweight CNN against bigger options and chose the lighter model with augmentation.
- **R (Result):** Comparable accuracy with lower compute needs.
- **Learning:** Efficiency is part of responsible engineering.

### Q44. How do you balance short-term results with long-term impact? **[RISES: Stewardship, Excellence]**
Deliver the short-term goal without sacrificing foundations (security, maintainability), track compromises explicitly and schedule time to fix them. Long-term thinking is valuable only if you still deliver today.

### Q45. Tell me about a time you helped someone grow. (STAR) **[RISES: Stewardship, Respect]**
- **S (Situation):** A junior teammate lacked confidence with backend APIs.
- **T (Task):** Help them become independent.
- **A (Action):** I paired on a first integration, explained the reasoning, then let them lead the next one and gave feedback.
- **R (Result):** They later handled a similar task alone.
- **Learning:** Teaching multiplies impact.

### Q46. How do you reduce waste in your work? **[RISES: Stewardship, Excellence]**
Avoid duplicated effort through clear ownership, automate repetitive steps, remove unused code and indexes, and choose efficient designs. Wasted time and resources are costs for everyone.

### Q47. What contribution have you made to your college or community? **[RISES: Stewardship, Service]**
[Truthful.] For example: mentoring juniors for hackathons, sharing notes, organizing or helping with a technical event. If limited, say what you intend to start.

### Q48. A quick hack would work now but could cause problems later. What do you do? **[RISES: Stewardship, Integrity]**
Assess the risk and the deadline, use the hack only if it's truly necessary and safe, document it as debt with a plan to fix it and inform the team. I wouldn't quietly leave a hidden risk.

### Q49. How do you think about responsible use of resources like cloud cost and data? **[RISES: Stewardship]**
Use only what's needed, clean up unused resources, minimize collecting sensitive data, retain data only as long as necessary and monitor usage. Responsible use protects both the company and customers.

### Q50. What legacy do you want to leave in your team? **[RISES: Stewardship, Respect]**
That the code and documentation were clear, that people felt supported and that someone who joined after me could learn faster because of what I left behind.

## 11F. CROSS-VALUE AND SCENARIO QUESTIONS

### Q51. Tell me one story that shows all five values. (STAR) **[RISES: all]**
- **S (Situation):** The Gagangiri microplastic detection project.
- **T (Task):** Deliver a useful prototype for the company.
- **A (Action):** **Respect:** I made sure every teammate's ideas shaped the solution. **Integrity:** we reported the true 80.9% accuracy and its limits. **Service:** we built the dashboard around operators' decisions. **Excellence:** disciplined experiments, validation and tuning. **Stewardship:** we documented everything for handover, on a project aimed at safer water.
- **R (Result):** A working prototype, honest results and a project the company could continue.
- **Learning:** Keep this to about 90 seconds.

### Q52. Which of the five values is hardest for you? **[RISES: Excellence, Integrity]**
Be honest: e.g., "Delegation is my challenge with Respect (trusting others fully), because I tend to take too much on myself. I've started giving clear ownership and checking in less." Always show action.

### Q53. How would you rank the five values for yourself? **[RISES: all]**
There's no right order; explain your reasoning. Example: "In a bank, Integrity is the foundation, because everything else depends on trust. Then Service, since the customer is the purpose; Excellence, which delivers quality; Respect, which makes teams work; and Stewardship, which keeps it all sustainable." Stress that you see them as connected.

### Q54. Can values ever conflict? Give an example. (STAR) **[RISES: Service, Integrity, Excellence]**
- **S (Situation):** A customer or stakeholder wants something fast (Service) that would breach a policy or cut quality (Integrity, Excellence). [Example: a client asks you to present a favourable but misleading chart.]
- **T (Task):** Resolve the conflict without losing trust.
- **A (Action):** I'd put Integrity first, explain what can't be done and why, then find the best compliant way to serve the need, such as an honest framing that still shows progress.
- **R (Result):** The customer keeps trust and the work stays compliant.
- **Learning:** Integrity sets the boundary; Service and Excellence work inside it.

### Q55. How did you demonstrate RISES in a hackathon? **[RISES: all]**
*Respect:* shared credit. *Integrity:* demoed only what worked. *Service:* built for real users. *Excellence:* tested the core flow and rehearsed. *Stewardship:* documented and planned for continuation. Hackathons taught me values under pressure.

### Q56. A client asks you to do something unethical, but it would win business. What do you do? **[RISES: Integrity, Service]**
Decline respectfully, explain that I can't proceed because it breaches ethical and regulatory standards, offer a compliant alternative and escalate if pressured. Long-term trust is worth more than one deal.

### Q57. How do you stay true to your values under pressure? **[RISES: Integrity, Excellence]**
Decide in advance what lines I won't cross, ask "would I be comfortable if this were public?", slow down when stressed and seek advice when unsure. Values are easiest to keep when you've thought about them before the pressure arrives.

### Q58. How would your friends or teammates say you live these values? **[RISES: Respect, Integrity]**
They'd say I'm fair about credit (Respect), honest even when it's inconvenient (Integrity), helpful (Service), disciplined about quality (Excellence) and that I leave things organized for others (Stewardship). Add a real example or quote if you can.

### Q59. Which value would you like to develop most at Barclays? **[RISES: Stewardship, Excellence]**
For example: "Stewardship at scale: understanding how decisions in a large bank affect customers, colleagues and communities over years, and learning from experienced people how to lead responsibly." Choose honestly.

### Q60. How would you measure whether you're living RISES day to day? **[RISES: all]**
Ask: Did I treat everyone with respect? Was I honest, including about my mistakes? Did I help someone I serve? Did I do work I'm proud of? Did I leave something better? I'd also invite feedback from peers and managers.

---

# SECTION 12: FINAL TIPS AND CHECKLIST

**Before the interview**
- Prepare 6 STAR stories and reuse them: *(1) N+1 optimization, (2) JWT refresh bug, (3) overfitting in the ML project, (4) SIH leadership, (5) team conflict, (6) a mistake or failure.* One story can answer many questions.
- Research the company: products, news, values, recent projects and tech stack.
- Keep your resume in hand and be ready for **every** line.
- Prepare to explain the date overlap (Globant Jan–Apr 2026, Gagangiri Feb–May 2026) clearly and truthfully.
- Prepare your Q1 self-introduction until it's smooth but not robotic.

**During the interview**
- Smile, speak slowly, make eye contact, and structure answers: point → example → result.
- Keep answers 60-120 seconds. Stop when you're done.
- If you don't know, say so, then explain your approach.
- Never criticize former teams, colleges or companies.
- Listen to the whole question before answering, and ask for clarification if needed.

**Barclays-specific**
- Learn the five values and the one-line idea behind each; prepare **two STAR stories per value** (Section 11 has a map to your resume).
- Banking interviews reward calm honesty: if you don't know a banking term, say so and explain how you'd learn it.
- Show that you respect rules and controls (security, compliance, confidentiality) while still being practical.
- Weave a value into an answer in your own words once, and never recite the acronym like a slogan.
- Re-read the Purpose and Values page on home.barclays and the job description the night before.

**Honesty check**
Every [bracket] must be replaced with real facts. If something isn't true, delete it. A smaller true answer beats a bigger false one.

*You've got this. Best of luck for your placement!*
