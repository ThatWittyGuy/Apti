Absolutely. Let’s build this the same way as the **GROUP BY notes**: not just syntax, but a way to **translate an English interview question → identify the date/time pattern → write the SQL → verify the output**.

I’ll cover **MySQL + PostgreSQL**, with differences called out explicitly.

# SQL DATE & TIME — Complete Interview + Coding Guide

This is your **from-zero → interview-ready** guide.

---

# 0. The most important idea

Almost every SQL date/time question can be reduced to one of these patterns:

| English question            | SQL pattern                                |
| --------------------------- | ------------------------------------------ |
| Get year                    | `YEAR(date)` / `EXTRACT(YEAR FROM date)`   |
| Get month                   | `MONTH(date)` / `EXTRACT(MONTH FROM date)` |
| Get day                     | `DAY(date)` / `EXTRACT(DAY FROM date)`     |
| Filter a date               | `WHERE date = ...`                         |
| Filter a date range         | `WHERE date >= ... AND date < ...`         |
| Last 7 days                 | current date/time − interval               |
| Current month               | month-start ≤ date < next-month-start      |
| Group by month              | truncate/extract month                     |
| Group by year               | extract year                               |
| Difference between dates    | `DATEDIFF` / date subtraction              |
| Difference in hours         | `TIMESTAMPDIFF` / timestamp subtraction    |
| Add days                    | `+ INTERVAL`                               |
| Find latest record          | `ORDER BY date DESC LIMIT 1`               |
| Find earliest record        | `ORDER BY date LIMIT 1`                    |
| Latest record per user      | `ROW_NUMBER()`                             |
| Users active in last N days | date range                                 |
| Consecutive days            | `LAG()` + date arithmetic                  |
| Month-over-month            | `LAG()` after monthly aggregation          |
| Day-over-day                | `LAG()` after daily aggregation            |
| Weekend                     | day-of-week function                       |
| Age                         | date difference / age calculation          |
| Current timestamp           | `NOW()` / `CURRENT_TIMESTAMP`              |
| Convert timestamp to date   | `DATE(...)` / cast                         |
| Start of month              | `DATE_TRUNC` / date construction           |
| End of month                | next month − 1 day / `LAST_DAY()`          |
| Time between two timestamps | timestamp arithmetic                       |

The key is to **recognize the pattern before writing SQL**.

---

# 1. First understand DATE vs TIME vs TIMESTAMP

Suppose we have:

```text
orders
```

| order_id | customer_id | order_date | order_time | created_at          |
| -------: | ----------: | ---------- | ---------- | ------------------- |
|      101 |           1 | 2026-01-10 | 09:30:00   | 2026-01-10 09:30:00 |
|      102 |           1 | 2026-01-15 | 14:20:00   | 2026-01-15 14:20:00 |
|      103 |           2 | 2026-02-05 | 18:45:00   | 2026-02-05 18:45:00 |
|      104 |           3 | 2026-02-20 | 21:10:00   | 2026-02-20 21:10:00 |

There are three important concepts.

### DATE

Only calendar date:

```text
2026-01-10
```

### TIME

Only time:

```text
09:30:00
```

### TIMESTAMP / DATETIME

Date + time:

```text
2026-01-10 09:30:00
```

---

# 2. MySQL vs PostgreSQL terminology

This is an important interview distinction.

| Concept                  | MySQL                            | PostgreSQL                               |
| ------------------------ | -------------------------------- | ---------------------------------------- |
| Date                     | `DATE`                           | `DATE`                                   |
| Time                     | `TIME`                           | `TIME`                                   |
| Date + time              | `DATETIME` / `TIMESTAMP`         | `TIMESTAMP`                              |
| Timezone-aware timestamp | `TIMESTAMP` has special behavior | `TIMESTAMPTZ`                            |
| Current date             | `CURDATE()`                      | `CURRENT_DATE`                           |
| Current timestamp        | `NOW()`                          | `NOW()`                                  |
| Extract year             | `YEAR(x)`                        | `EXTRACT(YEAR FROM x)`                   |
| Extract month            | `MONTH(x)`                       | `EXTRACT(MONTH FROM x)`                  |
| Date truncation          | usually `DATE_FORMAT()`          | `DATE_TRUNC()`                           |
| Date difference          | `DATEDIFF()`                     | date subtraction                         |
| Timestamp difference     | `TIMESTAMPDIFF()`                | timestamp subtraction / `EXTRACT`        |
| Add interval             | `DATE_ADD()`                     | `+ INTERVAL`                             |
| Subtract interval        | `DATE_SUB()`                     | `- INTERVAL`                             |
| Last day of month        | `LAST_DAY()`                     | usually `DATE_TRUNC(...) + INTERVAL ...` |

---

# 3. The English → SQL translation method

When you see a date/time question, ask these questions **in order**.

### Step 1 — What time object?

Is the question talking about:

```text
date
time
timestamp
month
year
week
```

### Step 2 — Is it filtering or calculating?

Example:

> Find orders from January.

That's **filtering**.

```sql
WHERE order_date >= ...
AND order_date < ...
```

But:

> How many days between order and delivery?

That's **calculation**.

---

### Step 3 — Is aggregation involved?

> How many orders per month?

That's:

```text
DATE/TIME
   ↓
extract/truncate month
   ↓
GROUP BY
   ↓
COUNT
```

---

### Step 4 — Is it asking for "latest", "earliest", "previous", etc.?

Then think:

```text
ORDER BY
LIMIT
```

or:

```text
ROW_NUMBER()
LAG()
LEAD()
```

---

# 4. Current date and current time

## MySQL

```sql
SELECT CURDATE();
```

Example output:

```text
2026-08-23
```

Current time:

```sql
SELECT CURTIME();
```

Current timestamp:

```sql
SELECT NOW();
```

Example:

```text
2026-08-23 14:35:21
```

---

## PostgreSQL

```sql
SELECT CURRENT_DATE;
```

Current time:

```sql
SELECT CURRENT_TIME;
```

Current timestamp:

```sql
SELECT CURRENT_TIMESTAMP;
```

or:

```sql
SELECT NOW();
```

---

# 5. Extract year, month, day

Suppose:

```text
created_at
-------------------
2026-05-17 14:30:00
```

## MySQL

### Year

```sql
SELECT YEAR(created_at)
FROM orders;
```

Output:

```text
2026
```

### Month

```sql
SELECT MONTH(created_at)
FROM orders;
```

Output:

```text
5
```

### Day

```sql
SELECT DAY(created_at)
FROM orders;
```

Output:

```text
17
```

---

## PostgreSQL

Use `EXTRACT()`:

```sql
SELECT EXTRACT(YEAR FROM created_at)
FROM orders;
```

Output:

```text
2026
```

Month:

```sql
SELECT EXTRACT(MONTH FROM created_at)
FROM orders;
```

Day:

```sql
SELECT EXTRACT(DAY FROM created_at)
FROM orders;
```

---

# 6. Extracting other date components

Very common in interviews.

```text
YEAR
MONTH
DAY
QUARTER
WEEK
DAY OF WEEK
DAY OF YEAR
```

## PostgreSQL

```sql
EXTRACT(YEAR FROM created_at)
EXTRACT(MONTH FROM created_at)
EXTRACT(DAY FROM created_at)
EXTRACT(QUARTER FROM created_at)
EXTRACT(WEEK FROM created_at)
EXTRACT(DOW FROM created_at)
EXTRACT(DOY FROM created_at)
```

---

# 7. The first BIG pattern: filter by date

Suppose:

```text
orders
```

|  id | order_date |
| --: | ---------- |
|   1 | 2026-01-10 |
|   2 | 2026-01-15 |
|   3 | 2026-02-05 |
|   4 | 2026-02-20 |

Question:

> Find orders placed on January 15, 2026.

Simple:

```sql
SELECT *
FROM orders
WHERE order_date = '2026-01-15';
```

---

# 8. Filtering a date range

Question:

> Find orders between January 1 and January 31.

A beginner might write:

```sql
WHERE order_date BETWEEN '2026-01-01' AND '2026-01-31'
```

For a pure `DATE`, that's fine.

But timestamps make this dangerous.

Suppose:

```text
created_at
-------------------
2026-01-31 10:00:00
2026-01-31 18:30:00
2026-02-01 00:00:00
```

If you write:

```sql
WHERE created_at BETWEEN '2026-01-01' AND '2026-01-31'
```

the upper bound effectively means:

```text
2026-01-31 00:00:00
```

So you may accidentally exclude the entire rest of January 31.

---

# 9. The GOLDEN DATE RANGE PATTERN

For timestamps, prefer:

```sql
WHERE created_at >= '2026-01-01'
  AND created_at <  '2026-02-01'
```

This means:

```text
>= January 1 00:00:00
<
February 1 00:00:00
```

Therefore it includes:

```text
Jan 1 00:00
Jan 15 14:30
Jan 31 23:59:59
```

but excludes:

```text
Feb 1 00:00
```

This is one of the **most useful SQL date patterns to memorize**.

### Mental model

Think:

```text
[start, next_start)
```

instead of:

```text
[start, end_of_day]
```

---

# 10. Why `BETWEEN` can be dangerous with timestamps

Avoid:

```sql
WHERE created_at BETWEEN '2026-01-01'
                     AND '2026-01-31'
```

Prefer:

```sql
WHERE created_at >= '2026-01-01'
  AND created_at < '2026-02-01'
```

This also works beautifully for:

- days
- months
- quarters
- years
- arbitrary periods

---

# 11. Find today's records

Question:

> Find all orders created today.

If `created_at` is a timestamp:

## MySQL

```sql
SELECT *
FROM orders
WHERE created_at >= CURDATE()
  AND created_at < CURDATE() + INTERVAL 1 DAY;
```

## PostgreSQL

```sql
SELECT *
FROM orders
WHERE created_at >= CURRENT_DATE
  AND created_at < CURRENT_DATE + INTERVAL '1 day';
```

### Why not this?

```sql
WHERE DATE(created_at) = CURDATE()
```

It works logically, but applying a function to the column can make index usage less effective.

Prefer the **range condition** when performance matters.

---

# 12. Yesterday

## MySQL

