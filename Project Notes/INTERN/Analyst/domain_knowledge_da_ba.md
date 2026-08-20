# Domain Knowledge — Data & Business Analyst Fresher
**Topics:** Funnel Analysis · Churn · Cohort Analysis · A/B Testing · Retention · Engagement · Revenue Metrics · Growth Metrics · Product Analytics

---

## Table of Contents
1. [Funnel Analysis](#1-funnel-analysis)
2. [Churn Analysis](#2-churn-analysis)
3. [Cohort Analysis](#3-cohort-analysis)
4. [A/B Testing](#4-ab-testing)
5. [Retention & Engagement](#5-retention--engagement)
6. [Revenue & Business Metrics](#6-revenue--business-metrics)
7. [Customer Metrics](#7-customer-metrics)
8. [Product Analytics Concepts](#8-product-analytics-concepts)
9. [Data Strategy Concepts](#9-data-strategy-concepts)
10. [Quick-Fire Definitions](#10-quick-fire-definitions)

---

## 1. Funnel Analysis

### What is a Funnel?
A funnel is a sequence of steps a user takes to complete a goal. At each step, some users drop off — so the shape narrows, like a funnel. The goal is to understand *where* and *why* users drop off so you can improve conversion.

### Classic E-commerce Funnel
```
Awareness → Visit Site → View Product → Add to Cart → Checkout → Payment → Order Placed
  100,000      60,000       40,000        15,000        8,000       5,500      5,000
```

**Conversion Rate at each step:**
- Visit → View Product: 40,000 / 60,000 = **66.7%**
- View → Add to Cart: 15,000 / 40,000 = **37.5%** ← biggest drop — investigate this
- Add to Cart → Checkout: 8,000 / 15,000 = **53.3%**
- Checkout → Payment: 5,500 / 8,000 = **68.75%**
- Payment → Order: 5,000 / 5,500 = **90.9%**

**Overall funnel conversion = 5,000 / 100,000 = 5%**

### Key Questions Funnel Analysis Answers
- Where is the biggest drop-off? (prioritise fixing that step)
- Is drop-off consistent across segments? (maybe mobile users drop at payment more than desktop)
- Has a recent change improved or worsened conversion at a specific step?

### Funnel Metrics You Should Know
| Metric | Formula |
|---|---|
| Step Conversion Rate | Users completing step N / Users entering step N |
| Overall Conversion Rate | Users completing final step / Users entering first step |
| Drop-off Rate | 1 - Step Conversion Rate |
| Time to Convert | Average time from first step to last step |

### How to Analyse a Funnel — Framework
1. **Define the funnel steps** precisely — what counts as "viewed product"? A 1-second page load or 10 seconds of engagement?
2. **Calculate conversion at each step**
3. **Identify the biggest drop-off** — that's your biggest opportunity
4. **Segment the funnel** — by device, channel, region, user type — does drop-off differ across segments?
5. **Hypothesise causes** — UX problem? Slow page load? Price shock? Lack of trust signals?
6. **Recommend action** — A/B test a fix at that specific step

### Connecting to Your Internship
> "In my internship I worked with order data for an e-commerce client. While I didn't explicitly build a funnel, I analysed conversion-adjacent metrics — like how many customers who added items eventually completed an order — and segmented them by channel (website vs app vs offline) to identify where the app was outperforming."

---

## 2. Churn Analysis

### What is Churn?
Churn is when a customer stops using your product or service. For subscription businesses it's explicit (they cancel). For transactional businesses like e-commerce, it's implicit (they just stop buying).

### Churn Rate Formula
```
Churn Rate = Customers Lost in Period / Customers at Start of Period × 100
```

**Example:**
- Start of month: 10,000 customers
- Lost during month: 500
- Churn Rate = 500 / 10,000 × 100 = **5% monthly churn**

### Types of Churn
| Type | Description | Example |
|---|---|---|
| Customer Churn | Customer stops buying/using | User cancels Netflix |
| Revenue Churn | Revenue lost from churned customers | Lost ₹2L MRR this month |
| Gross Churn | Total revenue lost, no offset | ₹2L lost |
| Net Churn | Gross churn minus expansion revenue | ₹2L lost, ₹0.5L gained from upgrades → Net ₹1.5L |
| Voluntary Churn | Customer chose to leave | Cancelled subscription |
| Involuntary Churn | Payment failure, expiry | Credit card lapsed |

### Why Does Churn Matter?
- Acquiring a new customer costs 5–7× more than retaining an existing one
- High churn destroys LTV (Lifetime Value) — you spend to acquire but don't recoup
- Churn compounds — 5% monthly churn = ~46% annual churn (almost half your base gone yearly)

### Churn for Transactional Businesses (like your retail client)
There's no explicit "cancel." You define a churn window:
> "A customer is churned if they have not made a purchase in the last 90 days."

This is called **dormancy-based churn** or **inactivity churn**. The threshold depends on your business's natural purchase frequency.

### SQL — Identifying Churned Customers
```sql
-- Customers who haven't ordered in the last 90 days (churned)
SELECT
    c.customer_id,
    c.name,
    c.tier,
    MAX(o.order_date) AS last_order_date,
    CURRENT_DATE - MAX(o.order_date) AS days_since_last_order
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
    AND o.status = 'completed'
GROUP BY c.customer_id, c.name, c.tier
HAVING MAX(o.order_date) < CURRENT_DATE - INTERVAL '90 days'
    OR MAX(o.order_date) IS NULL  -- never ordered at all
ORDER BY days_since_last_order DESC;
```

### Churn Drivers — How to Find Them
Segment churned customers and compare to retained ones:
- **Demographics:** Which tier, region, city churns most?
- **Behaviour:** Did churned customers have fewer orders before churning? Lower order values?
- **Experience:** Did churned customers have more returns? More complaints?
- **Recency:** Were they acquired during a discount campaign (promo-hunters, not loyal)?

### Reducing Churn — Analyst's Role
1. Identify at-risk customers *before* they churn (predictive — advanced, but the concept is important)
2. Understand *why* they churned — survey data, return reasons, support tickets
3. Segment churned users — high-value churners need different treatment than low-value ones
4. Recommend interventions — win-back campaigns, loyalty programs, product fixes

---

## 3. Cohort Analysis

### What is a Cohort?
A cohort is a group of users who share a common starting event in the same time period — usually the month they first purchased or signed up.

**Example:**
- **January 2025 cohort** = all customers who made their first purchase in January 2025
- **February 2025 cohort** = all customers who made their first purchase in February 2025

### Why Cohort Analysis?
It separates the *quality* of customers acquired in different periods. If you just look at overall retention, a flood of new low-quality users in Month 3 can mask that your Month 1 users were actually churning faster. Cohort analysis fixes this by following each group through time independently.

### Cohort Retention Table — What It Looks Like

| Cohort | Month 0 | Month 1 | Month 2 | Month 3 | Month 4 |
|---|---|---|---|---|---|
| Jan 2025 | 1,000 (100%) | 420 (42%) | 280 (28%) | 210 (21%) | 180 (18%) |
| Feb 2025 | 1,200 (100%) | 540 (45%) | 370 (31%) | 260 (22%) | — |
| Mar 2025 | 950 (100%)  | 380 (40%) | 260 (27%) | — | — |

**Reading it:**
- Month 0 = the cohort's acquisition month (always 100%)
- Month 1 = what % of that cohort came back and bought again the next month
- Feb cohort retained better at Month 1 (45% vs 42%) — maybe a better acquisition campaign?

### How to Build a Cohort Table in SQL

```sql
-- Step 1: Find each customer's first purchase month (cohort assignment)
WITH first_purchase AS (
    SELECT
        customer_id,
        DATE_TRUNC('month', MIN(order_date)) AS cohort_month
    FROM orders
    WHERE status = 'completed'
    GROUP BY customer_id
),

-- Step 2: Join back to all orders, calculate months since cohort
order_with_cohort AS (
    SELECT
        o.customer_id,
        fp.cohort_month,
        DATE_TRUNC('month', o.order_date) AS order_month,
        EXTRACT(YEAR FROM AGE(
            DATE_TRUNC('month', o.order_date),
            fp.cohort_month
        )) * 12 +
        EXTRACT(MONTH FROM AGE(
            DATE_TRUNC('month', o.order_date),
            fp.cohort_month
        )) AS months_since_cohort
    FROM orders o
    JOIN first_purchase fp ON o.customer_id = fp.customer_id
    WHERE o.status = 'completed'
)

-- Step 3: Count active users per cohort per month
SELECT
    cohort_month,
    months_since_cohort,
    COUNT(DISTINCT customer_id) AS active_customers
FROM order_with_cohort
GROUP BY cohort_month, months_since_cohort
ORDER BY cohort_month, months_since_cohort;
```

Then pivot this in Python/Excel to get the table format above.

### Cohort Analysis in Python (pivot)
```python
# After pulling the SQL result into df:
cohort_pivot = df.pivot_table(
    index='cohort_month',
    columns='months_since_cohort',
    values='active_customers',
    aggfunc='sum'
)

# Convert to retention % (divide each row by Month 0 value)
cohort_size = cohort_pivot[0]
retention_table = cohort_pivot.divide(cohort_size, axis=0) * 100
retention_table = retention_table.round(1)

import seaborn as sns
import matplotlib.pyplot as plt
sns.heatmap(retention_table, annot=True, fmt='.0f', cmap='Blues')
plt.title('Cohort Retention (%) by Month')
plt.tight_layout()
```

### What Good vs Bad Cohort Retention Looks Like
- **Good:** Retention stabilises after Month 2–3 (a "smile curve") — there's a loyal core
- **Bad:** Retention keeps dropping with no flattening — you have no loyal base, just one-time buyers
- **Improving:** Later cohorts have higher Month 1 retention than earlier ones — product/experience is getting better

### Connecting to Your Internship
> "Cohort analysis wasn't something I formally built, but the concept directly connects to work I did. When I found that Bronze-tier customers had 2× the return rate of Gold-tier customers, one possible explanation is that Bronze customers were acquired later through discount campaigns — a cohort-quality difference. Cohort analysis would be the natural next step to test that hypothesis."

---

## 4. A/B Testing

### What is A/B Testing?
A/B testing (also called split testing) is a controlled experiment where you show two versions of something to two randomly split groups — **Group A (control)** sees the existing version, **Group B (treatment)** sees the changed version. You measure which performs better on a defined metric.

### Why A/B Testing?
Without it, you're guessing. A/B testing gives causal evidence — you can say "this change *caused* the improvement" because everything else was held constant. It eliminates the influence of seasonality, external events, and user mix differences.

### Classic Example
- **Hypothesis:** Changing the checkout button from "Proceed" to "Place Order Now" will increase checkout completion rate
- **Control (A):** Button says "Proceed"
- **Treatment (B):** Button says "Place Order Now"
- **Metric:** Checkout completion rate (users who click → complete payment / users who reach checkout)
- **Run for:** 2 weeks, equal random split

### Key Terms

| Term | Meaning |
|---|---|
| Control Group | Group A — sees existing version |
| Treatment Group | Group B — sees new version |
| Hypothesis | What you expect to happen and why |
| Primary Metric | The metric the test is designed to improve |
| Guardrail Metric | A metric you must NOT make worse (e.g. don't improve clicks but hurt revenue) |
| Statistical Significance | Confidence that the result isn't due to random chance |
| p-value | Probability of seeing this result by chance if there's actually no difference; p < 0.05 means significant |
| Confidence Interval | Range of values where the true effect likely falls |
| Sample Size | How many users you need to detect a real difference |
| Minimum Detectable Effect (MDE) | The smallest improvement worth detecting |

### The A/B Testing Process
```
1. Define the problem → what are you trying to improve?
2. Form a hypothesis → if we change X, then Y will improve because Z
3. Define the primary metric + guardrail metrics
4. Calculate required sample size (based on baseline rate, MDE, significance level)
5. Randomly split users into control and treatment
6. Run the experiment for the required duration
7. Analyse results — is the difference statistically significant?
8. Make a decision — ship, don't ship, or run further tests
```

### Common Mistakes in A/B Testing
| Mistake | Why It's Bad |
|---|---|
| Stopping the test early when you see a good result | Inflates false positive rate — results aren't stable yet |
| Not randomising properly | Biased groups → invalid results |
| Testing too many things at once | Can't isolate which change caused the effect |
| Ignoring guardrail metrics | Treatment improves clicks but hurts revenue — net negative |
| Running during atypical periods | Festive season, sale events skew results |
| Small sample size | Underpowered — real differences won't be detectable |

### Statistical Significance — Simple Explanation
> "If we flip a coin 10 times and get 7 heads, that could just be luck. If we flip it 10,000 times and get 7,000 heads, it's almost certainly biased. Statistical significance is about having enough data that the result is unlikely to be due to random chance. We typically use p < 0.05 — meaning there's less than a 5% probability this result happened by chance."

### A/B vs Multivariate Testing
- **A/B test:** Two versions, one variable changed — clean, easy to interpret
- **A/B/n test:** More than two versions tested simultaneously
- **Multivariate test:** Multiple variables changed simultaneously — can find interactions but needs much larger sample sizes

### How to Talk About A/B Testing Without Doing It
> "I didn't run A/B tests directly in my internship — we worked with historical observational data. But the concept is directly relevant to my work. When I found that the App channel was growing 22% MoM versus the Website, a natural next question was: is the App inherently better, or are the users who choose to use the App just different? To answer that properly, you'd need an A/B test — randomise users into App vs Web versions. Without randomisation, you can't separate the channel effect from user self-selection."

---

## 5. Retention & Engagement

### What is Retention?
Retention measures how many users come back after their first visit/purchase. It's the most important signal of product-market fit.

```
Retention Rate (Day N) = Users who return on Day N / Users acquired on Day 0 × 100
```

### Retention Types
| Type | What It Measures |
|---|---|
| Day 1 Retention | % who come back the next day |
| Day 7 Retention | % who come back within a week |
| Day 30 Retention | % who come back within a month |
| Month 1 Retention | Cohort-level — % who transact in Month 1 after acquisition |

### The Retention Curve
```
100% ──┐
       │\
       │  \
       │    \___
       │        \____
       │              \__________
       └────────────────────────── Time
```
- **Steep drop early, then stabilises** — good. There's a retained core.
- **Keeps dropping with no flattening** — bad. No loyal user base.

### Engagement Metrics
Engagement = how actively users interact with your product. Varies by business type.

| Metric | Formula | Used By |
|---|---|---|
| DAU | Daily Active Users | Apps, social |
| WAU | Weekly Active Users | Tools, SaaS |
| MAU | Monthly Active Users | E-commerce, media |
| DAU/MAU Ratio | Stickiness — how often monthly users engage daily | Apps |
| Session Duration | Average time per session | Media, apps |
| Pages per Session | Avg pages viewed | E-commerce |
| Feature Adoption Rate | % of users using a specific feature | SaaS |

### DAU/MAU Ratio — Stickiness
```
Stickiness = DAU / MAU × 100
```
- **Facebook-level:** ~65% — users come back almost every day
- **Good:** 20–30%
- **Low:** < 10% — users come rarely, churn risk is high

### Connecting to Your Internship
> "In my internship we looked at repeat purchase behaviour — what % of customers who ordered in one month came back the next. That's essentially monthly retention for an e-commerce context. The Gold-tier customers had much higher repeat rates than Bronze-tier, which reinforced why they drove 52% of revenue despite being only 18% of the customer base."

---

## 6. Revenue & Business Metrics

### Core Revenue Metrics

**Gross Revenue vs Net Revenue**
```
Gross Revenue = Total order value before discounts
Net Revenue   = Gross Revenue - Discounts - Returns
```
Always clarify which one when someone says "revenue."

**GMV — Gross Merchandise Value**
Total value of all goods sold through a platform — used by marketplaces (Swiggy, Flipkart) where not all revenue belongs to the platform.
```
GMV ≠ Revenue. GMV = total transaction value. Revenue = platform's take (commission/fee).
```

**ARR / MRR — Annual/Monthly Recurring Revenue**
For subscription businesses.
```
MRR = Number of paying subscribers × Average subscription price per month
ARR = MRR × 12
```

**Revenue Growth Rate**
```
MoM Growth = (This Month Revenue - Last Month Revenue) / Last Month Revenue × 100
YoY Growth  = (This Year Revenue - Last Year Revenue) / Last Year Revenue × 100
```

### Profitability Metrics

**Gross Margin**
```
Gross Margin = (Revenue - Cost of Goods Sold) / Revenue × 100
```
If a phone sells for ₹20,000 and costs ₹13,000 to source: Gross Margin = 35%

**Contribution Margin**
Revenue minus variable costs (not fixed costs). Tells you how much each sale contributes to covering fixed costs and profit.

**EBITDA**
Earnings Before Interest, Taxes, Depreciation, Amortisation — a proxy for operational profitability. Used at the company level, not individual product level.

---

## 7. Customer Metrics

### CAC — Customer Acquisition Cost
```
CAC = Total Marketing + Sales Spend / Number of New Customers Acquired
```
If you spent ₹5 lakhs on ads and acquired 500 customers: CAC = ₹1,000

### LTV — Lifetime Value (also CLV — Customer Lifetime Value)
How much revenue a customer generates over their entire relationship with you.

**Simple LTV:**
```
LTV = Average Order Value × Purchase Frequency × Customer Lifespan
```

**Example:**
- Avg Order Value: ₹3,500
- Purchase Frequency: 4 orders/year
- Customer Lifespan: 3 years
- LTV = 3,500 × 4 × 3 = **₹42,000**

### LTV:CAC Ratio
The most important unit economics metric.
```
LTV:CAC = LTV / CAC
```
- **< 1:** You're losing money on every customer. Unsustainable.
- **1–3:** Breakeven or thin margins.
- **> 3:** Healthy — you're making 3× back what you spent to acquire.

### ARPU — Average Revenue Per User
```
ARPU = Total Revenue / Total Active Users
```
Useful for tracking whether you're extracting more or less value per user over time.

### NPS — Net Promoter Score
A customer satisfaction metric. Users are asked: *"How likely are you to recommend us to a friend? (0–10)"*
- **Promoters:** 9–10
- **Passives:** 7–8
- **Detractors:** 0–6

```
NPS = % Promoters - % Detractors
```
Ranges from -100 to +100. Above 50 is excellent. Below 0 is a serious problem.

### CSAT — Customer Satisfaction Score
```
CSAT = (Satisfied respondents / Total respondents) × 100
```
Usually measured right after a transaction or support interaction.

### Connecting to Your Internship
> "In my returns analysis I was essentially building the groundwork for LTV analysis — understanding which customer tiers drove the most revenue, which churned fastest, and which had the highest return rates. Gold-tier customers had lower return rates and higher order values, meaning their LTV was disproportionately higher than their 18% share of the customer base suggested."

---

## 8. Product Analytics Concepts

### North Star Metric
A single metric that best captures the core value your product delivers. Everything else is in service of moving this number.

| Company | North Star Metric |
|---|---|
| Spotify | Time spent listening |
| Airbnb | Nights booked |
| WhatsApp | Messages sent per day |
| E-commerce (your client) | Could be: GMV, repeat purchase rate, or active buyers/month |

Why it matters: without a North Star, teams optimise for different things and pull in different directions.

### AARRR Framework (Pirate Metrics)
A framework for thinking about the user journey and where to focus.

| Stage | What it measures | Example Metric |
|---|---|---|
| **A**cquisition | How do users find you? | New visitors, CAC |
| **A**ctivation | Do they have a good first experience? | First purchase conversion |
| **R**etention | Do they come back? | Day 30 retention |
| **R**eferral | Do they tell others? | NPS, referral rate |
| **R**evenue | Do they pay? | ARPU, LTV |

### RFM Analysis — Recency, Frequency, Monetary
A classic customer segmentation method used in e-commerce.

| Dimension | Question | Measure |
|---|---|---|
| **R**ecency | How recently did they buy? | Days since last order |
| **F**requency | How often do they buy? | Number of orders in a period |
| **M**onetary | How much do they spend? | Total or avg order value |

Customers are scored on each dimension (e.g. 1–5) and segmented:
- **High R, High F, High M** = Champions (most valuable, protect these)
- **High R, Low F, Low M** = New customers (nurture)
- **Low R, High F, High M** = At-risk / lapsed high-value (win-back priority)
- **Low R, Low F, Low M** = Lost (deprioritise)

This directly connects to the tier system in your internship — Gold/Silver/Bronze is essentially a simplified RFM segmentation.

### Product-Market Fit (PMF)
The degree to which a product satisfies a strong market demand. Sean Ellis's test: if > 40% of users say they'd be "very disappointed" if they could no longer use the product, you likely have PMF. Data signal: retention curve flattens (there's a retained core).

### Session, Event, Pageview
- **Session:** A single visit — starts when a user lands, ends after inactivity (typically 30 min)
- **Event:** A specific action within a session — click, scroll, add to cart, play video
- **Pageview:** Loading a specific page — a session can have many pageviews

### Conversion Rate Optimisation (CRO)
The practice of systematically improving the percentage of users who take a desired action — through UX changes, copy changes, personalisation, A/B testing. An analyst's role: measure baseline conversion, identify drop-off points, size the opportunity, validate changes with A/B tests.

---

## 9. Data Strategy Concepts

### OLTP vs OLAP
| | OLTP | OLAP |
|---|---|---|
| Full name | Online Transaction Processing | Online Analytical Processing |
| Purpose | Day-to-day operations | Analytics and reporting |
| Query type | Many small, fast reads/writes | Few large, complex reads |
| Example | Your PostgreSQL orders DB | A data warehouse |
| Optimised for | Write speed | Read/query speed |

### Data Warehouse vs Database
- **Database (OLTP):** Optimised for transactions — inserts, updates, deletes. Row-oriented storage. Your PostgreSQL is this.
- **Data Warehouse:** Optimised for analytics — large-scale aggregations across historical data. Column-oriented storage. Examples: Snowflake, BigQuery, Redshift.

In practice: transactional data flows from the database → ETL process → data warehouse → analysts query the warehouse.

### ETL vs ELT
- **ETL (Extract, Transform, Load):** Data is transformed *before* loading into the warehouse. Traditional approach.
- **ELT (Extract, Load, Transform):** Data is loaded raw first, then transformed inside the warehouse. Modern approach — possible because warehouses like BigQuery are powerful enough to handle transformations.

### Data Governance
Policies and standards that ensure data is accurate, consistent, secure, and properly used. As an analyst this means: using agreed metric definitions, not sharing sensitive data outside approved channels, documenting your work so others can reproduce it.

### Data Quality Dimensions
| Dimension | What it means |
|---|---|
| Accuracy | Does the data reflect reality? |
| Completeness | Are there missing values? |
| Consistency | Does the same entity have consistent values across tables? |
| Timeliness | Is the data fresh enough for the use case? |
| Uniqueness | Are there duplicates? |
| Validity | Are values within expected ranges and formats? |

### Connecting to Your Internship
> "In my internship, data quality was a real issue — I found 47 negative order values that were clearly data entry errors, and about 3% of records had null region values from before the field was added to the system. These are Completeness and Validity issues in data quality terms. I documented each issue and flagged the negative values to the engineering team, which is exactly what data governance practice would require."

---

## 10. Quick-Fire Definitions

| Term | One-Liner |
|---|---|
| Funnel | Sequence of steps users take toward a goal; users drop off at each step |
| Conversion Rate | % of users who complete a desired step or action |
| Drop-off Rate | % of users who leave at a specific funnel step |
| Churn | Customers who stop buying or using the product |
| Churn Rate | Customers lost / customers at start of period × 100 |
| Cohort | Group of users who share a starting event in the same time period |
| Cohort Retention | % of a cohort that returns to buy/use in subsequent periods |
| A/B Test | Controlled experiment comparing two versions to find which performs better |
| Control Group | Group A — sees the existing version in an A/B test |
| Treatment Group | Group B — sees the new version in an A/B test |
| Statistical Significance | Confidence the result isn't due to random chance (p < 0.05) |
| p-value | Probability of observing the result by chance if no real difference exists |
| Retention | % of users who return after their first visit/purchase |
| DAU/MAU | Daily/Monthly Active Users — stickiness = DAU/MAU ratio |
| GMV | Gross Merchandise Value — total transaction value on a platform |
| MRR | Monthly Recurring Revenue — used by subscription businesses |
| CAC | Customer Acquisition Cost — marketing spend / new customers |
| LTV | Lifetime Value — total revenue from a customer over their relationship |
| LTV:CAC | Unit economics ratio; > 3 is healthy |
| ARPU | Average Revenue Per User |
| NPS | Net Promoter Score = % Promoters - % Detractors; ranges -100 to +100 |
| CSAT | Customer Satisfaction Score — % satisfied after an interaction |
| North Star Metric | Single metric that best captures core product value |
| AARRR | Acquisition, Activation, Retention, Referral, Revenue framework |
| RFM | Recency, Frequency, Monetary — customer segmentation method |
| PMF | Product-Market Fit — product satisfies a strong market need |
| CRO | Conversion Rate Optimisation — systematically improving conversion |
| OLTP | Online Transaction Processing — operational database |
| OLAP | Online Analytical Processing — analytical / warehouse queries |
| ETL | Extract, Transform, Load — data pipeline pattern |
| Data Warehouse | Analytical data store optimised for large read queries |
| Data Governance | Policies ensuring data is accurate, consistent, secure, and properly used |
| Gross Margin | (Revenue - COGS) / Revenue × 100 |
| Stickiness | DAU/MAU ratio — how often monthly users engage daily |
