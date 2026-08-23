Absolutely. **Window Functions** are one of the highest-value SQL topics for interviews and coding problems. The key is to stop thinking of them as “fancy functions” and instead learn the **English → pattern → window definition → query** translation.

# SQL Window Functions — Complete Master Notes

### MySQL + PostgreSQL | From Scratch → All Major Functions → Interview Patterns → Coding Questions → Cheatsheet

---

# 1. What is a Window Function?

A normal aggregate function **collapses rows**.

For example:

```sql
SELECT department, AVG(salary)
FROM employees
GROUP BY department;
```

Input:

| employee | department  | salary |
| -------- | ----------- | -----: |
| Alice    | Engineering |  70000 |
| Bob      | Engineering |  90000 |
| Charlie  | Sales       |  60000 |
| David    | Sales       |  80000 |

Output:

| department  | avg_salary |
| ----------- | ---------: |
| Engineering |      80000 |
| Sales       |      70000 |

You went from **4 rows → 2 rows**.

---

A window function is different.

It says:

> **Calculate something using related rows, but DON'T collapse the original rows.**

Example:

```sql
SELECT
    employee,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS dept_avg
FROM employees;
```

Output:

| employee | department  | salary | dept_avg |
| -------- | ----------- | -----: | -------: |
| Alice    | Engineering |  70000 |    80000 |
| Bob      | Engineering |  90000 |    80000 |
| Charlie  | Sales       |  60000 |    70000 |
| David    | Sales       |  80000 |    70000 |

Notice:

```text
4 rows → 4 rows
```

The average is added **beside each employee**.

That is the fundamental difference.

---

# 2. GROUP BY vs Window Function

This distinction is extremely important in interviews.

## GROUP BY

```sql
SELECT
    department,
    AVG(salary)
FROM employees
GROUP BY department;
```

Result:

```text
1 row per department
```

---

## Window function

```sql
SELECT
    employee,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    )
FROM employees;
```

Result:

```text
1 row per employee
+
department average
```

### Mental model

```text
GROUP BY
    ↓
COLLAPSES rows

WINDOW FUNCTION
    ↓
KEEPS rows
+
adds calculation
```

---

# 3. The basic syntax

General syntax:

```sql
function(...) OVER (
    PARTITION BY ...
    ORDER BY ...
    ROWS/RANGE ...
)
```

For example:

```sql
SUM(amount) OVER (
    PARTITION BY customer_id
    ORDER BY order_date
)
```

There are three major pieces:

```text
OVER
 ├── PARTITION BY
 ├── ORDER BY
 └── frame
```

Not every window function needs all three.

---

# 4. The three questions you should ask

Whenever you see a window-function question, ask:

### Question 1

> **Who should be compared/calculated together?**

→ `PARTITION BY`

### Question 2

> **What is the order?**

→ `ORDER BY`

### Question 3

> **Which rows should the calculation look at?**

→ window frame:

```sql
ROWS BETWEEN ...
```

This is the core mental model.

---

# 5. PARTITION BY

`PARTITION BY` divides rows into independent groups.

Example:

```sql
AVG(salary) OVER (
    PARTITION BY department
)
```

means:

```text
Engineering → calculate separately
Sales       → calculate separately
HR          → calculate separately
```

It is similar conceptually to `GROUP BY`, **but it does not collapse rows**.

---

# 6. PARTITION BY example

Data:

| employee | dept        | salary |
| -------- | ----------- | -----: |
| Alice    | Engineering |     70 |
| Bob      | Engineering |     90 |
| Charlie  | Engineering |     80 |
| David    | Sales       |     60 |
| Eve      | Sales       |     80 |

Query:

```sql
SELECT
    employee,
    dept,
    salary,
    AVG(salary) OVER (
        PARTITION BY dept
    ) AS dept_avg
FROM employees;
```

Output:

| employee | dept        | salary | dept_avg |
| -------- | ----------- | -----: | -------: |
| Alice    | Engineering |     70 |       80 |
| Bob      | Engineering |     90 |       80 |
| Charlie  | Engineering |     80 |       80 |
| David    | Sales       |     60 |       70 |
| Eve      | Sales       |     80 |       70 |

---

# 7. No PARTITION BY

If you omit `PARTITION BY`:

```sql
AVG(salary) OVER ()
```

the entire result set is one window.

Example:

```sql
SELECT
    employee,
    salary,
    AVG(salary) OVER () AS company_avg
FROM employees;
```

Output:

| employee | salary | company_avg |
| -------- | -----: | ----------: |
| Alice    |     70 |          76 |
| Bob      |     90 |          76 |
| Charlie  |     80 |          76 |
| David    |     60 |          76 |
| Eve      |     80 |          76 |

Mental translation:

> **Across everybody.**

---

# 8. ORDER BY inside OVER

This:

```sql
ORDER BY
```

inside `OVER` is **not necessarily the same thing** as the final query's `ORDER BY`.

Example:

```sql
SUM(amount) OVER (
    ORDER BY order_date
)
```

means:

> Process rows chronologically for the window calculation.

It doesn't necessarily determine the final displayed order.

You can still have:

```sql
ORDER BY customer_id;
```

at the end.

---

# 9. The four major categories

We'll organize window functions into:

```text
1. Ranking
   ROW_NUMBER
   RANK
   DENSE_RANK
   NTILE

2. Value / navigation
   LAG
   LEAD
   FIRST_VALUE
   LAST_VALUE
   NTH_VALUE

3. Aggregation
   SUM
   AVG
   COUNT
   MIN
   MAX

4. Distribution / statistical
   PERCENT_RANK
   CUME_DIST
   NTILE
```

We'll also cover:

```text
window frames
running totals
moving averages
year-over-year
month-over-month
top N per group
deduplication
gaps and islands
consecutive rows
session-like patterns
```

---

# PART 1 — RANKING FUNCTIONS

# 10. ROW_NUMBER()

`ROW_NUMBER()` gives every row a unique sequential number.

```sql
ROW_NUMBER() OVER (
    ORDER BY salary DESC
)
```

Example:

| employee | salary |
| -------- | -----: |
| Alice    |    100 |
| Bob      |     90 |
| Charlie  |     90 |
| David    |     80 |

Query:

```sql
SELECT
    employee,
    salary,
    ROW_NUMBER() OVER (
        ORDER BY salary DESC
    ) AS rn
FROM employees;
```

Output:

| employee | salary |  rn |
| -------- | -----: | --: |
| Alice    |    100 |   1 |
| Bob      |     90 |   2 |
| Charlie  |     90 |   3 |
| David    |     80 |   4 |

Even though Bob and Charlie tie, they receive different row numbers.

---

# 11. ROW_NUMBER() with PARTITION BY

This is one of the most important patterns.

Question:

> Rank employees within each department.

```sql
SELECT
    employee,
    department,
    salary,
    ROW_NUMBER() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS rn
FROM employees;
```

Output:

| employee | department  | salary |  rn |
| -------- | ----------- | -----: | --: |
| Bob      | Engineering |    100 |   1 |
| Charlie  | Engineering |     90 |   2 |
| Alice    | Engineering |     80 |   3 |
| Eve      | Sales       |     95 |   1 |
| David    | Sales       |     70 |   2 |

Notice the numbering **restarts at 1** for each department.

---

# 12. The most important ROW_NUMBER pattern

### Top employee per department

```sql
WITH ranked AS (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
)
SELECT *
FROM ranked
WHERE rn = 1;
```

This is a **must-know interview pattern**.

---

# 13. Why can't we directly use ROW_NUMBER() in WHERE?

You might try:

```sql
SELECT
    employee,
    ROW_NUMBER() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS rn
FROM employees
WHERE rn = 1;
```

This doesn't work because the window calculation occurs after `WHERE` in the logical processing order.

So use:

```text
CTE
or
subquery
```

Example:

```sql
WITH ranked AS (...)
SELECT *
FROM ranked
WHERE rn = 1;
```

---

# 14. RANK()

`RANK()` handles ties differently.

Data:

| employee | salary |
| -------- | -----: |
| Alice    |    100 |
| Bob      |     90 |
| Charlie  |     90 |
| David    |     80 |

```sql
SELECT
    employee,
    salary,
    RANK() OVER (
        ORDER BY salary DESC
    ) AS rnk
FROM employees;
```