```sql
WHERE created_at >= CURDATE() - INTERVAL 1 DAY
  AND created_at < CURDATE()
```

## PostgreSQL

```sql
WHERE created_at >= CURRENT_DATE - INTERVAL '1 day'
  AND created_at < CURRENT_DATE
```

---

# 13. Tomorrow

MySQL:

```sql
WHERE created_at >= CURDATE() + INTERVAL 1 DAY
  AND created_at < CURDATE() + INTERVAL 2 DAY
```

PostgreSQL:

```sql
WHERE created_at >= CURRENT_DATE + INTERVAL '1 day'
  AND created_at < CURRENT_DATE + INTERVAL '2 days'
```

---

# 14. Last 7 days

Question:

> Find orders created in the last 7 days.

MySQL:

```sql
SELECT *
FROM orders
WHERE created_at >= NOW() - INTERVAL 7 DAY;
```

PostgreSQL:

```sql
SELECT *
FROM orders
WHERE created_at >= NOW() - INTERVAL '7 days';
```

Important:

### "Last 7 × 24 hours"

means:

```sql
NOW() - INTERVAL 7 DAY
```

### "Previous 7 calendar days"

can mean something different.

For example, if today is August 23:

```text
August 16 through August 22
```

You need to understand the wording.

---

# 15. LAST N DAYS — INTERVIEW TRAP

Question:

> Find users active in the last 30 days.

Usually:

```sql
WHERE last_login >= CURRENT_TIMESTAMP - INTERVAL ...
```

But ask yourself:

**Does "last 30 days" mean the previous 30×24 hours or 30 calendar dates?**

That's an important business-definition distinction.

---

# 16. This month

Question:

> Find all orders from the current month.

## MySQL

```sql
SELECT *
FROM orders
WHERE order_date >= DATE_FORMAT(CURDATE(), '%Y-%m-01')
  AND order_date <  DATE_FORMAT(CURDATE() + INTERVAL 1 MONTH, '%Y-%m-01');
```

For timestamps, a similar range is preferable.

---

## PostgreSQL

Beautifully:

```sql
SELECT *
FROM orders
WHERE created_at >= DATE_TRUNC('month', CURRENT_TIMESTAMP)
  AND created_at <  DATE_TRUNC('month', CURRENT_TIMESTAMP)
                   + INTERVAL '1 month';
```

---

# 17. Previous month

PostgreSQL:

```sql
WHERE created_at >= DATE_TRUNC('month', CURRENT_DATE)
                   - INTERVAL '1 month'
  AND created_at <  DATE_TRUNC('month', CURRENT_DATE)
```

MySQL:

```sql
WHERE created_at >= DATE_FORMAT(
                        CURDATE() - INTERVAL 1 MONTH,
                        '%Y-%m-01'
                    )
  AND created_at < DATE_FORMAT(
                        CURDATE(),
                        '%Y-%m-01'
                    );
```

---

# 18. Current year

Question:

> Find all orders placed this year.

MySQL:

```sql
WHERE created_at >= MAKEDATE(YEAR(CURDATE()), 1)
  AND created_at <  MAKEDATE(YEAR(CURDATE()) + 1, 1)
```

Simpler conceptual version:

```sql
WHERE YEAR(created_at) = YEAR(CURDATE())
```

But the range version is generally better for indexed columns.

PostgreSQL:

```sql
WHERE created_at >= DATE_TRUNC('year', CURRENT_DATE)
  AND created_at <  DATE_TRUNC('year', CURRENT_DATE)
                   + INTERVAL '1 year'
```

---

# 19. Find records from a particular year

Question:

> Find orders from 2025.

Simple:

```sql
SELECT *
FROM orders
WHERE YEAR(order_date) = 2025;
```

MySQL.

PostgreSQL:

```sql
SELECT *
FROM orders
WHERE EXTRACT(YEAR FROM order_date) = 2025;
```

### Better performance pattern

MySQL:

```sql
WHERE order_date >= '2025-01-01'
  AND order_date <  '2026-01-01'
```

PostgreSQL:

```sql
WHERE order_date >= DATE '2025-01-01'
  AND order_date <  DATE '2026-01-01'
```

---

# 20. Group by year

Question:

> How many orders were placed each year?

Table:

| order_id | order_date |
| -------: | ---------- |
|        1 | 2024-01-10 |
|        2 | 2024-07-20 |
|        3 | 2025-02-15 |
|        4 | 2025-05-10 |
|        5 | 2025-12-01 |

## MySQL

```sql
SELECT
    YEAR(order_date) AS year,
    COUNT(*) AS total_orders
FROM orders
GROUP BY YEAR(order_date)
ORDER BY year;
```

Output:

| year | total_orders |
| ---: | -----------: |
| 2024 |            2 |
| 2025 |            3 |

PostgreSQL:

```sql
SELECT
    EXTRACT(YEAR FROM order_date) AS year,
    COUNT(*) AS total_orders
FROM orders
GROUP BY EXTRACT(YEAR FROM order_date)
ORDER BY year;
```

---

# 21. Group by month

Question:

> How many orders were placed in each month?

## MySQL

```sql
SELECT
    YEAR(order_date) AS year,
    MONTH(order_date) AS month,
    COUNT(*) AS total_orders
FROM orders
GROUP BY
    YEAR(order_date),
    MONTH(order_date)
ORDER BY year, month;
```

Output:

| year | month | total_orders |
| ---: | ----: | -----------: |
| 2026 |     1 |           10 |
| 2026 |     2 |           15 |
| 2026 |     3 |           12 |

---

# 22. VERY IMPORTANT: month alone is not enough

Don't do:

```sql
GROUP BY MONTH(order_date)
```

if your data spans multiple years.

Why?

January 2025 and January 2026 both become:

```text
1
```

You accidentally combine them.

Instead:

```sql
GROUP BY
    YEAR(order_date),
    MONTH(order_date)
```

or use a month bucket.

---

# 23. PostgreSQL month grouping

Use:

```sql
SELECT
    DATE_TRUNC('month', order_date) AS month,
    COUNT(*) AS total_orders
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY month;
```

Output:

| month      | total_orders |
| ---------- | -----------: |
| 2026-01-01 |           10 |
| 2026-02-01 |           15 |
| 2026-03-01 |           12 |

This is called **date truncation**.

---

# 24. What does `DATE_TRUNC` actually do?

Think:

```text
2026-08-23 14:35:42
```

Then:

```sql
DATE_TRUNC('year', ...)
```

becomes:

```text
2026-01-01 00:00:00
```

```sql
DATE_TRUNC('month', ...)
```

becomes:

```text
2026-08-01 00:00:00
```

```sql
DATE_TRUNC('day', ...)
```

becomes:

```text
2026-08-23 00:00:00
```

```sql
DATE_TRUNC('hour', ...)
```

becomes:

```text
2026-08-23 14:00:00
```

---

# 25. Month names

Question:

> Show January, February, etc.

MySQL:

```sql
SELECT
    MONTHNAME(order_date) AS month_name,
    COUNT(*) AS total_orders
FROM orders
GROUP BY MONTH(order_date), MONTHNAME(order_date)
ORDER BY MONTH(order_date);
```

Important:

Don't simply:

```sql
ORDER BY month_name
```

because alphabetically:

```text
April
August
December
February
...
```

That's not calendar order.

Use the numeric month for ordering.

---

# 26. PostgreSQL month names

```sql
SELECT
    TO_CHAR(order_date, 'Month') AS month_name,
    COUNT(*) AS total_orders
FROM orders
GROUP BY
    DATE_TRUNC('month', order_date),
    TO_CHAR(order_date, 'Month')
ORDER BY DATE_TRUNC('month', order_date);
```

---

# 27. Group by day

Question:

> How many orders happened each day?

MySQL:

```sql
SELECT
    DATE(created_at) AS order_day,
    COUNT(*) AS total_orders
FROM orders
GROUP BY DATE(created_at)
ORDER BY order_day;
```

PostgreSQL:

```sql
SELECT
    created_at::date AS order_day,
    COUNT(*) AS total_orders
FROM orders
GROUP BY created_at::date
ORDER BY order_day;
```

Or:

```sql
DATE_TRUNC('day', created_at)
```

---

# 28. Group by hour

Question:

> How many orders happened in each hour?

MySQL:

```sql
SELECT
    HOUR(created_at) AS hour,
    COUNT(*) AS total_orders
FROM orders
GROUP BY HOUR(created_at)
ORDER BY hour;
```

Output:

| hour | total_orders |
| ---: | -----------: |
|    9 |           10 |
|   10 |           15 |
|   11 |           21 |
|   12 |           30 |

But again, if multiple days are involved, you're combining every 9 AM across every date.

Sometimes that's exactly what you want:

> Which hour of the day gets the most orders?

Then this pattern is correct.

---

# 29. Day of week

Question:

> How many orders happen on each day of the week?

This is extremely common.

## MySQL

```sql
SELECT
    DAYNAME(order_date) AS day_name,
    COUNT(*) AS total_orders
FROM orders
GROUP BY DAYOFWEEK(order_date), DAYNAME(order_date)
ORDER BY DAYOFWEEK(order_date);
```

Be careful: MySQL's `DAYOFWEEK()` numbering is:

```text
1 Sunday
2 Monday
3 Tuesday
...
7 Saturday
```

---

## PostgreSQL

```sql
SELECT
    EXTRACT(DOW FROM order_date) AS dow,
    COUNT(*) AS total_orders
FROM orders
GROUP BY EXTRACT(DOW FROM order_date)
ORDER BY dow;
```

PostgreSQL:

```text
0 Sunday
1 Monday
2 Tuesday
...
6 Saturday
```

---

# 30. Weekend vs weekday

Question:

> Find orders placed on weekends.

MySQL:

```sql
SELECT *
FROM orders
WHERE DAYOFWEEK(order_date) IN (1, 7);
```

PostgreSQL:

```sql
SELECT *
FROM orders
WHERE EXTRACT(DOW FROM order_date) IN (0, 6);
```

---

# 31. Weekday

MySQL:

```sql
WHERE DAYOFWEEK(order_date) BETWEEN 2 AND 6
```

PostgreSQL:

