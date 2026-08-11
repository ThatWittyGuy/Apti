# Interview Prep Guide — Data / Business Analyst Fresher
**Globant Internship | Jan 2026 – Apr 2026**
All behavioural answers follow the STAR format: **S**ituation → **T**ask → **A**ction → **R**esult

---

## Table of Contents
1. [Self-Introduction](#1-self-introduction)
2. [HR & Motivation Questions](#2-hr--motivation-questions)
3. [Behavioural Questions — STAR Answers](#3-behavioural-questions--star-answers)
4. [Internship Deep-Dive Questions](#4-internship-deep-dive-questions)
5. [SQL Interview Questions](#5-sql-interview-questions)
6. [Python & Pandas Questions](#6-python--pandas-questions)
7. [Data Analysis Concepts](#7-data-analysis-concepts)
8. [Statistics Basics](#8-statistics-basics)
9. [Business Analyst Questions](#9-business-analyst-questions)
10. [Case Study Questions](#10-case-study-questions)
11. [Questions to Ask the Interviewer](#11-questions-to-ask-the-interviewer)
12. [Quick-Fire Definitions Table](#12-quick-fire-definitions-table)

---

## 1. Self-Introduction

**Formula:** Background → Internship → What you did → What you learned → What you want

> "I'm a final-year [degree] student from [college], Pune. During my internship at Globant India, I was part of the Analytics & Insights team supporting a retail e-commerce client in the consumer electronics space. Over three months I worked on SQL-based reporting, exploratory data analysis in Python using Pandas and Seaborn, PostgreSQL query optimisation through indexing, and an independent returns trend analysis where I identified the key drivers behind rising return rates. That experience gave me a solid foundation in working with real business data — cleaning it, querying it, analysing it, and translating findings into clear recommendations. I'm now looking for a full-time data or business analyst role where I can do exactly that."

**Keep it to 60–90 seconds. End with what you want, not just what you did.**

---

## 2. HR & Motivation Questions

---

**Q: Why do you want to be a Data Analyst?**

> "Honestly, the internship confirmed it for me. I went in thinking I'd enjoy the technical side — the SQL, the Python — and I did. But what I didn't expect was how satisfying it would be to take a business question, dig through the data, and find something actually useful. When I did the returns analysis and found that one region had a disproportionately high return rate because of a logistics issue — and the business team acted on that recommendation — it felt meaningful in a way that just writing code doesn't. Data analysis sits at the intersection of logical thinking and business impact, and that's exactly where I want to be."

---

**Q: Why this company / this role?**

> Tailor this to the company. General frame:
> "I'm looking for a role where I can work with real business data — not just dashboards someone else built, but getting into the data, understanding the problem, and building the analysis from scratch. From what I've read about [company], [something specific about them]. Given my internship experience with SQL, Python, and working in a client-facing analytics context, I think I can contribute quickly."

---

**Q: What is your biggest strength?**

> "I'm good at breaking down ambiguous problems into concrete, answerable questions. During my returns analysis at Globant, I was handed a vague business concern — 'returns are going up' — and had to independently decide what metric to use, how to segment it, and what cuts would be most informative. That kind of structured thinking — going from a fuzzy question to a clear analytical approach — is something I do naturally and enjoy."

---

**Q: What is your biggest weakness?**

> "Early in my internship I was focused on getting the analysis right but not always on documenting it clearly for others. My mentor pointed out that an undocumented Jupyter notebook is only useful to the person who wrote it. Since then I've been deliberate about adding comments, writing clean variable names, and updating team documentation whenever I finish a piece of work. It's become a habit."

---

**Q: Where do you see yourself in 3 years?**

> "I want to grow from executing analyses to shaping the questions themselves — being the person who tells the business what to look at, not just answering what they ask. In three years I'd like to be in a senior analyst or team lead role, with strong enough domain knowledge that I can spot the right questions before stakeholders even think to ask them. But right now I want to build those fundamentals in a team with good data culture."

---

**Q: Why should we hire you over other freshers?**

> "Most freshers have done SQL in coursework and maybe one or two college projects. I've spent three months doing it in a professional environment — with real messy data, real business stakeholders, and real consequences for getting it wrong. I've navigated data quality issues, reproduced existing reports to validate my understanding, done independent EDA, and optimised slow queries. I know what it's like to present a finding to someone who doesn't know what a LEFT JOIN is and still make it make sense to them."

---

## 3. Behavioural Questions — STAR Answers

---

**Q: Tell me about a time you solved a difficult problem.**

- **S:** In my third month at Globant, dashboard queries that fed into a client Tableau report were running very slowly — some taking 12–14 seconds. This was affecting business users who reviewed the dashboard in their weekly meetings.
- **T:** My mentor asked me to investigate the performance bottleneck and fix it.
- **A:** I first learned to use `EXPLAIN ANALYZE` in PostgreSQL, which shows the query execution plan. I could see the queries were doing full table scans (Seq Scan) across 4 million rows. I researched indexing, then created a composite index on `(status, order_date)` — the two columns most commonly filtered together — and a separate index on `order_id` in the order_items table to speed up joins. I also rewrote one inefficient query that was joining first and filtering after, by moving the filter into a CTE so it ran before the join.
- **R:** The main query went from ~14 seconds to under 2 seconds on the dev database — roughly an 85% reduction. In production terms, after accounting for concurrent load, it translated to a 30–40% improvement in response times. The business users noticed the dashboard was faster.

---

**Q: Tell me about a time you worked with incomplete or messy data.**

- **S:** During my EDA on the customer orders dataset at Globant, I loaded about 2.1 million order records from PostgreSQL into Python for analysis.
- **T:** Before I could run any meaningful analysis, I needed to understand the data quality issues and handle them appropriately.
- **A:** I ran a full audit first — checked shapes, data types, null counts, and duplicates. I found three issues: about 3% of orders had a null `region` field (the field wasn't collected before mid-2024), 47 rows had negative `total_amount` values which were clearly data entry errors, and some `total_amount` values were extreme outliers. For nulls in `region`, I filled with `'Unknown'` and included them in reporting with a note — dropping them would have biased regional analysis. The 47 negative values I excluded after documenting the decision. For outliers I flagged them separately rather than dropping — some were genuine high-value bulk orders. I documented every decision and its reasoning.
- **R:** The cleaned dataset was used for the full EDA that the marketing team used in their quarterly review. My mentor also noted that the flagged data quality issues — especially the negative values — were something the engineering team hadn't caught, and they raised a ticket to fix the source data entry validation.

---

**Q: Tell me about a time you worked in a team / collaborated.**

- **S:** During my internship, I was building SQL queries that produced CSV outputs for a Tableau dashboard another team member was building. We were working in parallel — I didn't know exactly how he was using the data, and he didn't know exactly what columns I was planning to output.
- **T:** I needed to make sure the format and naming of my outputs matched what his Tableau connection expected, without us being in constant back-and-forth.
- **A:** Before writing the final query, I exported a 10-row sample CSV and shared it with him for review. We had a quick 15-minute sync where he pointed out that he needed the date column formatted as `YYYY-MM-DD` (not the default timestamp format) and that the column named `rev` needed to be `revenue` to match his existing Tableau field names. We also agreed to put any format decisions into a shared Confluence page.
- **R:** The integration worked first time when I handed over the final output. More importantly, when I later added a new column, I updated the Confluence doc first — so there were no surprises. It was a small thing but it taught me that coordination before writing code saves more time than fixing mismatches after.

---

**Q: Tell me about a time you showed initiative / went beyond what was asked.**

- **S:** In Month 1, I was given the task of reproducing an existing sales report using SQL — essentially a validation exercise to check that I understood the schema correctly.
- **T:** The task was just to match the existing numbers. Nothing more was asked.
- **A:** I matched all the numbers correctly, but while doing it I noticed that the existing report included `pending` orders in the total, which was inflating the revenue figure. Completed orders only would give a more accurate picture. I also noticed the report had no regional breakdown — just a national total — even though the data had a `region` column. I extended my SQL to produce a version with regional segmentation and flagged the `pending` orders issue to my mentor with a query showing the difference.
- **R:** My mentor appreciated both findings. The `pending` orders issue was corrected in the report going forward. The regional breakdown I built was eventually incorporated into the formal monthly report template. My mentor mentioned it in my mid-internship feedback as an example of thinking like an analyst, not just a developer.

---

**Q: Tell me about a time you had to present or explain data to a non-technical person.**

- **S:** After completing the returns trend analysis, I had to present my findings to the analytics team and a business stakeholder from the client's operations side who had no technical background.
- **T:** I needed to communicate what I found — the rising return rate, which categories and regions were worst, and what was driving it — in a way that was clear and actionable without mentioning SQL, CTEs, or heatmaps.
- **A:** I built a one-page summary with three sections: "What we found" (the return rate rose from 6.2% to 9.8% in 3 months), "Where it's happening" (Accessories category, North region), and "What we recommend" (supplier quality checks, investigate logistics partner). For the visual, I included the heatmap but explained it in plain English: "darker red means more returns, and you can see North is darker across almost every row." I avoided technical jargon entirely and framed everything around business decisions.
- **R:** The client stakeholder said it was "the clearest analysis report they'd received." He immediately asked for a follow-up on the North region logistics issue, which became a separate workstream. My mentor used the one-pager format as a template for future reports.

---

**Q: Tell me about a time you made a mistake and how you handled it.**

- **S:** Early in Month 2, I was cross-checking my SQL output against an existing report and my regional revenue numbers didn't match. I assumed the discrepancy was a data issue and flagged it as a data quality problem in a Confluence note without digging deeper.
- **T:** My mentor noticed the note and asked me to investigate further before escalating.
- **A:** When I traced it properly, I found that my own SQL had a `INNER JOIN` between orders and order_items — which dropped orders that had no line items in the order_items table (a small data gap). The original report was using a different query that handled this differently. The data wasn't wrong — my join logic was. I corrected the query to a `LEFT JOIN`, confirmed the numbers matched, and updated the Confluence note to reflect that the issue was in my analysis, not the source data.
- **R:** The numbers aligned. I learned two things: first, always exhaust your own logic before escalating; second, LEFT vs INNER JOIN is not a minor syntax choice — it fundamentally changes what's included in your results. I haven't made that particular mistake again.

---

**Q: Tell me about a time you managed multiple tasks / prioritised.**

- **S:** In Month 2, I had two concurrent tasks: completing the customer behaviour EDA (a longer analysis piece) and responding to an ad-hoc request from a stakeholder who needed a quick breakdown of December sales by category before an end-of-month meeting in two days.
- **T:** I needed to deliver the ad-hoc request on time without completely dropping the EDA.
- **A:** I assessed both tasks. The ad-hoc request was simpler — three SQL queries I could write and share as a formatted Excel table in a few hours. The EDA was a week-long piece with no hard deadline that week. I did the ad-hoc request first thing the next morning, delivered it that afternoon, then returned to the EDA. I also flagged to my mentor that I'd context-switched so he was aware.
- **R:** The stakeholder got their December breakdown before the meeting. The EDA was completed on schedule. My mentor noted that telling him about the context switch was the right call — it meant he didn't wonder why EDA progress had slowed for a day.

---

## 4. Internship Deep-Dive Questions

---

**Q: Walk me through your internship end to end.**

> "Three months, three broad phases. Month 1 was foundation — I set up my environment, learned the PostgreSQL schema for a retail e-commerce client (five tables: customers, orders, order_items, products, returns), and wrote SQL queries to reproduce an existing monthly sales report as a learning exercise. Month 2 was analysis — I did a full customer behaviour EDA in Python using Pandas and Seaborn, exploring ~2.1 million order records to understand revenue by customer tier, regional performance, and order trends. Month 3 was optimisation and independent analysis — I diagnosed and fixed slow-running queries using EXPLAIN ANALYZE and indexing, bringing a 14-second query down to under 2 seconds, and independently led a returns trend analysis that identified why return rates were climbing and made business recommendations."

---

**Q: What tools and technologies did you use?**

> "PostgreSQL as the database, DBeaver as the query interface. Python — specifically Pandas for data manipulation, NumPy for numerical operations, Matplotlib and Seaborn for visualisation, and psycopg2 for connecting Python to PostgreSQL. All analysis was done in Jupyter Notebook. Git for version control. Excel and Confluence for documentation and sharing outputs with non-technical stakeholders. I also had exposure to Tableau at a data-feed level — I understood how the CSVs I produced connected to the dashboards."

---

**Q: You mentioned a 30–40% improvement in API response times. How did you measure that?**

> "I benchmarked using EXPLAIN ANALYZE before and after applying the optimisations. On the dev database, the specific query went from around 14 seconds to under 2 seconds — that's an 85% improvement in isolation. The 30–40% figure in my resume is a conservative estimate for the production environment, where you also have concurrent queries, connection overhead, and network latency that reduce the relative gain. I wanted to be honest about what I could actually attribute to my changes versus environmental factors."

---

**Q: Did you work with real data? What about data privacy?**

> "I worked on a sandboxed development database — a replica of the production schema with anonymised data. Customer names and emails were masked, and I never had direct access to the production database. This is standard practice. The dev DB was refreshed every week from production, so the structure and volume were realistic, but the PII was not real."

---

**Q: What was the most challenging part of your internship?**

> "The returns analysis was the hardest because there was no prescribed approach. Every other task had a clear brief — reproduce this report, clean this dataset, fix this query. The returns analysis was: 'return rates are going up, figure out why.' I had to define the metric, choose the dimensions to cut by, and decide what level of depth was appropriate for a 3-month intern. The ambiguity was uncomfortable at first but I found that having a structured framework — define metric, cut by dimensions, find variance, form hypothesis, test, recommend — made the open-ended feel manageable."

---

**Q: What was the most impactful thing you did?**

> "Two things stand out. The query optimisation had immediate, visible impact — the dashboard loads faster, business users notice that every day. The returns analysis had slower but potentially more significant business impact — if the recommendations around the North region logistics partner and the Accessories supplier quality checks are acted on, the financial effect of reducing a 12% return rate in that category could be substantial. I obviously can't take credit for those outcomes yet, but the analysis that drove the recommendation was mine."

---

## 5. SQL Interview Questions

---

**Q: What is the difference between WHERE and HAVING?**

> "`WHERE` filters rows *before* aggregation — it works on individual row values. `HAVING` filters *after* aggregation — it works on the output of aggregate functions like `SUM()` or `COUNT()`. You can't use `HAVING` without a `GROUP BY`. Example: `WHERE total_amount > 5000` filters individual orders. `HAVING SUM(total_amount) > 1000000` filters regions where total revenue exceeds a crore."

---

**Q: What is the difference between INNER JOIN, LEFT JOIN, RIGHT JOIN?**

> "INNER JOIN returns only rows with a match in *both* tables. LEFT JOIN returns all rows from the left table — matched or not — with NULLs on the right side where there's no match. RIGHT JOIN is the mirror of LEFT. In practice I use INNER and LEFT most often. Classic example: to calculate return rates, I used a LEFT JOIN from orders to returns. Not every order has a return — if I used INNER JOIN, all orders without returns would disappear and I'd have nothing to put in the denominator."

---

**Q: What is a CTE? Why use it instead of a subquery?**

> "A CTE (Common Table Expression) is a named temporary result set defined with `WITH`. It exists only for that query. Benefits over subqueries: it's readable — you give the result set a meaningful name. It's reusable — you can reference the same CTE multiple times in one query. And it's debuggable — you can test each named block independently. During query optimisation I used CTEs specifically to filter rows *early* before the JOIN, reducing the row count that downstream operations had to handle."

---

**Q: What are window functions? Give an example.**

> "Window functions perform calculations across a set of related rows without collapsing them into one — unlike GROUP BY which loses the row-level detail. They use `OVER()`. I used `LAG()` to calculate month-over-month revenue growth — `LAG(revenue) OVER (ORDER BY month)` gives you the previous month's revenue on the same row, so you can calculate the percentage change without a self-join. I also used `RANK() OVER (PARTITION BY category ORDER BY revenue DESC)` to rank products within their category."

---

**Q: Difference between RANK() and DENSE_RANK()?**

> "Both assign ranks based on ORDER BY. With ties, `RANK()` skips — if two rows tie for rank 2, the next is 4. `DENSE_RANK()` doesn't skip — the next after the tie is 3. For business use cases like 'top N per group', DENSE_RANK is usually more intuitive since there are no gaps."

---

**Q: How do you handle NULL values in SQL?**

> "NULLs can't be compared with `=` — you must use `IS NULL` or `IS NOT NULL`. Most aggregate functions (`SUM`, `AVG`) ignore NULLs automatically, but `COUNT(*)` counts every row including NULLs. Two functions I used constantly: `COALESCE(col, 0)` — returns the first non-NULL value, so I used it to turn NULL return counts (from LEFT JOINs with no match) into 0. `NULLIF(value, 0)` — returns NULL if the value equals 0, which I used in division denominators to prevent divide-by-zero errors."

---

**Q: What is indexing? When would you create one?**

> "An index is a B-tree data structure in PostgreSQL that allows the database to locate rows matching a condition without scanning the entire table. I'd create one on columns frequently used in WHERE, JOIN, or ORDER BY — especially on large tables. The trade-off: faster reads, slower writes, more disk space. Don't index every column — only where there's an actual performance problem. I created a composite index on `(status, order_date)` in the orders table, which cut a 14-second query to under 2 seconds. Column order in a composite index matters: put the equality filter first."

---

**Q: What does EXPLAIN ANALYZE do?**

> "It runs the query and shows the actual execution plan — what operations PostgreSQL performed, in what order, and how long each took. Key things I looked for: whether it's doing a Seq Scan (reading every row — bad on large tables) or an Index Scan (using an index — fast). It also shows estimated vs actual row counts — a big difference means the query planner's statistics are stale and you should run `ANALYZE` on the table."

---

**Q: What is the difference between UNION and UNION ALL?**

> "`UNION` combines two result sets and removes duplicates — slower because it needs a dedup step. `UNION ALL` keeps all rows including duplicates — faster. If you know the two sets won't overlap (e.g., two non-overlapping date ranges), always use `UNION ALL`."

---

**Q: Write a query to find duplicate records.**

```sql
SELECT customer_id, COUNT(*) AS occurrences
FROM customers
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

---

**Q: What is the difference between DELETE, TRUNCATE, and DROP?**

> "`DELETE` removes specific rows — you can add WHERE, it's transactional (rollback-able), fires triggers. `TRUNCATE` removes all rows very fast but can't filter by condition — still transactional in PostgreSQL. `DROP` removes the entire table, schema and all. In analytics I mostly read data — but understanding these is important for database design and admin discussions."

---

**Q: What is a subquery? When would you use it vs a CTE?**

> "A subquery is a query nested inside another query. I'd use a CTE when the logic is complex enough to need a meaningful name, when the same intermediate result is referenced more than once, or when readability matters (code review, documentation). Subqueries are fine for simple, single-use cases. In production code, CTEs are almost always preferred for maintainability."

---

## 6. Python & Pandas Questions

---

**Q: How do you connect Python to PostgreSQL?**

> "I used `psycopg2` as the database driver, which gives you a connection object you can pass directly into `pd.read_sql()`. The connection takes host, database name, user, and password."

```python
import psycopg2, pandas as pd
conn = psycopg2.connect(host="localhost", database="orion_dev",
                        user="analyst_user", password="password")
df = pd.read_sql("SELECT * FROM orders WHERE status = 'completed'", conn)
conn.close()
```

---

**Q: How do you handle missing values in Pandas?**

> "First audit: `df.isnull().sum()`. Then decide per column based on context. For categoricals like `region`, I fill with `'Unknown'` — dropping would bias regional analysis. For key identifiers like `order_id`, nulls mean corrupted data — drop them. For numerics, fill with median (more robust than mean against outliers) or drop, depending on volume. Always document what you did and why — cleaning decisions need to be reproducible."

---

**Q: What is groupby().agg() and how is it better than groupby().sum()?**

> "`groupby().sum()` gives you one thing. `groupby().agg()` lets you apply multiple functions at once and name the output columns explicitly — much cleaner for real analysis."

```python
summary = df.groupby('region').agg(
    total_orders    = ('order_id',     'count'),
    total_revenue   = ('total_amount', 'sum'),
    avg_order_value = ('total_amount', 'mean')
).reset_index()
```

---

**Q: What is the difference between merge() and concat()?**

> "`merge()` is a SQL-style JOIN — combines two DataFrames on a key column, with `how='inner'/'left'` etc. `concat()` stacks DataFrames vertically (adding rows, like UNION) or horizontally (adding columns). I used `merge()` to join orders with customers on `customer_id`. I used `concat()` when combining two months of data into one time-series DataFrame."

---

**Q: How do you detect outliers in Python?**

> "IQR method — calculate Q1, Q3, IQR = Q3 - Q1. Flag values below Q1 - 1.5×IQR or above Q3 + 1.5×IQR. Also useful visually: box plots immediately show outliers as points beyond the whiskers. The action depends on context — I never drop outliers automatically. In my internship I flagged them separately and reported to the business, because some were genuine high-value bulk orders."

---

**Q: What is np.where()?**

> "A vectorised conditional — like an if-else applied across an entire array at once. Much faster than using `apply()` with a Python function. I used it to create a `revenue_bucket` column: Low / Medium / High based on thresholds."

```python
df['bucket'] = np.where(df['net_amount'] < 1000, 'Low',
               np.where(df['net_amount'] < 5000, 'Medium', 'High'))
```

---

**Q: What is a DataFrame?**

> "A 2-dimensional labelled data structure in Pandas — rows and columns, like a SQL table or an Excel sheet in memory. Each column has a data type (int, float, object, datetime). It's the primary data structure I worked with for all analysis."

---

**Q: What is the difference between apply() and vectorised operations?**

> "`apply()` runs a Python function row-by-row — flexible but slow because it's a Python loop. Vectorised operations like `df['col'] * 2` or `df['col'].str.upper()` work on the entire array using NumPy — much faster. Best practice: always prefer vectorised unless the logic genuinely can't be expressed that way."

---

## 7. Data Analysis Concepts

---

**Q: What is EDA and what steps do you follow?**

> "Exploratory Data Analysis — understanding the data before doing any formal analysis. My steps: check shape and data types → count nulls → compute descriptive statistics (mean, median, std, percentiles) → visualise distributions (histograms, box plots) → check for duplicates and outliers → explore relationships between variables. The goal is to build intuition — and catch data quality issues — before drawing any conclusions."

---

**Q: What is the difference between mean, median, and mode?**

> "Mean is the average — sensitive to outliers. Median is the middle value — robust to outliers. Mode is the most frequent value — mainly useful for categorical data. In my EDA, the mean order value was ₹3,847 but the median was ₹3,199. The gap told me the distribution was right-skewed — a few very high-value orders pulled the mean up. I reported the median to stakeholders as the more representative 'typical' order value."

---

**Q: What is data granularity?**

> "The level of detail in a dataset. `order_items` has one row per product per order — high granularity. A monthly summary table has one row per month — low granularity. Knowing the granularity of your data prevents double-counting. Classic pitfall: if you join `orders` and `order_items` without being careful, an order's `total_amount` gets repeated once per line item — and your revenue sum is inflated."

---

**Q: What is a KPI? Give examples from your internship.**

> "A Key Performance Indicator — a specific, measurable metric the business tracks to gauge health. From the retail client: Total Revenue (gross and net), Number of Orders, Average Order Value, Return Rate, Revenue by Region, Revenue by Category, New vs Repeat Customers. These were the metrics I built queries and reports around."

---

**Q: What is the difference between a data analyst and a business analyst?**

> "Data Analyst is more technical — extracting, cleaning, analysing data, building reports and dashboards. Business Analyst focuses more on understanding business requirements, mapping processes, and recommending solutions — they bridge business and technical teams. In practice fresher roles often combine both. My internship was DA-heavy (SQL, Python, EDA) but had BA elements too — like defining the returns metric myself, structuring the analysis framework, and writing recommendations for the business."

---

## 8. Statistics Basics

---

**Q: What is standard deviation?**

> "A measure of how spread out values are from the mean. Low SD means values cluster tightly; high SD means wide spread. In my EDA, the SD of order values was ₹2,914 against a mean of ₹3,847 — indicating high variability. Knowing this stopped me from treating the mean as representative without checking the distribution."

---

**Q: What is correlation? What is the difference between correlation and causation?**

> "Correlation measures the strength and direction of a linear relationship between two variables — ranges from -1 to +1. Causation means one variable directly causes the other. Correlation doesn't imply causation — two things can move together due to a third confounding variable. In analytics we say 'X is associated with Y' or 'X correlates with Y' rather than 'X causes Y' unless we have experimental or causal evidence."

---

**Q: What is a normal distribution?**

> "A bell-shaped, symmetric distribution around the mean. Many statistical methods assume normality. Business data like revenue is often right-skewed — a long tail of high values. I always check distributions with histograms before applying any statistical method, rather than assuming normality."

---

**Q: What is the IQR method for outlier detection?**

> "IQR = Q3 - Q1. Anything below Q1 - 1.5×IQR or above Q3 + 1.5×IQR is flagged as an outlier. It's more robust than using mean ± 2 standard deviations because it doesn't assume a normal distribution. I used it in my EDA to flag unusually high and low order values, then investigated each group before deciding what to do with them."

---

**Q: What is skewness?**

> "Skewness describes the asymmetry of a distribution. Right-skewed (positive skew) — a long tail to the right, mean > median. Left-skewed (negative skew) — tail to the left, mean < median. My order value data was right-skewed — a few very large orders pulled the mean above the median. Understanding skewness tells you which central tendency measure to use and whether standard statistical assumptions hold."

---

## 9. Business Analyst Questions

---

**Q: How do you translate a vague business question into a data question?**

> "The process I follow: first, understand what decision the business is trying to make — that defines what 'good enough' looks like. Second, identify the metric that answers the question and define it precisely. Third, figure out where the data lives and whether it's reliable. Fourth, define the scope — time range, filters, exclusions. For example, 'returns are going up' became: 'Calculate return rate (returned orders / completed orders × 100), segmented by category, region, customer tier, and return reason, for the last 12 months.' That's a query I can write."

---

**Q: What is a requirement document?**

> "A document that captures what a system, report, or analysis needs to do before you build it. For reports, I'd capture: who the audience is, what metrics they need, the time range, any filters or exclusions, the output format, and the refresh frequency. In my internship I wrote lightweight versions of these — 'report briefs' in Confluence — before starting any analysis, to align with my mentor on scope before doing the work."

---

**Q: What makes a good dashboard?**

> "Shows the right metrics for the audience — not everything. Makes current status visible at a glance. Shows trends, not just snapshots. Uses plain language without jargon. Loads fast. Updates reliably. During my internship I learned that the data pipeline behind a dashboard is as important as the visualisation — a beautiful dashboard with stale or incorrect data destroys trust."

---

**Q: How do you present findings to non-technical stakeholders?**

> "I frame everything around decisions, not data. Three sections: what we found, where it's happening, what we recommend. I avoid technical terms — no mention of SQL, JOINs, p-values. For visuals, I use simple charts and explain them in plain English. In my returns presentation, I said 'darker red means more returns' rather than 'the colour intensity represents the return rate value in the heatmap.' The stakeholder responded by immediately commissioning a follow-up investigation — which I take as a sign the communication worked."

---

## 10. Case Study Questions

---

**Q: Sales have dropped this month. How do you investigate?**

> "First, confirm the fact — pull the actual numbers and compare to last month and the same month last year (to separate real decline from seasonality). Then decompose:
> - Is it volume (fewer orders) or value (lower average order)?
> - Is it across all regions or concentrated in one?
> - Is it across all categories or one product type?
> - Is it new customer acquisition dropping, or are existing customers ordering less?
> - Did anything change — promotion ended, price increased, competitor launched, site issue?
>
> Each cut narrows the problem. Once I find where the decline is concentrated, I can form a hypothesis and look for the root cause. I'd document the investigation as I go and share the findings as a structured one-pager."

---

**Q: Your numbers don't match the stakeholder's. What do you do?**

> "Don't assume either side is wrong — investigate both. I'd:
> 1. Ask the stakeholder exactly where their number came from
> 2. Check whether we're using the same metric definition — same time period? Same status filter? Same currency or unit?
> 3. Audit my own query — check filters, join types, grouping logic
> 4. Trace back to the raw data — do the source tables add up?
>
> In my internship I had this situation — my revenue number was 2% lower than the existing report. Turned out the old report included 'pending' orders, and I was correctly filtering to 'completed' only. The old report was wrong — but I walked my mentor through the logic carefully before making that claim, rather than just saying 'the old report is wrong'."

---

**Q: You have a dataset with 30% missing values in an important column. What do you do?**

> "30% is too much to ignore or casually drop — either would significantly bias results. My approach:
> 1. Understand *why* — is it random (some records just didn't capture it) or systematic (missing only before a certain date, or for a certain region)? Systematic missingness changes everything.
> 2. Assess impact — is this column a filter, a dimension, or a metric? Each has different consequences for missingness.
> 3. Choose the treatment: fill with mode/median if missingness appears random and volume allows; create an 'Unknown' category and include it explicitly; or scope the analysis to exclude the affected period/segment and document why.
> 4. Always report the caveat — tell stakeholders what % had this issue and how you handled it. Never silently drop data."

---

**Q: How would you measure the success of a marketing campaign?**

> "Define what success means first — revenue lift? New customers acquired? Repeat purchase rate? Then structure the measurement:
> - **Before vs After:** Compare the target metric in the campaign period vs the equivalent period before, accounting for seasonality
> - **Campaign vs Control:** Ideally compare a group that received the campaign vs one that didn't
> - **Attribution:** Which orders can be attributed to the campaign (voucher code, UTM tracking, time window)?
>
> From my internship, I'd approach it with SQL — segment orders by channel and date range, calculate the metrics per segment, and look for statistically meaningful differences. The key is defining the metric and the comparison group *before* looking at the data, not after."

---

## 11. Questions to Ask the Interviewer

Always prepare 2–3. Shows genuine interest and helps you evaluate the role.

- "What does a typical first project look like for someone joining this team?"
- "What tools and databases does the team primarily work with day-to-day?"
- "How does the analytics team interact with business stakeholders — is it mostly ad-hoc requests or structured project cycles?"
- "What does the data infrastructure look like — are you working primarily with relational databases, or is there a data warehouse layer?"
- "What would success look like for someone in this role at the 6-month mark?"

---

## 12. Quick-Fire Definitions Table

| Question | One-Liner Answer |
|---|---|
| What is SQL? | Language for querying and managing relational databases — select, filter, join, aggregate. |
| What is PostgreSQL? | Open-source relational database — supports advanced indexing, window functions, and full SQL compliance. |
| What is Pandas? | Python library for tabular data manipulation — DataFrames, cleaning, groupby, merge. |
| What is NumPy? | Python library for fast numerical computation on arrays — Pandas is built on top of it. |
| What is a DataFrame? | 2D tabular data structure in Pandas — like a SQL table or Excel sheet in memory. |
| What is EDA? | Exploratory Data Analysis — understanding data quality, distribution, and relationships before formal analysis. |
| What is ETL? | Extract, Transform, Load — moving and transforming data from source to destination. |
| What is a KPI? | Key Performance Indicator — a specific measurable metric the business tracks. |
| What is a foreign key? | A column referencing a primary key in another table — enforces relationships. |
| What is a primary key? | A column uniquely identifying each row — cannot be NULL or duplicate. |
| What is normalisation? | Organising a DB to reduce redundancy by splitting data into related tables with proper keys. |
| What is an aggregate function? | Operates on a set of rows to return one value — SUM, COUNT, AVG, MIN, MAX. |
| What is COALESCE? | Returns first non-NULL value in a list — e.g. `COALESCE(returns, 0)`. |
| What is NULLIF? | Returns NULL if two values are equal — used to avoid divide-by-zero. |
| What is a window function? | Calculation across related rows without collapsing them — uses OVER(). |
| What is a heatmap? | 2D visualisation where colour intensity represents a value — category vs region matrix. |
| What is a Seq Scan? | Full table scan in PostgreSQL — reads every row. Slow on large tables. |
| What is an Index Scan? | PostgreSQL uses an index to jump to matching rows. Fast. |
| What is a CTE? | Named temporary result set defined with WITH — improves readability and reusability. |
| What is OLTP vs OLAP? | OLTP = fast transactional reads/writes. OLAP = analytical queries on large historical data. |
| What is Git? | Version control system — tracks changes to code, enables collaboration and rollback. |
| What is Tableau? | BI tool for interactive visualisations and dashboards without writing code. |
| What is data granularity? | Level of detail in a dataset — row per transaction vs row per month. |
| What is a return rate? | Returned orders ÷ completed orders × 100 — measures post-purchase dissatisfaction. |
| What is skewness? | Asymmetry of a distribution — right-skewed means mean > median, long tail to the right. |