Output:

| employee | salary | rank |
| -------- | -----: | ---: |
| Alice    |    100 |    1 |
| Bob      |     90 |    2 |
| Charlie  |     90 |    2 |
| David    |     80 |    4 |

Notice:

```text
1
2
2
4
```

There is a gap after the tie.

---

# 15. DENSE_RANK()

`DENSE_RANK()` also gives tied rows the same rank.

But it does **not skip numbers**.

```sql
SELECT
    employee,
    salary,
    DENSE_RANK() OVER (
        ORDER BY salary DESC
    ) AS dr
FROM employees;
```

Output:

| employee | salary | dense_rank |
| -------- | -----: | ---------: |
| Alice    |    100 |          1 |
| Bob      |     90 |          2 |
| Charlie  |     90 |          2 |
| David    |     80 |          3 |

Compare:

```text
ROW_NUMBER
1 2 3 4

RANK
1 2 2 4

DENSE_RANK
1 2 2 3
```

---

# 16. ROW_NUMBER vs RANK vs DENSE_RANK

Memorize this table.

| Salary | ROW_NUMBER | RANK | DENSE_RANK |
| -----: | ---------: | ---: | ---------: |
|    100 |          1 |    1 |          1 |
|     90 |          2 |    2 |          2 |
|     90 |          3 |    2 |          2 |
|     80 |          4 |    4 |          3 |
|     70 |          5 |    5 |          4 |

### Translation

`ROW_NUMBER`:

> Give every row a unique position.

`RANK`:

> Give ties the same rank, leaving gaps.

`DENSE_RANK`:

> Give ties the same rank, without gaps.

---

# 17. Which ranking function should I use?

### "Give me exactly one row"

Usually:

```sql
ROW_NUMBER()
```

### "Give me everyone tied for first"

Usually:

```sql
RANK()
```

or:

```sql
DENSE_RANK()
```

depending on how subsequent ranks should behave.

### "Find the second-highest salary"

Be careful.

If you mean:

> second distinct salary

use:

```sql
DENSE_RANK()
```

---

# 18. Second-highest salary

Data:

| employee | salary |
| -------- | -----: |
| Alice    |    100 |
| Bob      |     90 |
| Charlie  |     90 |
| David    |     80 |

Question:

> Find the second-highest distinct salary.

```sql
WITH ranked AS (
    SELECT
        employee,
        salary,
        DENSE_RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
)
SELECT *
FROM ranked
WHERE rnk = 2;
```

Output:

| employee | salary |
| -------- | -----: |
| Bob      |     90 |
| Charlie  |     90 |

---

# 19. Second-highest salary: another method

Using a subquery:

```sql
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```

This returns the salary, not necessarily the employee.

The window-function method is more flexible.

---

# 20. Top N per group

Extremely common.

Question:

> Find the top 3 highest-paid employees in each department.

```sql
WITH ranked AS (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
)
SELECT *
FROM ranked
WHERE rn <= 3;
```

---

# 21. Top 3 including ties

This is different.

Question:

> Find the top 3 salary ranks in each department, including ties.

Use:

```sql
WITH ranked AS (
    SELECT
        e.*,
        DENSE_RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
)
SELECT *
FROM ranked
WHERE rnk <= 3;
```

### Important distinction

```text
ROW_NUMBER <= 3
```

→ exactly 3 rows per group, assuming enough rows.

```text
DENSE_RANK <= 3
```

→ up to 3 distinct ranking levels, potentially more than 3 rows because of ties.

---

# 22. NTILE()

`NTILE(n)` divides ordered rows into approximately equal buckets.

Example:

```sql
NTILE(4) OVER (
    ORDER BY salary DESC
)
```

This creates:

```text
1 → top 25%
2 → next 25%
3 → next 25%
4 → bottom 25%
```

Example with 8 rows:

| employee | salary | quartile |
| -------- | -----: | -------: |
| A        |    100 |        1 |
| B        |     95 |        1 |
| C        |     90 |        2 |
| D        |     85 |        2 |
| E        |     80 |        3 |
| F        |     75 |        3 |
| G        |     70 |        4 |
| H        |     65 |        4 |

Query:

```sql
SELECT
    employee,
    salary,
    NTILE(4) OVER (
        ORDER BY salary DESC
    ) AS quartile
FROM employees;
```

---

# 23. NTILE use cases

Questions like:

> Divide customers into 10 spending groups.

```sql
NTILE(10) OVER (
    ORDER BY total_spending DESC
)
```

Or:

> Put employees into salary quartiles.

```sql
NTILE(4) OVER (
    ORDER BY salary
)
```

---

# PART 2 — VALUE / NAVIGATION FUNCTIONS

# 24. LAG()

`LAG()` looks at a previous row.

This is one of the most useful functions for coding questions.

Example:

| month | sales |
| ----- | ----: |
| Jan   |   100 |
| Feb   |   120 |
| Mar   |   150 |

Query:

```sql
SELECT
    month,
    sales,
    LAG(sales) OVER (
        ORDER BY month
    ) AS previous_sales
FROM monthly_sales;
```

Output:

| month | sales | previous_sales |
| ----- | ----: | -------------: |
| Jan   |   100 |           NULL |
| Feb   |   120 |            100 |
| Mar   |   150 |            120 |

---

# 25. LAG syntax

```sql
LAG(column, offset, default)
OVER (...)
```

Example:

```sql
LAG(sales, 1)
```

means previous row.

```sql
LAG(sales, 2)
```

means two rows earlier.

---

# 26. LAG with default

```sql
LAG(sales, 1, 0)
OVER (
    ORDER BY month
)
```

Output:

| month | sales | previous |
| ----- | ----: | -------: |
| Jan   |   100 |        0 |
| Feb   |   120 |      100 |
| Mar   |   150 |      120 |

Without default:

```text
Jan → NULL
```

---

# 27. LAG + difference

Very common interview question:

> Calculate month-over-month sales change.

```sql
SELECT
    month,
    sales,
    sales - LAG(sales) OVER (
        ORDER BY month
    ) AS change
FROM monthly_sales;
```

Output:

| month | sales | change |
| ----- | ----: | -----: |
| Jan   |   100 |   NULL |
| Feb   |   120 |     20 |
| Mar   |   150 |     30 |

---

# 28. LAG + percentage change

```sql
SELECT
    month,
    sales,
    ROUND(
        100.0 *
        (sales - LAG(sales) OVER (ORDER BY month))
        / NULLIF(
            LAG(sales) OVER (ORDER BY month),
            0
        ),
        2
    ) AS pct_change
FROM monthly_sales;
```

Conceptually:

```text
current - previous
------------------ × 100
    previous
```

The `NULLIF(..., 0)` prevents division by zero.

---

# 29. LAG + PARTITION BY

Suppose sales exist for multiple products:

| product | month | sales |
| ------- | ----- | ----: |
| A       | Jan   |   100 |
| A       | Feb   |   120 |
| B       | Jan   |   200 |
| B       | Feb   |   180 |

We want previous month **for each product**.

```sql
SELECT
    product,
    month,
    sales,
    LAG(sales) OVER (
        PARTITION BY product
        ORDER BY month
    ) AS previous_sales
FROM sales;
```

Output:

| product | month | sales | previous |
| ------- | ----- | ----: | -------: |
| A       | Jan   |   100 |     NULL |
| A       | Feb   |   120 |      100 |
| B       | Jan   |   200 |     NULL |
| B       | Feb   |   180 |      200 |

This is a major pattern:

```text
LAG
+
PARTITION BY entity
+
ORDER BY time
```

---

# 30. LEAD()

`LEAD()` is the opposite of `LAG()`.

`LAG`:

```text
previous
←
current
```

`LEAD`:

```text
current
→
next
```

Example:

```sql
SELECT
    month,
    sales,
    LEAD(sales) OVER (
        ORDER BY month
    ) AS next_sales
FROM monthly_sales;
```

Output:

| month | sales | next_sales |
| ----- | ----: | ---------: |
| Jan   |   100 |        120 |
| Feb   |   120 |        150 |
| Mar   |   150 |       NULL |

---

# 31. LAG vs LEAD