```sql
WHERE EXTRACT(DOW FROM order_date) BETWEEN 1 AND 5
```

---

# 32. Quarter

There are four quarters:

```text
Q1 = Jan-Mar
Q2 = Apr-Jun
Q3 = Jul-Sep
Q4 = Oct-Dec
```

MySQL:

```sql
SELECT
    YEAR(order_date) AS year,
    QUARTER(order_date) AS quarter,
    COUNT(*) AS total_orders
FROM orders
GROUP BY YEAR(order_date), QUARTER(order_date)
ORDER BY year, quarter;
```

PostgreSQL:

```sql
SELECT
    EXTRACT(YEAR FROM order_date) AS year,
    EXTRACT(QUARTER FROM order_date) AS quarter,
    COUNT(*) AS total_orders
FROM orders
GROUP BY
    EXTRACT(YEAR FROM order_date),
    EXTRACT(QUARTER FROM order_date)
ORDER BY year, quarter;
```

---

# 33. Fiscal year

Interview questions sometimes ask:

> Company's financial year starts in April. Group sales by financial year.

This is where simple `YEAR()` is insufficient.

If:

```text
January 2026
```

belongs to FY2025-26,

then you need custom logic.

Conceptually:

```sql
CASE
    WHEN MONTH(order_date) >= 4
        THEN YEAR(order_date)
    ELSE YEAR(order_date) - 1
END
```

MySQL:

```sql
SELECT
    CASE
        WHEN MONTH(order_date) >= 4
            THEN YEAR(order_date)
        ELSE YEAR(order_date) - 1
    END AS fiscal_year,
    SUM(amount) AS revenue
FROM orders
GROUP BY
    CASE
        WHEN MONTH(order_date) >= 4
            THEN YEAR(order_date)
        ELSE YEAR(order_date) - 1
    END;
```

---

# 34. Date arithmetic

Now we move from:

```text
extract
filter
group
```

to:

```text
calculate
```

Suppose:

```text
start_date = 2026-01-10
```

Add 10 days.

MySQL:

```sql
SELECT DATE_ADD('2026-01-10', INTERVAL 10 DAY);
```

Output:

```text
2026-01-20
```

PostgreSQL:

```sql
SELECT DATE '2026-01-10' + INTERVAL '10 days';
```

Output:

```text
2026-01-20 00:00:00
```

---

# 35. Subtract dates

MySQL:

```sql
SELECT DATE_SUB('2026-01-20', INTERVAL 10 DAY);
```

PostgreSQL:

```sql
SELECT DATE '2026-01-20' - INTERVAL '10 days';
```

---

# 36. Adding months

MySQL:

```sql
SELECT DATE_ADD('2026-01-10', INTERVAL 2 MONTH);
```

PostgreSQL:

```sql
SELECT DATE '2026-01-10' + INTERVAL '2 months';
```

---

# 37. Difference between dates

Question:

> How many days did it take to ship an order?

Suppose:

| order_id | order_date | delivery_date |
| -------: | ---------- | ------------- |
|        1 | 2026-01-01 | 2026-01-05    |
|        2 | 2026-01-10 | 2026-01-13    |
|        3 | 2026-02-01 | 2026-02-10    |

---

## MySQL

```sql
SELECT
    order_id,
    DATEDIFF(delivery_date, order_date) AS delivery_days
FROM orders;
```

Output:

| order_id | delivery_days |
| -------: | ------------: |
|        1 |             4 |
|        2 |             3 |
|        3 |             9 |

---

## PostgreSQL

For dates:

```sql
SELECT
    order_id,
    delivery_date - order_date AS delivery_days
FROM orders;
```

Output:

| order_id | delivery_days |
| -------: | ------------: |
|        1 |             4 |
|        2 |             3 |
|        3 |             9 |

---

# 38. Difference between timestamps

Suppose:

```text
login_time  = 2026-01-01 10:00:00
logout_time = 2026-01-01 18:30:00
```

Question:

> How many hours did the employee work?

PostgreSQL:

```sql
SELECT logout_time - login_time AS duration
FROM attendance;
```

Output:

```text
08:30:00
```

For numeric hours:

```sql
SELECT
    EXTRACT(EPOCH FROM (logout_time - login_time)) / 3600
        AS hours_worked
FROM attendance;
```

Output:

```text
8.5
```

---

# 39. MySQL timestamp difference

MySQL:

```sql
SELECT
    TIMESTAMPDIFF(
        HOUR,
        login_time,
        logout_time
    ) AS hours_worked
FROM attendance;
```

Output:

```text
8
```

Notice something important:

`TIMESTAMPDIFF(HOUR, ...)` returns completed hours, not necessarily decimal hours.

For:

```text
8 hours 30 minutes
```

you get:

```text
8
```

If you need decimal hours, use seconds:

```sql
SELECT
    TIMESTAMPDIFF(
        SECOND,
        login_time,
        logout_time
    ) / 3600.0 AS hours_worked
FROM attendance;
```

Output:

```text
8.5000
```

---

# 40. TIMESTAMPDIFF units in MySQL

You can use:

```sql
SECOND
MINUTE
HOUR
DAY
WEEK
MONTH
QUARTER
YEAR
```

Example:

```sql
TIMESTAMPDIFF(DAY, start_date, end_date)
```

```sql
TIMESTAMPDIFF(MONTH, start_date, end_date)
```

```sql
TIMESTAMPDIFF(YEAR, birth_date, CURDATE())
```

---

# 41. Age calculation

Classic interview question:

> Find the age of every customer.

### MySQL

```sql
SELECT
    customer_id,
    TIMESTAMPDIFF(
        YEAR,
        birth_date,
        CURDATE()
    ) AS age
FROM customers;
```

### PostgreSQL

You can use:

```sql
SELECT
    customer_id,
    EXTRACT(
        YEAR FROM AGE(CURRENT_DATE, birth_date)
    ) AS age
FROM customers;
```

---

# 42. `AGE()` in PostgreSQL

Suppose:

```text
birth_date = 2000-05-10
today      = 2026-08-23
```

```sql
SELECT AGE(
    DATE '2026-08-23',
    DATE '2000-05-10'
);
```

Result conceptually:

```text
26 years 3 mons 13 days
```

Then:

```sql
EXTRACT(
    YEAR FROM AGE(...)
)
```

returns:

```text
26
```

---

# 43. Find people older than 30

Don't think:

```text
YEAR(current) - YEAR(birth)
```

because birthdays matter.

Use the database's date difference functions.

MySQL:

```sql
WHERE TIMESTAMPDIFF(
    YEAR,
    birth_date,
    CURDATE()
) > 30
```

PostgreSQL:

```sql
WHERE EXTRACT(
    YEAR FROM AGE(CURRENT_DATE, birth_date)
) > 30
```

---

# 44. Find records between two times

Suppose:

```text
09:00:00
10:30:00
13:00:00
18:00:00
```

Question:

> Find orders placed between 9 AM and noon.

MySQL:

```sql
SELECT *
FROM orders
WHERE TIME(created_at) >= '09:00:00'
  AND TIME(created_at) < '12:00:00';
```

PostgreSQL:

```sql
SELECT *
FROM orders
WHERE created_at::time >= TIME '09:00:00'
  AND created_at::time < TIME '12:00:00';
```

For indexed timestamp columns, however, if you know the actual date range, prefer timestamp ranges.

---

# 45. Business hours

Question:

> Find transactions made during business hours, 9 AM–5 PM.

MySQL:

```sql
WHERE TIME(created_at) >= '09:00:00'
  AND TIME(created_at) < '17:00:00'
```

PostgreSQL:

```sql
WHERE created_at::time >= TIME '09:00:00'
  AND created_at::time < TIME '17:00:00'
```

---

# 46. Time ranges crossing midnight

This is a classic trap.

Suppose night shift:

```text
22:00 → 06:00
```

You cannot write:

```sql
WHERE time >= '22:00'
  AND time < '06:00'
```

because no time can satisfy both.

Instead:

```sql
WHERE time >= '22:00'
   OR time < '06:00'
```

Mental picture:

```text
22:00 ───────── 24:00
00:00 ───────── 06:00
```

So:

```text
OR
```

not:

```text
AND
```

---

# 47. Find latest record

Question:

> Find the latest order.

```sql
SELECT *
FROM orders
ORDER BY created_at DESC
LIMIT 1;
```

PostgreSQL and MySQL both support this.

---

# 48. Find earliest record

```sql
SELECT *
FROM orders
ORDER BY created_at ASC
LIMIT 1;
```

---

# 49. Latest order for each customer

Now the question changes.

> Find the latest order for every customer.

This is **not**:

```sql
GROUP BY customer_id
```

because we need the entire row associated with the latest date.

Use a window function.

```sql
SELECT *
FROM (
    SELECT
        o.*,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY created_at DESC
        ) AS rn
    FROM orders o
) x
WHERE rn = 1;
```

This is one of the most important SQL interview patterns.

---

# 50. Why `MAX()` alone isn't enough

Suppose:

| customer_id | order_id | created_at |
| ----------: | -------: | ---------- |
|           1 |      101 | Jan 10     |
|           1 |      102 | Jan 20     |
|           1 |      103 | Jan 25     |

This:

```sql
SELECT
    customer_id,
    MAX(created_at)
FROM orders
GROUP BY customer_id;
```

gives:

| customer_id | max_date |
| ----------: | -------- |
|           1 | Jan 25   |

But maybe you need:

```text
order_id = 103
amount = 500
product = Laptop
```

`MAX(created_at)` doesn't automatically give you the row.

Use:

```sql
ROW_NUMBER()
```

---

# 51. Latest row per group — universal pattern

Memorize:

```sql
SELECT *
FROM (
    SELECT
        t.*,
        ROW_NUMBER() OVER (
            PARTITION BY group_column
            ORDER BY date_column DESC
        ) AS rn
    FROM table_name t
) x
WHERE rn = 1;
```

Examples:

```text
latest order per customer
latest login per user
latest salary per employee
latest status per ticket
latest transaction per account
```

Same pattern.

---

# 52. Latest record using PostgreSQL `DISTINCT ON`

PostgreSQL has a beautiful shortcut:

