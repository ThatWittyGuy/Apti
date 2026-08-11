# Globant India Pvt. Ltd. — Internship Knowledge Base
**Role:** Intern – Software Developer (Analytics) | College Trainee
**Duration:** January 2026 – April 2026 (3 Months)
**Location:** Pune, India
**Tech Stack:** Python (Pandas, NumPy, Matplotlib, Seaborn), SQL, PostgreSQL, Jupyter Notebook, Git, DBeaver, Excel

---

## Table of Contents
1. [Team & Context](#1-team--context)
2. [Month 1 — Onboarding, Schema & SQL Foundations](#2-month-1)
3. [Month 2 — EDA, Python & Customer Analysis](#3-month-2)
4. [Month 3 — Query Optimisation & Returns Analysis](#4-month-3)
5. [Database Schema & ER Diagram](#5-database-schema--er-diagram)
6. [SQL Reference — All Key Queries](#6-sql-reference)
7. [Python Reference — All Key Patterns](#7-python-reference)
8. [Glossary](#8-glossary)

---

## 1. Team & Context

**Team:** Analytics & Insights Team (within Data Engineering division)
**Reporting to:** Priya Nair (Senior Data Analyst)
**Mentor:** Rohan Desai (Data Engineer)
**Team size:** ~8 people including 2 other interns

**Client & Project:**
The team supported an internal retail e-commerce client (referred to as **Project Orion**) that sold consumer electronics across India — Smartphones, Laptops, Tablets, Accessories, TVs. The analytics team's job was to maintain reporting pipelines, run ad-hoc analysis, and surface insights for business stakeholders (marketing and sales leads).

**Day-to-day tools:**
- **DBeaver** — GUI to write and run PostgreSQL queries
- **Jupyter Notebook** — Python-based analysis and EDA
- **Git + GitHub** — version control for scripts
- **Confluence** — documentation
- **Jira** — task tracking
- **Excel / Google Sheets** — sharing quick outputs with non-technical stakeholders

---

> ### 💬 Interview Tooltip — Team & Context
> **Likely questions:**
> - *"Tell me about the company/team you interned with."*
> - *"What kind of data did you work with?"*
> - *"Who did you report to and how was work assigned?"*
>
> **Quick answers:**
> - Globant is a global software development company. My team was the Analytics & Insights team supporting a retail e-commerce client in the consumer electronics space.
> - Data was transactional — customer orders, products, returns. Structured, stored in PostgreSQL.
> - Work was assigned via Jira tickets. I reported to a senior data analyst and had a data engineer as my technical mentor. Weekly syncs with the team, daily standups.

---

## 2. Month 1
### Onboarding, Schema Understanding & SQL Foundations
**January 2026**

---

### Week 1–2: Setup & Onboarding

- Set up PostgreSQL 15 locally, connected via DBeaver
- Set up Python 3.11 with conda — installed `pandas`, `numpy`, `matplotlib`, `seaborn`, `psycopg2`
- Got read-only access to `orion_dev` — a sandboxed replica of the production database (anonymised, refreshed weekly)
- Read through internal SQL style guide: snake_case naming, always alias tables, prefer CTEs over nested subqueries
- Studied the existing database schema and wrote notes on each table in Confluence

---

### Week 3–4: First SQL Task — Reproducing the Monthly Sales Report

**Context:** The team ran a Monthly Sales Summary Report (MSR) for the business team. A senior analyst pulled numbers from PostgreSQL manually. My job in Month 1 was to **understand the data, learn the schema, and reproduce those same numbers in SQL** — as a validation and learning exercise.

This involved writing queries from scratch and cross-checking my outputs against the existing report. If my numbers matched, I understood the data correctly.

**What I learned:**
- How to navigate a multi-table relational schema
- When to use `LEFT JOIN` vs `INNER JOIN`
- How `GROUP BY` and aggregate functions work
- `DATE_TRUNC()` for time-based grouping in PostgreSQL
- `NULLIF()` to prevent division-by-zero in rate calculations

---

> ### 💬 Interview Tooltip — Month 1
> **Likely questions:**
> - *"What did you do in the first few weeks of your internship?"*
> - *"Have you worked with real databases? What was the schema like?"*
> - *"How did you get started — did someone guide you or were you independent?"*
>
> **Quick answers:**
> - First two weeks were onboarding — setup, understanding the schema, reading documentation.
> - Yes, PostgreSQL database with 5 core tables: customers, orders, order_items, products, returns. Classic e-commerce transactional schema.
> - My mentor walked me through the schema and gave me a task — reproduce an existing report using SQL. That forced me to understand the joins and aggregations properly. I documented everything in Confluence.

---

## 3. Month 2
### EDA, Python Analysis & Customer Behaviour
**February 2026**

---

### Project: Customer Behaviour EDA — `customer_eda.ipynb`

**Context / Why:**
The marketing team wanted to understand *who* their customers were — which segments were most valuable, which regions drove revenue, whether Gold-tier customers actually ordered more, and how order trends looked over time. My mentor assigned me to do a proper EDA and summarise findings in a written report for the team.

**Dataset:**
- ~85,000 customer records
- ~2.1M order rows (dev DB sample covering Jan 2025 – Dec 2025)

---

### Step 1: Load Data from PostgreSQL into Python

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import psycopg2

conn = psycopg2.connect(
    host="localhost",
    database="orion_dev",
    user="analyst_user",
    password="password"
)

df_customers = pd.read_sql("SELECT * FROM customers", conn)
df_orders    = pd.read_sql("SELECT * FROM orders WHERE status = 'completed'", conn)

# Merge customers onto orders
df = df_orders.merge(df_customers, on='customer_id', how='left')
df['order_date'] = pd.to_datetime(df['order_date'])

conn.close()
```

---

### Step 2: Data Cleaning

```python
# 1. Audit nulls
print(df.isnull().sum())
# Found: ~3% of orders had null 'region' — field wasn't collected before mid-2024
df['region'] = df['region'].fillna('Unknown')

# 2. Check data types
print(df.dtypes)

# 3. Find and remove bad records
# total_amount had 47 negative values — data entry errors, excluded
df = df[df['total_amount'] >= 0]

# 4. Check duplicates on order_id
print(df.duplicated(subset=['order_id']).sum())  # 0 — clean

# 5. Derived columns
df['month']      = df['order_date'].dt.to_period('M')
df['net_amount'] = df['total_amount'] - df['discount_amount']
```

**Key cleaning decisions I documented:**
- Null `region` → filled with `'Unknown'`, kept in dataset, reported separately
- Negative `total_amount` → 47 rows, treated as data entry errors, dropped
- No duplicate `order_id`s found

---

### Step 3: Descriptive Statistics

```python
print(df['total_amount'].describe())
# count    2,099,953
# mean     3,847.22
# std      2,914.08
# min      0.00
# 25%      1,499.00
# 50%      3,199.00    <-- median, more representative than mean
# 75%      5,799.00
# max      89,999.00
```

**Observation:** High standard deviation (₹2,914) relative to mean (₹3,847) — wide spread. Median (₹3,199) is lower than mean → right-skewed distribution, pulled up by high-value bulk orders.

---

### Step 4: Key Analyses & Visualisations

**Revenue by Customer Tier**
```python
tier_rev = df.groupby('tier').agg(
    total_revenue    = ('total_amount', 'sum'),
    total_orders     = ('order_id',     'count'),
    avg_order_value  = ('total_amount', 'mean')
).reset_index().sort_values('total_revenue', ascending=False)

plt.figure(figsize=(8,5))
sns.barplot(data=tier_rev, x='tier', y='total_revenue', palette='Blues_d')
plt.title('Total Revenue by Customer Tier')
plt.ylabel('Revenue (INR)')
plt.tight_layout()
plt.savefig('charts/revenue_by_tier.png', dpi=150)
```

**Monthly Order Trend**
```python
monthly = df.groupby('month').agg(
    orders  = ('order_id',     'count'),
    revenue = ('total_amount', 'sum')
).reset_index()
monthly['month'] = monthly['month'].astype(str)

fig, ax1 = plt.subplots(figsize=(12,5))
ax2 = ax1.twinx()
ax1.bar(monthly['month'],  monthly['orders'],  color='steelblue', alpha=0.6)
ax2.plot(monthly['month'], monthly['revenue'], color='tomato', linewidth=2)
ax1.set_ylabel('Orders')
ax2.set_ylabel('Revenue (INR)')
plt.title('Monthly Orders and Revenue — 2025')
plt.tight_layout()
plt.savefig('charts/monthly_trend.png', dpi=150)
```

**Percentage of Total — Category Revenue**
```python
# Joining order_items to get category-level view
df_items = pd.read_sql("""
    SELECT oi.category, oi.quantity, oi.unit_price, o.order_date
    FROM order_items oi
    JOIN orders o ON oi.order_id = o.order_id
    WHERE o.status = 'completed'
""", conn)

cat_rev = df_items.groupby('category').apply(
    lambda x: (x['quantity'] * x['unit_price']).sum()
).reset_index()
cat_rev.columns = ['category', 'revenue']
cat_rev['pct_of_total'] = (cat_rev['revenue'] / cat_rev['revenue'].sum() * 100).round(2)
cat_rev = cat_rev.sort_values('revenue', ascending=False)
print(cat_rev)
```

---

### Key Findings from EDA

1. **Gold tier = 18% of customers, 52% of revenue** — classic Pareto/80-20 pattern
2. **South region** had highest order volume; **West region** had highest average order value (₹5,200 vs overall ₹3,847)
3. **Smartphones** = top category by revenue; **Accessories** = highest return rate (12.4%)
4. **Post-festive drop:** January orders were ~30% lower than December — expected seasonal pattern
5. **Negative values (47 rows)** flagged as data quality issue — raised to data engineering team

---

> ### 💬 Interview Tooltip — EDA & Python
> **Likely questions:**
> - *"Have you used Python for data analysis? What libraries?"*
> - *"Walk me through how you cleaned data."*
> - *"What did your EDA reveal? What was a key insight?"*
> - *"What is the difference between mean and median? Which did you use and why?"*
> - *"How do you handle missing values?"*
>
> **Quick answers:**
> - Yes — Pandas for data manipulation, NumPy for numerical ops, Matplotlib and Seaborn for visualisation. All in Jupyter Notebook.
> - Standard flow: audit nulls → fix data types → handle bad records → remove duplicates → derive new columns. Always document decisions.
> - Key insight: Gold tier was only 18% of customers but drove 52% of revenue. Also found 47 negative order values — flagged as a data quality issue to the engineering team.
> - Mean was ₹3,847 but median was ₹3,199 — right-skewed distribution because of a few very high-value bulk orders. Median was more representative for reporting to business stakeholders.
> - Depended on the column. For `region` (categorical), I filled nulls with `'Unknown'` and kept them. For `order_id` (key identifier), I would have dropped them — but there were none. For numeric columns I'd assess whether to fill with median or drop based on volume.

---

## 4. Month 3
### Query Optimisation & Returns Analysis
**March – April 2026**

---

### Part A: Query Performance Optimisation — Project QPO-Analytics-01

**Context / Problem:**
In early March, the team noticed dashboard queries were running slowly — some taking 12–18 seconds. The `orders` table had grown to ~4 million rows in production. My mentor asked me to investigate and fix the bottleneck queries.

---

### Step 1: Understanding EXPLAIN ANALYZE

```sql
-- Show the query plan WITH actual execution times
EXPLAIN ANALYZE
SELECT region, SUM(total_amount)
FROM orders
WHERE status = 'completed'
  AND order_date >= CURRENT_DATE - INTERVAL '90 days'
GROUP BY region;
```

**What I learned to read:**
| Term | Meaning |
|---|---|
| `Seq Scan` | Full table scan — reads every row. Slow on large tables. |
| `Index Scan` | Uses an index to jump to matching rows. Fast. |
| `Bitmap Heap Scan` | Builds a bitmap of matching rows, then fetches. Middle ground. |
| `rows=` (estimated vs actual) | If far apart, statistics are stale — run `ANALYZE` on the table |
| `actual time=` | Real execution time in milliseconds |

**Before optimisation:** Seq Scan on 4M rows → **~14 seconds**

---

### Step 2: Creating Indexes

**What is an index?**
A B-tree data structure that lets PostgreSQL find matching rows without scanning the whole table. Like a book's index — jump to the page rather than reading cover to cover.

**Trade-off:** Speeds up reads, slows down writes (INSERT/UPDATE/DELETE), uses disk space. Don't index everything.

**Index 1: Composite index on `(status, order_date)`**

Most queries filter on both `status = 'completed'` AND a date range. A composite index covering both is ideal. `status` goes first because it's an equality filter (more selective first).

```sql
CREATE INDEX idx_orders_status_date
ON orders (status, order_date DESC);
```

**Index 2: Index on `order_items(order_id)` — for JOIN performance**

```sql
CREATE INDEX idx_order_items_order_id
ON order_items (order_id);
```

Without this, every JOIN between `orders` and `order_items` required a full scan of `order_items`.

**Index 3: Index on `orders(customer_id)`**

```sql
CREATE INDEX idx_orders_customer_id
ON orders (customer_id);
```

**Verification:**
```sql
EXPLAIN ANALYZE
SELECT region, SUM(total_amount)
FROM orders
WHERE status = 'completed'
  AND order_date >= CURRENT_DATE - INTERVAL '90 days'
GROUP BY region;
-- Before: Seq Scan — ~14 seconds
-- After:  Index Scan using idx_orders_status_date — ~1.8 seconds
```

**Result: ~85% reduction in query execution time on the dev database.**
Conservative estimate for production (with concurrent queries, network): **30–40% improvement in response times.**

---

### Step 3: Query Rewriting with CTEs

**Problem pattern — inefficient:**
```sql
-- Old style: comma JOIN, SELECT *, filtering after subquery
SELECT *
FROM (
    SELECT o.order_id, o.region, o.total_amount, o.order_date,
           c.tier, c.state
    FROM orders o, customers c
    WHERE o.customer_id = c.customer_id
) sub
WHERE sub.status = 'completed'
  AND sub.order_date >= '2025-10-01';
```

Issues: `SELECT *` pulls unnecessary columns; old comma-JOIN syntax; filter applied after the full join is computed.

**Rewritten — optimised:**
```sql
WITH completed_orders AS (
    SELECT
        order_id,
        region,
        total_amount,
        order_date,
        customer_id
    FROM orders
    WHERE status = 'completed'          -- filter EARLY, before the JOIN
      AND order_date >= '2025-10-01'
)
SELECT
    co.region,
    co.total_amount,
    co.order_date,
    c.tier,
    c.state
FROM completed_orders co
JOIN customers c ON co.customer_id = c.customer_id;
```

**Why this is better:** Filtering inside the CTE reduces the row count before the JOIN happens — smaller dataset for PostgreSQL to work with.

---

> ### 💬 Interview Tooltip — Query Optimisation & Indexing
> **Likely questions:**
> - *"Have you done any SQL query optimisation?"*
> - *"What is an index? When would you create one?"*
> - *"What is EXPLAIN ANALYZE?"*
> - *"What is a composite index? Does column order matter?"*
> - *"What are the trade-offs of indexing?"*
>
> **Quick answers:**
> - Yes — identified slow queries using EXPLAIN ANALYZE, created composite indexes on the `orders` table, brought a 14-second query down to under 2 seconds.
> - An index is a B-tree structure that lets the DB find rows without scanning the whole table. Create on columns used in WHERE, JOIN, ORDER BY — especially on large tables with high cardinality.
> - EXPLAIN ANALYZE runs the query and shows the actual execution plan — whether it's doing a Seq Scan or Index Scan, and how long each step took. It's the primary diagnostic tool for slow queries.
> - Yes, column order matters. Put the equality filter column first (e.g. `status`), then the range filter column (`order_date`). The DB can use the index efficiently only up to the first range column.
> - Indexes speed up reads but slow down writes and use disk space. You don't index every column — only those causing actual performance problems.

---

### Part B: Returns Trend Analysis — `returns_analysis_Q4_2025.ipynb`

**Context / Why:**
The client noticed return rates were climbing and asked: *"Why are returns going up?"* This was an open-ended analytical task — no prescribed format. I had to define the metric, choose the cuts, do the analysis, and present findings with recommendations.

**This is your best story for BA interviews — open-ended problem, self-directed analysis, business recommendation.**

---

### Approach

```
1. Define the metric: Return Rate = (Returned Orders / Completed Orders) × 100
2. Cut by: Time (monthly trend), Category, Region, Customer Tier, Return Reason
3. Identify which dimension shows the most variance → that's the driver
4. Form a hypothesis → test with data
5. Write findings + recommendations
```

---

### Key SQL — Returns Analysis

**Monthly return rate trend:**
```sql
WITH order_counts AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        COUNT(order_id)                  AS total_orders
    FROM orders
    WHERE status IN ('completed', 'returned')
    GROUP BY DATE_TRUNC('month', order_date)
),
return_counts AS (
    SELECT
        DATE_TRUNC('month', return_date) AS month,
        COUNT(return_id)                  AS total_returns,
        SUM(refund_amount)                AS total_refunds
    FROM returns
    GROUP BY DATE_TRUNC('month', return_date)
)
SELECT
    oc.month,
    oc.total_orders,
    COALESCE(rc.total_returns, 0) AS total_returns,
    ROUND(
        COALESCE(rc.total_returns, 0)::NUMERIC
        / NULLIF(oc.total_orders, 0) * 100, 2
    ) AS return_rate_pct
FROM order_counts oc
LEFT JOIN return_counts rc ON oc.month = rc.month
ORDER BY oc.month;
```

**Return reasons by category:**
```sql
SELECT
    oi.category,
    r.reason,
    COUNT(r.return_id)   AS return_count,
    SUM(r.refund_amount) AS refund_value
FROM returns r
JOIN orders o      ON r.order_id    = o.order_id
JOIN order_items oi ON o.order_id  = oi.order_id
GROUP BY oi.category, r.reason
ORDER BY oi.category, return_count DESC;
```

---

### Python — Heatmap of Return Rate by Category × Region

```python
df_ret = pd.read_sql("""
    SELECT
        oi.category,
        o.region,
        COUNT(DISTINCT o.order_id)  AS total_orders,
        COUNT(DISTINCT r.return_id) AS total_returns
    FROM order_items oi
    JOIN orders o    ON oi.order_id = o.order_id
    LEFT JOIN returns r ON o.order_id = r.order_id
    WHERE o.order_date >= '2025-01-01'
    GROUP BY oi.category, o.region
""", conn)

df_ret['return_rate'] = (
    df_ret['total_returns'] / df_ret['total_orders'].replace(0, np.nan) * 100
).round(2)

pivot = df_ret.pivot(index='category', columns='region', values='return_rate')

plt.figure(figsize=(10,6))
sns.heatmap(pivot, annot=True, fmt='.1f', cmap='YlOrRd', linewidths=0.5)
plt.title('Return Rate (%) by Category and Region')
plt.tight_layout()
plt.savefig('charts/return_rate_heatmap.png', dpi=150)
```

---

### Findings

| Finding | Detail |
|---|---|
| Overall return rate | Rose from **6.2% (Oct 2025)** to **9.8% (Jan 2026)** — a 58% increase |
| Worst category | **Accessories** — 12.4% return rate |
| Best category | **Laptops** — 3.1% return rate |
| Worst region | **North** — disproportionately high across all categories |
| Top return reason | **"Defective product"** for Electronics; **"Not needed"** for Accessories |
| Customer tier | **Bronze tier** had 2× the return rate of Gold tier |

### Recommendations I Made

1. Improve quality checks for Accessories suppliers — highest return rate, mostly "defective"
2. Investigate the North region's last-mile delivery partner — systemic issue across categories
3. Add detailed product descriptions and videos for Accessories — reduce "not needed" / wrong expectation returns
4. Flag Bronze-tier high returners — potential policy review needed

---

> ### 💬 Interview Tooltip — Returns Analysis
> **Likely questions:**
> - *"Tell me about a project where you worked independently."*
> - *"How do you approach an open-ended analytical problem?"*
> - *"Give an example of a finding that led to a business recommendation."*
> - *"What is a return rate and how did you calculate it?"*
> - *"What is COALESCE? What is NULLIF?"*
>
> **Quick answers:**
> - The returns analysis — I was given an open-ended question ("why are returns going up?"), defined my own metric, chose my own cuts, and presented findings with recommendations. No one prescribed the format.
> - My approach: define the metric precisely → cut it by multiple dimensions → find where variance is highest → form a hypothesis → test it → recommend action.
> - North region had a disproportionately high return rate across all categories — not limited to one product type — which suggested the issue was in delivery/logistics, not the product itself. I recommended investigating the last-mile partner.
> - Return Rate = (Returned Orders / Completed Orders) × 100. I used a LEFT JOIN from orders to returns — not every order has a return — and COALESCE to treat NULLs (no return) as 0.
> - `COALESCE(value, default)` returns the first non-NULL value — I used it to turn NULL return counts into 0. `NULLIF(value, 0)` returns NULL if the value equals 0 — I used it in the denominator of the return rate to avoid division by zero.

---

## 5. Database Schema & ER Diagram

### Table Definitions

```sql
CREATE TABLE customers (
    customer_id  SERIAL PRIMARY KEY,
    name         VARCHAR(100),
    email        VARCHAR(150),
    city         VARCHAR(50),
    state        VARCHAR(50),
    tier         VARCHAR(20),    -- 'Gold', 'Silver', 'Bronze'
    signup_date  DATE,
    is_active    BOOLEAN DEFAULT TRUE
);

CREATE TABLE orders (
    order_id        SERIAL PRIMARY KEY,
    customer_id     INT REFERENCES customers(customer_id),
    order_date      DATE NOT NULL,
    status          VARCHAR(20),   -- 'completed', 'cancelled', 'returned', 'pending'
    channel         VARCHAR(20),   -- 'website', 'app', 'offline'
    region          VARCHAR(30),   -- 'North', 'South', 'East', 'West', 'Central'
    total_amount    NUMERIC(12,2),
    discount_amount NUMERIC(10,2),
    created_at      TIMESTAMP DEFAULT NOW()
);

CREATE TABLE order_items (
    item_id    SERIAL PRIMARY KEY,
    order_id   INT REFERENCES orders(order_id),
    product_id INT NOT NULL,
    quantity   INT NOT NULL,
    unit_price NUMERIC(10,2),
    category   VARCHAR(50)   -- 'Smartphones','Laptops','Accessories','Tablets','TVs'
);

CREATE TABLE products (
    product_id    SERIAL PRIMARY KEY,
    product_name  VARCHAR(150),
    brand         VARCHAR(50),
    category      VARCHAR(50),
    sub_category  VARCHAR(50),
    cost_price    NUMERIC(10,2),
    selling_price NUMERIC(10,2),
    is_active     BOOLEAN DEFAULT TRUE
);

CREATE TABLE returns (
    return_id     SERIAL PRIMARY KEY,
    order_id      INT REFERENCES orders(order_id),
    return_date   DATE,
    reason        VARCHAR(100),  -- 'defective','wrong_item','not_needed','damaged'
    refund_amount NUMERIC(10,2)
);
```

### ER Diagram

```
customers (customer_id PK)
    │
    └──< orders (order_id PK, customer_id FK)
              │
              ├──< order_items (item_id PK, order_id FK, product_id FK)
              │          │
              │          └──> products (product_id PK)
              │
              └──< returns (return_id PK, order_id FK)
```

### Relationships

| Relationship | Type | Note |
|---|---|---|
| customers → orders | One-to-Many | One customer, many orders |
| orders → order_items | One-to-Many | One order, many line items |
| products → order_items | One-to-Many | One product in many orders |
| orders → returns | One-to-One (max) | At most one return per order |

---

> ### 💬 Interview Tooltip — Schema & ER
> **Likely questions:**
> - *"Can you draw / explain the database schema you worked with?"*
> - *"What is a foreign key?"*
> - *"What is the difference between a primary key and a foreign key?"*
> - *"Why do you need order_items as a separate table?"*
>
> **Quick answers:**
> - Five tables: customers, orders, order_items, products, returns. Standard e-commerce transactional schema. Can sketch the ER diagram easily.
> - A foreign key is a column that references the primary key of another table — it enforces that you can't have an order for a non-existent customer.
> - Primary key uniquely identifies each row in its own table (can't be NULL or duplicate). Foreign key references a primary key in another table to create a relationship.
> - Because one order can contain multiple products. If you stored product info inside the orders table, you'd either repeat the order header for every product (redundancy) or lose the line-item detail. `order_items` is the many-side of a one-to-many between orders and products.

---

## 6. SQL Reference — All Key Queries

### Basic Aggregation — Sales by Region
```sql
SELECT
    region,
    DATE_TRUNC('month', order_date)             AS month,
    COUNT(order_id)                              AS total_orders,
    SUM(total_amount)                            AS gross_revenue,
    SUM(total_amount - discount_amount)          AS net_revenue,
    ROUND(AVG(total_amount), 2)                  AS avg_order_value
FROM orders
WHERE status = 'completed'
  AND order_date >= '2025-01-01'
GROUP BY region, DATE_TRUNC('month', order_date)
ORDER BY month, net_revenue DESC;
```

### Multi-Table JOIN — Top Products by Revenue
```sql
SELECT
    p.product_name,
    p.category,
    p.brand,
    SUM(oi.quantity)                AS units_sold,
    SUM(oi.quantity * oi.unit_price) AS total_revenue
FROM order_items oi
JOIN orders   o ON oi.order_id   = o.order_id
JOIN products p ON oi.product_id = p.product_id
WHERE o.status    = 'completed'
  AND o.order_date BETWEEN '2025-12-01' AND '2025-12-31'
GROUP BY p.product_name, p.category, p.brand
ORDER BY total_revenue DESC
LIMIT 10;
```

### LEFT JOIN — Return Rate by Category
```sql
SELECT
    oi.category,
    COUNT(DISTINCT o.order_id)  AS total_orders,
    COUNT(DISTINCT r.return_id) AS total_returns,
    ROUND(
        COUNT(DISTINCT r.return_id)::NUMERIC
        / NULLIF(COUNT(DISTINCT o.order_id), 0) * 100, 2
    ) AS return_rate_pct
FROM order_items oi
JOIN   orders o  ON oi.order_id = o.order_id
LEFT JOIN returns r ON o.order_id = r.order_id
WHERE o.order_date >= '2025-01-01'
GROUP BY oi.category
ORDER BY return_rate_pct DESC;
```

### CTE — Completed Orders Then Join
```sql
WITH completed_orders AS (
    SELECT order_id, customer_id, region, total_amount, order_date
    FROM orders
    WHERE status = 'completed'
      AND order_date >= '2025-10-01'
)
SELECT
    co.region,
    c.tier,
    COUNT(co.order_id)       AS orders,
    SUM(co.total_amount)     AS revenue,
    ROUND(AVG(co.total_amount), 2) AS avg_order_value
FROM completed_orders co
JOIN customers c ON co.customer_id = c.customer_id
GROUP BY co.region, c.tier
ORDER BY revenue DESC;
```

### Window Function — MoM Revenue Growth
```sql
WITH monthly AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(total_amount)                AS revenue
    FROM orders
    WHERE status = 'completed'
    GROUP BY DATE_TRUNC('month', order_date)
)
SELECT
    month,
    revenue,
    LAG(revenue) OVER (ORDER BY month)   AS prev_month_revenue,
    ROUND(
        (revenue - LAG(revenue) OVER (ORDER BY month))
        / NULLIF(LAG(revenue) OVER (ORDER BY month), 0) * 100, 2
    ) AS mom_growth_pct
FROM monthly
ORDER BY month;
```

### Window Function — Rank Products Within Category
```sql
SELECT
    category,
    product_name,
    total_revenue,
    RANK() OVER (PARTITION BY category ORDER BY total_revenue DESC) AS rank_in_category
FROM (
    SELECT
        oi.category,
        p.product_name,
        SUM(oi.quantity * oi.unit_price) AS total_revenue
    FROM order_items oi
    JOIN products p ON oi.product_id = p.product_id
    JOIN orders   o ON oi.order_id   = o.order_id
    WHERE o.status = 'completed'
    GROUP BY oi.category, p.product_name
) sub;
```

### Find Duplicates
```sql
SELECT customer_id, COUNT(*) AS occurrences
FROM customers
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

### Running Total
```sql
SELECT
    order_date,
    total_amount,
    SUM(total_amount) OVER (
        ORDER BY order_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total
FROM orders
WHERE status = 'completed'
ORDER BY order_date;
```

---

## 7. Python Reference — All Key Patterns

### Load from PostgreSQL
```python
import psycopg2
import pandas as pd

conn = psycopg2.connect(host="localhost", database="orion_dev",
                        user="analyst_user", password="password")
df = pd.read_sql("SELECT * FROM orders WHERE status = 'completed'", conn)
conn.close()
```

### Data Audit
```python
print(df.shape)
print(df.dtypes)
print(df.isnull().sum())
print(df.duplicated(subset=['order_id']).sum())
print(df.describe())
```

### Cleaning
```python
df['order_date']    = pd.to_datetime(df['order_date'])
df['total_amount']  = pd.to_numeric(df['total_amount'], errors='coerce')
df['region']        = df['region'].fillna('Unknown')
df                  = df[df['total_amount'] >= 0]
df.drop_duplicates(subset=['order_id'], keep='first', inplace=True)
df['net_amount']    = df['total_amount'] - df['discount_amount']
df['month']         = df['order_date'].dt.to_period('M')
```

### GroupBy + Agg
```python
summary = df.groupby('region').agg(
    total_orders    = ('order_id',     'count'),
    gross_revenue   = ('total_amount', 'sum'),
    avg_order_value = ('total_amount', 'mean')
).reset_index()
summary['avg_order_value'] = summary['avg_order_value'].round(2)
```

### Percentage of Total
```python
cat_rev = df.groupby('category')['total_amount'].sum().reset_index()
cat_rev['pct_of_total'] = (cat_rev['total_amount'] / cat_rev['total_amount'].sum() * 100).round(2)
```

### Outlier Detection — IQR Method
```python
Q1  = df['total_amount'].quantile(0.25)
Q3  = df['total_amount'].quantile(0.75)
IQR = Q3 - Q1
outlier_mask = (df['total_amount'] < Q1 - 1.5*IQR) | (df['total_amount'] > Q3 + 1.5*IQR)
print(f"Outliers: {outlier_mask.sum()}")
df_flagged  = df[outlier_mask]    # flag for reporting
df_clean    = df[~outlier_mask]   # use for main analysis
```

### np.where — Conditional Column
```python
import numpy as np
df['revenue_bucket'] = np.where(
    df['net_amount'] < 1000, 'Low',
    np.where(df['net_amount'] < 5000, 'Medium', 'High')
)
```

### Pivot + Heatmap
```python
pivot = df.pivot(index='category', columns='region', values='return_rate')
sns.heatmap(pivot, annot=True, fmt='.1f', cmap='YlOrRd')
plt.title('Return Rate by Category and Region')
plt.tight_layout()
plt.savefig('charts/heatmap.png', dpi=150)
```

### Time-Series Resampling
```python
df_ts = df.set_index('order_date').resample('W')['total_amount'].sum().reset_index()
df_ts.columns = ['week', 'weekly_revenue']
```

---

## 8. Glossary

| Term | Meaning |
|---|---|
| EDA | Exploratory Data Analysis — understanding data before formal analysis |
| KPI | Key Performance Indicator — a measurable business metric |
| Seq Scan | Sequential Scan — PostgreSQL reads every row; slow on large tables |
| Index Scan | PostgreSQL uses an index to jump to matching rows; fast |
| CTE | Common Table Expression — a named temp result set using `WITH` |
| Composite Index | An index on two or more columns |
| Cardinality | Number of unique values in a column |
| MoM | Month-over-Month — comparing a metric to the prior month |
| Return Rate | Returned orders ÷ completed orders × 100 |
| IQR | Interquartile Range — Q3 minus Q1; used for outlier detection |
| Data Freshness | How recent the data is; the dev DB was refreshed weekly |
| COALESCE | Returns first non-NULL value — used to replace NULLs with 0 |
| NULLIF | Returns NULL if value equals a given value — prevents divide-by-zero |
| EXPLAIN ANALYZE | PostgreSQL command to show actual query execution plan + timings |
| B-tree Index | Default index type in PostgreSQL; works for equality and range queries |
| Granularity | Level of detail in a dataset (row per transaction vs row per month) |
| NULL | SQL's representation of a missing or unknown value |
| NaN | Python/NumPy's representation of a missing numeric value |
| Skewed Distribution | Mean ≠ median — tail pulled toward high or low values |
| VACUUM ANALYZE | PostgreSQL maintenance — reclaims space + refreshes query planner stats |