| Function | Looks    |
| -------- | -------- |
| `LAG()`  | backward |
| `LEAD()` | forward  |

Think:

```text
LAG  ← previous
LEAD → next
```

---

# 32. FIRST_VALUE()

Returns the first value in the window according to its ordering.

Example:

```sql
SELECT
    employee,
    department,
    salary,
    FIRST_VALUE(salary) OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS highest_salary
FROM employees;
```

Output:

| employee | department  | salary | highest_salary |
| -------- | ----------- | -----: | -------------: |
| Alice    | Engineering |    100 |            100 |
| Bob      | Engineering |     90 |            100 |
| Charlie  | Engineering |     80 |            100 |
| David    | Sales       |     70 |             70 |
| Eve      | Sales       |     60 |             70 |

---

# 33. FIRST_VALUE() as "group maximum"

Because:

```sql
ORDER BY salary DESC
```

puts the highest salary first.

Therefore:

```sql
FIRST_VALUE(salary)
```

returns the highest salary.

If you instead use:

```sql
ORDER BY salary ASC
```

the first value becomes the lowest salary.

---

# 34. FIRST_VALUE() use cases

Questions:

> Show each employee's department's highest salary.

```sql
FIRST_VALUE(salary) OVER (
    PARTITION BY department
    ORDER BY salary DESC
)
```

> Show each employee's department's lowest salary.

```sql
FIRST_VALUE(salary) OVER (
    PARTITION BY department
    ORDER BY salary ASC
)
```

---

# 35. LAST_VALUE() — IMPORTANT TRAP

`LAST_VALUE()` is one of the easiest window functions to misunderstand.

Suppose:

```sql
LAST_VALUE(salary) OVER (
    PARTITION BY department
    ORDER BY salary
)
```

Many people expect:

> Give me the last salary in the entire department.

But because of the default window frame, that may not happen.

The frame can end at the **current row**, so `LAST_VALUE()` may simply return the current row's value.

---

# 36. Correct LAST_VALUE pattern

If you want the true last value across the entire partition:

```sql
LAST_VALUE(salary) OVER (
    PARTITION BY department
    ORDER BY salary
    ROWS BETWEEN UNBOUNDED PRECEDING
             AND UNBOUNDED FOLLOWING
)
```

Now the window includes the entire partition.

This is a very important interview trick.

---

# 37. Window frame

Now we need to understand:

```sql
ROWS BETWEEN ...
```

A frame determines:

> Which rows around the current row are included in the calculation?

Example:

```sql
SUM(amount) OVER (
    ORDER BY order_date
    ROWS BETWEEN UNBOUNDED PRECEDING
             AND CURRENT ROW
)
```

means:

```text
first row
   ↓
all rows
   ↓
current row
```

This creates a running total.

---

# PART 3 — AGGREGATE WINDOW FUNCTIONS

# 38. SUM() OVER()

A normal:

```sql
SUM(amount)
```

aggregates.

But:

```sql
SUM(amount) OVER (...)
```

is a window calculation.

Example:

| date  | amount |
| ----- | -----: |
| Jan 1 |    100 |
| Jan 2 |    200 |
| Jan 3 |     50 |

Query:

```sql
SELECT
    date,
    amount,
    SUM(amount) OVER (
        ORDER BY date
    ) AS running_total
FROM sales;
```

Output:

| date  | amount | running_total |
| ----- | -----: | ------------: |
| Jan 1 |    100 |           100 |
| Jan 2 |    200 |           300 |
| Jan 3 |     50 |           350 |

---

# 39. Explicit running-total frame

For clarity, write:

```sql
SUM(amount) OVER (
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
             AND CURRENT ROW
)
```

Translation:

```text
from first row
through current row
```

This is one of the most useful SQL patterns.

---

# 40. Running total per customer

Question:

> Calculate each customer's cumulative spending over time.

```sql
SELECT
    customer_id,
    order_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY customer_id
        ORDER BY order_date
        ROWS BETWEEN UNBOUNDED PRECEDING
             AND CURRENT ROW
    ) AS running_total
FROM orders;
```

Example:

| customer | date | amount | running_total |
| -------- | ---- | -----: | ------------: |
| A        | Jan  |    100 |           100 |
| A        | Feb  |     50 |           150 |
| A        | Mar  |    200 |           350 |
| B        | Jan  |    300 |           300 |
| B        | Feb  |    100 |           400 |

The running total resets for each customer.

---

# 41. The universal running-total pattern

Memorize:

```sql
SUM(value) OVER (
    PARTITION BY entity
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

English:

> Calculate cumulative value for each entity over time.

---

# 42. Running COUNT

Same concept:

```sql
COUNT(*) OVER (
    PARTITION BY customer_id
    ORDER BY order_date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

Question:

> How many orders has each customer placed up to this point?

---

# 43. Running MAX

Question:

> What is the highest price seen so far?

```sql
MAX(price) OVER (
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
) AS max_so_far
```

Example:

| date | price | max_so_far |
| ---- | ----: | ---------: |
| Jan  |   100 |        100 |
| Feb  |   120 |        120 |
| Mar  |   110 |        120 |
| Apr  |   150 |        150 |

---

# 44. Running MIN

Same:

```sql
MIN(price) OVER (
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

---

# 45. Running AVG

```sql
AVG(amount) OVER (
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

Example:

| date | amount | running_avg |
| ---- | -----: | ----------: |
| Jan  |     10 |          10 |
| Feb  |     20 |          15 |
| Mar  |     30 |          20 |
| Apr  |     40 |          25 |

---

# 46. Moving average

This is different from a running average.

Question:

> Calculate the 3-day moving average.

Use:

```sql
AVG(amount) OVER (
    ORDER BY date
    ROWS BETWEEN 2 PRECEDING
             AND CURRENT ROW
)
```

Example:

| date  | amount | 3-row average |
| ----- | -----: | ------------: |
| Jan 1 |     10 |            10 |
| Jan 2 |     20 |            15 |
| Jan 3 |     30 |            20 |
| Jan 4 |     40 |            30 |
| Jan 5 |     50 |            40 |

Notice:

```text
Jan 4:
20 + 30 + 40
--------------
      3
= 30
```

---

# 47. Window-frame cheat sheet

### Current row only

```sql
ROWS BETWEEN CURRENT ROW AND CURRENT ROW
```

### Previous row + current

```sql
ROWS BETWEEN 1 PRECEDING AND CURRENT ROW
```

### Previous 2 + current

```sql
ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
```

### Current + next 2

```sql
ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING
```

### Entire partition

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
         AND UNBOUNDED FOLLOWING
```

### Beginning → current

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
```

---

# 48. ROWS vs RANGE — important

This is an advanced but important distinction.

### `ROWS`

Means physical rows.

```sql
ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
```

means:

> current row + previous 2 rows.

### `RANGE`

Works based on the ordering values and peer rows.

This becomes important when there are ties.

For predictable row-count-based moving calculations, `ROWS` is often what you want.

---

# 49. Why explicit ROWS is often safer

Suppose:

| date  | amount |
| ----- | -----: |
| Jan 1 |    100 |
| Jan 1 |    200 |
| Jan 2 |    300 |

If you write:

```sql
SUM(amount) OVER (
    ORDER BY date
)
```

the database's default frame behavior can involve peer rows with the same ordering value.

If your intended meaning is:

> accumulate one physical row at a time

use:

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
```

This is a very useful habit.

---

# 50. Partition + running total + ordering

This pattern appears constantly:

```sql
SUM(amount) OVER (
    PARTITION BY customer_id
    ORDER BY order_date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

Break it down:

```text
PARTITION BY customer_id
        ↓
separate customers

ORDER BY order_date
        ↓
chronological order

ROWS ...
        ↓
from beginning through current
```

---

# 51. Aggregate window without ordering

```sql
SUM(amount) OVER (
    PARTITION BY customer_id
)
```

means:

> Total spending for each customer, repeated on every order row.

Example:

| customer | amount | customer_total |
| -------- | -----: | -------------: |
| A        |    100 |            350 |
| A        |     50 |            350 |
| A        |    200 |            350 |
| B        |    300 |            400 |
| B        |    100 |            400 |

No running calculation because there is no `ORDER BY`.

---

# 52. Difference between total and running total

### Total per customer

```sql
SUM(amount) OVER (
    PARTITION BY customer_id
)
```

### Running total

```sql
SUM(amount) OVER (
    PARTITION BY customer_id
    ORDER BY order_date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

The presence of:

```sql
ORDER BY
```

changes the calculation from a partition-wide aggregate into an ordered window calculation.

---

# PART 4 — COMMON INTERVIEW PATTERNS

# 53. Pattern: Top 1 per group

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees
)
SELECT *
FROM ranked
WHERE rn = 1;
```

English:

> Highest-paid employee in every department.

---

# 54. Pattern: Top N per group

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees
)
SELECT *
FROM ranked
WHERE rn <= 3;
```

English:

> Top 3 employees in each department.

---

# 55. Pattern: Top N including ties

```sql
WITH ranked AS (
    SELECT
        *,
        DENSE_RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
)
SELECT *
FROM ranked
WHERE rnk <= 3;
```

---

# 56. Pattern: previous row

```sql
LAG(value) OVER (
    PARTITION BY entity
    ORDER BY date
)
```

English:

> Previous value for each entity.

---

# 57. Pattern: next row

```sql
LEAD(value) OVER (
    PARTITION BY entity
    ORDER BY date
)
```

English:

> Next value for each entity.

---

# 58. Pattern: difference from previous

```sql
value -
LAG(value) OVER (
    PARTITION BY entity
    ORDER BY date
)
```

English:

> Change from previous period.

---

# 59. Pattern: percentage growth

```sql
100.0 *
(
    value -
    LAG(value) OVER (
        PARTITION BY entity
        ORDER BY date
    )
)
/
NULLIF(
    LAG(value) OVER (
        PARTITION BY entity
        ORDER BY date
    ),
    0
)
```

English:

> Percentage change from previous period.

---

# 60. Pattern: running total

```sql
SUM(value) OVER (
    PARTITION BY entity
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

English:

> Cumulative value.

---

# 61. Pattern: moving average

```sql
AVG(value) OVER (
    PARTITION BY entity
    ORDER BY date
    ROWS BETWEEN 2 PRECEDING
         AND CURRENT ROW
)
```

English:

> 3-row moving average.

---

# 62. Pattern: group average beside every row

```sql
AVG(value) OVER (
    PARTITION BY group_id
)
```

English:

> Show each row and its group's average.

---

# 63. Pattern: difference from group average

```sql
value -
AVG(value) OVER (
    PARTITION BY group_id
)
```

Example:

```sql
SELECT
    employee,
    salary,
    salary - AVG(salary) OVER (
        PARTITION BY department
    ) AS difference_from_avg
FROM employees;
```

---

# 64. Pattern: percentage of group total

This is a beautiful window-function pattern.

Question:

> What percentage of department salary does each employee represent?

```sql
SELECT
    employee,
    department,
    salary,
    100.0 * salary /
        SUM(salary) OVER (
            PARTITION BY department
        ) AS pct_of_dept_salary
FROM employees;
```

Example:

| employee | dept | salary |   pct |
| -------- | ---- | -----: | ----: |
| Alice    | Eng  |     70 | 43.75 |
| Bob      | Eng  |     90 | 56.25 |

Because:

```text
70 / (70 + 90) × 100
```

---

# 65. Pattern: cumulative percentage

```sql
SELECT
    employee,
    salary,
    100.0 *
    SUM(salary) OVER (
        ORDER BY salary DESC
        ROWS BETWEEN UNBOUNDED PRECEDING
             AND CURRENT ROW
    )
    /
    SUM(salary) OVER () AS cumulative_pct
FROM employees;
```

This can be useful for Pareto-style analysis.

---

# 66. Pattern: first purchase per customer

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY order_date
        ) AS rn
    FROM orders
)
SELECT *
FROM ranked
WHERE rn = 1;
```

English:

> First order for each customer.

---

# 67. Pattern: latest record per customer

Extremely common.

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY updated_at DESC
        ) AS rn
    FROM customer_records
)
SELECT *
FROM ranked
WHERE rn = 1;
```

English:

> Most recent record for each customer.

---

# 68. Deduplication pattern

Suppose:

| email                                 | name    | updated_at |
| ------------------------------------- | ------- | ---------- |
| [a@example.com](mailto:a@example.com) | Alice   | Jan 1      |
| [a@example.com](mailto:a@example.com) | Alice A | Feb 1      |
| [b@example.com](mailto:b@example.com) | Bob     | Jan 5      |

Keep the latest row per email:

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY email
            ORDER BY updated_at DESC
        ) AS rn
    FROM users
)
SELECT *
FROM ranked
WHERE rn = 1;
```

This is one of the most practical uses of `ROW_NUMBER()`.

---

# 69. Delete duplicates using ROW_NUMBER

In MySQL/PostgreSQL, the exact delete strategy can vary, but conceptually:

```text
1. Partition by duplicate key
2. Order by which row you want to keep
3. Assign ROW_NUMBER
4. Keep rn = 1
5. Remove rn > 1
```

For example:

```sql
ROW_NUMBER() OVER (
    PARTITION BY email
    ORDER BY updated_at DESC
)
```

means:

```text
rn = 1 → newest
rn = 2 → second newest
rn = 3 → third newest
```

---

# 70. Pattern: month-over-month

```sql
SELECT
    month,
    revenue,
    revenue -
        LAG(revenue) OVER (
            ORDER BY month
        ) AS difference
FROM monthly_revenue;
```

---

# 71. Pattern: year-over-year

If data contains yearly observations:

```sql
SELECT
    year,
    revenue,
    revenue -
        LAG(revenue) OVER (
            ORDER BY year
        ) AS yoy_difference
FROM yearly_revenue;
```

For multiple products:

```sql
LAG(revenue) OVER (
    PARTITION BY product_id
    ORDER BY year
)
```

---

# 72. Pattern: compare current row with next row

Example:

> Calculate days until the next event.

```sql
SELECT
    event_date,
    LEAD(event_date) OVER (
        ORDER BY event_date
    ) - event_date AS days_until_next
FROM events;
```

PostgreSQL handles date subtraction naturally as an interval/number of days depending on types. In MySQL, you might use:

```sql
DATEDIFF(
    LEAD(event_date) OVER (ORDER BY event_date),
    event_date
)
```

---

# 73. Pattern: find where a value changed

Suppose:

| date  | status   |
| ----- | -------- |
| Jan 1 | active   |
| Jan 2 | active   |
| Jan 3 | inactive |
| Jan 4 | inactive |
| Jan 5 | active   |

Use:

```sql
SELECT
    date,
    status,
    LAG(status) OVER (
        ORDER BY date
    ) AS previous_status
FROM account_status;
```

Then:

```sql
WITH x AS (
    SELECT
        *,
        LAG(status) OVER (
            ORDER BY date
        ) AS previous_status
    FROM account_status
)
SELECT *
FROM x
WHERE previous_status IS DISTINCT FROM status;
```

In MySQL, NULL-safe comparison can use:

```sql
NOT (previous_status <=> status)
```

or an appropriate NULL-aware `CASE` expression.

---

# 74. Pattern: detect first row of a group

```sql
CASE
    WHEN LAG(category) OVER (
        ORDER BY date
    ) = category
    THEN 0
    ELSE 1
END
```

This can identify boundaries between groups.

This leads into one of the most advanced interview patterns:

**gaps and islands**.

---

# PART 5 — GAPS AND ISLANDS

# 75. What are "gaps and islands"?

Suppose a user logs in on:

```text
Jan 1
Jan 2
Jan 3
Jan 7
Jan 8
Jan 10
```

We want consecutive login streaks:

```text
Jan 1-Jan 3 → streak 1
Jan 7-Jan 8 → streak 2
Jan 10      → streak 3
```

This is called a **gaps-and-islands** problem.

Window functions are one of the main tools for solving it.

---

# 76. LAG-based island detection

First:

```sql
SELECT
    user_id,
    login_date,
    LAG(login_date) OVER (
        PARTITION BY user_id
        ORDER BY login_date
    ) AS previous_date
FROM logins;
```

Then compare dates.

Conceptually:

```text
current_date - previous_date = 1 day
    ↓
same streak

otherwise
    ↓