```sql
SELECT DISTINCT ON (customer_id)
    *
FROM orders
ORDER BY customer_id, created_at DESC;
```

This returns the latest row for each customer.

But this is **PostgreSQL-specific**.

For cross-database interview answers, `ROW_NUMBER()` is safer.

---

# 53. Second latest order

Question:

> Find the second latest order for each customer.

```sql
SELECT *
FROM (
    SELECT
        o.*,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY created_at DESC
        ) AS rn
    FROM orders o
) x
WHERE rn = 2;
```

---

# 54. Latest 3 orders per customer

Change:

```sql
WHERE rn = 1
```

to:

```sql
WHERE rn <= 3
```

Pattern:

```sql
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY created_at DESC
)
```

---

# 55. Previous order date

Question:

> For every order, show the customer's previous order date.

Use `LAG()`:

```sql
SELECT
    customer_id,
    order_id,
    order_date,
    LAG(order_date) OVER (
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS previous_order_date
FROM orders;
```

Example:

| customer | order | date   | previous |
| -------: | ----: | ------ | -------- |
|        1 |   101 | Jan 5  | NULL     |
|        1 |   102 | Jan 10 | Jan 5    |
|        1 |   103 | Jan 25 | Jan 10   |
|        2 |   104 | Feb 1  | NULL     |
|        2 |   105 | Feb 20 | Feb 1    |

---

# 56. Days between consecutive orders

Now combine:

```text
LAG
+
date difference
```

MySQL:

```sql
SELECT
    customer_id,
    order_id,
    order_date,
    DATEDIFF(
        order_date,
        LAG(order_date) OVER (
            PARTITION BY customer_id
            ORDER BY order_date
        )
    ) AS days_since_previous
FROM orders;
```

Depending on MySQL version/query restrictions, you may prefer a CTE/subquery because window-function results are often clearer when separated.

```sql
WITH x AS (
    SELECT
        customer_id,
        order_id,
        order_date,
        LAG(order_date) OVER (
            PARTITION BY customer_id
            ORDER BY order_date
        ) AS previous_date
    FROM orders
)
SELECT
    customer_id,
    order_id,
    order_date,
    DATEDIFF(order_date, previous_date) AS days_since_previous
FROM x;
```

PostgreSQL:

```sql
WITH x AS (
    SELECT
        customer_id,
        order_id,
        order_date,
        LAG(order_date) OVER (
            PARTITION BY customer_id
            ORDER BY order_date
        ) AS previous_date
    FROM orders
)
SELECT
    customer_id,
    order_id,
    order_date,
    order_date - previous_date AS days_since_previous
FROM x;
```

---

# 57. Month-over-month growth

This is a **very common interview pattern**.

Suppose monthly revenue is:

| month | revenue |
| ----- | ------: |
| Jan   |   10000 |
| Feb   |   12000 |
| Mar   |   15000 |

We want:

| month | revenue | previous | growth |
| ----- | ------: | -------: | -----: |
| Jan   |   10000 |     NULL |   NULL |
| Feb   |   12000 |    10000 |    20% |
| Mar   |   15000 |    12000 |    25% |

First aggregate:

```sql
WITH monthly_sales AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(amount) AS revenue
    FROM orders
    GROUP BY DATE_TRUNC('month', order_date)
)
```

Then:

```sql
SELECT
    month,
    revenue,
    LAG(revenue) OVER (
        ORDER BY month
    ) AS previous_revenue
FROM monthly_sales;
```

Then percentage:

```sql
WITH monthly_sales AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(amount) AS revenue
    FROM orders
    GROUP BY DATE_TRUNC('month', order_date)
),
x AS (
    SELECT
        month,
        revenue,
        LAG(revenue) OVER (
            ORDER BY month
        ) AS previous_revenue
    FROM monthly_sales
)
SELECT
    month,
    revenue,
    previous_revenue,
    ROUND(
        100.0 * (revenue - previous_revenue)
        / NULLIF(previous_revenue, 0),
        2
    ) AS growth_pct
FROM x;
```

---

# 58. The general time-series pattern

Memorize this:

```text
RAW DATA
   ↓
GROUP BY date period
   ↓
aggregate
   ↓
LAG / LEAD
   ↓
compare
```

This solves:

- day-over-day sales
- month-over-month sales
- year-over-year sales
- previous login
- previous transaction
- previous status
- change in price
- change in balance

---

# 59. Day-over-day

```sql
WITH daily AS (
    SELECT
        DATE(order_date) AS day,
        SUM(amount) AS revenue
    FROM orders
    GROUP BY DATE(order_date)
),
x AS (
    SELECT
        day,
        revenue,
        LAG(revenue) OVER (
            ORDER BY day
        ) AS previous_day_revenue
    FROM daily
)
SELECT *
FROM x;
```

PostgreSQL version:

```sql
DATE_TRUNC('day', order_date)
```

instead of:

```sql
DATE(order_date)
```

---

# 60. Year-over-year

First aggregate yearly:

```sql
WITH yearly AS (
    SELECT
        YEAR(order_date) AS year,
        SUM(amount) AS revenue
    FROM orders
    GROUP BY YEAR(order_date)
)
SELECT
    year,
    revenue,
    LAG(revenue) OVER (
        ORDER BY year
    ) AS previous_year_revenue
FROM yearly;
```

PostgreSQL:

```sql
WITH yearly AS (
    SELECT
        EXTRACT(YEAR FROM order_date) AS year,
        SUM(amount) AS revenue
    FROM orders
    GROUP BY EXTRACT(YEAR FROM order_date)
)
SELECT
    year,
    revenue,
    LAG(revenue) OVER (
        ORDER BY year
    ) AS previous_year_revenue
FROM yearly;
```

---

# 61. `LEAD()` — future date

`LAG()` means:

```text
previous row
```

`LEAD()` means:

```text
next row
```

Example:

```sql
SELECT
    customer_id,
    order_date,
    LEAD(order_date) OVER (
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS next_order_date
FROM orders;
```

---

# 62. Days until next order

```sql
WITH x AS (
    SELECT
        customer_id,
        order_date,
        LEAD(order_date) OVER (
            PARTITION BY customer_id
            ORDER BY order_date
        ) AS next_order_date
    FROM orders
)
SELECT
    customer_id,
    order_date,
    next_order_date,
    next_order_date - order_date AS days_until_next
FROM x;
```

PostgreSQL.

---

# 63. Find customers who haven't ordered recently

Question:

> Find customers whose last order was more than 90 days ago.

Step 1:

```sql
MAX(order_date)
```

Step 2:

compare with current date.

PostgreSQL:

```sql
SELECT
    customer_id,
    MAX(order_date) AS last_order_date
FROM orders
GROUP BY customer_id
HAVING MAX(order_date)
       < CURRENT_DATE - INTERVAL '90 days';
```

MySQL:

```sql
SELECT
    customer_id,
    MAX(order_date) AS last_order_date
FROM orders
GROUP BY customer_id
HAVING MAX(order_date)
       < CURDATE() - INTERVAL 90 DAY;
```

Notice:

```text
GROUP BY → HAVING
```

because we're filtering an aggregate result.

---

# 64. Customers who ordered in the last 30 days

```sql
SELECT DISTINCT customer_id
FROM orders
WHERE order_date >= CURRENT_DATE - INTERVAL '30 days';
```

MySQL:

```sql
SELECT DISTINCT customer_id
FROM orders
WHERE order_date >= CURDATE() - INTERVAL 30 DAY;
```

---

# 65. Customers who ordered in both January and February

This is a classic interview problem.

We need:

```text
customer has Jan order
AND
customer has Feb order
```

One solution:

```sql
SELECT customer_id
FROM orders
GROUP BY customer_id
HAVING
    SUM(
        CASE
            WHEN order_date >= '2026-01-01'
             AND order_date < '2026-02-01'
            THEN 1 ELSE 0
        END
    ) > 0
AND
    SUM(
        CASE
            WHEN order_date >= '2026-02-01'
             AND order_date < '2026-03-01'
            THEN 1 ELSE 0
        END
    ) > 0;
```

This pattern generalizes to:

```text
bought product A and B
logged in Monday and Tuesday
used feature X and Y
purchased in multiple months
```

---

# 66. Customers who ordered in consecutive months

More advanced.

We can first get unique customer/month combinations:

```sql
WITH months AS (
    SELECT DISTINCT
        customer_id,
        DATE_TRUNC('month', order_date) AS month
    FROM orders
)
SELECT *
FROM months;
```

Then use `LAG()`:

```sql
WITH months AS (
    SELECT DISTINCT
        customer_id,
        DATE_TRUNC('month', order_date) AS month
    FROM orders
),
x AS (
    SELECT
        customer_id,
        month,
        LAG(month) OVER (
            PARTITION BY customer_id
            ORDER BY month
        ) AS previous_month
    FROM months
)
SELECT *
FROM x
WHERE month = previous_month + INTERVAL '1 month';
```

This is an important advanced pattern:

```text
deduplicate periods
      ↓
LAG
      ↓
date arithmetic
      ↓
compare
```

---

# 67. Consecutive days

Question:

> Find users who logged in on consecutive days.

Start:

```sql
WITH logins AS (
    SELECT DISTINCT
        user_id,
        login_date::date AS login_date
    FROM user_logins
)
```

Then:

```sql
WITH logins AS (
    SELECT DISTINCT
        user_id,
        login_date::date AS login_date
    FROM user_logins
),
x AS (
    SELECT
        user_id,
        login_date,
        LAG(login_date) OVER (
            PARTITION BY user_id
            ORDER BY login_date
        ) AS previous_date
    FROM logins
)
SELECT *
FROM x
WHERE login_date = previous_date + INTERVAL '1 day';
```

---

# 68. Gaps and islands — advanced date pattern

This is one of the most powerful interview concepts.

Suppose a user logged in:

```text
Jan 1
Jan 2
Jan 3
Jan 7
Jan 8
Jan 10
```

We want groups:

```text
Group 1:
Jan 1
Jan 2
Jan 3

Group 2:
Jan 7
Jan 8

Group 3:
Jan 10
```

This is called the **gaps and islands problem**.

One common technique:

