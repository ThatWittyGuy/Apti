# 🎯 03_Scenarios & Case Studies — PM/Product Management Intern Interview
### Live Problem-Solving, Situational Judgment & AI Product Scenarios

---

> **How to use this document:** Interviewers will present you with a situation and say "walk me through what you'd do." They are testing your judgment, structure, and PM instincts — not a perfect answer. Read every scenario, then cover the answer and say your response aloud. Practice until your thinking feels automatic, not scripted.

> **The Golden Rule for Scenario Questions:**
> Never just answer *what* you'd do — always explain *why*, and always show that you thought about the people involved, the tradeoffs, and what you'd communicate to whom.

> **Structure for Scenario Answers (use this every time):**
> 1. **Clarify** — What do I need to understand first?
> 2. **Assess** — What's the real problem and its impact?
> 3. **Act** — What concrete steps do I take?
> 4. **Communicate** — Who do I tell, what do I say, when?
> 5. **Follow up** — What do I monitor afterward?

---

## TABLE OF CONTENTS

1. [Sprint & Delivery Crisis Scenarios](#part-1-sprint--delivery-crisis-scenarios)
2. [Stakeholder & Conflict Scenarios](#part-2-stakeholder--conflict-scenarios)
3. [Risk, Blocker & Escalation Scenarios](#part-3-risk-blocker--escalation-scenarios)
4. [Scope & Requirements Scenarios](#part-4-scope--requirements-scenarios)
5. [Team & Resource Scenarios](#part-5-team--resource-scenarios)
6. [AI Product-Specific PM Scenarios](#part-6-ai-product-specific-pm-scenarios)
7. [Tool & Process Scenarios](#part-7-tool--process-scenarios)
8. [Budget & Resource Allocation Scenarios](#part-8-budget--resource-allocation-scenarios)
9. [Communication & Documentation Scenarios](#part-9-communication--documentation-scenarios)
10. [Intern-Specific Situational Scenarios](#part-10-intern-specific-situational-scenarios)
11. [Live Case Study Walkthroughs](#part-11-live-case-study-walkthroughs)

---

## PART 1: Sprint & Delivery Crisis Scenarios

---

### SCENARIO 1: The Mid-Sprint Collapse

**Interviewer:** "You're a PM intern supporting a 2-week sprint. It's Day 8 of 10, and the team has completed only 4 of 12 planned story points. The sprint goal was to deliver a working user authentication module. Your PM is traveling and is only reachable for 30 minutes tonight. What do you do?"

---

**Step 1 — Clarify what I need to understand first:**
Before doing anything, I'd spend 15 minutes getting a complete picture. I'd check the Jira board: which stories are Done, which are In Progress, which haven't started? For every story In Progress or blocked, I'd quickly ping the owner in Slack: "Hey, quick check-in — can you give me a 2-line status on [story] and whether you expect to complete it by EOD Day 10?"

**Step 2 — Assess the real situation:**
Eight story points remain in 2 days. The critical question is: are those 8 points genuinely unbuildable in 2 days (a capacity/blocker problem), or are items mostly done but not marked complete (a Jira hygiene problem)? These are very different situations.

I'd also assess: Is the sprint goal — working authentication — achievable even if we don't finish all 12 points? Sometimes 8 of the 12 points deliver the core goal; the remaining 4 are enhancements. In that case, the sprint goal might still be met even if velocity is low.

**Step 3 — Act immediately on what I can:**
I'd identify and remove every blocker I have authority to remove. Is someone waiting for a design asset? I'd follow up with the designer directly. Is someone waiting for a PR review that's been sitting for 2 days? I'd tag the reviewer urgently in Slack: "This is blocking sprint completion — can you prioritize this review today?"

I'd prepare a concise sprint status snapshot: stories completed, stories in progress with realistic completion likelihood, stories at risk of not completing, and impact on the sprint goal. I'd have this ready before my 30-minute call with the PM.

**Step 4 — Communicate with the PM:**
In my 30-minute window, I'd present the situation in 5 minutes: "Sprint 5 is at significant risk. We've completed 4/12 points. Here's the breakdown: 3 stories are actively in progress and likely to complete — that gets us to ~10 points. 2 stories are blocked by [specific blocker]. I've already escalated [blocker A]. The sprint goal — working auth — is achievable if we complete the 3 in-progress stories. I recommend [Option A: accept partial completion and carry over 2 stories] or [Option B: descope the 2 enhancement stories now and protect the core goal]. Which do you prefer?"

I give the PM the information and options — I don't make the final call on sprint scope.

**Step 5 — Follow up:**
After the call, I'd communicate the decision to the team clearly: which stories are in scope, which are being carried over, and why. I'd update Jira immediately. I'd prepare the sprint review agenda to reflect the actual output honestly — not spin it.

**What I'd avoid:** Panicking, hiding the situation from the PM until they return, or unilaterally changing sprint scope without authorization.

---

### SCENARIO 2: Three Days Before Launch — Critical Bug Found

**Interviewer:** "It's 3 days before a product launch. QA just found a critical bug — the payment processing flow crashes for 15% of users on Android. The engineering team says it will take 4–5 days to fix properly. Launch is Monday. What do you do?"

---

**My immediate response:**

First, I'd gather complete information before making any recommendation. I'd ask QA and engineering three questions:
1. Is there a workaround — even a partial one — that reduces the crash rate?
2. Is the crash data-safe, or could it cause data corruption or financial transactions to partially process?
3. Is there a quick patch that addresses the Android-specific issue, even if imperfect?

**Assess the options — there are four:**

**Option A — Delay the launch by 5 days.** Most conservative. Launch with a fully fixed product. Costs: marketing, announcements, team morale, stakeholder trust in delivery timelines.

**Option B — Launch without Android payment support.** Temporarily disable the payment flow on Android only (show a 'Coming soon' or 'Use web browser' message). Revenue impact for Android users, but no crashes. Transparent about the limitation.

**Option C — Launch to a subset (soft launch).** Release to a limited user group — say, 10% of users or iOS only — monitor carefully, fix the bug, then expand.

**Option D — Launch with the bug and aggressive monitoring.** Only if the bug is non-critical from a data/financial safety perspective and the crash rate is acceptable. Not recommended for a payment flow.

**Communicate with stakeholders:**
I'd bring this to the PM and relevant decision-makers immediately with the four options structured above, each with its trade-offs clearly stated. I'd not make the final call — but I'd have a clear recommendation: Option B (disable Android payments temporarily) or Option A (delay by 5 days), depending on business priorities.

I'd also ensure Marketing is aware immediately — if there are pre-scheduled launch announcements, they need to know in case the date changes.

**What demonstrates PM maturity here:** I don't catastrophize, I don't hide the problem, I don't assume the answer. I gather facts, structure the trade-offs, and bring decision-makers what they need to decide quickly.

---

### SCENARIO 3: The Overcommitted Sprint

**Interviewer:** "During sprint planning, you notice the team has committed to 45 story points when their average velocity is 28. The PM isn't in the room. What do you do?"

---

**My response:**

I'd raise it — calmly, factually, and immediately — rather than letting a bad sprint plan get locked in.

I'd say something like: "Before we finalize this sprint, I want to flag something. Based on our last four sprints, our average velocity is 28 points. We're currently committing to 45. That's 60% above our typical throughput. I want to make sure we're either accounting for why this sprint is different, or we're consciously choosing to over-commit with a plan for what we'll cut if needed."

This is not me overriding the team — it's me doing my job as the person tracking sprint health. It opens a conversation.

**Possible responses from the team and how I'd handle them:**
- "We have an extra person this sprint" → I'd ask: how much does that increase capacity, specifically? 28 + X extra points?
- "We'll just work harder" → I'd note this gently: sustained overcommitment leads to burnout and technical debt. Agile's sustainable pace principle exists for a reason.
- "We know some of these stories will carry over, we're just being optimistic" → This is the most problematic answer. A sprint commitment is a commitment, not a wish list.

**What I'd do after raising it:**
If the team still decides to commit to 45 points, I'd document it: "Sprint 6 committed to 45 points against a velocity of 28. Team is aware of the risk." This creates a record and sets up an honest retrospective conversation if/when things go wrong.

I'd also prepare my status report to flag this sprint as 🟡 Amber from Day 1 — not because the team will definitely fail, but because the risk is already visible and stakeholders deserve to know.

---

### SCENARIO 4: Stakeholder Requests a Demo of Unfinished Work

**Interviewer:** "The VP of Product wants to see a live demo of a feature on Friday. The feature is only 60% done and has known bugs. The engineering team says it's not demo-ready. How do you handle this?"

---

**My response:**

This is a communication and expectation-management scenario, not a technical one.

**Step 1 — Understand the VP's intent:**
Why does the VP want the demo? Is it to validate direction before the team continues? To report to external stakeholders? To make a go/no-go decision? The reason shapes the options.

**Step 2 — Be honest with the VP, proactively:**
I'd communicate clearly and ahead of time — not on Friday morning. Something like: "The [feature] is currently 60% complete and has known issues that would surface in a live demo. The engineering team believes a live demo this Friday risks misrepresenting the current state. I'd like to suggest one of two alternatives: [A] a walkthrough of the completed screens with a clear note on what's still in progress, or [B] moving the demo to [specific date] when it will be ready. Which would be more useful for your purposes?"

**Step 3 — Never ask the team to rush a demo for cosmetic reasons:**
Engineering teams lose trust in PM when they're asked to fake quality for appearances. I'd advocate internally for honesty over optics.

**Step 4 — If the VP insists on Friday:**
I'd work with the team to identify the subset of the feature that IS working and demo only that — with clear framing to the VP: "We're showing you the completed portions — authentication and user profile — which are stable. The dashboard is still under development and will be ready by [date]."

**The key principle:** Never let a stakeholder demo shape what goes into the sprint retroactively. The timeline drives the demo, not the other way around — unless it's a genuine priority re-ordering that goes through proper channels.

---

### SCENARIO 5: Sprint Velocity Suddenly Drops

**Interviewer:** "Your team's sprint velocity has dropped from 32 points per sprint to 18 for two consecutive sprints. Your PM asks you to investigate. What do you do?"

---

**My investigation approach:**

A velocity drop has several possible root causes, and I'd investigate systematically rather than assuming.

**Categories to investigate:**

**1. Capacity change:** Were team members absent? On leave? Pulled into another project? I'd check: who was in both low-velocity sprints versus the higher-velocity sprints? If someone critical was absent or only at 50% capacity, the math might simply explain the drop.

**2. Story quality/estimation issues:** Were the committed stories significantly larger than estimated? Did they uncover hidden complexity? I'd look at the sprint retrospective notes — were stories consistently "larger than expected"? This suggests estimation calibration is needed.

**3. Technical debt / increased rework:** Is the team spending more time fixing bugs than building features? A rising bug count mid-sprint absorbs points without showing as completed stories. I'd check the bug count trend across sprints.

**4. External blockers:** Were there consistent blockers — environment issues, API delays, design assets not ready? I'd scan standup notes for recurring "blocked by" mentions.

**5. Process overhead increase:** Were new processes introduced — longer code review cycles, new QA gates, additional meetings? These absorb capacity.

**6. Team dynamics:** Is there interpersonal tension, leadership change, or morale issue? This is harder to quantify but real.

**What I'd bring to the PM:**
After a day of investigation, I'd present findings: "I've identified [X] as the most likely primary cause of the velocity drop, based on [specific data]. There are also contributing factors of [Y and Z]. My recommended actions are: [specific suggestions per root cause]. I'd also recommend we raise this in the next retrospective so the team can weigh in."

I wouldn't present this as finger-pointing — I'd frame it as "here's what the data shows and what we can do about it."

---

## PART 2: Stakeholder & Conflict Scenarios

---

### SCENARIO 6: Two Stakeholders Want Opposite Things

**Interviewer:** "The Head of Marketing wants the product launch on October 1st to coincide with a major industry event. The Head of Engineering says the product won't be ready until October 20th. Both report to the same VP. You're the PM intern supporting this project. What do you do?"

---

**My response:**

This is a priority conflict that needs to be resolved at the right level — not by me, but with my preparation and framing.

**Step 1 — Clarify both positions fully:**
Before bringing this to the VP, I'd have a short conversation with each stakeholder to understand their position fully. What specifically does October 1st unlock for Marketing — is it the event itself, or the announcement? Can they announce without a live product? What specifically is not ready by October 1st for Engineering — is it the full product, or specific features? Is there a subset that could be launched?

**Step 2 — Identify the real options:**
Usually in conflicts like this, the solution is one of:
- **Hard October 1st launch** with reduced scope (engineering focuses on critical features only)
- **Soft launch October 1st** — limited user group or beta — full launch October 20th
- **Announcement October 1st**, product available October 20th — decouple the marketing moment from the product availability
- **October 20th launch** — Marketing finds an alternative moment

**Step 3 — Document and present to the VP:**
I'd prepare a one-page brief for the PM/VP showing: the conflict, each stakeholder's position and rationale, the 4 options above with trade-offs for each, and a recommended option with reasoning. I'd NOT make the final call — this is above my authority and involves organizational priorities.

**Step 4 — After the decision:**
Once the VP decides, I'd ensure both stakeholders are aligned through a written communication, update the project plan accordingly, and monitor for scope or timeline changes that flow from the decision.

**Key behavior:** I don't pick sides. I don't let the conflict fester. I surface it to the right level with structured options as quickly as possible.

---

### SCENARIO 7: A Senior Engineer Ignores Your Follow-Ups

**Interviewer:** "You've followed up three times with a senior engineer about a missing technical specification that's blocking two other teams. They haven't responded to your Slack messages or emails. What do you do?"

---

**My response:**

**First — check my approach:** Have I been clear about the impact and urgency? "Hey, any update on the spec?" is easy to deprioritize. "The design team and QA team are blocked on Sprint 6 stories until this spec is available — we need it by EOD tomorrow to stay on track" is much harder to ignore. I'd review my messages to see if I was this specific.

**Second — change the channel:** If three Slack messages haven't worked, I'd try a different modality. A quick call or walking to their desk (if in-person) often gets a response when async messages don't. Engineers are often heads-down and message-avoidant, not deliberately ignoring me.

**Third — understand the root cause:** When I do reach them, I'd ask why before assuming. Are they blocked themselves? Did they not understand what was needed? Are they overloaded with other P0 work? The answer changes my response.

**Fourth — escalate if still unresolved:** If I genuinely can't reach them and two teams are blocked, I'd bring it to the PM with clear context: "I've followed up 3 times over [timeframe] via Slack and email with no response. Two teams are blocked. The impact is [specific: Sprint 6 may miss its goal]. I'd like your guidance — should you reach out directly, or should we find an alternative way to unblock the dependent teams?"

**What I'd never do:** Complain about the engineer to the PM without having genuinely tried to resolve it myself first, or publicly call them out in a standup in a way that creates tension.

---

### SCENARIO 8: Stakeholder Keeps Changing Requirements

**Interviewer:** "A key business stakeholder has changed the requirements for a feature three times in the last two sprints. Each change has caused rework for the engineering team. How do you handle this?"

---

**My response:**

Recurring requirement changes usually have an underlying cause, and the solution depends on what that cause is.

**Root causes to diagnose:**
- The stakeholder doesn't have a clear vision and is figuring it out through iteration (not unusual in early product stages)
- The stakeholder keeps hearing from end users and incorporating feedback (legitimate but needs process)
- The stakeholder feels like they can change requirements freely because there's no explicit cost made visible to them
- The requirements-gathering process at the start was insufficient

**My approach:**

**Immediate:** I'd request a dedicated session with the stakeholder — not to complain, but to build a clear, documented requirements baseline. "I'd like to spend an hour with you to lock down the core requirements for this feature before we continue development. I want to make sure we build exactly what you need."

**Make the cost visible:** Respectfully, not accusatorially, I'd show what changes cost: "Each time we change the requirements mid-sprint, it typically costs the engineering team 2–3 days of rework, which delays the feature by a sprint. I want us to make sure changes are worth that cost before we action them."

**Implement a change request process:** Any requirement change after sprint planning should go through a written change request: what's changing, why, what's the estimated impact on timeline, who approved it. This creates a deliberate moment before changes take effect — it doesn't block change, but it makes the decision conscious.

**Distinguish urgent vs. non-urgent changes:** Some changes are genuinely important and should be made immediately. Others are preferences that can be deferred to v2. Help the stakeholder distinguish between "this changes what the feature does fundamentally" vs. "this would be nicer but isn't blocking."

**Retrospective agenda item:** I'd raise this pattern (anonymized/constructively) in the retrospective as a process improvement: "We've seen mid-sprint requirement changes impact velocity in the last two sprints — how should we handle these going forward?"

---

## PART 3: Risk, Blocker & Escalation Scenarios

---

### SCENARIO 9: A Critical Dependency Is Delayed by an External Vendor

**Interviewer:** "Your project depends on a third-party API from an external vendor. They've just told you the API won't be ready for another 3 weeks, but your sprint assumes it will be available next Monday. Two engineers are assigned to work that requires this API. What do you do?"

---

**My response:**

This is a dependency risk that's just materialized into an issue. My response is immediate and parallel-tracked.

**Track 1 — Mitigate the impact on the engineers:**
Two engineers are about to be blocked. I'd work with the tech lead to identify: (1) Can we build a mock API internally that mimics the expected behavior, so frontend/integration work can continue? Mock APIs are standard practice exactly for this situation — the interface (endpoints, request/response format) can often be agreed upon even before the real implementation is ready. (2) Are there other backlog items these engineers could work on productively while the dependency resolves?

I'd bring both options to the tech lead and get a decision within the day.

**Track 2 — Validate and escalate the vendor delay:**
I'd confirm the delay in writing with the vendor — "Can you confirm the new availability date in writing and the reason for the delay?" Verbal updates can be misunderstood. I'd also understand if the 3-week delay is firm or could be shortened with escalation.

**Track 3 — Update the project plan:**
I'd immediately flag this to the PM with full context: what's delayed, by how long, which sprint stories are affected, what the downstream impact is on milestones, and what mitigation I've already set in motion.

**Track 4 — Stakeholder communication:**
If the vendor delay impacts a milestone or release date, stakeholders need to know proactively — not when the milestone is missed.

**Update the risk register:**
This goes in as a realized risk with the status updated and the mitigation actions documented.

---

### SCENARIO 10: A Team Member Reveals a Blocker They've Had for a Week

**Interviewer:** "In today's standup, a developer mentions that they've been blocked for the past 5 days by a missing design asset. They didn't mention it in any of the previous standups. This blocking issue has caused significant sprint delay. How do you handle this?"

---

**My response:**

There are two things to handle here: the immediate blocker and the underlying process issue.

**Immediate — unblock right now:**
I'd reach out to the design lead directly after standup: "We have a critical block — [developer] needs [specific design asset] to complete [story X]. This has been pending for 5 days and we're at risk of missing the sprint goal. Can you prioritize this today? What do you need from me to make that happen?"

**Understand the root cause of the silence:**
After unblocking, I'd have a private, non-confrontational conversation with the developer: "I'm glad you mentioned the block today. I want to make sure we catch these earlier in the future — was there a reason you held off raising it in standup? I want to understand so we can make it easier to flag blockers." Common reasons: they thought they'd resolve it themselves, they didn't want to seem incompetent, they forgot the standup is for exactly this purpose.

**Address the process in the retrospective:**
This is a standup culture issue — standups exist specifically to surface blockers. I'd add it to the retrospective as a discussion item (anonymized): "We had a 5-day blocker that wasn't surfaced in standup. How do we make sure we're all comfortable raising blocks early? What would make standups feel safer for this?"

**Review my own monitoring:**
As the PM intern, I should have been tracking this story's progress more carefully. If a story hasn't moved for 5 days, that's a signal I should have caught by Day 3 and asked about proactively. I'd commit to a daily scan of stories that haven't moved on the board.

---

### SCENARIO 11: You Discover a Budget Overrun — Your Team Has Spent 80% of the Budget Halfway Through the Project

**Interviewer:** "You're maintaining the project budget tracker and you notice the project has consumed 80% of its budget but is only 50% complete in terms of deliverables. You're a PM intern. What do you do?"

---

**My response:**

This is a serious issue that I need to handle carefully given my seniority level.

**Step 1 — Verify the data before escalating:**
I'd double-check my tracker and the source data. Is the 80% calculation based on actual spend, committed spend, or projected spend? Is the budget category breakdown showing where the overage is concentrated? I don't want to escalate inaccurate numbers.

**Step 2 — Understand what's driving the overrun:**
Before escalating, I'd try to understand the cause. Categories to check: Were there unplanned expenses (additional cloud compute, unexpected vendor fees)? Was original budget based on underestimated scope? Has the team grown? Have there been scope changes that increased resource usage without corresponding budget increases?

**Step 3 — Escalate immediately to the PM with full context:**
This is beyond my authority to resolve, but it's absolutely my responsibility to flag. I'd bring: the current spend vs. budget vs. completion percentage, a breakdown of where the budget has gone, my analysis of the likely cause, and a projected timeline to budget exhaustion at the current burn rate.

I'd frame it: "I'm flagging a significant budget risk. We've consumed 80% of budget at 50% project completion. At current burn rate, we'll exhaust the budget approximately [X weeks] before project completion. I've prepared a breakdown of where the spend is concentrated. I'd like your guidance on how to proceed."

**Step 4 — Prepare options for the PM to consider:**
- Identify categories where spend can be reduced going forward
- Determine if scope can be cut to fit remaining budget
- Assess whether a budget increase request is warranted
- Evaluate if timeline extension reduces per-period costs

**What I'd never do:** Adjust the budget numbers to make them look better, hide the overrun, or try to resolve a budget issue at my seniority level without escalating.

---

## PART 4: Scope & Requirements Scenarios

---

### SCENARIO 12: Scope Creep Request in the Middle of a Sprint

**Interviewer:** "It's Day 6 of a 10-day sprint. A senior stakeholder sends a Slack message: 'Can we add a CSV export feature to the dashboard? It should only take a day or two.' How do you respond?"

---

**My response:**

This is a textbook scope creep scenario, and how I handle it will demonstrate my PM judgment.

**What I don't do:** Say "sure" immediately, or say "absolutely not" dismissively.

**My Slack response (immediate):**
"Thanks for flagging this — CSV export is definitely a useful feature. Let me check the current sprint status and get back to you within the hour with what this would mean for our current commitments."

**What I check:**
- How much capacity is left in the sprint? (Day 6 of 10 — 4 days remaining)
- Is the sprint goal already at risk without this addition?
- How long would CSV export actually take? "A day or two" is a stakeholder estimate — I'd verify with the engineering team

**My follow-up response to the stakeholder (within the hour):**
"Quick update on CSV export: I've checked with the team and here's where we stand. The engineering team estimates 2–3 days for a proper implementation (with error handling and the file format your team would expect). Our current sprint is committed to [X stories] and we have 4 days remaining. Adding CSV export would likely displace [specific story Y] which is also committed.

I'd like to propose two options:
- Option A: We add CSV export to this sprint, and defer [Story Y] to Sprint 7. We'd need your confirmation that this trade-off is acceptable.
- Option B: We add CSV export as the highest-priority item in Sprint 7's backlog, and it would be delivered by [date].

My recommendation is Option B — it protects this sprint's commitments while ensuring CSV export gets prioritized immediately. But happy to discuss if your business need is more urgent. Could we have a quick 10-minute call?"

**Why this response is good:** I'm not blocking the stakeholder — I'm making the trade-off explicit and giving them the choice. I'm not making unilateral sprint scope decisions. I'm being responsive (within an hour) and solution-oriented.

---

### SCENARIO 13: Requirements Are Ambiguous and the Team Has Already Started Building

**Interviewer:** "You join a project mid-stream and discover that the engineering team has been building a feature for 2 weeks based on verbal discussions with the Product Owner. There's no written requirements document. You review the work and it doesn't match what the PO described to you verbally. What do you do?"

---

**My response:**

This is a misalignment risk that needs to be addressed before more work is built on a potentially wrong foundation.

**Step 1 — Get full clarity before raising alarm:**
I'd have individual conversations with both the PO and the lead developer — separately — asking each to describe the feature they're building/expecting. I'd document what each says. Then I'd compare.

**Step 2 — Identify the gap specifically:**
The gap is probably not "everything is wrong" — it's likely specific elements. Maybe the core functionality aligns but the user flow is different, or a specific feature behavior is misunderstood. I'd articulate the gap precisely.

**Step 3 — Facilitate a rapid alignment session:**
I'd schedule a 1-hour meeting with the PO, tech lead, and any designer involved: "I want to make sure we're all building the same thing. I've put together a brief document summarizing the feature as I understand it from each perspective. I'd like 30 minutes to walk through it together and confirm alignment before we go further." I'd come with a written summary of both versions as a discussion anchor.

**Step 4 — Document the agreed requirements immediately after:**
The output of that meeting is a written requirements summary that everyone reviews and confirms — even a one-page document. This becomes the baseline.

**Step 5 — Assess rework impact:**
After alignment, I'd work with the tech lead to understand: how much of the 2 weeks of work is salvageable, and what needs to be rebuilt? This is an honest conversation — and the result goes into a status update to the PM with the rework scope and timeline impact.

**What I'd build going forward:** A requirements documentation process so this doesn't happen again. Even a lightweight one-page brief per feature, reviewed before coding starts, prevents this.

---

### SCENARIO 14: The PM Asks You to Accept a User Story That Doesn't Meet the Definition of Done

**Interviewer:** "In the sprint review, your PM asks you to mark a user story as 'Done' even though QA has flagged two minor bugs that aren't fixed yet. The PM says 'it's close enough and we need to show progress.' What do you do?"

---

**My response:**

This is an integrity and judgment scenario. I'd handle it with honesty and professionalism — not compliance.

**What I'd not do:** Silently mark it Done. The Definition of Done exists specifically to ensure quality consistency. Overriding it sets a precedent that the DoD is optional — which undermines every future sprint review.

**My response to the PM:**
"I hear you on the pressure to show progress, and I want to help us get there. But if we mark this Done with two open QA bugs, we're breaking our Definition of Done — and in the sprint review, stakeholders will see it as complete when it isn't. That's a trust risk.

Can I suggest an alternative? We demo the story in the review as 'feature complete with two minor defects currently being tracked in Jira — expected fix in Sprint 7.' That's transparent, still shows the feature, and doesn't compromise our quality standard. The two bugs go into the sprint 7 backlog as P1 items. Does that work?"

**If the PM insists:**
I'd note it formally — updating the story with a comment: "Marked Done per PM direction on [date] with two open QA bugs [Bug IDs]. These are tracked in Sprint 7 backlog." This creates a paper trail without being defiant.

**The bigger picture:** If this happens repeatedly, it's a systemic quality process issue that should be raised in a retrospective or one-on-one with the PM: "I've noticed we've been marking stories Done before they fully meet our DoD. I'm concerned about the quality debt this might be creating. Can we talk about how we want to handle this going forward?"

---

## PART 5: Team & Resource Scenarios

---

### SCENARIO 15: A Key Team Member Suddenly Leaves Mid-Project

**Interviewer:** "A senior backend engineer who owns 40% of the remaining sprint work puts in their resignation today. They'll be gone in 2 weeks. The project has 6 weeks of work remaining. What do you do?"

---

**My response:**

This is a resource risk that requires immediate action on multiple tracks.

**Track 1 — Knowledge transfer (urgent):**
The most critical thing is to capture the departing engineer's knowledge before they leave. Within the next 2–3 days, I'd work with the tech lead to identify: What code or components only they understand? What architectural decisions did they make that aren't documented? I'd schedule knowledge transfer sessions immediately and ensure documentation is created for every undocumented area they own.

**Track 2 — Redistribute in-flight work:**
I'd work with the tech lead to assess the engineer's remaining assigned stories. Which are in progress and need handoff? Which haven't started and can be redistributed? I'd facilitate this with the tech lead — I don't assign engineering work myself, but I'd help structure the conversation and ensure it happens quickly.

**Track 3 — Revise the project plan:**
With the PM, I'd assess the honest impact. If this engineer owned 40% of remaining work and the team can only absorb 20% of it through redistribution, there's a 20% gap. Options: extend the timeline, reduce scope, bring in a contractor, prioritize brutally. I'd prepare this analysis for the PM.

**Track 4 — Stakeholder communication:**
If this impacts the delivery date or scope, stakeholders need to know promptly. I'd draft the stakeholder communication for the PM to review and send.

**Track 5 — Transition planning:**
Over the 2 remaining weeks, I'd help structure the departing engineer's time: what percentage on knowledge transfer vs. completing their own work? This is a PM coordination task.

---

### SCENARIO 16: Two Team Members Are in Open Conflict

**Interviewer:** "Two developers on your team have been having increasingly hostile exchanges in Slack — publicly. It's affecting the team atmosphere and people are starting to choose sides. You're the PM intern. What do you do?"

---

**My response:**

As an intern, I need to be careful about the scope of my role here — conflict resolution at a team level is primarily the PM's or manager's responsibility. But I also can't ignore it.

**What I can do immediately:**
I'd speak to the PM or team manager privately: "I've noticed there have been increasingly tense exchanges between [Name A] and [Name B] in the team Slack. It seems to be affecting team morale and I wanted to flag it so you're aware." I'd share the specific instances with context — not as gossip, but as information needed to address it.

**What I wouldn't do:**
Take sides, wade into the public Slack conflict myself, or try to mediate a conflict between two senior developers as an intern — that's not my authority and would likely make it worse.

**In terms of the meeting environment:**
I'd ensure that standups and sprint ceremonies are structured enough that personal tension doesn't derail them. Clear agendas, time-boxing, and factual focus reduce the opportunity for interpersonal friction to escalate in meetings.

**If asked by the PM to help:**
If the PM asks me to facilitate a process conversation (not a personal one), I'd do so with care — focusing strictly on project and process topics, keeping it professional, and documenting outcomes.

---

## PART 6: AI Product-Specific PM Scenarios

> These are scenarios you're uniquely likely to face in this role. Your Gagangiri experience makes you credible here — use it.

---

### SCENARIO 17: The AI Model Isn't Hitting Its Accuracy Target

**Interviewer:** "You're PM intern on an AI product project. The ML team committed to hitting 85% model accuracy by end of Sprint 4. It's now end of Sprint 5 and accuracy is at 71%. The product launch depends on this. Stakeholders are asking for updates. What do you do?"

---

**My response:**

I have direct experience with this situation from Gagangiri Foods, and it taught me that AI model development is fundamentally different from traditional software delivery in one critical way: the outcome is uncertain, iterative, and not fully within the team's control. A feature can be built to spec; a model's accuracy depends on data, architecture choices, and experimentation.

**Step 1 — Understand why, specifically:**
I'd work with the ML team to understand the root cause of the accuracy gap. Is it:
- Insufficient or poor-quality training data? (Data problem)
- Model architecture not suited to the problem? (Technical approach problem)
- Evaluation methodology issues — are we testing on the right dataset? (Measurement problem)
- The 85% target was incorrectly estimated at the start? (Expectation problem)

The answer changes everything.

**Step 2 — Get a realistic path to target:**
Once I understand the cause, I'd ask the ML team: what would it take to reach 85%? More data? Different model? More iterations? How long? I need a credible forecast — not "we'll get there," but "with 500 more labeled samples and two more training iterations, we estimate reaching 82–85% within 3 weeks."

**Step 3 — Communicate honestly with stakeholders:**
I'd bring this to the PM first, then support the PM in communicating to stakeholders. The framing would be honest and forward-looking: "The model is currently at 71% accuracy against our 85% target. The team has identified [specific cause] as the primary factor. The plan is [specific action], which we estimate will get us to target by [date]. This shifts our launch timeline by approximately [X weeks]. I want to discuss the options for how we handle this."

**Step 4 — Revisit the target itself:**
In some cases, 71% may actually be acceptable for a first version if the use case allows for it. I'd bring this as a question to the product owner: "Is 85% a hard requirement, or is it a target? If the product can launch at 75% with a clear improvement roadmap, is that worth considering?" This is a legitimate PM question — not giving up, but ensuring the target is calibrated to real business need.

**What I never do:** Promise accuracy targets to stakeholders that the ML team hasn't committed to. AI timelines are uncertain — overpromising destroys credibility.

---

### SCENARIO 18: Training Data Is Delayed and Blocks the Entire AI Pipeline

**Interviewer:** "Your AI project's model training depends on labeled data from a third-party annotation vendor. The vendor has delivered 30% of the labeled dataset with quality issues — mislabeled samples. Your ML engineer says they can't train a reliable model with this data. The vendor says they need 4 more weeks. Your sprint plan assumed full data by end of last week. What do you do?"

---

**My response:**

Data dependency issues are the most common AI project risk, and I've managed a version of this at Gagangiri. My response:

**Track 1 — Assess the 30% available data:**
I'd work with the ML engineer to understand: Is any subset of the available data usable? Even if 30% arrived with quality issues, maybe 15% is good enough to begin preliminary model training — not production training, but enough to validate the architecture and pipeline. This "train on what we have" approach prevents complete team idleness.

**Track 2 — Address the quality issue directly:**
I'd have a direct conversation with the vendor: What specifically is wrong with the labeling? Can we provide clearer labeling guidelines to prevent the same issues with the remaining 70%? Can we reject the mislabeled samples and get them relabeled as priority? Getting a quality fix process in place now prevents receiving more bad data in 4 weeks.

**Track 3 — Build a mock/synthetic data plan:**
Depending on the problem domain, the team could generate synthetic training data or use transfer learning from a related public dataset to keep model development progressing. I'd ask the ML team: "What can we realistically build while waiting for the full dataset?"

**Track 4 — Revise the project plan and escalate:**
This is a critical path delay. I'd update the project plan immediately with the revised timeline (4 weeks + QA of data when delivered) and escalate to the PM with impact assessment. If the launch date is fixed, we'd need to discuss scope trade-offs or whether a simpler model approach might reach an acceptable accuracy threshold sooner.

**Track 5 — Contractual/vendor escalation:**
If the vendor has a quality SLA, the PM should know about it — this may be a contract compliance issue. I'd surface this to the PM with the documentation of what was committed vs. delivered.

---

### SCENARIO 19: The AI Feature Produces Inconsistent Outputs in Production

**Interviewer:** "Your team shipped an AI-powered recommendation feature last week. Users are now reporting that the recommendations seem random and inconsistent — what they see changes every time they reload the page. Engineering says the model is behaving as designed. Stakeholders want this fixed immediately. What do you do?"

---

**My response:**

This is a production incident that involves both a technical issue and a stakeholder management challenge.

**Step 1 — Understand what "working as designed" means:**
I'd get clarity from engineering: why does the model produce different recommendations on reload? Is this:
- Expected probabilistic behavior (the model has randomness built in as an exploration strategy)?
- A caching/serving issue (the model is being called with different inputs each time)?
- A feature engineering bug (user context isn't being passed correctly)?

The phrase "working as designed" might be technically accurate but user-experience wrong. A recommendation feature that shows different results every second is not useful, even if the model technically works.

**Step 2 — Is this a bug or a product decision?**
This becomes a product question: should recommendations be stable between reloads? For most recommendation UX, yes — users expect some stability. If the design didn't account for this behavior explicitly, it's a product gap, not a pure engineering bug.

**Step 3 — Immediate action for users:**
Is there a quick fix? A caching layer that stabilizes recommendations for a session (e.g., recommendations only refresh every 4 hours) might solve the user experience issue without changing the model. I'd ask engineering if this is a quick implementation.

**Step 4 — Stakeholder communication:**
I'd communicate to stakeholders: "We've identified the cause of the inconsistent recommendations — [explanation in plain language]. The engineering team is implementing a [fix description] which will stabilize the experience. Expected resolution: [timeframe]. We're monitoring user feedback in the meantime."

**Step 5 — Retrospective item:**
How did this behavior not get caught in QA? Was testing done in a way that would surface random/inconsistent outputs? This is a testing process improvement.

---

### SCENARIO 20: Stakeholders Want to Add More AI Features Before the Model Is Proven

**Interviewer:** "Your product team is excited about AI and wants to add 5 new AI features to the next sprint — anomaly detection, natural language search, predictive analytics, personalization, and an AI chatbot. Your ML team currently has 1 ML engineer and is still validating the core model. What do you do?"

---

**My response:**

This is an enthusiasm management and prioritization scenario — the business is excited about AI, which is great, but needs to be channeled productively.

**Step 1 — Validate the capacity reality:**
One ML engineer cannot build 5 AI features in one sprint. Period. I'd map out what each feature actually requires: data, model training, evaluation, integration, QA. A realistic estimate for each would show that each feature is a multi-sprint effort.

**Step 2 — Prioritize against business value:**
I'd facilitate or support a MoSCoW prioritization conversation with the product team: Which of these 5 features, if it works well, would have the most impact on users and the business? Which is most technically feasible given the current model and data? Which has the clearest success metric?

**Step 3 — Sequence, don't reject:**
I'd reframe the conversation: "We can build all 5 of these over the next few months. Let's sequence them so each one is done well rather than all 5 being done partially. My recommendation is [Priority 1: feature X] because [reason]. Once that's shipped and we've validated the ML pipeline, we can accelerate the next one."

**Step 4 — Protect the core model validation:**
I'd advocate for completing the core model validation first: "Before we add new AI features, we should ensure our foundation is solid — the current model needs to be production-validated. Building more AI on an unvalidated foundation creates compounding risk. Let's complete validation in this sprint and start Feature X in Sprint 7."

This kind of reasoned, respectful pushback on scope is exactly what a PM intern should be able to do.

---

## PART 7: Tool & Process Scenarios

---

### SCENARIO 21: Jira Is a Mess — Nobody Is Updating Tickets

**Interviewer:** "You've just joined a team as a PM intern. You look at the Jira board and it's completely out of date — stories are in 'To Do' that have been completed, no story points are assigned, and there are 200 backlog items with no priority. How do you approach fixing this?"

---

**My response:**

This is a common real-world situation and fixing it requires a mix of immediate cleanup and cultural change.

**Phase 1 — Immediate triage (first week):**
I wouldn't try to fix everything at once. I'd focus on the active sprint first: sync with each team member to understand the real status of every in-progress story. Update Jira to reflect reality. At the end of day 1, the sprint board should accurately reflect what's actually happening.

**Phase 2 — Backlog cleanup (first month):**
200 unprioritized backlog items is a "backlog cemetery." I'd work with the Product Owner to do a backlog pruning session: For each item, ask "Is this still relevant? Do we plan to work on this in the next 3 months?" Items that are old, irrelevant, or superseded get archived (not deleted — you might need them for reference). Survivors get priority assigned.

**Phase 3 — Build the habit of keeping Jira updated:**
Rules are only useful if people follow them. I'd identify why Jira isn't being updated — is it friction (too many fields to fill in), habit (nobody did it before), or time pressure (people don't see value in it)? I'd address the root cause: simplify the required fields, make Jira part of the daily standup routine ("open the board together"), and make the benefit tangible (show how the burndown chart works when Jira is accurate).

**Phase 4 — Set standards and be consistent:**
I'd introduce a simple rule: "Before each standup, each person updates their story status in Jira." And I'd model it myself — visible, consistent behavior from the PM team matters.

---

### SCENARIO 22: The Team Is Using Three Different Tools and Nothing Is Synced

**Interviewer:** "You discover the engineering team tracks work in Jira, the design team uses Trello, and the PM uses a spreadsheet. None of them are in sync. This has led to missed dependencies and duplicated work. What do you recommend?"

---

**My response:**

Tool fragmentation is a real coordination problem, but the solution isn't "force everyone onto one tool immediately" — that creates resistance and disruption.

**Short term (this week):**
Identify the minimum information that needs to be shared across teams. Typically this is: what's being worked on, when it's due, and what depends on what. I'd create a simple master coordination view — even a shared Google Sheet — that each tool feeds into weekly. Not perfect, but functional.

**Medium term (next month):**
Facilitate a conversation with leads from each team: "We have three tools producing three different pictures of the same project. This is causing [specific problems — cite real examples of missed dependencies or duplicated work]. I'd like us to align on how we coordinate. The options are: consolidate to one tool, establish a sync process between tools, or define clear ownership boundaries so each tool has a distinct purpose."

**The consolidation conversation:**
If consolidating to one tool, Jira is the natural choice for engineering-led teams — it has the most robust Agile features. Design teams often resist because Jira isn't design-friendly — but Jira can link to Figma; design work can be tracked as Jira stories without the designers using Jira daily.

**What I wouldn't do:** Unilaterally migrate everyone to one tool without buy-in. Tool migrations are disruptive and will fail without team support.

---

## PART 8: Budget & Resource Allocation Scenarios

---

### SCENARIO 23: You're Asked to Track the Project Budget — Where Do You Start?

**Interviewer:** "Your PM asks you to set up and maintain the project budget tracker. You've never done this before. What do you do and what does the tracker look like?"

---

**My response:**

**Step 1 — Understand the budget scope:**
I'd ask the PM: What categories of spend does this budget cover? Common categories in a software project include: personnel (salaries/contractor fees, often tracked as hours × rate), software licenses/tools, cloud infrastructure costs (AWS/GCP/Azure spend), third-party vendors/APIs, training data costs (very relevant for AI projects), and miscellaneous (travel, equipment).

**Step 2 — Get the baseline:**
What is the total approved budget, broken down by category? What has already been committed or spent? This is my starting point.

**Step 3 — Build the tracker:**
I'd create a budget tracker in Google Sheets or Excel with the following structure:

```
Budget Category | Approved Budget | Committed | Actual Spend | Remaining | % Used | Status
Personnel       | $50,000        | $48,000   | $32,000      | $18,000   | 64%    | 🟢
Cloud Infra     | $8,000         | $6,500    | $4,200       | $3,800    | 52.5%  | 🟢
Vendor API      | $5,000         | $5,000    | $3,500       | $1,500    | 70%    | 🟡
Training Data   | $10,000        | $12,000   | $9,000       | -$2,000   | 120%   | 🔴
Total           | $73,000        | $71,500   | $48,700      | $21,300   | 66.7%  | 🟡
```

Key columns:
- **Approved Budget:** What was sanctioned
- **Committed:** What is contracted/obligated (whether invoiced yet or not)
- **Actual Spend:** What has been invoiced and paid
- **Remaining:** Approved minus Committed (not Actual — committed money is effectively spent)
- **% Used:** Committed / Approved
- **Status:** RAG — Red if over budget, Amber if >80% committed, Green otherwise

**Step 4 — Establish update cadence:**
Budget tracking is only useful if it's current. I'd establish a weekly update process — pulling actuals from the finance/accounting system and updating committed amounts as new contracts are signed.

**Step 5 — Flag anomalies immediately:**
Any category approaching 80% of budget before 80% of project completion is an amber flag. Any category over 100% is a red flag requiring immediate escalation.

---

### SCENARIO 24: Resource Crunch — Two Critical Tasks Need the Same Person

**Interviewer:** "You're tracking resource allocation and discover that your only ML engineer is assigned to both model retraining (a sprint commitment) and a data pipeline bug fix (a P1 issue) simultaneously. Both are due this week. What do you do?"

---

**My response:**

This is an over-allocation issue that needs to be resolved quickly and explicitly — not silently absorbed by the engineer.

**Step 1 — Clarify priorities:**
I'd bring this to the PM: "I've noticed [ML engineer] is assigned to both [retraining task] and [bug fix] this week, and both are listed as high priority. I want to make sure we're explicit about which one takes precedence, so they're not splitting their focus and under-delivering on both."

**Step 2 — Assess each task's urgency and impact:**
- The P1 bug fix: what's the production impact if not fixed this week? Is it user-facing? Financial? 
- The model retraining: what's blocked if this doesn't happen this week? Does it affect a milestone?

Usually a P1 bug fix takes precedence over planned sprint work — but this needs to be an explicit decision, not an assumption.

**Step 3 — Is there an alternative for either task?**
Can the bug fix be handled by another engineer (even if slower)? Can the retraining be deferred one sprint without critical impact? Identifying alternatives gives the PM options.

**Step 4 — Update the sprint plan to reflect reality:**
If the ML engineer is now doing the bug fix, their sprint stories need to be reassigned or deferred. I'd update Jira to reflect the actual plan and communicate the change to relevant stakeholders.

**What I avoid:** Letting the engineer quietly carry both tasks and then fail to deliver either well. Making the trade-off explicit is better for everyone.

---

## PART 9: Communication & Documentation Scenarios

---

### SCENARIO 25: You Need to Write a Status Report With Bad News

**Interviewer:** "The project is significantly behind schedule — the beta launch has slipped by 3 weeks. Write the key points of the status report you'd send to leadership."

---

**Model Status Report Content:**

```
PROJECT STATUS REPORT
Project: [AI Product Name]
Date: [Date]
Reporting Period: [Sprint X / Week Y]
Overall Status: 🔴 RED

EXECUTIVE SUMMARY
The beta launch has been rescheduled from [original date] to [new date] — 
a 3-week delay. The delay is primarily attributed to [specific cause: 
e.g., QA environment downtime (10 days) and scope addition of the 
authentication module in Sprint 4]. Actions are in progress to prevent 
further slippage.

MILESTONE STATUS
| Milestone          | Original Date | Revised Date | Status       |
|--------------------|---------------|--------------|--------------|
| Feature Complete   | May 15        | May 22       | 🟡 Delayed  |
| QA Sign-Off        | May 22        | June 1       | 🔴 Delayed  |
| Beta Launch        | May 30        | June 20      | 🔴 Delayed  |
| Production Launch  | June 30       | July 21      | 🟡 At Risk  |

ROOT CAUSE OF DELAY
1. QA environment was unavailable for 10 days (April 28 – May 8) — 
   blocked testing of payment and auth modules
2. Auth module scope was expanded in Sprint 4 (+5 story points, 
   not accounted for in original plan)

CURRENT STATUS
- Completed: User onboarding, dashboard, API integration (100%)
- In Progress: Payment flow (75%), Analytics module (40%)
- Not Started: Admin panel, export features

RISK & ISSUES
🔴 [Issue] Production launch is now at risk if any additional delays 
occur. We have zero float remaining on the critical path.
  → Action: Scope review scheduled for [date] to identify features 
    that can be deferred to v1.1

ACTIONS REQUIRED FROM LEADERSHIP
• Decision needed by [date]: Should we defer admin panel to v1.1 
  to protect the June 20 beta date?
• Confirmation needed: Is the revised July 21 production launch 
  acceptable to the business?

NEXT STEPS
- Complete payment flow and analytics module (Sprint 8)
- Conduct scope review to protect revised dates (June 3)
- Stakeholder communication plan for revised dates (June 4)
```

**Key principles demonstrated:**
- Lead with the overall status clearly labeled
- State the bad news directly — don't bury it
- Explain the root cause — leadership deserves to understand why
- Show what's being done about it — not just the problem
- Make any required decisions or inputs explicit and time-bound

---

### SCENARIO 26: You're Asked to Prepare the Agenda for a Sprint Retrospective

**Interviewer:** "Your Scrum Master is out sick and asks you to prepare and facilitate tomorrow's sprint retrospective. You've never run one before. How do you prepare?"

---

**My response and the agenda I'd create:**

**Preparation:**
I'd review: What stories were committed vs. completed? Were there any major blockers or incidents? What came up repeatedly in standups? I'd also inform the team in advance and ask them to come with thoughts on what went well and what didn't.

**Retrospective Agenda (60 minutes):**

```
SPRINT [X] RETROSPECTIVE
Facilitator: [Your name]
Date/Time: [Details]
Format: Start/Stop/Continue

[0:00–0:05] OPENING (5 min)
- Quick reminder: Retrospective norms — safe space, no blame, 
  focus on process not people, what's said here stays here
- Sprint summary: committed X points, completed Y points

[0:05–0:20] INDIVIDUAL REFLECTION (15 min)
- Each person writes sticky notes (in Miro/Notion/physical):
  - GREEN: What went well this sprint? (Keep/Continue)
  - RED: What didn't go well? (Stop/Reduce)  
  - BLUE: What should we try next sprint? (Start/Improve)
- Silent writing — no discussion yet

[0:20–0:35] SHARE & CLUSTER (15 min)
- Each person shares their notes briefly
- I group similar themes together visually
- No debate yet — just sharing and organizing

[0:35–0:50] DISCUSS TOP THEMES (15 min)
- Vote on top 2–3 themes to discuss (dot voting)
- Structured discussion on each: What's the root cause? 
  What can we actually change?
- No more than 5 minutes per theme

[0:50–0:58] ACTION ITEMS (8 min)
- For each agreed improvement: What specifically will we do? 
  Who owns it? By when? 
- These go into the next sprint as process improvement tasks

[0:58–1:00] CLOSE (2 min)
- Thank the team
- Confirm action items are documented
- Announce where minutes will be shared (within 24 hours)
```

**Post-meeting:**
Send written minutes with action items to all attendees. Track action items in the sprint backlog or coordination tracker. Review status at next retrospective.

---

## PART 10: Intern-Specific Situational Scenarios

---

### SCENARIO 27: Your PM Disagrees With Something You've Flagged

**Interviewer:** "You've flagged a risk that you believe is significant — a vendor delay that could impact the launch date. Your PM dismisses it and says 'it'll be fine, the vendor always comes through.' Two weeks later, the vendor delays and the launch slips. How do you handle this conversation with your PM?"

---

**My response:**

This scenario is really asking: Can you handle being right when someone senior was wrong, without being arrogant or damaging the relationship?

**In the moment — two weeks later:**
I would not say "I told you so" — ever. That's not constructive and it damages the relationship.

I'd approach the PM with empathy and focus on solutions: "The vendor delay has materialized as I mentioned a few weeks ago. I know we hoped it would resolve itself. Let's focus on what we do now — I've already mapped out the impact and have three options for how we might recover. Can we review them?"

**In the longer conversation:**
If the relationship allows it, I might gently raise the pattern as a process learning: "One thing I'm reflecting on from this project — when I flagged the vendor risk, I don't think I made the impact case clearly enough for it to feel urgent. I want to learn how to escalate risks more effectively. Would it be useful to establish a threshold for what qualifies as a 'flag to the PM' versus something I can monitor myself?"

This frames it as my learning, not their failure — while gently establishing that risk flagging should be taken seriously.

**The principle:** Being right is not the goal. The goal is to improve the process so risks are caught and acted on earlier next time. Mature PM interns know this.

---

### SCENARIO 28: You're Overwhelmed and Can't Meet All Your Commitments

**Interviewer:** "It's Thursday. You have a status report due Friday, sprint retrospective notes to send, three follow-up action items from this week's meetings, and a new request from a stakeholder to analyze a project risk. You can realistically complete two of these by Friday. What do you do?"

---

**My response:**

**Step 1 — Prioritize ruthlessly by impact:**

- **Status report:** External-facing, stakeholders expect it Friday. Missing it without communication is a trust issue. → High priority.
- **Retrospective notes:** Internal, team needs them but a 1-day delay won't break anything. → Medium priority — can be sent Saturday morning.
- **Three follow-up action items:** Are any of them blocking someone else? If yes, those come before the status report. If no, they can be completed early next week with a heads-up. → Medium/low depending on dependencies.
- **Stakeholder risk analysis:** New, and likely less urgent than it feels. I'd check: "When do you need this by?" Often the answer is "by next week" — not today.

**Step 2 — Communicate proactively:**
I would not silently drop something and hope nobody notices. I'd send a quick note to relevant parties: "I'm managing [X] this week — the retrospective notes will be with you by Saturday morning instead of Friday. Is that workable?" and "I'll have the risk analysis to you by [specific date] — does that timeline work, or is there a specific decision this is feeding into?"

**Step 3 — Tell the PM:**
"I want to flag that I have more on my plate this week than I can complete with full quality by Friday. Here's what I'm prioritizing: [list]. Is this the right order, or should I adjust?" This is ownership — not complaining, not silently failing, but proactively managing.

**Step 4 — Learn for the future:**
How did I get into this position? Was it poor planning, unexpected additions, or scope beyond my capacity? I'd build a weekly prioritization habit — every Monday, map the week's tasks, flag potential conflicts before they become Friday crises.

---

### SCENARIO 29: You Spot an Error in Something the PM Shared With Leadership

**Interviewer:** "You review a status report your PM sent to leadership and spot an error — a milestone is shown as complete when it's actually still in progress. The report was sent an hour ago. What do you do?"

---

**My response:**

**Immediate action — inform the PM privately:**
I'd message the PM directly: "Hey, I just reviewed the status report sent to leadership. I noticed the [milestone name] is marked as complete — based on the tracker it's still in progress. I wanted to flag this in case a correction is needed. Happy to draft the correction if useful."

I'd be factual, not alarmed. Errors happen. My job is to catch them and help fix them — not to make my PM feel embarrassed.

**If the error is minor:**
The PM may decide a correction isn't worth sending a follow-up to leadership for a minor item. That's their call.

**If the error could mislead decision-making:**
A leadership team might make decisions based on a milestone being complete when it isn't — e.g., approving marketing spend for a launch, scheduling a board presentation. In this case, a correction email is important. I'd support the PM in drafting it: "A quick correction to the status report shared this morning — [Milestone X] is currently in progress, expected completion [date]. Apologies for the earlier inaccuracy."

**What I'd build going forward:**
A habit of reviewing status reports against the live tracker before they go out. Even a 5-minute check before the PM sends catches most errors.

---

### SCENARIO 30: Your First Week — How Do You Onboard Effectively?

**Interviewer:** "It's your first week as a PM intern. Walk me through how you'd onboard and get up to speed."

---

**My first-week plan:**

**Day 1–2 — Understand the landscape:**
- Meet the PM and understand the current project state, immediate priorities, and what they need from me in the first 30 days
- Get access to all tools: Jira, Notion/Confluence, Slack, Google Drive
- Read all existing project documentation: project charter, requirements docs, sprint history, risk register, current status report
- Review the Jira board to understand what's in the current sprint, what's upcoming, what's been completed recently

**Day 3–4 — Meet the people:**
- Brief 1:1s with each team member (15–20 minutes) — not to ask work questions, just to understand their role, what they're working on, and what makes the PM process harder or easier from their perspective
- These conversations are gold — I learn the informal dynamics, the real blockers, and the pain points faster than any document

**Day 5 — Participate and contribute:**
- Attend standup, sprint review, or retrospective if happening that week
- Take notes, observe, don't try to take over
- By end of week 1, send my PM a document: "Here's what I understand about the project, here are the open questions I have, and here are 2–3 things I've already noticed that I'd like to help address." This shows initiative and creates an early feedback loop.

**First 30 days — build habits:**
- Own the status report cadence
- Be the person who sends meeting agendas before meetings and minutes after
- Start tracking risks and action items in a consistent format
- Ask lots of questions — but do the research first so questions are specific, not lazy

---

*[Part 2 — Live Case Study Walkthroughs continues in next section]*

---

## PART 11: Live Case Study Walkthroughs

> These are end-to-end case studies that interviewers may walk you through step-by-step, asking follow-up questions at each stage. Practice these like a conversation, not a monologue.

---

### CASE STUDY A: Launch an AI Feature From Scratch

**Interviewer:** "Walk me through how you, as a PM intern, would support the end-to-end delivery of a new AI-powered feature — say, an AI chatbot for customer support — from requirements to launch."

---

**This is a multi-phase walkthrough. Speak through each phase.**

---

#### Phase 1: Discovery & Requirements (Weeks 1–2)

"The first thing I'd do is understand the WHY before touching any plan or backlog.

I'd work with the Product Manager to understand:
- What problem does this chatbot solve? (Reduce support ticket volume? 24/7 availability? Faster resolution time?)
- Who are the users? (External customers? Internal support agents?)
- What does success look like? (Resolution rate, CSAT score, deflection rate, response time?)
- What are the constraints? (Budget, technology stack, compliance requirements?)

I'd facilitate or support a requirements-gathering session with stakeholders — the customer success team, product, engineering, and legal/compliance if there are data privacy considerations.

Output: A requirements brief covering:
- Problem statement
- User personas
- Functional requirements (what the chatbot must do)
- Non-functional requirements (response time <2s, 99.9% uptime, GDPR compliance)
- Out of scope (what it won't do in v1)
- Success metrics (target: 40% ticket deflection rate in first month)
- Dependencies (training data source, LLM API, integration with existing helpdesk tool)

I'd circulate this for review and get sign-off from the Product Manager and key stakeholders before any technical work begins."

---

#### Phase 2: Planning (Weeks 2–3)

"With requirements agreed, I'd support the PM and tech lead in sprint planning.

Key work items I'd help structure into the backlog:

**Infrastructure/Setup Epic:**
- Set up LLM API integration (OpenAI/Anthropic API or fine-tuned model)
- Set up vector database for knowledge base retrieval (RAG architecture)
- Set up development and staging environments

**Data Epic:**
- Collect and curate historical support tickets for training/retrieval
- Build and test knowledge base with product documentation
- Define conversation flows and fallback logic

**Core Feature Epic:**
- Build chat widget UI
- Build backend conversation handler
- Integrate with existing helpdesk system (Zendesk/Freshdesk)
- Implement session management and conversation history

**Testing Epic:**
- Unit and integration tests
- Conversation quality testing (does the bot answer correctly?)
- Edge case testing (what happens when the bot doesn't know the answer?)
- Load testing

**Launch Epic:**
- Staged rollout plan (5% → 25% → 100% of traffic)
- Monitoring dashboard (response accuracy, escalation rate, latency)
- Rollback plan if things go wrong

I'd estimate this as approximately 8–10 sprints of work for a team of 4–5 people. I'd create a high-level milestone timeline:
- End of Sprint 2: Core chatbot prototype working internally
- End of Sprint 5: Beta with 5% user traffic
- End of Sprint 8: Full production rollout

I'd also build the risk register at this stage — key risks: LLM response quality not meeting accuracy threshold, knowledge base incomplete, integration with helpdesk system taking longer than estimated, regulatory compliance flagging."

---

#### Phase 3: Execution & Tracking (Weeks 4–16)

"During execution, my daily and weekly responsibilities would be:

**Daily:**
- Review Jira board: Any stories stuck? Any new blockers? Burndown chart trending correctly?
- Attend standup: Note blockers, action items, who's waiting on what
- Follow up on anything flagged yesterday that hasn't moved

**Weekly:**
- Update project tracker and milestone status
- Prepare and send status report (RAG status, milestone health, top risks, next week focus)
- Review risk register — new risks, status of mitigations
- Ensure meeting agendas are out 24 hours before any scheduled meetings
- Send retrospective and planning meeting minutes within 24 hours

**Key things I'd monitor specifically for an AI feature:**
- Model/chatbot response quality metrics — is accuracy improving sprint over sprint?
- Training data completeness — is the knowledge base being populated on schedule?
- Integration status — is the helpdesk integration on track? This is often a critical path item
- Beta feedback — once live with 5% traffic, are users escalating to human agents at the expected rate?

**Escalations I'd be ready to make:**
- If the LLM accuracy on our test cases falls below threshold → escalate to PM, discuss whether to switch model approach or get more training data
- If the helpdesk integration is delayed → flag dependency risk to PM, explore whether we can launch with a simpler integration first
- If any sprint is >30% behind pace by Day 6 → bring to PM with analysis before the sprint review, not at it"

---

#### Phase 4: QA & Pre-Launch (Weeks 14–16)

"Pre-launch for an AI feature has extra complexity compared to regular software.

Standard QA: functionality testing, regression testing, load testing, security testing.

AI-specific QA:
- **Conversation quality review:** QA team manually reviews 100+ sample conversations. Is the bot answering correctly? Is it hallucinating? Is it staying on topic?
- **Edge case library:** What happens when users ask off-topic questions? What happens when the user is angry or uses offensive language? What's the fallback behavior?
- **Escalation flow testing:** When the bot says 'I don't know,' does the handoff to a human agent work correctly?
- **Bias and safety review:** Is the bot producing any responses that could be legally or reputationally problematic?

I'd maintain a QA sign-off checklist and ensure nothing goes to production without each item cleared. I'd track open bugs by priority — P0 bugs block launch, P1 bugs go into the first post-launch sprint.

UAT: I'd coordinate a User Acceptance Testing session with the customer success team — they use the chatbot as real customers would, and their sign-off is required before launch."

---

#### Phase 5: Launch & Post-Launch (Weeks 16–18)

"Launch day for an AI feature should be boring — because the preparation was thorough.

I'd ensure:
- Staged rollout is configured (5% traffic first — not 100%)
- Monitoring dashboard is live and accessible to the team
- Rollback plan is documented and everyone knows the trigger conditions
- On-call rotation is set up for the first 48 hours
- Communication is ready for internal stakeholders and marketing

Post-launch week:
- Daily review of chatbot metrics: resolution rate, escalation rate, response latency, user ratings
- Collect any early user feedback and categorize
- Any P0 issues get immediate attention
- Status update to stakeholders summarizing launch health

Sprint after launch:
- Bug fixes from launch monitoring
- Incorporate beta user feedback into next sprint backlog
- Schedule a post-launch retrospective: What went well? What would we do differently?
- Begin roadmap for v1.1 enhancements"

---

**Likely follow-up questions the interviewer will ask:**

**"What's the single biggest risk in this project?"**
"The LLM response quality. You can build excellent infrastructure and a beautiful UI, but if the chatbot gives wrong answers or hallucinates, users lose trust immediately and it's very hard to recover. I'd invest heavily in knowledge base quality and testing, and I'd set a clear accuracy threshold that must be met before any user traffic is turned on."

**"What would you do if the launch deadline was moved up by 4 weeks?"**
"I'd immediately assess the critical path and identify what can be cut from v1 without compromising core value. Admin features, advanced analytics, and 'nice to have' conversation flows are candidates for deferral. I'd also assess whether more resources could help — but adding engineers to a late project often makes it later. I'd bring the PM a clear options brief: what we can deliver by the earlier date, and what the trade-off is."

**"How would you track the project budget?"**
"I'd build a budget tracker with categories: LLM API costs (this can be significant and variable with AI), cloud infrastructure, engineering hours, QA, and any third-party tools. For AI projects specifically, the LLM API cost is worth monitoring closely — it's usage-based and can scale unexpectedly once traffic increases. I'd set alert thresholds and monitor weekly."

---

### CASE STUDY B: Rescue a Failing Project

**Interviewer:** "You've been assigned to support a project that's 3 months behind schedule, team morale is low, and the stakeholders are losing trust. As a PM intern, what's your approach?"

---

**My structured response:**

#### Week 1: Understand Before You Act

"My first instinct would NOT be to immediately make changes or propose solutions. The biggest mistake in a recovery situation is acting on incomplete information.

Week 1 is entirely diagnostic:

**Understand the current state:**
- Where exactly is the project against the original plan? Not just 'behind' — specifically which stories/features are done, in progress, or not started?
- What's the honest forecast? If we continue at current velocity, when do we actually finish?
- What is the approved scope, and has scope changed since the original plan?

**Understand what went wrong:**
- When did the project start slipping? What was happening at that point?
- Is the schedule delay due to one big event (technical failure, team change, requirement change) or accumulated small slippages?
- Are there recurring blockers that keep coming up?

**Understand the people:**
- Brief 1:1s with every team member: 'What's the biggest thing preventing you from moving faster?' and 'What would make the biggest difference right now?'
- These conversations are invaluable and will surface things no Jira report ever will

**Understand the stakeholder expectations:**
- What do stakeholders actually need and by when? Is the original deadline driven by a genuine business need (a regulatory deadline, a client contract) or an original estimate that was always aggressive?
- Sometimes a 3-month 'delay' becomes acceptable when stakeholders understand what they're getting and why

**Output of Week 1:** A situation assessment document with:
- Current state vs. planned state (specific, quantified)
- Root cause analysis
- Stakeholder expectation summary
- Team sentiment summary"

---

#### Week 2: Realistic Re-Planning

"With the diagnosis complete, I'd work with the PM on a realistic recovery plan.

**First: Scope re-evaluation**
The original scope was clearly not achievable in the original timeline. The question is: what subset of the original scope can we deliver, and when?

I'd facilitate a MoSCoW prioritization with the Product Owner:
- Must have for launch (non-negotiable)
- Should have (important but deferrable to v1.1)
- Could have (nice to have)
- Won't have this release

This conversation is difficult but essential. It's better to deliver a smaller, complete product on a credible timeline than continue promising the full scope with no credibility.

**Second: Capacity re-evaluation**
What is the team's realistic capacity? Is anyone burned out? Overallocated? Are there skills gaps? A tired team working 60-hour weeks is not more productive — they're building technical debt and making more mistakes.

**Third: Build a credible new plan**
A recovery plan that says 'we'll work harder' is not a plan. A recovery plan shows:
- Revised scope (what's in, what's out)
- Revised timeline with realistic sprint-by-sprint milestones
- Specific actions to address each root cause identified
- A buffer — if the honest estimate is 6 weeks, plan for 8 weeks. Credibility requires not missing the recovery deadline

**Fourth: Get alignment before announcing**
I'd review the recovery plan with the PM and key stakeholders before the wider team. The stakeholders need to accept the revised scope and timeline. Their buy-in is essential — otherwise the 'recovery' just becomes another plan that loses trust when it slips."

---

#### Week 3+: Execution With Visible Progress

"The hardest part of a recovery is restoring team morale and stakeholder trust simultaneously.

**For the team:**
- Make wins visible. When a story is completed, celebrate it — even small wins matter to a demoralized team
- Remove every blocker with extreme urgency. Nothing rebuilds team confidence like a PM who follows through on removing obstacles
- Protect the team from stakeholder pressure — the PM absorbs the external heat so the team can focus
- Be honest about the new plan. Teams respect honesty more than optimism

**For stakeholders:**
- Overcommunicate. Weekly status reports at minimum, with specific metrics: 'This week we completed X story points, bringing us to Y% of revised scope complete'
- Show the trend improving. If velocity was 8 points in the crisis period and is now 20 points with a stable team and clear plan, show that graph
- Keep promises small and keep them. One kept promise rebuilds more trust than ten big promises

**Process changes I'd implement:**
- Daily blockers check (not just standup — a dedicated 5-minute async message: 'Is anyone blocked?')
- End-of-day story status update in Jira (required, takes 2 minutes)
- Weekly retrospective until the project is back on track — don't wait a sprint to course correct
- Clear definition of done enforced without exception — no 'good enough' to hit a milestone

**My role specifically as PM intern:**
I'd be the operational engine of the recovery — tracking everything, following up relentlessly, escalating blockers within hours not days, keeping documentation current, and sending status updates that are honest and specific. The PM sets the strategy; I execute the coordination with precision."

---

### CASE STUDY C: Sprint Planning From Scratch

**Interviewer:** "Walk me through how you'd prepare for and support a sprint planning session for a team of 6 building an AI analytics dashboard. The sprint is 2 weeks. It's Monday morning."

---

**My complete walkthrough:**

#### Friday Before (Pre-Sprint Preparation)

"Sprint planning quality depends almost entirely on preparation done before the meeting.

**My pre-planning checklist:**

**1. Backlog refinement is current:**
I'd verify with the Product Owner that the top 10–15 backlog items are:
- Written clearly in user story format with acceptance criteria
- Estimated in story points (or scheduled for estimation in planning)
- Prioritized in order
- Free of blockers that would prevent a developer from starting them

If the backlog isn't refined, I'd flag this to the PM and PO on Thursday: 'Sprint planning is Monday — do we have at least 2 sprints worth of refined stories ready? If not, can we schedule a 1-hour grooming session Friday?'

**2. Team capacity is known:**
I'd check with each team member: 'Are you available full capacity next sprint, or do you have any planned leave, interviews, or context switching?' I'd create a simple capacity table:

| Team Member | Role | Capacity (%) | Available Points |
|-------------|------|-------------|-----------------|
| Aryan | ML Eng | 100% | 10 |
| Priya | FE Dev | 80% (1 day off) | 8 |
| Rohit | BE Dev | 100% | 10 |
| Shreya | QA | 100% | 8 |
| Total | | | ~36 points |

Based on velocity of 32 and adjusted capacity of ~36, I'd recommend committing to 30–32 points to leave buffer.

**3. Previous sprint is closed:**
All stories from the previous sprint should be in Done status. Incomplete stories need a decision: carry over into this sprint (with a clear reason) or move back to backlog.

**4. Logistics:**
- Calendar invite sent to all attendees (dev team, PO, PM, QA lead)
- Jira sprint created and ready
- Meeting link/room booked
- Agenda sent 24 hours in advance"

---

#### Monday Morning: The Planning Meeting

"**Meeting structure I'd prepare:**

```
SPRINT 7 PLANNING — AI Analytics Dashboard
Duration: 3 hours (for 2-week sprint with 6 people)
Attendees: Full Scrum team + Product Owner

AGENDA:
[0:00–0:10] Sprint 6 recap (10 min)
  - What did we complete? 
  - What carried over and why?
  - Velocity: actual vs. planned

[0:10–0:20] Sprint 7 goal (10 min)
  - PO presents the desired sprint goal in 1–2 sentences
  - Example: 'By end of Sprint 7, the dashboard displays 
    real-time ML model accuracy metrics with historical 
    trend charts'
  - Team confirms they understand and agree with the goal

[0:20–1:20] Story walkthrough (60 min)
  - PO presents stories in priority order
  - For each story: PO explains the requirement
  - Team asks clarifying questions
  - Team estimates (if not already done in refinement)
  - Decision: in or out of sprint
  - Continue until we've filled capacity

[1:20–1:45] Task breakdown (25 min)
  - For each committed story, team identifies sub-tasks
  - Assign initial owners
  - Identify dependencies between stories

[1:45–1:55] Commitment check (10 min)
  - Review full sprint backlog
  - Team confirms: 'Can we deliver these stories and achieve 
    the sprint goal?'
  - Any last concerns or adjustments

[1:55–2:00] Close (5 min)
  - Confirm sprint starts today
  - Confirm standup time daily (9:30am)
  - Confirm sprint review and retro dates
```

**My role during the meeting:**
- Take notes on everything: which stories were discussed, any concerns raised, estimates if I'm tracking them, decisions made
- Track time — gently redirect if one story is consuming 20 minutes of discussion
- Capture action items in real time
- Update Jira with sprint assignments as they're confirmed (or immediately after)

**Post-meeting (same day):**
- Send meeting minutes with sprint goal, committed stories, capacity used, and any open questions/action items
- Ensure Jira sprint is fully set up: all stories in Sprint 7, assigned, in To Do status, story points populated
- Send a quick message to the team: 'Sprint 7 is live in Jira — let's have a great sprint. First standup is tomorrow at 9:30am'"

---

**Interviewer follow-up: "A developer says in planning they can't estimate a story because it's too technically ambiguous. What do you do?"**

"This is the right thing for the developer to raise — it's far better than guessing wrong.

My response: 'Good catch. What specifically is unclear about this story? Is it the scope, the technical approach, or both?'

If it's scope: The PO needs to clarify or rewrite the story. If they can clarify it in the room in 5 minutes, great. If not, the story goes back to the backlog for refinement before next sprint.

If it's technical approach: A spike might be appropriate — a time-boxed investigation task (e.g., 'Spend 4 hours researching the best approach for X and document findings') that goes into this sprint, so the actual story can be properly estimated and built in the next sprint.

I'd never pressure a developer to estimate something genuinely unclear — the false precision creates more problems than it solves."

---

### CASE STUDY D: Stakeholder Presentation Prep

**Interviewer:** "Your PM asks you to prepare a 5-slide project status presentation for the VP of Product for Friday. The project is 60% complete and on track. What do the slides contain?"

---

**My 5-slide structure:**

**Slide 1: Project Snapshot (Title Slide)**
```
[Project Name] — Status Update
[Date] | Presented by: [PM Name]

Overall Status: 🟢 ON TRACK

Project: AI-Powered Analytics Dashboard
Phase: Execution (Sprint 7 of 12)
Target Launch: [Date]
```

**Slide 2: Progress at a Glance**
```
WHERE WE ARE

[Visual: progress bar at 60%]

✅ COMPLETED (Sprints 1–6):
• User authentication & onboarding
• Data ingestion pipeline
• Core dashboard framework
• ML model integration (v1)

🔄 IN PROGRESS (Sprint 7):
• Real-time metrics display
• Historical trend charts

📅 UPCOMING (Sprints 8–12):
• Predictive analytics module
• Export & reporting features
• Performance optimization
• UAT & Launch

Velocity: Consistent at 30–32 pts/sprint
```

**Slide 3: Milestone Timeline**
```
KEY MILESTONES

[Visual: horizontal timeline]

✅ April 15 — Core Architecture Complete
✅ May 1  — Data Pipeline Live
✅ May 20 — ML Integration Complete
🔄 June 10 — Dashboard Beta (Internal)    [ON TRACK]
📅 July 1  — UAT Sign-Off                 [ON TRACK]
📅 July 21 — Production Launch            [ON TRACK]
```

**Slide 4: Risks & Watchpoints**
```
RISKS & WATCHPOINTS

🟡 AMBER — Third-party charting library license
   Risk: License renewal due June 15; if not renewed, 
   visual components need to be rebuilt
   Mitigation: IT procurement contacted; renewal in progress
   Owner: [Name] | Resolution by: June 8

🟢 LOW — Team capacity in Sprint 9 (1 engineer on leave)
   Mitigation: Sprint 9 planned at 80% capacity
   Impact: None expected on launch timeline

No RED risks at this time.
```

**Slide 5: Decisions & Next Steps**
```
ACTIONS & NEXT STEPS

DECISIONS NEEDED FROM LEADERSHIP:
• None at this time ✅

NEXT MILESTONES:
• Sprint 7 Review: June 3 (Dashboard Beta internal demo)
• Stakeholder UAT Kickoff: June 15
• Launch Go/No-Go: July 15

TEAM FOCUS THIS SPRINT:
• Complete real-time metrics display
• Begin historical chart development
• Resolve charting library licensing (amber risk)

NEXT UPDATE: [Date]
```

**Key principles for this deck:**
- No dense text — VPs scan, they don't read
- RAG status visible on slide 1 — the most important signal
- Lead with what's working, then flag risks
- Decisions and next steps always on the final slide — what do you need from them?
- Keep it to 5 slides — if it needs more, it's a document, not a presentation

---

## APPENDIX: Quick-Reference Scenario Response Frameworks

### Framework 1: The STAR-P Method (for PM scenarios)
> Standard STAR extended with a "Process" element

- **S**ituation — Context, project, team, timeline
- **T**ask — Your specific responsibility
- **A**ction — Step-by-step what you did (most important — be specific)
- **R**esult — Quantified outcome if possible
- **P**rocess — What process/habit did you build from this? (Shows maturity)

---

### Framework 2: The "Clarify-Assess-Act-Communicate" Model
> For live "what would you do" scenarios

1. **Clarify** — What information do I need before acting?
2. **Assess** — What is the real problem and its impact?
3. **Act** — What specific steps do I take, in what order?
4. **Communicate** — Who needs to know what, when, and how?
5. **Follow up** — What do I monitor? What's my check-in cadence?

---

### Framework 3: The Trade-Off Presentation
> For any scenario involving competing priorities or decisions needed from leadership

"I see [X] options here:

**Option A — [Name]:** [What it involves]. Trade-off: [what it protects vs. what it costs].

**Option B — [Name]:** [What it involves]. Trade-off: [what it protects vs. what it costs].

**Option C — [Name]:** [What it involves]. Trade-off: [what it protects vs. what it costs].

My recommendation is Option [X] because [specific reason tied to project priorities]. However, this is a business decision and I'd want your input before proceeding."

---

### Framework 4: The Escalation Template
> For any blocker/risk escalation to your PM

```
ESCALATION: [Brief title]
Date: [Date]

WHAT: [Specific issue in 1–2 sentences]

IMPACT: [What happens if this isn't resolved — 
         specific effect on timeline/scope/budget]

WHAT I'VE TRIED: [Steps already taken to resolve]

OPTIONS:
1. [Option A + pros/cons]
2. [Option B + pros/cons]

RECOMMENDATION: [What I think we should do + why]

DECISION NEEDED BY: [Specific date — not "soon"]
```

---

### Framework 5: The Stakeholder Conflict Resolution Path
> For scenarios involving stakeholders who want different things

1. **Listen to both sides fully** — understand each position and its rationale
2. **Find the shared goal** — there's always something they both want (usually: successful product, on time)
3. **Map the options** — what are the real choices given the constraints?
4. **Quantify the trade-offs** — don't just say "it'll cost more time" — say how much
5. **Escalate to the right level** — the person with authority over both stakeholders makes the call
6. **Document and communicate the decision** — everyone hears the same thing at the same time

---

### 20 "What Would You Do?" Quick-Fire Questions With Short Answers

**Q: A developer says a story will take 3 weeks. It's estimated at 5 points. What do you do?**
> Flag the discrepancy immediately. Call a quick conversation: "Help me understand — we estimated 5 points, which would typically take 2–3 days. What's making this 3 weeks?" Either the estimate is wrong and needs revision, or there's a dependency or complexity we missed. Update the plan honestly.

**Q: A stakeholder asks for a feature that's completely out of scope. How do you respond?**
> "That's an interesting idea. It's not in the current scope — adding it would impact [X]. I'd suggest we log it as a backlog item for the next planning cycle so it can be properly evaluated and prioritized. Can I add it there?"

**Q: The QA team found 40 bugs on the day before launch. What do you do?**
> Triage immediately: P0 (launch blockers), P1 (serious but launchable with fix plan), P2 (can go to post-launch backlog). Only P0s block launch. Present the triage to leadership with a go/no-go recommendation.

**Q: Sprint review is in 2 hours and 3 stories aren't done. What do you do?**
> Don't demo them. Be honest in the review: "We committed to 10 stories and completed 7. Here are the 7 we'll demo. The 3 remaining are carrying into Sprint 8 for reasons X, Y, Z." Transparency > covering up.

**Q: Your PM asks you to do something you don't know how to do. How do you respond?**
> "I haven't done that before, but I'm confident I can figure it out. Can I have [timeframe] to research and come back with a draft? I'll flag if I hit something I'm genuinely stuck on." Never fake expertise.

**Q: Two stories have a dependency but are assigned to the same sprint. How do you handle?**
> Link them as dependent in Jira. Ensure Story A is started first. Flag in standup if Story A isn't progressing — it blocks Story B. Consider whether Story B should be in the next sprint if Story A is high risk.

**Q: The client calls your PM directly and gives conflicting direction to what the team has been working on. What do you do?**
> Document what the client said immediately. Bring it to the PM as soon as possible: "Client said X, which conflicts with our current plan of Y. We need alignment before the team continues." Don't let the team work on two conflicting directions.

**Q: You realize you sent incorrect information in yesterday's status report. What do you do?**
> Correct it immediately. Send a follow-up: "A correction to yesterday's status report — [specific item] was incorrect. The accurate information is [correct details]. Apologies for the confusion." Sitting on it is far worse than correcting it.

**Q: A stakeholder praises your work directly, bypassing your PM. What do you do?**
> Thank them genuinely. Then loop in your PM: "Just wanted to let you know [Stakeholder] had positive feedback on [X]. I wanted to make sure you were aware." No glory-grabbing. The PM should know about stakeholder relationships.

**Q: The engineering team says they can't give an estimate for a feature because it's too new and uncertain. What do you do?**
> Propose a spike: a time-boxed (e.g., 2-day) investigation task where the engineer researches the approach and produces a written assessment and estimate. The spike goes into this sprint; the actual feature goes into next sprint with a real estimate.

**Q: You're in a standup and it's going 45 minutes. What do you do?**
> Gently interrupt: "Let's take this discussion offline between [specific people] and keep the standup moving. Can we schedule 30 minutes after this to resolve it?" Time-box and protect the standup format.

**Q: A story has been 'In Progress' for 8 days in a 10-day sprint. What do you do?**
> Immediate 1:1 with the owner: "I see [story] has been in progress for most of the sprint — where are you with it? Is it nearly done, or is something blocking it?" Don't assume — ask. Then take action based on the answer.

**Q: The team wants to add technical debt cleanup to every sprint. The PO says there's no room. What do you do?**
> Facilitate the conversation: "Technical debt accumulates cost over time — how much is appropriate to address each sprint?" A common solution is a standing 10–15% of sprint capacity reserved for technical debt. Present this as an option to the PO with the trade-off clearly stated.

**Q: You're asked to prepare for a meeting you know nothing about. What do you do?**
> Research: read the relevant Jira tickets, Notion pages, previous meeting notes. Then ask the PM or meeting organizer: "Can you give me 5 minutes before the meeting to understand my role — am I taking notes, tracking actions, or contributing to the discussion?" Preparation prevents looking unprepared.

**Q: A team member says standup is a waste of time. How do you respond?**
> "Tell me more — what specifically isn't useful about it?" Listen first. Maybe standups have become status reports to the manager rather than team syncs. Maybe they're too long. Address the specific concern. Then re-examine whether standup format should be adjusted.

**Q: Your sprint burndown is perfect — completely flat for 5 days. Is this good?**
> No — a flat burndown means no work is being completed. I'd immediately investigate: Are stories blocked? Are they 95% done but not being marked complete? Is the team working but not updating Jira? A flat burndown early is a warning signal, not silence.

**Q: A key design decision needs to be made and the designer is on leave. What do you do?**
> First: Can the decision wait until they return without impacting the sprint? If yes, wait. If no: Is there a secondary decision-maker (design lead, another designer)? If not, document the two options with trade-offs and escalate to the PM for a temporary decision, noting that the designer should review and confirm when they return.

**Q: How do you handle a stakeholder who keeps scheduling last-minute meetings that disrupt the team?**
> Have a proactive conversation: "I want to make sure we're giving you the right information at the right time. We have a weekly status report on [day] and I'm happy to add a standing 20-minute sync. Would that work? I want to reduce the need for ad-hoc meetings that interrupt the engineering team's focus time." Solve the underlying need.

**Q: What's the first thing you check when you arrive at work every morning as a PM intern?**
> Three things in order: (1) Jira sprint board — did anything move overnight, are any stories stuck, any new blockers added? (2) Slack — any urgent messages from the team or stakeholders that need a response before standup? (3) My daily task list — what are the 3 most important things I must get done today? Then standup happens and I adjust from there.

**Q: What does 'done' mean to you?**
> "Done means the story meets every criterion in our Definition of Done — code reviewed and merged, unit tests passing, QA tested and signed off, documentation updated if required, and deployed to at least the staging environment. Not 'mostly done,' not 'code complete,' not 'we just need to test it.' Done means done."

---

*End of 03_scenarios_and_case_studies.md*

> **How to use this document before your interview:** Pick 5 scenarios randomly and answer them aloud without reading the answers. Record yourself. Listen back. Are you structured? Are you specific? Do you show judgment or just knowledge? Adjust. Then do 5 more. The goal is for the frameworks to become instinct — so when a novel scenario comes up in the interview, you reach naturally for the right structure.
