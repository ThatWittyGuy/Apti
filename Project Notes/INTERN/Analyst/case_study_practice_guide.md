# Case Study Practice Guide — Data / Business Analyst Fresher
**How to structure your thinking, talk through problems out loud, and answer case questions under pressure**

---

## Table of Contents
1. [How to Approach Any Case Question](#1-how-to-approach-any-case-question)
2. [The Universal Analysis Framework](#2-the-universal-analysis-framework)
3. [Case Type 1 — Metric Has Changed](#3-case-type-1--metric-has-changed)
4. [Case Type 2 — Investigate a Business Problem](#4-case-type-2--investigate-a-business-problem)
5. [Case Type 3 — Should We Launch / Do This?](#5-case-type-3--should-we-launch--do-this)
6. [Case Type 4 — How Would You Measure Success?](#6-case-type-4--how-would-you-measure-success)
7. [Case Type 5 — Data Quality Problem](#7-case-type-5--data-quality-problem)
8. [Case Type 6 — Stakeholder Conflict / Numbers Don't Match](#8-case-type-6--stakeholder-conflict--numbers-dont-match)
9. [Case Type 7 — Product / Feature Decision](#9-case-type-7--product--feature-decision)
10. [Case Type 8 — Estimation / Guesstimate](#10-case-type-8--estimation--guesstimate)
11. [Worked Full Cases — Practice These Out Loud](#11-worked-full-cases--practice-these-out-loud)
12. [Phrases to Use While Thinking Out Loud](#12-phrases-to-use-while-thinking-out-loud)
13. [Common Mistakes in Case Interviews](#13-common-mistakes-in-case-interviews)

---

## 1. How to Approach Any Case Question

The interviewer is not testing whether you get the "right" answer. They are testing:
- **Structure** — can you break a messy problem into clear parts?
- **Logic** — does your thinking follow naturally from the problem?
- **Communication** — can you explain your reasoning while thinking?
- **Business sense** — do your conclusions make sense in the real world?

### The Golden Rules

**Rule 1: Never dive straight into answering.**
Take 30–60 seconds to structure your thoughts. Say: *"Give me a moment to organise my thinking."* Interviewers respect this.

**Rule 2: Clarify before you analyse.**
Ask 1–2 scoping questions before starting. This shows you don't make assumptions. Example: *"Before I dig in — when you say 'sales are down', are we talking about order volume, revenue, or both?"*

**Rule 3: State your structure out loud.**
Say: *"I'd like to approach this by first confirming the metric, then breaking it down by dimension, then looking for causes."* This signals structured thinking before you even answer.

**Rule 4: Think out loud throughout.**
Interviewers want to follow your reasoning, not just hear your conclusion. Say what you're considering and why you're ruling things in or out.

**Rule 5: End with a recommendation.**
Even if uncertain, commit to a direction. *"Based on what I've found, I'd recommend X as the first action, with the caveat that we'd need to validate Y before committing fully."*

---

## 2. The Universal Analysis Framework

Use this skeleton for almost any case. Adapt the sections to fit.

```
STEP 1 — CLARIFY
  → What exactly is the metric / problem?
  → What is the time period? What is the comparison baseline?
  → Any known context? (recent launches, campaigns, external events?)

STEP 2 — CONFIRM THE FACT
  → Is the number real or a reporting/data issue?
  → Check the data source, the definition, the time window

STEP 3 — DECOMPOSE
  → Break the metric into its components
  → Cut by dimensions: time, geography, segment, channel, product, device

STEP 4 — ISOLATE
  → Where is the problem concentrated?
  → Is it broad (affects everything) or narrow (one segment, one region)?

STEP 5 — HYPOTHESISE CAUSES
  → Internal causes: product change, pricing change, UX bug, supply issue
  → External causes: competitor launch, seasonality, macro event

STEP 6 — PRIORITISE INVESTIGATION
  → What data would confirm or rule out each hypothesis?
  → Start with the hypothesis that's easiest to test and highest impact

STEP 7 — RECOMMEND
  → What action do you recommend?
  → What would you measure to know if it's working?
```

---

## 3. Case Type 1 — Metric Has Changed

**Examples:**
- "Revenue dropped 15% last month. What happened?"
- "Our return rate jumped from 6% to 10%. Investigate."
- "App installs are up but orders are flat. Why?"

### Framework: DECOMPOSE → ISOLATE → HYPOTHESISE

**Step 1: Clarify the metric**
> "When you say revenue dropped — is that gross revenue, net revenue after discounts, or net after returns? And compared to last month or same month last year?"

**Step 2: Confirm it's real**
> "First I'd check whether this is a data issue — reporting pipeline delayed, definition changed, time zone mismatch — before assuming the business has a problem."

**Step 3: Decompose the metric**

Revenue = Number of Orders × Average Order Value

So a revenue drop means:
- Fewer orders (volume problem), or
- Lower average order value (value problem), or
- Both

**Step 4: Cut by dimensions**
Work through a checklist:
- **Time:** Sudden drop on one day vs gradual decline over weeks? Sudden = likely a specific event or bug. Gradual = trend issue.
- **Geography:** All regions down, or just one? If just North — logistics or regional issue.
- **Channel:** Website, app, offline — which channel is down? If app only — app bug or update.
- **Category:** All products or one category? If just Accessories — supplier issue, bad reviews.
- **Customer type:** New customers down, existing customers stable? Or both? If new only — acquisition/marketing issue.
- **Device:** Mobile vs desktop (if web) — conversion rate drop on mobile = UX issue.

**Step 5: Hypothesise causes**
- Internal: pricing change, promotion ended, site bug, checkout issue, stock-out
- External: competitor launched a sale, festival season ended, economic downturn

**Step 6: What data to pull**
- Orders by day (to find the exact date it changed)
- Orders by region, channel, category
- Funnel conversion rates (did drop-off increase at a specific step?)
- Any deployments or changes made around that date

**Step 7: Recommend**
> "Based on this breakdown, if the drop is concentrated in the app channel starting on a specific date, the first action is to check whether an app update was released that day and test the checkout flow on the current version."

---

### Worked Example: "Revenue dropped 15% last month"

**You say:**
> "Before I start — a couple of quick clarifications. Is the 15% drop versus last month or versus the same month last year? And are we looking at gross revenue or net of discounts?"

*(Interviewer answers: vs last month, gross revenue)*

> "Okay. I'd start by confirming this is a real business problem and not a data issue — check that the reporting pipeline ran correctly and the date filter is right. Assuming it's real: revenue is orders times average order value, so either volume is down, ticket size is down, or both. I'd pull that split first.

> If volume is down — I'd then cut by channel, region, and category to find where the drop is concentrated. A broad drop across everything suggests a macro cause — maybe a promotion ended, maybe there's a market downturn. A narrow drop in one region or channel suggests something specific — a logistics issue, an app bug, a stock-out.

> I'd also look at the funnel — did conversion rates at any step change? If the drop-off at checkout specifically went up, that's a UX or payment issue. If it's at the product view stage, maybe pricing changed or search rankings fell.

> Finally I'd check: did we do anything different last month? A price increase, a campaign that stopped, an app update?

> My first recommendation would be: pull the daily order chart to find the exact date the drop started. That single data point will narrow down the cause dramatically — was it a gradual decline or a cliff on one day?"

---

## 4. Case Type 2 — Investigate a Business Problem

**Examples:**
- "Customer complaints about delivery are increasing. How would you investigate?"
- "Our repeat purchase rate is declining. What's going on?"
- "High-value customers seem to be churning. Investigate."

### Framework: DEFINE → MEASURE → SEGMENT → DIAGNOSE → RECOMMEND

**Step 1: Define the problem precisely**
> "What metric defines 'complaints increasing'? Is it complaint volume, complaint rate (complaints per order), or resolution time?"

**Step 2: Establish the baseline and trend**
> "How long has this been happening? Is it a sudden jump or a gradual increase? What was the rate before?"

**Step 3: Segment**
> "I'd cut complaints by: delivery partner, region, product category, order size, time of day/week. The goal is to find where it's concentrated."

**Step 4: Diagnose**
> "If complaints are concentrated in one region with one delivery partner — logistics partner issue. If concentrated in one category — packaging or product quality issue. If spread evenly — systemic process problem."

**Step 5: Recommend**
> "Depending on the diagnosis — audit the specific delivery partner, change packaging for fragile categories, or review the end-to-end process."

---

### Worked Example: "Repeat purchase rate declining"

**You say:**
> "First — how are we defining repeat purchase rate? Is it the % of customers who made more than one purchase in a given period, or the % who return within a specific time window after their first purchase?"

*(Interviewer: % who make a second purchase within 60 days of first)*

> "Got it. So this is essentially early retention — Month 0 to Month 2 in cohort terms. A decline here could mean the customers we're acquiring are lower quality, or the experience after first purchase has gotten worse.

> I'd split the investigation into two tracks: acquisition quality and post-purchase experience.

> On acquisition quality — I'd compare the cohorts where repeat rate is declining to earlier cohorts. Were these customers acquired through a different channel? A discount campaign tends to attract one-time buyers who won't return at full price.

> On post-purchase experience — I'd look at what happens after the first order. Are return rates higher for this group? Are delivery times longer? Are they getting follow-up communication? Did anything change in the email or notification flow?

> I'd also look at the category of their first purchase. Customers who buy Accessories as their first item might have lower repeat intent than someone who buys a Smartphone — the purchase cycle is different.

> My recommendation: first pull the cohort retention table and identify exactly when the decline started. If it maps to a specific campaign or channel launch, that's your acquisition quality problem. If it's across all acquisition sources, look at the post-purchase journey."

---

## 5. Case Type 3 — Should We Launch / Do This?

**Examples:**
- "We're thinking of launching a loyalty program. Should we?"
- "Should we expand to Tier 2 cities?"
- "Should we add a 'Buy Now Pay Later' payment option?"

### Framework: GOAL → SIZE → FEASIBILITY → RISK → MEASURE → RECOMMENDATION

**Step 1: Clarify the goal**
> "What's the primary objective — increase revenue, improve retention, acquire new customers, or all three?"

**Step 2: Size the opportunity**
> "How large is the potential market or impact? How many customers would this affect? What's the estimated revenue uplift?"

**Step 3: Assess feasibility**
> "Do we have the data, tech, and operational capacity to do this? What would it cost?"

**Step 4: Identify risks**
> "What could go wrong? Does it cannibalise existing revenue? Does it add operational complexity?"

**Step 5: How would you measure success?**
> "What metric defines success? What's the baseline? How long before we expect to see impact?"

**Step 6: Recommend**
> "Based on this — I'd recommend yes/no/pilot, with these conditions."

---

### Worked Example: "Should we launch a loyalty program?"

**You say:**
> "To evaluate this properly I'd want to understand the goal first. Is the primary objective to increase repeat purchases, increase average order value, or reduce churn among high-value customers?"

*(Interviewer: reduce churn among existing customers)*

> "Okay — so this is a retention play, not an acquisition play. That shapes the design and the success metric.

> First I'd size the problem. What's the current churn rate among the customer segments we want to retain? From my internship experience, if Gold-tier customers are churning at even 5% monthly, a loyalty program that reduces that by 20% could have a significant revenue impact given they contribute disproportionately to total revenue.

> Second, I'd look at what the data tells us about what drives loyalty. Are high-repeat customers concentrated in certain regions, certain categories, or acquired through certain channels? A loyalty program should be designed to reinforce the behaviours that already correlate with retention.

> On risks — the main risk is that we attract deal-hunters who game the points system without becoming genuinely loyal. Amazon Prime works because the benefits (fast delivery) are tied to transaction behaviour, not just spend thresholds. We'd want to design the program similarly.

> To measure success, I'd define a control group — customers who qualify for the program but are randomly excluded — and compare their retention rate to enrolled customers over 6 months. That's essentially an A/B test on the loyalty program.

> My recommendation: before a full launch, pilot with a small segment — say Gold-tier customers in one region — measure retention impact over 3 months, then decide on full rollout."

---

## 6. Case Type 4 — How Would You Measure Success?

**Examples:**
- "We just launched a new homepage redesign. How would you measure if it worked?"
- "We ran a promotional campaign last month. Was it successful?"
- "How would you measure the success of a new customer support chatbot?"

### Framework: GOAL → PRIMARY METRIC → GUARDRAIL METRICS → BASELINE → TIMEFRAME → METHOD

**Step 1: Clarify the goal**
> "What was the redesign trying to achieve — higher conversion, longer session time, lower bounce rate?"

**Step 2: Define the primary metric**
> "The primary metric should directly measure the goal. For a homepage redesign aimed at conversion: checkout conversion rate (visitors → orders)."

**Step 3: Guardrail metrics**
> "What must we not make worse? Even if conversion improves, if average order value drops or return rate spikes, that's a net loss. Define guardrails upfront."

**Step 4: Baseline**
> "What was the metric before the change? You need a pre-period to compare against."

**Step 5: Timeframe**
> "How long before we expect impact? A UX change can show results in 1–2 weeks. A loyalty program might take 3–6 months. Define this upfront or you'll keep moving the goalposts."

**Step 6: Method**
> "Ideally A/B test — show old and new versions to randomly split users simultaneously. If A/B isn't possible (change is too big or already live), use pre/post comparison with seasonality adjustment."

---

### Worked Example: "Was our promotional campaign successful?"

**You say:**
> "To answer this I'd need to define what success looks like — was the goal revenue, new customer acquisition, or moving specific inventory?

> Assuming it was revenue: I'd compare revenue during the campaign period to the equivalent period before, adjusted for seasonality. I'd also compare to the same period last year if possible.

> But revenue alone isn't sufficient. I'd also check:
> - **Margin impact:** Discounts increase revenue volume but reduce margin. Did net margin hold up?
> - **New vs existing customers:** Did the campaign bring new customers, or did existing customers just pull forward purchases they'd have made anyway?
> - **Post-campaign drop:** Did revenue fall sharply after the campaign ended? A big post-campaign dip suggests we cannibilised future sales.
> - **Return rate:** Did the promotion attract customers who returned goods at a higher rate?

> A campaign that drove ₹10L in incremental revenue but attracted customers with a 30% return rate and a sharp post-campaign revenue drop may have been net negative.

> My success definition would be: incremental net revenue (above baseline, after returns and discounts) from a meaningful proportion of new customers, with no significant post-campaign demand cannibilisation."

---

## 7. Case Type 5 — Data Quality Problem

**Examples:**
- "Our dashboard shows two different revenue numbers in two different places. What do you do?"
- "The data team says 10,000 orders this month, the business team says 12,000. Who's right?"
- "You notice negative values in the revenue column. What do you do?"

### Framework: DON'T PANIC → TRACE → COMPARE DEFINITIONS → FIX → DOCUMENT

**Step 1: Don't assume which side is wrong**
> "I'd resist the urge to say either number is correct. Both might be right under different definitions."

**Step 2: Trace each number back to its source**
> "Where does each number come from? Different tables? Different queries? Different tools pulling from different refresh times?"

**Step 3: Compare definitions**
> "Are both using the same filters? Same time period? Same order status (completed vs all)? Same currency? Same timezone?"

**Step 4: Find the discrepancy**
> "Once definitions are aligned, if numbers still differ — look at the JOIN logic, the deduplication, the aggregation level."

**Step 5: Fix and document**
> "Correct whichever is wrong, document the correct definition in a shared place, and put a note in the dashboard so others don't hit the same confusion."

---

### Worked Example: "Two reports show different revenue numbers"

**You say:**
> "First thing I'd do is not escalate immediately — I'd investigate before making any claims.

> I'd pull both queries or report definitions and compare them side by side. The most common causes of discrepancy are: different order status filters (one includes 'pending', one only 'completed'), different date filters (order_date vs created_at vs payment_date — these can differ if payments are processed next day), different levels of aggregation (one is summing order_items.unit_price × quantity, the other is summing orders.total_amount — and there's a data gap between the two), or different join types causing row multiplication.

> In my internship I had exactly this — my revenue figure was 2% lower than an existing report. Turned out the existing report included pending orders in the total. I was correctly filtering to completed only. I walked my mentor through the SQL diff and confirmed the existing report was the one with the error.

> Once found, I'd fix the incorrect query, add a comment in both places documenting the correct definition, and recommend we create a single shared metric definition document — so 'revenue' means one thing across the whole team."

---

## 8. Case Type 6 — Stakeholder Conflict / Numbers Don't Match

**Examples:**
- "The marketing team says the campaign worked. The finance team says it didn't. How do you resolve this?"
- "Your manager disagrees with your analysis. What do you do?"
- "A stakeholder is using a different metric than you. Who is right?"

### Framework: UNDERSTAND BOTH SIDES → ALIGN ON DEFINITION → DATA ARBITRATES → COMMUNICATE

**Step 1: Understand both perspectives**
> "What metric is each team using? What's their basis for their conclusion?"

**Step 2: Check for definition differences**
> "Often both sides are right — they're just measuring different things. Marketing might be measuring gross revenue; Finance might be measuring net margin. Both numbers are correct — they answer different questions."

**Step 3: Let data arbitrate**
> "Pull both sets of numbers transparently. Show the calculation behind each. Agree on which metric is most relevant to the decision being made."

**Step 4: Communicate diplomatically**
> "Frame it as: 'both perspectives have merit, here's what the data says about each.' Never make a stakeholder feel attacked — your job is to clarify, not to win."

---

## 9. Case Type 7 — Product / Feature Decision

**Examples:**
- "Should we add a product recommendation engine to the homepage?"
- "Should we remove the offline channel and go digital-only?"
- "Should we introduce free delivery above ₹999?"

### Framework: GOAL → IMPACT SIZING → DATA EVIDENCE → RISKS → TEST PLAN → DECISION

**Step 1: Goal**
> "What's this trying to achieve — more revenue, better UX, lower cost?"

**Step 2: Impact sizing**
> "How many users does this touch? What's the estimated uplift? Back of envelope: if recommendation engine increases click-through by 10% and 5% of those clicks convert, and we have 100,000 homepage visitors per month at ₹3,500 avg order... that's 100,000 × 10% × 5% × 3,500 = ₹17.5L potential uplift per month."

**Step 3: Data evidence**
> "What does the current data tell us? Are users who engage with existing recommendations converting better? That would validate the hypothesis before building."

**Step 4: Risks**
> "What are the downside scenarios? Recommendation engine could show irrelevant products and damage trust. Free delivery above a threshold could be gamed — users add cheap items to hit the minimum."

**Step 5: Test plan**
> "Before full rollout — A/B test with a small user segment. Measure primary metric (conversion) and guardrails (AOV, return rate)."

**Step 6: Decision**
> "If the A/B test shows statistically significant uplift in conversion with no degradation in guardrails — roll out."

---

### Worked Example: "Should we introduce free delivery above ₹999?"

**You say:**
> "The goal I'd assume is to increase average order value — customers top up their carts to hit the free delivery threshold. So the key metric is AOV, with revenue and margin as guardrails.

> First I'd look at the current order value distribution. What % of orders are between ₹800 and ₹999 — those are the customers most likely to add items to hit the threshold. If that's a large segment, the uplift potential is significant.

> I'd then estimate: if 20% of orders in the ₹800–999 range top up to ₹999, and average top-up is ₹150, with 10,000 such orders per month — that's ₹1.5L in additional revenue. Against that, I'd model the margin impact of the free delivery cost — if average delivery cost is ₹80 and we're now subsidising it on all orders above ₹999, the net is: incremental revenue minus incremental delivery cost.

> Risks: customers might split large orders into smaller ones below the threshold to pay less total. Unlikely but worth monitoring. Also, the ₹999 threshold might be too high or too low — the right threshold depends on the order value distribution.

> Test plan: roll out to 50% of users for 4 weeks. Measure AOV, conversion rate, revenue per visitor, and order frequency. If AOV rises and margin holds — roll out fully.

> My recommendation: pilot it. The data setup to evaluate this exists — it's a standard A/B test — and the potential upside is meaningful enough to test."

---

## 10. Case Type 8 — Estimation / Guesstimate

**Examples:**
- "How many Swiggy orders are placed in Mumbai on a weekday?"
- "Estimate the number of smartphones sold in India per year."
- "How many ATMs are there in Pune?"

### Framework: STRUCTURE → ASSUMPTIONS → CALCULATE → SANITY CHECK

These test your ability to make reasonable assumptions and arrive at a defensible number — not the exact answer.

**Step 1: Structure the problem**
Break it into parts you can estimate. Top-down (population → penetration) or bottom-up (unit × frequency).

**Step 2: State your assumptions clearly**
> "I'm going to assume X — feel free to correct me if that's off."

**Step 3: Calculate step by step**
Do the arithmetic out loud. Round numbers for simplicity.

**Step 4: Sanity check**
> "Does this number feel right? Let me cross-check it against something I know."

---

### Worked Example: "How many DA/BA job openings are there in India right now?"

**You say:**
> "Let me structure this. I'll estimate based on the number of companies in India that would employ a data analyst.

> India has roughly 1.4 billion people. The formal corporate sector employs maybe 50 million people. Of those, let me segment by company size:

> Large companies (500+ employees): maybe 5,000 companies. Each might have 10–30 data roles. Say average 15. That's 75,000 data roles total, with maybe 5–10% open at any time = 4,000–7,000 openings.

> Mid-size companies (50–500 employees): maybe 100,000 companies. Each might have 1–3 data roles. Say average 2. That's 200,000 data roles, with 5% open = 10,000 openings.

> Startups and small firms: harder to estimate, let's say they add another 5,000.

> Total rough estimate: **15,000–20,000 DA/BA job openings at any point in time** across India.

> Sanity check: job portals like LinkedIn and Naukri typically show 20,000–50,000 data analyst listings — my estimate is on the conservative side but in the right order of magnitude. I'd say the true number is probably 20,000–30,000 active openings."

---

### Worked Example: "Estimate the revenue of a mid-sized food delivery app in India"

**You say:**
> "I'll go bottom-up — orders per day × take rate.

> Let's say this app operates in 20 cities. Average city has maybe 50,000 orders per day on the platform — more in metros, fewer in Tier 2. So total: 20 × 50,000 = 1 million orders per day.

> Average order value: ₹300 (a typical food delivery order in India).

> GMV per day: 1 million × ₹300 = ₹30 crore per day.

> The app takes roughly 20–25% commission from restaurants. Let's say 22%.

> Revenue per day: ₹30 crore × 22% = ₹6.6 crore per day.

> Annual revenue: ₹6.6 crore × 365 = roughly ₹2,400 crore per year — about ₹24 billion.

> Sanity check: Swiggy reported revenues of around ₹8,000–11,000 crore — our fictional mid-sized app at roughly one-quarter of Swiggy feels reasonable."

---

## 11. Worked Full Cases — Practice These Out Loud

**Instructions:** Read the case, cover the answer, and actually speak your answer out loud for 3–5 minutes. Then read the model answer and compare. The gap between reading and speaking is where most candidates fail.

---

### Full Case A: "Our app's Day 7 retention dropped from 35% to 22% this month."

**Model answer to speak out loud:**

> "Before I start — a couple of clarifications. Day 7 retention meaning users who open the app on Day 7 after install, or users who complete a second purchase within 7 days? And this is for all users or a specific segment?

> Assuming it's all users, second session within 7 days. First thing I'd check is whether this is real or a measurement issue — did the analytics tracking change? Did the definition of 'active' change?

> Assuming it's real: a drop from 35% to 22% is a 37% relative decline — that's significant and sudden. Sudden changes point to a specific event, not a gradual trend.

> I'd look at the timeline — on exactly which date did this start? If it maps to an app update, a backend change, or a marketing campaign that drove lower-quality installs, that's your culprit.

> Then I'd segment: did retention drop for all new users, or only for users acquired through a specific channel? If users acquired through a paid campaign have 15% Day 7 retention while organic users are at 34%, the campaign is bringing in low-intent users who install and don't engage.

> I'd also look at what users are doing — or not doing — in Days 1–6. If Day 1 retention is also down, the onboarding experience broke. If Day 1 is fine but Day 7 dropped, users are starting but not finding enough value to return.

> My first action: pull the retention by acquisition channel and by app version. Those two cuts will point to whether this is a product problem, an onboarding problem, or an acquisition quality problem. Each has a different fix."

---

### Full Case B: "We want to reduce cart abandonment. How would you approach this?"

**Model answer to speak out loud:**

> "Cart abandonment is when a user adds items to cart but doesn't complete the purchase. The abandonment rate is: users who abandon cart / users who add to cart × 100.

> To approach this — first I'd establish the baseline. What's our current cart abandonment rate, and how does it compare to industry benchmarks? E-commerce typically sees 60–80% cart abandonment, so context matters.

> Next, I'd segment abandonment by where in the checkout funnel users drop. Do they abandon immediately after cart, at the shipping details page, at payment, or at order confirmation? Each drop-off point has a different cause:
> - At cart: price shock when they see the total with shipping. Fix: show shipping cost earlier, offer free delivery threshold.
> - At shipping details: friction in form filling. Fix: autofill, saved addresses.
> - At payment: payment method not available, trust issues, OTP friction. Fix: add UPI, Buy Now Pay Later, cleaner payment UI.
> - At order confirmation: technical error or slow load. Fix: engineering bug.

> I'd also segment by device — mobile checkout abandonment is typically much higher than desktop because forms are harder to fill on mobile.

> To measure impact of any fix: A/B test. Change one thing at a time, measure abandonment at that specific step.

> My recommendation: pull the funnel drop-off by step and by device first. The data will tell us which step and which segment to fix first — don't guess."

---

### Full Case C: "Gold-tier customers are generating less revenue this quarter vs last. Why?"

**Model answer to speak out loud:**

> "Gold-tier customer revenue declining is a high-priority problem given they typically contribute disproportionately to total revenue.

> First — decompose the revenue change. Gold-tier revenue = Number of Gold customers × Order frequency × Average order value. Which of these three changed?

> If the number of Gold customers declined — are fewer customers reaching Gold status, or are Gold customers churning? Different problem.
> If order frequency declined — Gold customers are ordering less often. Why? Did our product quality drop, did a competitor launch, did their purchase cycle change?
> If average order value declined — Gold customers are spending less per order. Are they buying cheaper categories, or are they using more discounts?

> I'd also check if Gold-tier definition changed — if the threshold to qualify as Gold was raised, some customers may have been reclassified to Silver, making the Gold bucket appear smaller.

> Then I'd look for external context: did a competitor launch a loyalty program? Did we reduce benefits for Gold-tier members? Did a macro event (economic slowdown, festive season ending) hit high-ticket purchases harder?

> My immediate data pulls: Gold customer count this quarter vs last, their order frequency distribution, their top categories bought, and any change in discount usage. I'd also look at whether previously Gold customers have been reclassified.

> First recommendation: confirm whether this is fewer Gold customers, less activity per Gold customer, or lower spend per order — then focus the investigation on whichever dimension is moving most."

---

## 12. Phrases to Use While Thinking Out Loud

Memorise these — they buy you thinking time and signal structured thinking.

**To start:**
- "Before I dive in, let me clarify a couple of things..."
- "Give me a moment to structure my thinking."
- "I'd like to approach this systematically — starting with confirming the metric, then breaking it down."

**To decompose:**
- "I'd break this metric into its components — [X] equals [A] times [B], so a change must come from one or both."
- "Let me cut this by a few dimensions — time, geography, channel, and segment."
- "The first thing I'd check is whether this is isolated or broad — is it one region or all regions?"

**To hypothesise:**
- "There are broadly two types of causes here — internal and external. On the internal side..."
- "My leading hypothesis is [X] because [Y]. I'd test that by looking at [Z]."
- "I'd rule out [X] first because it's the easiest to check — if [Y] data shows Z, we can eliminate it."

**To conclude:**
- "Based on what the data would likely show, my recommendation is..."
- "I'd start with [action] because it's the highest-impact, lowest-effort investigation."
- "The key question this leaves open is [X] — I'd want to validate that before committing fully."

**If you don't know something:**
- "I'm not certain of the exact benchmark here, but I'd estimate roughly [X] — does that align with what you're seeing?"
- "I don't have that data point in front of me, but my approach would be to [how you'd find it]."

---

## 13. Common Mistakes in Case Interviews

| Mistake | What to Do Instead |
|---|---|
| Jumping to a solution immediately | Always clarify and structure first — 30–60 seconds of thinking is not wasted time |
| Being too narrow — looking at only one dimension | Work through a checklist: time, geo, segment, channel, product, device |
| Saying "I'd look at the data" without saying which data | Be specific: "I'd pull daily orders by channel and look for the date it changed" |
| Not quantifying | Put numbers on things — even rough estimates show business sense |
| Being too technical | Interviewers want business thinking, not just SQL. Translate findings into decisions |
| Presenting findings without a recommendation | Always end with: "Based on this, I'd recommend X" |
| Agreeing with the interviewer when they push back | If you're right, defend it calmly: "I understand that perspective — my reasoning is..." |
| Treating every problem the same | Tailor your approach: a UX drop-off needs funnel thinking; a churn problem needs cohort thinking |
| Forgetting guardrail metrics | Always ask: what must we NOT make worse? |
| Only considering internal causes | External factors (seasonality, competitors, macro) often explain more than internal ones |