```sql
ROW_NUMBER()
```

combined with date arithmetic.

PostgreSQL example:

```sql
WITH days AS (
    SELECT DISTINCT
        user_id,
        login_date::date AS login_date
    FROM logins
),
numbered AS (
    SELECT
        user_id,
        login_date,
        login_date
          - ROW_NUMBER() OVER (
                PARTITION BY user_id
                ORDER BY login_date
            )::int AS grp
    FROM days
)
SELECT
    user_id,
    MIN(login_date) AS start_date,
    MAX(login_date) AS end_date,
    COUNT(*) AS consecutive_days
FROM numbered
GROUP BY user_id, grp;
```

This can solve:

- longest login streak
- consecutive purchases
- consecutive active days
- consecutive attendance
- consecutive temperature increases
- consecutive status periods

---

# 69. Find the longest consecutive login streak

Building on the previous query:

```sql
WITH days AS (
    SELECT DISTINCT
        user_id,
        login_date::date AS login_date
    FROM logins
),
numbered AS (
    SELECT
        user_id,
        login_date,
        login_date
          - ROW_NUMBER() OVER (
                PARTITION BY user_id
                ORDER BY login_date
            )::int AS grp
    FROM days
),
streaks AS (
    SELECT
        user_id,
        grp,
        COUNT(*) AS streak_length
    FROM numbered
    GROUP BY user_id, grp
)
SELECT
    user_id,
    MAX(streak_length) AS longest_streak
FROM streaks
GROUP BY user_id;
```

This is an **advanced interview pattern worth learning separately**.

---

# 70. Start of month

## PostgreSQL

Extremely easy:

```sql
DATE_TRUNC('month', date_column)
```

Example:

```text
2026-08-23
```

→

```text
2026-08-01 00:00:00
```

---

## MySQL

A common pattern:

```sql
DATE_FORMAT(date_column, '%Y-%m-01')
```

Result:

```text
2026-08-01
```

---

# 71. End of month

## MySQL

Easy:

```sql
LAST_DAY(order_date)
```

Example:

```text
2026-02-15
```

→

```text
2026-02-28
```

---

## PostgreSQL

Use:

```sql
DATE_TRUNC('month', order_date)
+ INTERVAL '1 month'
- INTERVAL '1 day'
```

For timestamp output.

Or construct a date as needed.

---

# 72. First day of current month

PostgreSQL:

```sql
DATE_TRUNC('month', CURRENT_DATE)
```

MySQL:

```sql
DATE_FORMAT(CURDATE(), '%Y-%m-01')
```

---

# 73. Last day of current month

MySQL:

```sql
LAST_DAY(CURDATE())
```

PostgreSQL:

```sql
DATE_TRUNC('month', CURRENT_DATE)
+ INTERVAL '1 month'
- INTERVAL '1 day'
```

---

# 74. Number of days in a month

MySQL:

```sql
DAY(LAST_DAY('2026-02-01'))
```

Output:

```text
28
```

PostgreSQL:

```sql
EXTRACT(
    DAY FROM (
        DATE_TRUNC('month', DATE '2026-02-01')
        + INTERVAL '1 month'
        - INTERVAL '1 day'
    )
)
```

---

# 75. Leap year

A leap year generally satisfies:

```text
divisible by 4
AND
not divisible by 100
unless divisible by 400
```

SQL can test this mathematically, but in practical SQL questions it's often easier to use date functions.

For example, if you need the number of days in February:

```text
28 → normal year
29 → leap year
```

MySQL:

```sql
DAY(LAST_DAY('2024-02-01'))
```

Output:

```text
29
```

---

# 76. Formatting dates

This is different from extracting.

**Extracting**:

```text
2026
```

**Formatting**:

```text
2026-08
```

or:

```text
Aug 2026
```

---

## MySQL

```sql
DATE_FORMAT(
    created_at,
    '%Y-%m-%d'
)
```

Output:

```text
2026-08-23
```

```sql
DATE_FORMAT(
    created_at,
    '%Y-%m'
)
```

Output:

```text
2026-08
```

```sql
DATE_FORMAT(
    created_at,
    '%M %Y'
)
```

Output:

```text
August 2026
```

---

# 77. PostgreSQL formatting

Use:

```sql
TO_CHAR()
```

```sql
TO_CHAR(
    created_at,
    'YYYY-MM-DD'
)
```

Output:

```text
2026-08-23
```

```sql
TO_CHAR(
    created_at,
    'YYYY-MM'
)
```

Output:

```text
2026-08
```

```sql
TO_CHAR(
    created_at,
    'Month YYYY'
)
```

Output:

```text
August 2026
```

---

# 78. Formatting vs filtering — important

Don't do this unnecessarily:

```sql
WHERE DATE_FORMAT(created_at, '%Y-%m') = '2026-08'
```

Prefer:

```sql
WHERE created_at >= '2026-08-01'
  AND created_at < '2026-09-01'
```

Why?

Because the second approach preserves a clean range condition on the original column and is generally friendlier to indexes.

---

# 79. Casting timestamp to date

PostgreSQL:

```sql
SELECT created_at::date
FROM orders;
```

or:

```sql
CAST(created_at AS DATE)
```

MySQL:

```sql
SELECT DATE(created_at)
FROM orders;
```

---

# 80. Casting date to timestamp

PostgreSQL:

```sql
SELECT order_date::timestamp
FROM orders;
```

MySQL:

```sql
CAST(order_date AS DATETIME)
```

---

# 81. Null dates

Suppose:

| employee | joined_at  |
| -------- | ---------- |
| A        | 2025-01-01 |
| B        | NULL       |

If you calculate:

```sql
CURRENT_DATE - joined_at
```

the result is `NULL` for B.

Remember:

```text
anything + NULL = NULL
anything - NULL = NULL
```

Use:

```sql
COALESCE()
```

when a fallback makes business sense.

Example:

```sql
COALESCE(joined_at, CURRENT_DATE)
```

But don't blindly replace NULL dates; NULL often means **unknown**, not zero.

---

# 82. Count orders per month including months with ZERO orders

This is a classic interview upgrade.

Suppose:

```text
January → 10
February → 0
March → 15
```

A simple:

```sql
GROUP BY month
```

returns:

```text
January 10
March 15
```

February disappears.

Why?

Because **there is no row to group**.

To show zero-count periods, generate a calendar/month series and `LEFT JOIN`.

PostgreSQL:

```sql
WITH months AS (
    SELECT generate_series(
        DATE '2026-01-01',
        DATE '2026-12-01',
        INTERVAL '1 month'
    ) AS month
)
SELECT
    m.month,
    COUNT(o.order_id) AS order_count
FROM months m
LEFT JOIN orders o
    ON o.order_date >= m.month
   AND o.order_date <  m.month + INTERVAL '1 month'
GROUP BY m.month
ORDER BY m.month;
```

This is a **very useful PostgreSQL interview pattern**.

---

# 83. Why `COUNT(*)` can be wrong with LEFT JOIN

Consider:

```sql
FROM months m
LEFT JOIN orders o ...
```

Use:

```sql
COUNT(o.order_id)
```

not:

```sql
COUNT(*)
```

because `COUNT(*)` counts the generated month row even when no order matched.

Example:

```text
month       order_id
January     101
February    NULL
March       103
```

```sql
COUNT(*)
```

could give:

```text
1
1
1
```

while:

```sql
COUNT(order_id)
```

gives:

```text
1
0
1
```

Very important.

---

# 84. MySQL calendar generation

MySQL does not have PostgreSQL's `generate_series()` built-in.

You can use:

- recursive CTEs
- calendar tables
- numbers/tally tables

Example recursive CTE:

```sql
WITH RECURSIVE months AS (
    SELECT DATE('2026-01-01') AS month

    UNION ALL

    SELECT month + INTERVAL 1 MONTH
    FROM months
    WHERE month < '2026-12-01'
)
SELECT *
FROM months;
```

This generates:

```text
2026-01-01
2026-02-01
...
2026-12-01
```

---

# 85. Time zones — extremely important

Now we reach one of the most misunderstood SQL topics.

Suppose a user in India performs an action at:

```text
10:00 AM IST
```

Another user in New York sees the same event as:

```text
12:30 AM EST
```

It is the **same instant**, different local representations.

You need to distinguish:

```text
instant
```

from:

```text
local time
```

---

# 86. PostgreSQL timezone types

PostgreSQL has:

```sql
TIMESTAMP
```

and:

```sql
TIMESTAMPTZ
```

Important conceptual distinction:

### `timestamp without time zone`

A calendar date/time with no timezone interpretation.

```text
2026-08-23 10:00:00
```

### `timestamptz`

An absolute instant represented according to a timezone.

This distinction is critical when designing systems.

---

# 87. PostgreSQL timezone conversion

Example:

```sql
SELECT
    created_at AT TIME ZONE 'Asia/Kolkata'
FROM events;
```

You can convert/display an instant in a desired timezone.

Common timezone names:

```text
Asia/Kolkata
America/New_York
Europe/London
UTC
```

Use IANA timezone names rather than manually hardcoding offsets when timezone rules matter.

---

# 88. MySQL time zones

MySQL supports timezone conversion through:

```sql
CONVERT_TZ()
```

Example:

```sql
SELECT CONVERT_TZ(
    created_at,
    'UTC',
    'Asia/Kolkata'
)
FROM events;
```

Conceptually:

```text
UTC
 ↓
Asia/Kolkata
```

---

# 89. UTC storage — interview principle

A common production approach is:

```text
Store timestamps consistently in UTC
            ↓
Convert to user's timezone at presentation/query boundaries
```

But the correct implementation depends on the application's requirements.

The important interview point is:

> Don't confuse a local wall-clock time with an absolute instant.

---

# 90. Daylight Saving Time

This matters especially with:

```text
America/New_York
Europe/London
etc.
```

A timezone is not simply:

```text
UTC + fixed number
```

because daylight-saving rules can change the offset.

That's why:

```text
America/New_York
```

is better than blindly storing:

```text
UTC-5
```

when timezone-aware behavior is required.

India does not currently observe DST, but applications serving global users still need to handle it.

---

# 91. `DATE` vs `TIMESTAMP` — interview question