new streak
```

---

# 77. Create a group number

PostgreSQL example:

```sql
WITH x AS (
    SELECT
        user_id,
        login_date,
        CASE
            WHEN login_date =
                 LAG(login_date) OVER (
                     PARTITION BY user_id
                     ORDER BY login_date
                 ) + INTERVAL '1 day'
            THEN 0
            ELSE 1
        END AS new_group
    FROM logins
),
y AS (
    SELECT
        *,
        SUM(new_group) OVER (
            PARTITION BY user_id
            ORDER BY login_date
            ROWS UNBOUNDED PRECEDING
        ) AS grp
    FROM x
)
SELECT *
FROM y;
```

Then:

```sql
GROUP BY user_id, grp
```

to analyze each streak.

This pattern is advanced but extremely valuable for SQL interviews.

---

# PART 6 — DISTRIBUTION FUNCTIONS

# 78. PERCENT_RANK()

`PERCENT_RANK()` gives a relative rank between 0 and 1.

Formula conceptually:

```text
(rank - 1)
-----------
(total_rows - 1)
```

Example:

```sql
SELECT
    employee,
    salary,
    PERCENT_RANK() OVER (
        ORDER BY salary
    ) AS pct_rank
FROM employees;
```

For 5 ordered rows:

```text
first  → 0
second → 0.25
third  → 0.50
fourth → 0.75
fifth  → 1
```

---

# 79. CUME_DIST()

`CUME_DIST()` answers:

> What fraction of rows are less than or equal to this row's value?

Example:

```sql
SELECT
    employee,
    salary,
    CUME_DIST() OVER (
        ORDER BY salary
    ) AS cumulative_distribution
FROM employees;
```

Useful for percentile-style analysis.

---

# 80. PERCENT_RANK vs CUME_DIST

| Function         | Meaning               |
| ---------------- | --------------------- |
| `PERCENT_RANK()` | relative rank         |
| `CUME_DIST()`    | cumulative proportion |

Don't confuse them.

---

# PART 7 — VALUE FUNCTIONS IN DETAIL

# 81. NTH_VALUE()

`NTH_VALUE(value, n)` returns the nth value in the window frame.

Example:

```sql
NTH_VALUE(salary, 2) OVER (
    ORDER BY salary DESC
)
```

Conceptually:

> Give me the second value according to the window ordering.

Be careful with the window frame, just like `LAST_VALUE()`.

---

# 82. FIRST_VALUE vs MIN

Sometimes:

```sql
FIRST_VALUE(salary) OVER (
    PARTITION BY department
    ORDER BY salary
)
```

may produce the same value as:

```sql
MIN(salary) OVER (
    PARTITION BY department
)
```

But they express different ideas.

`MIN`:

> minimum value.

`FIRST_VALUE`:

> value of the first row according to this ordering.

The distinction becomes important when you're selecting **some other column** associated with the first row.

---

# 83. Example: first order's amount

Suppose:

| customer | date | amount |
| -------- | ---- | -----: |
| A        | Jan  |    100 |
| A        | Feb  |    500 |
| A        | Mar  |    200 |

You want:

> Amount of customer's first order.

You could use:

```sql
FIRST_VALUE(amount) OVER (
    PARTITION BY customer_id
    ORDER BY order_date
)
```

This gives:

```text
100
```

for every A row.

That's different from:

```sql
MIN(amount)
```

which gives:

```text
100
```

here only by coincidence.

If amounts were:

```text
500
100
200
```

`MIN(amount)` = 100, while first order amount = 500.

---

# PART 8 — MULTIPLE WINDOWS

# 84. You can use multiple window functions together

Example:

```sql
SELECT
    employee,
    department,
    salary,

    ROW_NUMBER() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS row_num,

    RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS rank_num,

    AVG(salary) OVER (
        PARTITION BY department
    ) AS dept_avg

FROM employees;
```

Output:

| employee | dept | salary | row_num | rank | avg |
| -------- | ---- | -----: | ------: | ---: | --: |
| Alice    | Eng  |    100 |       1 |    1 |  90 |
| Bob      | Eng  |     90 |       2 |    2 |  90 |
| Charlie  | Eng  |     80 |       3 |    3 |  90 |

---

# 85. Named windows

If you're repeating the same window definition, SQL allows named windows.

PostgreSQL and MySQL support the `WINDOW` clause.

Example:

```sql
SELECT
    employee,
    salary,
    ROW_NUMBER() OVER w AS rn,
    RANK() OVER w AS rnk,
    DENSE_RANK() OVER w AS dr
FROM employees
WINDOW w AS (
    PARTITION BY department
    ORDER BY salary DESC
);
```

This avoids repeating:

```sql
PARTITION BY department
ORDER BY salary DESC
```

---

# PART 9 — WINDOW FUNCTIONS + CTE

# 86. CTE is your friend

A very common structure is:

```sql
WITH ranked AS (
    SELECT
        ...,
        ROW_NUMBER() OVER (...) AS rn
    FROM ...
)
SELECT ...
FROM ranked
WHERE rn <= 3;
```

Think:

```text
Step 1
calculate window value

       ↓

Step 2
filter using it
```

This is one of the most important practical patterns.

---

# 87. Window function inside a CTE

Example:

> Latest order for each customer.

```sql
WITH ranked_orders AS (
    SELECT
        o.*,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY order_date DESC
        ) AS rn
    FROM orders o
)
SELECT
    *
FROM ranked_orders
WHERE rn = 1;
```

---

# PART 10 — INTERVIEW PROBLEMS

# 88. Problem: Highest salary per department

```sql
WITH ranked AS (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
)
SELECT
    *
FROM ranked
WHERE rn = 1;
```

If ties should all be included:

```sql
WITH ranked AS (
    SELECT
        e.*,
        RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
)
SELECT *
FROM ranked
WHERE rnk = 1;
```

---

# 89. Problem: Third-highest salary

If "third highest" means third **distinct** salary:

```sql
WITH ranked AS (
    SELECT
        *,
        DENSE_RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
)
SELECT *
FROM ranked
WHERE rnk = 3;
```

---

# 90. Problem: Latest order for every customer

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY order_date DESC, order_id DESC
        ) AS rn
    FROM orders
)
SELECT *
FROM ranked
WHERE rn = 1;
```

Notice the tie-breaker:

```sql
ORDER BY order_date DESC, order_id DESC
```

This is a very good practice.

---

# 91. Why tie-breakers matter

Suppose:

| order_id | customer | date  |
| -------: | -------- | ----- |
|      101 | A        | Jan 1 |
|      102 | A        | Jan 1 |

If you write:

```sql
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY order_date DESC
)
```

both rows tie on `order_date`.

Which gets `rn = 1`?

The database may choose either.

If you need deterministic behavior:

```sql
ORDER BY order_date DESC, order_id DESC
```

Now:

```text
102 → rn 1
101 → rn 2
```

This is a very important interview/production tip.

---

# 92. Problem: Find employees above their department average

```sql
WITH x AS (
    SELECT
        e.*,
        AVG(salary) OVER (
            PARTITION BY department
        ) AS dept_avg
    FROM employees e
)
SELECT
    employee,
    department,
    salary,
    dept_avg
FROM x
WHERE salary > dept_avg;
```

---

# 93. Problem: Find employees earning the maximum salary in their department

```sql
WITH x AS (
    SELECT
        e.*,
        MAX(salary) OVER (
            PARTITION BY department
        ) AS dept_max
    FROM employees e
)
SELECT *
FROM x
WHERE salary = dept_max;
```

Notice this includes ties automatically.

This is a beautiful alternative to ranking.

---

# 94. Problem: Difference from department maximum

```sql
SELECT
    employee,
    department,
    salary,
    MAX(salary) OVER (
        PARTITION BY department
    ) - salary AS gap_from_max
FROM employees;
```

---

# 95. Problem: Percentage of department salary

```sql
SELECT
    employee,
    department,
    salary,
    ROUND(
        100.0 * salary /
        SUM(salary) OVER (
            PARTITION BY department
        ),
        2
    ) AS salary_percentage
FROM employees;
```

---

# 96. Problem: Running total