### Use `DATE` when:

You care only about:

```text
2026-08-23
```

Examples:

- birthday
- holiday
- due date
- business date

### Use timestamp when:

You care about an exact moment:

```text
2026-08-23 14:32:15
```

Examples:

- transaction
- login
- API request
- payment
- event

### Use time when:

Only time-of-day matters:

```text
09:00:00
```

Examples:

- store opening time
- daily schedule

---

# 92. Common interview trap: birthday

A birthday usually shouldn't be stored as:

```text
TIMESTAMP
```

if the business meaning is simply:

```text
1999-07-12
```

Use:

```sql
DATE
```

because the time of birth isn't relevant to many business calculations.

---

# 93. Common interview trap: transaction time

A transaction should usually preserve the actual event time.

```text
2026-08-23 14:31:25
```

So:

```sql
TIMESTAMP / TIMESTAMPTZ
```

depending on database and timezone requirements.

---

# 94. Find today's birthdays

Suppose:

```text
birth_date
```

contains:

```text
1990-08-23
1985-03-10
2001-08-23
```

We want:

```text
August 23
```

MySQL:

```sql
WHERE MONTH(birth_date) = MONTH(CURDATE())
  AND DAY(birth_date) = DAY(CURDATE())
```

PostgreSQL:

```sql
WHERE EXTRACT(MONTH FROM birth_date)
        = EXTRACT(MONTH FROM CURRENT_DATE)
  AND EXTRACT(DAY FROM birth_date)
        = EXTRACT(DAY FROM CURRENT_DATE)
```

---

# 95. Find employees who completed 1 year today

This is a very common business query.

Conceptually:

```text
joining date + 1 year = today
```

MySQL:

```sql
WHERE DATE_ADD(joining_date, INTERVAL 1 YEAR)
      = CURDATE()
```

PostgreSQL:

```sql
WHERE joining_date + INTERVAL '1 year'
      = CURRENT_DATE
```

For large indexed tables, you can also formulate conditions around the target date rather than transforming the column.

---

# 96. Employees with work anniversary this month

MySQL:

```sql
WHERE MONTH(joining_date) = MONTH(CURDATE())
```

PostgreSQL:

```sql
WHERE EXTRACT(MONTH FROM joining_date)
    = EXTRACT(MONTH FROM CURRENT_DATE)
```

---

# 97. Find orders placed after 6 PM

MySQL:

```sql
WHERE TIME(created_at) >= '18:00:00'
```

PostgreSQL:

```sql
WHERE created_at::time >= TIME '18:00:00'
```

---

# 98. Find orders placed during lunch

Suppose:

```text
12:00 – 14:00
```

```sql
WHERE created_at::time >= TIME '12:00:00'
  AND created_at::time < TIME '14:00:00'
```

MySQL:

```sql
WHERE TIME(created_at) >= '12:00:00'
  AND TIME(created_at) < '14:00:00'
```

---

# 99. Find the busiest hour

Suppose we want:

> Which hour gets the most orders?

MySQL:

```sql
SELECT
    HOUR(created_at) AS hour,
    COUNT(*) AS orders
FROM orders
GROUP BY HOUR(created_at)
ORDER BY orders DESC
LIMIT 1;
```

PostgreSQL:

```sql
SELECT
    EXTRACT(HOUR FROM created_at) AS hour,
    COUNT(*) AS orders
FROM orders
GROUP BY EXTRACT(HOUR FROM created_at)
ORDER BY orders DESC
LIMIT 1;
```

---

# 100. Find busiest day of week

MySQL:

```sql
SELECT
    DAYNAME(order_date) AS day_name,
    COUNT(*) AS total_orders
FROM orders
GROUP BY
    DAYOFWEEK(order_date),
    DAYNAME(order_date)
ORDER BY total_orders DESC
LIMIT 1;
```

---

# 101. Find the longest gap between orders

Advanced but extremely useful.

First:

```sql
LAG()
```

Then:

```text
current date - previous date
```

PostgreSQL:

```sql
WITH x AS (
    SELECT
        customer_id,
        order_date,
        LAG(order_date) OVER (
            PARTITION BY customer_id
            ORDER BY order_date
        ) AS previous_order
    FROM orders
),
gaps AS (
    SELECT
        customer_id,
        order_date,
        order_date - previous_order AS gap
    FROM x
    WHERE previous_order IS NOT NULL
)
SELECT *
FROM gaps
ORDER BY gap DESC;
```

---

# 102. Find customers with no order in the last 90 days

```sql
SELECT
    customer_id,
    MAX(order_date) AS last_order
FROM orders
GROUP BY customer_id
HAVING MAX(order_date)
       < CURRENT_DATE - INTERVAL '90 days';
```

But what about customers who **never ordered**?

They won't exist in `orders`.

So if you have:

```text
customers
orders
```

use:

```sql
SELECT
    c.customer_id,
    MAX(o.order_date) AS last_order
FROM customers c
LEFT JOIN orders o
    ON o.customer_id = c.customer_id
GROUP BY c.customer_id
HAVING MAX(o.order_date) IS NULL
    OR MAX(o.order_date) < CURRENT_DATE - INTERVAL '90 days';
```

This is an excellent interview example combining:

```text
LEFT JOIN
+
GROUP BY
+
MAX
+
HAVING
+
date arithmetic
```

---

# 103. Find orders in the current week

This is more dialect-sensitive because **week definitions vary**.

The important interview lesson:

> Always clarify what the business considers the first day of the week.

For example:

```text
Monday → Sunday
```

vs.

```text
Sunday → Saturday
```

PostgreSQL can use:

```sql
DATE_TRUNC('week', CURRENT_DATE)
```

which follows PostgreSQL's week semantics.

Then:

```sql
WHERE created_at >= DATE_TRUNC('week', CURRENT_DATE)
  AND created_at < DATE_TRUNC('week', CURRENT_DATE)
                   + INTERVAL '1 week'
```

---

# 104. ISO week

PostgreSQL:

```sql
EXTRACT(ISOYEAR FROM date_column)
EXTRACT(WEEK FROM date_column)
```

Be careful:

**ISO week-year isn't always the same as calendar year.**

For dates around New Year's, this matters.

This is an excellent "gotcha" interviewers sometimes use.

---

# 105. Week number — hidden trap

Suppose:

```text
January 1
```

It may belong to the last ISO week of the previous ISO year depending on the calendar.

Therefore:

```text
week number
```

alone is not always a safe grouping key.

For ISO reporting, consider:

```text
ISOYEAR + WEEK
```

rather than just:

```text
WEEK
```

---

# 106. Date comparisons

You can compare dates directly.

```sql
WHERE start_date < end_date
```

Examples:

### Orders delivered late

```sql
WHERE delivery_date > promised_date
```

### Expired products

```sql
WHERE expiry_date < CURRENT_DATE
```

### Active subscriptions

```sql
WHERE start_date <= CURRENT_DATE
  AND end_date >= CURRENT_DATE
```

---

# 107. Active between two dates

Suppose a subscription:

```text
start_date
end_date
```

Question:

> Was this subscription active on January 15?

```sql
WHERE start_date <= '2026-01-15'
  AND end_date >= '2026-01-15'
```

This pattern is extremely important.

---

# 108. Detect overlapping date ranges

Suppose:

```text
A: start_a → end_a
B: start_b → end_b
```

They overlap if:

```text
start_a <= end_b
AND
start_b <= end_a
```

SQL:

```sql
WHERE start_a <= end_b
  AND start_b <= end_a
```

This solves:

- booking conflicts
- employee shifts
- room reservations
- subscriptions
- campaigns
- project periods

---

# 109. Non-overlap condition

Two intervals do NOT overlap when:

```text
A ends before B starts
OR
B ends before A starts
```

```sql
end_a < start_b
OR
end_b < start_a
```

Then overlap is the negation:

```sql
NOT (
    end_a < start_b
    OR
    end_b < start_a
)
```

---

# 110. Date filtering with indexes — VERY IMPORTANT

Suppose:

```sql
INDEX(created_at)
```

Bad-ish pattern:

```sql
WHERE YEAR(created_at) = 2026
```

because the database may need to evaluate the function against many rows.

Better:

```sql
WHERE created_at >= '2026-01-01'
  AND created_at <  '2027-01-01'
```

Same business meaning, better index-friendly shape.

Similarly:

Instead of:

```sql
WHERE DATE(created_at) = '2026-08-23'
```

prefer:

```sql
WHERE created_at >= '2026-08-23'
  AND created_at <  '2026-08-24'
```

---

# 111. The "function on column" rule

When filtering:

### Less index-friendly pattern

```sql
WHERE FUNCTION(date_column) = value
```

Examples:

```sql
WHERE YEAR(created_at) = 2026
```

```sql
WHERE DATE(created_at) = '2026-08-23'
```

```sql
WHERE DATE_FORMAT(created_at, ...) = ...
```

### Better pattern

Transform the **constant/boundaries**, not the indexed column:

```sql
WHERE created_at >= start
  AND created_at < end
```

This is one of the best SQL performance habits you can develop.

---

# 112. DATE vs DATETIME/TIMESTAMP filtering

Suppose:

```text
created_at = 2026-08-23 15:45:30
```

This does **not** equal:

```sql
'2026-08-23'
```

unless the database performs implicit conversion in a particular way.

So for an entire day, use:

```sql
created_at >= '2026-08-23'
AND created_at < '2026-08-24'
```

---

# 113. Half-open intervals — memorize this

The most robust mental model:

```text
[start, end)
```

means:

```text
>= start
< end
```

Examples:

### Day

```text
[2026-08-23, 2026-08-24)
```

### Month

```text
[2026-08-01, 2026-09-01)
```

### Year

```text
[2026-01-01, 2027-01-01)
```

### Hour

```text
[10:00, 11:00)
```

This eliminates many boundary bugs.

---

# 114. `BETWEEN` vs half-open ranges

For dates:

```sql
BETWEEN '2026-01-01' AND '2026-01-31'
```

may be okay.

For timestamps, prefer:

```sql
>= '2026-01-01'
AND < '2026-02-01'
```

Why?

Because the next boundary is exact.

---

# 115. Date truncation vs date extraction

These look similar but mean different things.

### Extraction

```sql
EXTRACT(MONTH FROM created_at)
```

gives:

```text
8
```

### Truncation

```sql
DATE_TRUNC('month', created_at)
```

gives:

```text
2026-08-01 00:00:00
```

Think:

```text
EXTRACT → number
TRUNCATE → bucket/start point
```

---

# 116. When to use `EXTRACT`

Use it when you want:

```text
year
month
day
hour
quarter
week
```

Example:

```sql
SELECT
    EXTRACT(YEAR FROM created_at)
FROM orders;
```

---

# 117. When to use `DATE_TRUNC`

Use it when you want to create a **time bucket**.

Examples:

```sql
DATE_TRUNC('day', created_at)
DATE_TRUNC('week', created_at)
DATE_TRUNC('month', created_at)
DATE_TRUNC('quarter', created_at)
DATE_TRUNC('year', created_at)
```

Perfect for:

```sql
GROUP BY
```

and time-series analysis.

---

# 118. The complete time-series recipe

When asked:

> Revenue by month

Think:

```text
1. Identify month
2. Aggregate
3. Group
4. Order
```

PostgreSQL:

```sql
SELECT
    DATE_TRUNC('month', order_date) AS month,
    SUM(amount) AS revenue
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY month;
```

MySQL:

```sql
SELECT
    DATE_FORMAT(order_date, '%Y-%m-01') AS month,
    SUM(amount) AS revenue
FROM orders
GROUP BY DATE_FORMAT(order_date, '%Y-%m-01')
ORDER BY month;
```

---

# 119. The complete "previous period" recipe

Question:

> Show monthly sales and previous month's sales.

Think:

```text
monthly aggregation
       ↓
LAG
```

```sql
WITH monthly AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(amount) AS revenue
    FROM orders
    GROUP BY DATE_TRUNC('month', order_date)
)
SELECT
    month,
    revenue,
    LAG(revenue) OVER (
        ORDER BY month
    ) AS previous_revenue
FROM monthly;
```

---

# 120. The complete "latest per entity" recipe

Question:

> Latest order per customer.

Think:

```text
PARTITION BY customer
ORDER BY date DESC
ROW_NUMBER
```

```sql
WITH ranked AS (
    SELECT
        o.*,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY created_at DESC
        ) AS rn
    FROM orders o
)
SELECT *
FROM ranked
WHERE rn = 1;
```

---

# 121. The complete "date difference" recipe

Question:

> Days between A and B.

Think:

```text
B - A
```

MySQL:

```sql
DATEDIFF(end_date, start_date)
```

PostgreSQL:

```sql
end_date - start_date
```

For timestamps:

```text
MySQL → TIMESTAMPDIFF
PostgreSQL → timestamp subtraction / EXTRACT(EPOCH ...)
```

---

# 122. The complete "date range" recipe

Question:

> Records between start and end.

For timestamps:

```sql
WHERE timestamp_column >= start
  AND timestamp_column < end
```

This is probably the single most important date filtering pattern.

---

# 123. The complete "current period" recipe

### Today

```text
today_start → tomorrow_start
```

### This month

```text
month_start → next_month_start
```

### This year

```text
year_start → next_year_start
```

### This week

```text
week_start → next_week_start
```

The general algorithm is:

```text
current period start
        ↓
next period start
        ↓
WHERE date >= start
  AND date < next_start
```

---

# 124. DATE/TIME interview question classification

When you see a question, classify it.

## Type A — Extract

> Find the year.

```sql
YEAR()
EXTRACT()
```

---

## Type B — Filter

> Find orders from March.

```sql
WHERE date >= start
AND date < end
```

---

## Type C — Group

> Sales by month.

```sql
GROUP BY month_bucket
```

---

## Type D — Calculate difference

> Days between order and delivery.

```sql
DATEDIFF
```

or:

```sql
end_date - start_date
```

---

## Type E — Add/subtract

> Orders due 7 days after purchase.

```sql
date + interval
```

---

## Type F — Latest/earliest

> Latest transaction.

```sql
ORDER BY date DESC
LIMIT 1
```

---

## Type G — Latest per entity

> Latest transaction per customer.

```sql
ROW_NUMBER()
```

---

## Type H — Previous/next

> Previous transaction date.

```sql
LAG()
```

> Next transaction date.

```sql
LEAD()
```

---

## Type I — Time series comparison

> Month-over-month growth.

```text
GROUP BY month
→ LAG
→ calculation
```

---

## Type J — Consecutive dates

> Longest login streak.

```text
DISTINCT dates
→ ROW_NUMBER
→ gaps/islands
→ GROUP BY
```

---

# 125. Common traps

## Trap 1

```sql
WHERE YEAR(date_column) = 2026
```

Better for indexed filtering:

```sql
WHERE date_column >= '2026-01-01'
AND date_column < '2027-01-01'
```

---

## Trap 2

```sql
BETWEEN '2026-01-01' AND '2026-01-31'
```

with timestamps.

Prefer:

```sql
>= Jan 1
< Feb 1
```

---

## Trap 3

```sql
GROUP BY MONTH(date)
```

when multiple years exist.

Use:

```text
year + month
```

---

## Trap 4

```sql
MAX(date)
```

when you need the entire latest row.

Use:

```text
ROW_NUMBER()
```

---

## Trap 5

Night shift:

```text
22:00 → 06:00
```

Don't use:

```sql
time >= 22:00
AND time < 06:00
```

Use:

```sql
time >= 22:00
OR time < 06:00
```

---

## Trap 6

Counting missing dates.

`GROUP BY` doesn't create missing periods.

Use:

```text
calendar table / generate_series
```

and:

```text
LEFT JOIN
```

---

## Trap 7

`COUNT(*)` after a `LEFT JOIN`.

For zero-count categories:

```sql
COUNT(right_table.id)
```

---

## Trap 8

Month name ordering:

```sql
ORDER BY month_name
```

may produce alphabetical order.

Use:

```text
month number
```

or:

```text
month bucket
```

---

## Trap 9

Week numbers.

A week can cross a year boundary.

Be careful with:

```text
week
```

without:

```text
year
```

---

## Trap 10

Timezone.

Don't assume:

```text
10:00 AM
```

means the same instant everywhere.

---

# 126. MySQL vs PostgreSQL cheat table

| Task                 | MySQL             | PostgreSQL                     |
| -------------------- | ----------------- | ------------------------------ |
| Current date         | `CURDATE()`       | `CURRENT_DATE`                 |
| Current time         | `CURTIME()`       | `CURRENT_TIME`                 |
| Current timestamp    | `NOW()`           | `NOW()`                        |
| Year                 | `YEAR(x)`         | `EXTRACT(YEAR FROM x)`         |
| Month                | `MONTH(x)`        | `EXTRACT(MONTH FROM x)`        |
| Day                  | `DAY(x)`          | `EXTRACT(DAY FROM x)`          |
| Hour                 | `HOUR(x)`         | `EXTRACT(HOUR FROM x)`         |
| Quarter              | `QUARTER(x)`      | `EXTRACT(QUARTER FROM x)`      |
| Day name             | `DAYNAME(x)`      | `TO_CHAR(x,'Day')`             |
| Month name           | `MONTHNAME(x)`    | `TO_CHAR(x,'Month')`           |
| Date → string        | `DATE_FORMAT()`   | `TO_CHAR()`                    |
| String → date        | `STR_TO_DATE()`   | `TO_DATE()`                    |
| Month bucket         | `DATE_FORMAT()`   | `DATE_TRUNC('month', x)`       |
| Date difference      | `DATEDIFF()`      | `date2 - date1`                |
| Timestamp difference | `TIMESTAMPDIFF()` | `timestamp2 - timestamp1`      |
| Add days             | `DATE_ADD()`      | `+ INTERVAL '1 day'`           |
| Subtract days        | `DATE_SUB()`      | `- INTERVAL '1 day'`           |
| Month end            | `LAST_DAY()`      | truncate + interval            |
| Date cast            | `DATE(x)`         | `x::date`                      |
| Time cast            | `TIME(x)`         | `x::time`                      |
| Timezone conversion  | `CONVERT_TZ()`    | `AT TIME ZONE`                 |
| Generate dates       | recursive CTE     | `generate_series()`            |
| Latest per group     | `ROW_NUMBER()`    | `ROW_NUMBER()` / `DISTINCT ON` |

---

# 127. DATE/TIME syntax patterns to memorize

## Current

```sql
-- MySQL
CURDATE()
NOW()

-- PostgreSQL
CURRENT_DATE
NOW()
```

---

## Extract

```sql
-- MySQL
YEAR(date)
MONTH(date)
DAY(date)
HOUR(timestamp)

-- PostgreSQL
EXTRACT(YEAR FROM date)
EXTRACT(MONTH FROM date)
EXTRACT(DAY FROM date)
EXTRACT(HOUR FROM timestamp)
```

---

## Add

```sql
-- MySQL
DATE_ADD(date, INTERVAL 7 DAY)

-- PostgreSQL
date + INTERVAL '7 days'
```

---

## Subtract

```sql
-- MySQL
DATE_SUB(date, INTERVAL 7 DAY)

-- PostgreSQL
date - INTERVAL '7 days'
```

---

## Difference

```sql
-- MySQL
DATEDIFF(end_date, start_date)

-- PostgreSQL
end_date - start_date
```

---

## Timestamp difference

```sql
-- MySQL
TIMESTAMPDIFF(HOUR, start_time, end_time)

-- PostgreSQL
EXTRACT(
    EPOCH FROM (end_time - start_time)
) / 3600
```

---

# 128. The MASTER English → SQL dictionary

This is the part I recommend memorizing.