```sql
SELECT
    order_date,
    amount,
    SUM(amount) OVER (
        ORDER BY order_date
        ROWS BETWEEN UNBOUNDED PRECEDING
             AND CURRENT ROW
    ) AS running_total
FROM orders;
```

---

# 97. Problem: 7-day moving average

If each row represents a day:

```sql
AVG(sales) OVER (
    ORDER BY sale_date
    ROWS BETWEEN 6 PRECEDING
         AND CURRENT ROW
)
```

But **important**:

`ROWS` means seven rows, not necessarily seven calendar days.

If dates are missing, that's not necessarily a true 7-calendar-day average.

For a true calendar-day window, the solution can require date-range logic or a calendar table, depending on the database and exact requirement.

This distinction is a frequent source of subtle bugs.

---

# 98. Problem: Previous month's revenue

```sql
SELECT
    month,
    revenue,
    LAG(revenue) OVER (
        ORDER BY month
    ) AS previous_revenue
FROM revenue;
```

---

# 99. Problem: Revenue growth

```sql
WITH x AS (
    SELECT
        month,
        revenue,
        LAG(revenue) OVER (
            ORDER BY month
        ) AS previous_revenue
    FROM revenue
)
SELECT
    month,
    revenue,
    previous_revenue,
    100.0 *
        (revenue - previous_revenue)
        / NULLIF(previous_revenue, 0)
        AS growth_pct
FROM x;
```

Using a CTE makes the query easier to read.

---

# 100. Problem: Find consecutive duplicate values

Suppose:

| date  | status |
| ----- | ------ |
| Jan 1 | A      |
| Jan 2 | A      |
| Jan 3 | B      |
| Jan 4 | B      |
| Jan 5 | B      |

Use:

```sql
SELECT
    date,
    status,
    LAG(status) OVER (
        ORDER BY date
    ) AS previous_status
FROM events;
```

Then compare:

```sql
CASE
    WHEN status = previous_status
    THEN 1
    ELSE 0
END
```

This is the beginning of many streak/island problems.

---

# PART 11 — ADVANCED WINDOW THINKING

# 101. Window functions don't necessarily mean ranking

A common beginner mistake is:

> Window function = ranking.

No.

Window functions are a broader concept:

```text
Window
├── Ranking
├── Navigation
├── Aggregation
└── Distribution
```

For example:

```sql
AVG(...) OVER (...)
```

is a window function.

```sql
SUM(...) OVER (...)
```

is a window function.

```sql
LAG(...) OVER (...)
```

is a window function.

---

# 102. Window functions don't reduce rows

This is the defining property.

If input has:

```text
100 rows
```

then a window-function SELECT normally still has:

```text
100 rows
```

unless some later operation filters/groups them.

For example:

```sql
SELECT
    *,
    ROW_NUMBER() OVER (...)
FROM employees;
```

still returns one output row per employee.

---

# 103. Window function vs GROUP BY

Question:

> What is average salary per department?

Use GROUP BY:

```sql
SELECT
    department,
    AVG(salary)
FROM employees
GROUP BY department;
```

Question:

> Show every employee and their department's average salary.

Use window:

```sql
SELECT
    employee,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    )
FROM employees;
```

---

# 104. Window function vs subquery

Sometimes both are possible.

Example:

> Employees whose salary is above department average.

Window:

```sql
WITH x AS (
    SELECT
        *,
        AVG(salary) OVER (
            PARTITION BY department
        ) AS avg_salary
    FROM employees
)
SELECT *
FROM x
WHERE salary > avg_salary;
```

This is often much easier to reason about than a correlated subquery.

---

# 105. Multiple PARTITION columns

You can partition by multiple columns.

```sql
PARTITION BY country, department
```

Meaning:

```text
country + department
```

defines the group.

Example:

```sql
AVG(salary) OVER (
    PARTITION BY country, department
)
```

---

# 106. Multiple ORDER BY columns

Also possible:

```sql
ROW_NUMBER() OVER (
    PARTITION BY department
    ORDER BY salary DESC, employee_id
)
```

This gives deterministic ordering when salary ties.

---

# 107. Window ORDER BY vs final ORDER BY

Very important.

This:

```sql
ROW_NUMBER() OVER (
    ORDER BY salary DESC
)
```

controls the ranking.

This:

```sql
ORDER BY employee_name
```

controls the final displayed result.

They are independent.

Example:

```sql
SELECT
    employee,
    salary,
    ROW_NUMBER() OVER (
        ORDER BY salary DESC
    ) AS rn
FROM employees
ORDER BY employee;
```

You rank by salary but display alphabetically.

---

# 108. Can window functions be nested?

Generally you cannot directly do:

```sql
SUM(
    LAG(value) OVER (...)
) OVER (...)
```

in one SELECT expression.

Instead use a CTE/subquery:

```sql
WITH x AS (
    SELECT
        *,
        LAG(value) OVER (...) AS previous_value
    FROM t
)
SELECT
    *,
    SUM(previous_value) OVER (...)
FROM x;
```

Think:

```text
Window calculation #1
        ↓
CTE/subquery
        ↓
Window calculation #2
```

---

# 109. Named window definitions

If you repeatedly use:

```sql
PARTITION BY customer_id
ORDER BY order_date
```

you can define:

```sql
WINDOW w AS (
    PARTITION BY customer_id
    ORDER BY order_date
);
```

Then:

```sql
SUM(amount) OVER w
```

and:

```sql
LAG(amount) OVER w
```

This can improve readability.

---

# PART 12 — MYSQL VS POSTGRESQL

# 110. Window function support

Both modern MySQL and PostgreSQL support the major window functions discussed here:

```text
ROW_NUMBER
RANK
DENSE_RANK
NTILE
LAG
LEAD
FIRST_VALUE
LAST_VALUE
NTH_VALUE
SUM OVER
AVG OVER
COUNT OVER
MIN/MAX OVER
PERCENT_RANK
CUME_DIST
```

The broad concepts and syntax are very similar.

---

# 111. One important version consideration

For MySQL, window functions were introduced in:

```text
MySQL 8.0
```

So if an interviewer gives you:

```text
MySQL 5.7
```

you cannot rely on window functions.

You would need older techniques such as:

```text
subqueries
self joins
variables (historically)
aggregation
```

For modern coding interviews, assume MySQL 8+ unless stated otherwise.

---

# 112. PostgreSQL-specific useful feature: FILTER

PostgreSQL supports:

```sql
COUNT(*) FILTER (
    WHERE status = 'completed'
)
OVER (...)
```

For example:

```sql
SELECT
    customer_id,
    order_date,
    COUNT(*) FILTER (
        WHERE status = 'completed'
    ) OVER (
        PARTITION BY customer_id
    ) AS completed_orders
FROM orders;
```

MySQL commonly uses conditional aggregation instead:

```sql
SUM(
    CASE
        WHEN status = 'completed'
        THEN 1
        ELSE 0
    END
) OVER (
    PARTITION BY customer_id
)
```

---

# PART 13 — TRICKS AND PITFALLS

# 113. Trick: Always ask "what is the partition?"

Question:

> Rank each employee within their department.

Immediately translate:

```text
within each department
        ↓
PARTITION BY department
```

---

# 114. Trick: "overall" means no partition

Question:

> Rank employees across the company.

```sql
RANK() OVER (
    ORDER BY salary DESC
)
```

No:

```sql
PARTITION BY
```

---

# 115. Trick: "within each" means PARTITION BY

Phrases such as:

> within each department

> for every customer

> separately for each product

> per store

usually signal:

```sql
PARTITION BY ...
```

---

# 116. Trick: "previous" means LAG

Keywords:

```text
previous
prior
last period
previous month
previous transaction
yesterday's value
```

Think:

```sql
LAG()
```

---

# 117. Trick: "next" means LEAD

Keywords:

```text
next
following
upcoming
next transaction
next event
```

Think:

```sql
LEAD()
```

---

# 118. Trick: "cumulative" means running window

Keywords:

```text
running total
cumulative
so far
to date
year-to-date
month-to-date
```

Think:

```sql
SUM(...) OVER (
    ...
    ORDER BY ...
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

---

# 119. Trick: "moving" means bounded frame

Keywords:

```text
moving average
rolling average
last 3 days
previous 7 rows
rolling sum
```

Think:

```sql
ROWS BETWEEN N PRECEDING AND CURRENT ROW
```

---

# 120. Trick: "top N per group"

Keywords:

```text
top 3 per department
highest 2 per category
best 5 for each store
```

Think:

```sql
ROW_NUMBER() OVER (
    PARTITION BY group
    ORDER BY metric DESC
)
```

then:

```sql
WHERE rn <= N
```

---

# 121. Trick: "including ties"

Keywords:

```text
including ties
all employees tied for first
top 3 salary levels
```

Think:

```text
RANK()
or
DENSE_RANK()
```

---

# 122. Trick: "distinct rank"

If question says:

> second-highest distinct salary

Think:

```sql
DENSE_RANK()
```

---

# 123. Trick: "exactly N rows"

If the requirement is:

> return exactly 3 employees from each department

Think:

```sql
ROW_NUMBER() <= 3
```

rather than `RANK()`.

---

# 124. Trick: deterministic ROW_NUMBER

If there can be ties:

```sql
ROW_NUMBER() OVER (
    PARTITION BY department
    ORDER BY salary DESC, employee_id
)
```

Always ask:

> What should happen if the primary sort column ties?

Add a tie-breaker.

---

# 125. Trick: LAST_VALUE needs frame awareness

Don't blindly write:

```sql
LAST_VALUE(x) OVER (
    ORDER BY date
)
```

If you want the last value in the whole partition:

```sql
LAST_VALUE(x) OVER (
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND UNBOUNDED FOLLOWING
)
```

This is one of the most useful advanced window-function tips.

---

# 126. Trick: repeated LAG

Instead of:

```sql
SELECT
    sales -
    LAG(sales) OVER (...) AS diff,
    (sales -
     LAG(sales) OVER (...))
     / LAG(sales) OVER (...) AS pct
```

consider a CTE:

```sql
WITH x AS (
    SELECT
        *,
        LAG(sales) OVER (...) AS previous_sales
    FROM sales
)
SELECT
    *,
    sales - previous_sales AS difference,
    100.0 * (sales - previous_sales)
        / NULLIF(previous_sales, 0) AS pct_change
FROM x;
```

Much cleaner.

---

# 127. Trick: NULL handling

`LAG()` naturally produces NULL when there is no previous row.

Example:

```text
first row → NULL
```

Don't automatically replace it with 0.

Ask what the business meaning is.

If appropriate:

```sql
COALESCE(
    LAG(amount) OVER (...),
    0
)
```

---

# 128. Trick: missing dates

This is a major analytical trap.

Suppose:

```text
Jan 1
Jan 2
Jan 10
```

Then:

```sql
LAG(value)
```

gives the previous **available row**, not necessarily the previous calendar day.

So:

```text
Jan 10 → Jan 2
```

not:

```text
Jan 9
```

If the question says:

> previous calendar day

you may need a calendar/date table.

---

# 129. Trick: ROWS doesn't mean days

This:

```sql
ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
```

means:

> 7 rows

not necessarily:

> 7 days.

If your table has missing dates, these are different concepts.

---

# 130. Trick: duplicate timestamps

If:

```text
ORDER BY timestamp
```

contains duplicates, ordering may be ambiguous.

Use a secondary key:

```sql
ORDER BY timestamp, event_id
```

This makes row order deterministic.

---

# 131. Window functions and NULLs

Aggregate window functions generally follow aggregate NULL behavior.

For example:

```sql
AVG(salary) OVER (...)
```

ignores NULL salary values.

But:

```sql
LAG(salary)
```

can return NULL because the previous value itself may be NULL.

Always distinguish:

```text
no previous row
```

from:

```text
previous row exists but value is NULL
```

---

# PART 14 — A COMPLETE EXAMPLE

Let's build a realistic problem.

### `sales`

| customer | date   | amount |
| -------- | ------ | -----: |
| Alice    | Jan 1  |    100 |
| Alice    | Jan 5  |    200 |
| Alice    | Jan 10 |    150 |
| Bob      | Jan 2  |    300 |
| Bob      | Jan 7  |    100 |
| Bob      | Jan 12 |    500 |

Question:

> For each customer, show each transaction, previous transaction amount, running total, transaction rank by amount, and percentage of the customer's total spending.

That's a perfect window-function problem.

---

## Step 1 — Previous transaction

```sql
LAG(amount) OVER (
    PARTITION BY customer
    ORDER BY date
)
```

---

## Step 2 — Running total

```sql
SUM(amount) OVER (
    PARTITION BY customer
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

---

## Step 3 — Rank transactions by amount

```sql
RANK() OVER (
    PARTITION BY customer
    ORDER BY amount DESC
)
```

---

## Step 4 — Customer total

```sql
SUM(amount) OVER (
    PARTITION BY customer
)
```

---

## Step 5 — Percentage

```sql
100.0 * amount /
SUM(amount) OVER (
    PARTITION BY customer
)
```

---

## Final query

```sql
SELECT
    customer,
    date,
    amount,

    LAG(amount) OVER (
        PARTITION BY customer
        ORDER BY date
    ) AS previous_amount,

    SUM(amount) OVER (
        PARTITION BY customer
        ORDER BY date
        ROWS BETWEEN UNBOUNDED PRECEDING
             AND CURRENT ROW
    ) AS running_total,

    RANK() OVER (
        PARTITION BY customer
        ORDER BY amount DESC
    ) AS amount_rank,

    ROUND(
        100.0 * amount /
        SUM(amount) OVER (
            PARTITION BY customer
        ),
        2
    ) AS pct_of_customer_total

FROM sales
ORDER BY customer, date;
```

Output:

| customer | date   | amount | previous | running | rank | % total |
| -------- | ------ | -----: | -------: | ------: | ---: | ------: |
| Alice    | Jan 1  |    100 |     NULL |     100 |    3 |   22.22 |
| Alice    | Jan 5  |    200 |      100 |     300 |    1 |   44.44 |
| Alice    | Jan 10 |    150 |      200 |     450 |    2 |   33.33 |
| Bob      | Jan 2  |    300 |     NULL |     300 |    2 |   33.33 |
| Bob      | Jan 7  |    100 |      300 |     400 |    3 |   11.11 |
| Bob      | Jan 12 |    500 |      100 |     900 |    1 |   55.56 |

This single query demonstrates:

```text
PARTITION BY
ORDER BY
LAG
RANK
SUM OVER
running total
percentage of total
```

---

# PART 15 — HOW TO TRANSLATE ENGLISH INTO WINDOW SQL

This is probably the most useful section for interviews.

## English:

> Rank employees by salary.

Translate:

```text
rank
 ↓
RANK()
```

```sql
RANK() OVER (
    ORDER BY salary DESC
)
```

---

## English:

> Rank employees within each department.

```text
within each department
        ↓
PARTITION BY department
```

```sql
RANK() OVER (
    PARTITION BY department
    ORDER BY salary DESC
)
```

---

## English:

> Give each employee a unique position.

```sql
ROW_NUMBER()
```

---

## English:

> Find previous month's revenue.

```sql
LAG(revenue)
```

---

## English:

> Find next month's revenue.

```sql
LEAD(revenue)
```

---

## English:

> Calculate cumulative revenue.

```sql
SUM(revenue) OVER (
    ORDER BY month
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

---

## English:

> Calculate cumulative revenue per customer.

```sql
SUM(revenue) OVER (
    PARTITION BY customer_id
    ORDER BY month
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

---

## English:

> Calculate 3-month moving average.

```sql
AVG(revenue) OVER (
    ORDER BY month
    ROWS BETWEEN 2 PRECEDING
         AND CURRENT ROW
)
```

---

## English:

> Show each employee's department average.

```sql
AVG(salary) OVER (
    PARTITION BY department
)
```

---

## English:

> Show each employee's percentage of department salary.

```sql
salary /
SUM(salary) OVER (
    PARTITION BY department
)
```

---

## English:

> Find the latest record for every customer.

```text
latest
 ↓
ORDER BY date DESC

every customer
 ↓
PARTITION BY customer

one row
 ↓
ROW_NUMBER
```

```sql
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY updated_at DESC
)
```

Then:

```sql
WHERE rn = 1
```

---

# PART 16 — MASTER PATTERN MAP

```text
WINDOW FUNCTIONS
│
├── RANKING
│   ├── ROW_NUMBER()
│   │   └── unique position
│   │
│   ├── RANK()
│   │   └── ties + gaps
│   │
│   ├── DENSE_RANK()
│   │   └── ties + no gaps
│   │
│   └── NTILE(n)
│       └── buckets
│
├── VALUE / NAVIGATION
│   ├── LAG()
│   │   └── previous
│   │
│   ├── LEAD()
│   │   └── next
│   │
│   ├── FIRST_VALUE()
│   │   └── first
│   │
│   ├── LAST_VALUE()
│   │   └── last
│   │
│   └── NTH_VALUE()
│       └── nth
│
├── AGGREGATION
│   ├── SUM() OVER
│   ├── AVG() OVER
│   ├── COUNT() OVER
│   ├── MIN() OVER
│   └── MAX() OVER
│
└── DISTRIBUTION
    ├── PERCENT_RANK()
    └── CUME_DIST()
```

---

# PART 17 — THE WINDOW FRAME CHEATSHEET

| Requirement          | Window                                                     |
| -------------------- | ---------------------------------------------------------- |
| Entire partition     | `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` |
| Start → current      | `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`         |
| Previous 1 + current | `ROWS BETWEEN 1 PRECEDING AND CURRENT ROW`                 |
| Previous 2 + current | `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW`                 |
| Current + next 1     | `ROWS BETWEEN CURRENT ROW AND 1 FOLLOWING`                 |
| Previous 7 + current | `ROWS BETWEEN 7 PRECEDING AND CURRENT ROW`                 |

---

# PART 18 — ULTIMATE INTERVIEW CHEATSHEET

## Ranking

### Unique row number

```sql
ROW_NUMBER() OVER (
    PARTITION BY group_col
    ORDER BY value DESC
)
```

### Rank with gaps

```sql
RANK() OVER (
    PARTITION BY group_col
    ORDER BY value DESC
)
```

### Rank without gaps

```sql
DENSE_RANK() OVER (
    PARTITION BY group_col
    ORDER BY value DESC
)
```

### Buckets

```sql
NTILE(4) OVER (
    ORDER BY value
)
```

---

# Value functions

### Previous

```sql
LAG(value) OVER (
    PARTITION BY group_col
    ORDER BY date
)
```

### Next

```sql
LEAD(value) OVER (
    PARTITION BY group_col
    ORDER BY date
)
```

### First

```sql
FIRST_VALUE(value) OVER (
    PARTITION BY group_col
    ORDER BY date
)
```

### Last

```sql
LAST_VALUE(value) OVER (
    PARTITION BY group_col
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND UNBOUNDED FOLLOWING
)
```

---

# Aggregation

### Group total

```sql
SUM(value) OVER (
    PARTITION BY group_col
)
```

### Running total

```sql
SUM(value) OVER (
    PARTITION BY group_col
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

### Moving average

```sql
AVG(value) OVER (
    ORDER BY date
    ROWS BETWEEN 2 PRECEDING
         AND CURRENT ROW
)
```

### Group average

```sql
AVG(value) OVER (
    PARTITION BY group_col
)
```

### Running maximum

```sql
MAX(value) OVER (
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

---

# Interview pattern cheatsheet

| English question            | SQL pattern                                               |
| --------------------------- | --------------------------------------------------------- |
| Rank all employees          | `RANK() OVER (ORDER BY ...)`                              |
| Rank within department      | `RANK() OVER (PARTITION BY ... ORDER BY ...)`             |
| Unique ranking              | `ROW_NUMBER()`                                            |
| Ranking with ties           | `RANK()`                                                  |
| Distinct ranking            | `DENSE_RANK()`                                            |
| Top 3 per department        | `ROW_NUMBER() ... WHERE rn <= 3`                          |
| Top 3 including ties        | `DENSE_RANK() ... WHERE rnk <= 3`                         |
| Previous value              | `LAG()`                                                   |
| Next value                  | `LEAD()`                                                  |
| Change from previous        | `value - LAG(value)`                                      |
| % change                    | `(value - LAG(value))/LAG(value)`                         |
| First value                 | `FIRST_VALUE()`                                           |
| Last value                  | `LAST_VALUE()` + explicit full frame                      |
| Total per group             | `SUM() OVER (PARTITION BY ...)`                           |
| Running total               | `SUM() OVER (... ORDER BY ... ROWS ...)`                  |
| Moving average              | `AVG() OVER (... ROWS N PRECEDING ...)`                   |
| Group average beside row    | `AVG() OVER (PARTITION BY ...)`                           |
| % of group total            | `value / SUM(value) OVER (...)`                           |
| Highest per group           | `ROW_NUMBER()` / `RANK()` / `MAX() OVER`                  |
| Latest per group            | `ROW_NUMBER() ... ORDER BY date DESC`                     |
| Deduplicate                 | `ROW_NUMBER() OVER (PARTITION BY key ORDER BY date DESC)` |
| Quartiles                   | `NTILE(4)`                                                |
| Percentile position         | `PERCENT_RANK()`                                          |
| Cumulative distribution     | `CUME_DIST()`                                             |
| Consecutive/streak problems | `LAG()` + cumulative `SUM()`                              |

---

# The 10 patterns I would memorize first

If you're preparing for SQL interviews, don't try to memorize 100 queries. Master these **10 templates**:

### 1. Top 1 per group

```sql
ROW_NUMBER() OVER (
    PARTITION BY group_col
    ORDER BY metric DESC
)
```

---

### 2. Top N per group

```sql
WITH x AS (
    SELECT *,
        ROW_NUMBER() OVER (
            PARTITION BY group_col
            ORDER BY metric DESC
        ) rn
    FROM table
)
SELECT *
FROM x
WHERE rn <= N;
```

---

### 3. Top N with ties

```sql
DENSE_RANK() OVER (
    PARTITION BY group_col
    ORDER BY metric DESC
)
```

---

### 4. Previous value

```sql
LAG(value) OVER (
    PARTITION BY entity
    ORDER BY date
)
```

---

### 5. Change from previous

```sql
value -
LAG(value) OVER (
    PARTITION BY entity
    ORDER BY date
)
```

---

### 6. Running total

```sql
SUM(value) OVER (
    PARTITION BY entity
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
         AND CURRENT ROW
)
```

---

### 7. Moving average

```sql
AVG(value) OVER (
    PARTITION BY entity
    ORDER BY date
    ROWS BETWEEN 2 PRECEDING
         AND CURRENT ROW
)
```

---

### 8. Group average beside every row

```sql
AVG(value) OVER (
    PARTITION BY group_col
)
```

---

### 9. Percentage of group

```sql
100.0 * value /
SUM(value) OVER (
    PARTITION BY group_col
)
```

---

### 10. Latest record per group

```sql
WITH x AS (
    SELECT *,
        ROW_NUMBER() OVER (
            PARTITION BY group_col
            ORDER BY date DESC
        ) rn
    FROM table
)
SELECT *
FROM x
WHERE rn = 1;
```

---

# Final mental model

When translating an English question into a window function, use this:

```text
                  WINDOW QUESTION
                         │
           ┌─────────────┼──────────────┐
           ↓             ↓              ↓
       "within each"   "ordered by"   "which rows?"
           │             │              │
           ↓             ↓              ↓
      PARTITION BY    ORDER BY       FRAME
```

Then identify the operation:

```text
"position/rank"
       ↓
ROW_NUMBER / RANK / DENSE_RANK

"previous"
       ↓
LAG

"next"
       ↓
LEAD

"first/last"
       ↓
FIRST_VALUE / LAST_VALUE

"total/average/count"
       ↓
SUM / AVG / COUNT OVER

"cumulative"
       ↓
ORDER BY + UNBOUNDED PRECEDING

"moving/rolling"
       ↓
ROWS BETWEEN N PRECEDING AND CURRENT ROW
```

And the **single most important distinction** to remember:

> **`PARTITION BY` answers "WHO belongs in the same window?"**
> **`ORDER BY` answers "IN WHAT ORDER?"**
> **The frame answers "WHICH ROWS around the current row?"**

Once those three become automatic, window-function questions stop looking complicated—they become combinations of a relatively small number of patterns.