| If question says...  | Think...                                   |
| -------------------- | ------------------------------------------ |
| today                | current-date range                         |
| yesterday            | previous-date range                        |
| tomorrow             | next-date range                            |
| last N days          | current timestamp − interval               |
| this week            | week start → next week                     |
| last week            | previous week range                        |
| this month           | month start → next month                   |
| last month           | previous month start → current month start |
| this year            | year start → next year                     |
| year                 | `YEAR` / `EXTRACT`                         |
| month                | `MONTH` / `DATE_TRUNC`                     |
| day                  | `DATE` / `DATE_TRUNC`                      |
| hour                 | `HOUR` / `EXTRACT`                         |
| weekday              | day-of-week                                |
| weekend              | Saturday/Sunday                            |
| quarter              | `QUARTER`                                  |
| age                  | date difference                            |
| days between         | date subtraction                           |
| hours between        | timestamp difference                       |
| after                | `>`                                        |
| before               | `<`                                        |
| on or after          | `>=`                                       |
| on or before         | `<=`                                       |
| between timestamps   | `>= start AND < end`                       |
| latest               | `DESC`                                     |
| earliest             | `ASC`                                      |
| latest per customer  | `ROW_NUMBER()`                             |
| previous             | `LAG()`                                    |
| next                 | `LEAD()`                                   |
| consecutive          | `LAG + date arithmetic`                    |
| streak               | gaps and islands                           |
| monthly trend        | monthly aggregation                        |
| previous month       | `LAG()`                                    |
| growth               | `(current - previous) / previous`          |
| active on date       | `start <= date AND end >= date`            |
| overlapping          | `start_a <= end_b AND start_b <= end_a`    |
| zero activity months | calendar + `LEFT JOIN`                     |
| timezone             | convert/interpret timezone                 |
| month name           | formatting                                 |
| fiscal year          | `CASE`                                     |
| birthday             | month + day                                |

---

# 129. A powerful 7-step approach for any date question

When you get an interview question, literally do this on paper:

### ① Identify the date column

```text
created_at?
order_date?
birth_date?
login_time?
```

### ② Identify the unit

```text
year?
month?
day?
hour?
```

### ③ Identify the operation

```text
filter?
group?
difference?
add?
compare?
```

### ④ Identify whether it's one row or multiple rows

One row:

```text
date arithmetic
```

Multiple rows:

```text
GROUP BY / window functions
```

### ⑤ Check whether you need the whole row

If yes and asking for latest/earliest:

```text
ROW_NUMBER()
```

### ⑥ Check boundaries

Ask:

```text
inclusive?
exclusive?
timestamp or date?
```

Usually:

```sql
>= start
< next_start
```

### ⑦ Check performance

Ask:

> Am I applying a function to an indexed date column?

If yes, consider a range predicate.

---

# 130. Interview pattern map

```text
                    DATE / TIME QUESTION
                           |
          +----------------+----------------+
          |                |                |
       FILTER            GROUP            CALCULATE
          |                |                |
      date range       month/year       difference
      today            day/hour        age
      last N days      quarter         duration
          |                |                |
       >= / <          GROUP BY          interval
                           |
                    +------+------+
                    |             |
                  TREND         LATEST
                    |             |
                  LAG()        ROW_NUMBER()
                    |             |
              previous period  per entity
                    |
                 growth
```

Advanced branch:

```text
LAG
 ↓
date arithmetic
 ↓
consecutive?
 ↓
Gaps & Islands
```

---

# 131. 15 interview questions you should be able to solve

After studying this, practice these without looking at the answers:

### Beginner

1. Find orders created today.
2. Find orders from the previous 7 days.
3. Extract year and month from an order date.
4. Find all orders from 2025.
5. Find orders placed on weekends.

### Intermediate

6. Count orders per month.
7. Find the latest order.
8. Find latest order for every customer.
9. Calculate delivery duration.
10. Find customers who haven't ordered in 90 days.

### Advanced

11. Calculate month-over-month revenue growth.
12. Find the previous order for each customer.
13. Find customers who ordered in consecutive months.
14. Find the longest login streak.
15. Generate a report containing months with zero orders.

If you can solve those 15 comfortably, you're in very good shape for SQL date/time interview questions.

---

# 132. THE DATE/TIME CHEATSHEET

## 🟢 MySQL

### Current

```sql
CURDATE()
CURTIME()
NOW()
CURRENT_TIMESTAMP
```

### Extract

```sql
YEAR(x)
MONTH(x)
DAY(x)
HOUR(x)
MINUTE(x)
SECOND(x)
QUARTER(x)
DAYOFWEEK(x)
DAYNAME(x)
MONTHNAME(x)
```

### Format

```sql
DATE_FORMAT(x, '%Y-%m-%d')
DATE_FORMAT(x, '%Y-%m')
DATE_FORMAT(x, '%M %Y')
```

### Add

```sql
DATE_ADD(x, INTERVAL 7 DAY)
DATE_ADD(x, INTERVAL 1 MONTH)
DATE_ADD(x, INTERVAL 1 YEAR)
```

### Subtract

```sql
DATE_SUB(x, INTERVAL 7 DAY)
DATE_SUB(x, INTERVAL 1 MONTH)
```

### Difference

```sql
DATEDIFF(end, start)

TIMESTAMPDIFF(
    DAY,
    start,
    end
)

TIMESTAMPDIFF(
    HOUR,
    start,
    end
)
```

### Month

```sql
LAST_DAY(x)

DATE_FORMAT(x, '%Y-%m-01')
```

### Timezone

```sql
CONVERT_TZ(
    x,
    'UTC',
    'Asia/Kolkata'
)
```

---

# 133. 🟣 PostgreSQL

### Current

```sql
CURRENT_DATE
CURRENT_TIME
CURRENT_TIMESTAMP
NOW()
```

### Extract

```sql
EXTRACT(YEAR FROM x)
EXTRACT(MONTH FROM x)
EXTRACT(DAY FROM x)
EXTRACT(HOUR FROM x)
EXTRACT(MINUTE FROM x)
EXTRACT(SECOND FROM x)
EXTRACT(QUARTER FROM x)
EXTRACT(WEEK FROM x)
EXTRACT(DOW FROM x)
```

### Truncate

```sql
DATE_TRUNC('day', x)
DATE_TRUNC('week', x)
DATE_TRUNC('month', x)
DATE_TRUNC('quarter', x)
DATE_TRUNC('year', x)
```

### Format

```sql
TO_CHAR(x, 'YYYY-MM-DD')
TO_CHAR(x, 'YYYY-MM')
TO_CHAR(x, 'Month YYYY')
```

### Add

```sql
x + INTERVAL '7 days'
x + INTERVAL '1 month'
x + INTERVAL '1 year'
```

### Subtract

```sql
x - INTERVAL '7 days'
```

### Difference

```sql
end_date - start_date
```

Timestamp:

```sql
end_timestamp - start_timestamp
```

Numeric seconds:

```sql
EXTRACT(
    EPOCH FROM (
        end_timestamp - start_timestamp
    )
)
```

### Age

```sql
AGE(current_date, birth_date)
```

### Cast

```sql
x::date
x::time
x::timestamp
```

### Timezone

```sql
x AT TIME ZONE 'Asia/Kolkata'
```

### Generate dates

```sql
generate_series(
    start_date,
    end_date,
    INTERVAL '1 day'
)
```

---

# 134. The 20 patterns I would memorize first

If you don't want to memorize everything immediately, learn these **20**:

### 1. Today

```sql
WHERE created_at >= CURRENT_DATE
  AND created_at < CURRENT_DATE + INTERVAL '1 day'
```

### 2. Date range

```sql
WHERE created_at >= :start
  AND created_at < :end
```

### 3. Year

```sql
EXTRACT(YEAR FROM created_at)
```

### 4. Month bucket

```sql
DATE_TRUNC('month', created_at)
```

### 5. Month grouping

```sql
GROUP BY DATE_TRUNC('month', created_at)
```

### 6. Date difference

```sql
end_date - start_date
```

### 7. MySQL date difference

```sql
DATEDIFF(end_date, start_date)
```

### 8. Add interval

```sql
date + INTERVAL '7 days'
```

### 9. Latest row

```sql
ORDER BY created_at DESC
LIMIT 1
```

### 10. Latest per group

```sql
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY created_at DESC
)
```

### 11. Previous row

```sql
LAG(date_column) OVER (...)
```

### 12. Next row

```sql
LEAD(date_column) OVER (...)
```

### 13. Month-over-month

```text
GROUP BY month
→ LAG
→ calculate growth
```

### 14. Weekend

```text
day-of-week ∈ Saturday/Sunday
```

### 15. Active interval

```sql
start_date <= target_date
AND end_date >= target_date
```

### 16. Overlap

```sql
start_a <= end_b
AND start_b <= end_a
```

### 17. Consecutive dates

```text
LAG
+
date difference
```

### 18. Streak

```text
ROW_NUMBER
+
date
→ gaps/islands
```

### 19. Missing dates

```text
calendar
+
LEFT JOIN
```

### 20. Performance

```text
Avoid transforming indexed date columns in WHERE.
Use range predicates.
```

---

# 135. Final mental model

Don't memorize 100 unrelated date functions.

Think of SQL DATE/TIME questions as **8 building blocks**:

```text
                 DATE/TIME
                     |
     +---------------+---------------+
     |               |               |
   EXTRACT          RANGE          ARITHMETIC
     |               |               |
 year/month       today/month      + days
 day/hour         last N days      - months
 quarter          between dates    difference
     |               |               |
     +---------------+---------------+
                     |
                  GROUPING
                     |
              day/month/year
                     |
             +-------+-------+
             |               |
           LAG            ROW_NUMBER
             |               |
        previous period   latest per group
             |
           growth
             |
        +----+----+
        |         |
    consecutive  trends
        |
   Gaps & Islands
```

And the **single most useful translation rule** is:

> **"From X until Y" → create a start boundary and an exclusive end boundary.**

For example:

```text
January 2026
```

becomes:

```sql
date >= '2026-01-01'
AND date <  '2026-02-01'
```

And:

> **"Latest/previous/consecutive" → think window functions.**

```text
latest per user     → ROW_NUMBER()
previous            → LAG()
next                → LEAD()
consecutive         → LAG + date arithmetic
streak              → gaps & islands
trend                → aggregation + LAG
```

And:

> **"By month/day/year" → think date bucket + GROUP BY.**

```text
PostgreSQL → DATE_TRUNC()
MySQL      → DATE_FORMAT()
```

That mental framework is much more valuable in interviews than memorizing individual functions.
