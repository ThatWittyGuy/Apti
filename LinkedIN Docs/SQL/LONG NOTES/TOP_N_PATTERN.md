# SQL Top-N / Bottom-N Problems

## From Scratch → English-to-SQL Translation → All Patterns → MySQL + PostgreSQL → Ties → Per-Group Top-N → Interview Problems → Tricks → Cheatsheet

Top-N questions are among the **most common SQL interview and coding-test patterns**.

Typical questions:

> Find the top 3 highest-paid employees.

> Find the second-highest salary.

> Find the top 3 employees in each department.

> Find the bottom 2 products by sales.

> Find the top 5 customers by total spending.

> Find the 3rd highest salary **without duplicates**.

> Find all employees tied for the highest salary in each department.

The key is that **"Top-N" can mean several different things**.

---

# 1. First: understand what "Top-N" means

Suppose we have:

### `employees`

| emp_id | name    | department  | salary |
| -----: | ------- | ----------- | -----: |
|      1 | Alice   | Engineering | 120000 |
|      2 | Bob     | Engineering | 100000 |
|      3 | Charlie | Engineering | 100000 |
|      4 | David   | Engineering |  90000 |
|      5 | Emma    | Sales       | 150000 |
|      6 | Frank   | Sales       | 120000 |
|      7 | Grace   | Sales       | 120000 |
|      8 | Henry   | Sales       |  80000 |

If asked:

> Top 3 employees by salary

There are at least **two possible interpretations**.

### Interpretation A — exactly 3 rows

```text
Emma     150000
Alice    120000
Frank    120000
```

### Interpretation B — top 3 salary levels

```text
150000
120000
100000
```

This could return:

```text
Emma
Alice
Frank
Grace
Bob
Charlie
```

because all employees with those salary levels qualify.

That distinction is **extremely important**.

---

# 2. The fundamental Top-N question

Whenever you see:

> Top N

ask:

```text
1. Top N rows?
2. Top N distinct values?
3. Top N per group?
4. Do ties count?
5. Exactly N rows or all tied rows?
```

This determines your SQL.

---

# 3. The simplest Top-N: `ORDER BY + LIMIT`

If you need:

> Top 3 employees by salary

MySQL:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

PostgreSQL:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

Same syntax.

Output:

| name  | salary |
| ----- | -----: |
| Emma  | 150000 |
| Alice | 120000 |
| Frank | 120000 |

---

# 4. Bottom-N

Reverse the ordering.

> Find the bottom 3 employees by salary.

```sql
SELECT *
FROM employees
ORDER BY salary ASC
LIMIT 3;
```

Output:

| name  | salary |
| ----- | -----: |
| Henry |  80000 |
| David |  90000 |
| Bob   | 100000 |

---

# 5. The core pattern

Memorize:

```sql
-- TOP N
ORDER BY column DESC
LIMIT N
```

```sql
-- BOTTOM N
ORDER BY column ASC
LIMIT N
```

This solves simple global Top-N problems.

---

# 6. English → SQL translation

### "Highest salary"

```sql
ORDER BY salary DESC
LIMIT 1
```

### "Lowest salary"

```sql
ORDER BY salary ASC
LIMIT 1
```

### "Top 5 salaries"

```sql
ORDER BY salary DESC
LIMIT 5
```

### "Bottom 5 salaries"

```sql
ORDER BY salary ASC
LIMIT 5
```

---

# 7. But here's the first major interview trap

Suppose:

| name  | salary |
| ----- | -----: |
| Emma  | 150000 |
| Alice | 120000 |
| Frank | 120000 |
| Grace | 120000 |
| Bob   | 100000 |

Question:

> Find the top 2 employees by salary.

This:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 2;
```

returns exactly:

```text
Emma
Alice
```

But what if the interviewer says:

> Include all employees tied for the second-highest salary.

Now `LIMIT` alone isn't enough.

---

# 8. `RANK()` solves tie-aware Top-N

Use:

```sql
RANK() OVER (
    ORDER BY salary DESC
)
```

Example:

```sql
SELECT
    name,
    salary,
    RANK() OVER (
        ORDER BY salary DESC
    ) AS rnk
FROM employees;
```

Output:

| name  | salary | rnk |
| ----- | -----: | --: |
| Emma  | 150000 |   1 |
| Alice | 120000 |   2 |
| Frank | 120000 |   2 |
| Grace | 120000 |   2 |
| Bob   | 100000 |   5 |

Notice the gap:

```text
1
2
2
2
5
```

---

# 9. Top 2 with ties

Now:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
) x
WHERE rnk <= 2;
```

Output:

| name  | salary |
| ----- | -----: |
| Emma  | 150000 |
| Alice | 120000 |
| Frank | 120000 |
| Grace | 120000 |

You requested:

```text
Top 2 ranks
```

and all employees tied within those ranks are returned.

---

# 10. `ROW_NUMBER()` vs `RANK()` vs `DENSE_RANK()`

This is one of the most important concepts.

Suppose salaries:

```text
150000
120000
120000
100000
90000
```

### `ROW_NUMBER()`

```text
1
2
3
4
5
```

Every row gets a unique number.

### `RANK()`

```text
1
2
2
4
5
```

Ties share rank, and gaps occur.

### `DENSE_RANK()`

```text
1
2
2
3
4
```

Ties share rank, but no gaps.

---

# 11. Which ranking function for Top-N?

### Exactly N rows

Use:

```sql
ROW_NUMBER()
```

or:

```sql
LIMIT
```

### Top N positions including ties

Usually:

```sql
RANK()
```

### Top N distinct values

Usually:

```sql
DENSE_RANK()
```

This distinction is extremely useful.

---

# 12. Top N distinct salaries

Question:

> Find the top 3 distinct salaries.

Use:

```sql
SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
) x
WHERE rnk <= 3;
```

Output:

```text
150000
120000
100000
```

---

# 13. Simpler way: `DISTINCT + ORDER BY + LIMIT`

If you only need the salary values:

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

This is much simpler.

Output:

| salary |
| -----: |
| 150000 |
| 120000 |
| 100000 |

---

# 14. Top 3 employees based on distinct salary levels

Now suppose you want **all employees** whose salaries are in the top 3 distinct salary levels.

```sql
SELECT *
FROM employees
WHERE salary IN (
    SELECT DISTINCT salary
    FROM employees
    ORDER BY salary DESC
    LIMIT 3
);
```

This is a classic subquery pattern.

Output:

| name  | salary |
| ----- | -----: |
| Emma  | 150000 |
| Alice | 120000 |
| Frank | 120000 |
| Grace | 120000 |
| Bob   | 100000 |

---

# 15. Important distinction

Compare:

### Exactly 3 employees

```sql
ORDER BY salary DESC
LIMIT 3
```

### Top 3 salary levels

```sql
SELECT DISTINCT salary
ORDER BY salary DESC
LIMIT 3
```

### All employees belonging to top 3 salary levels

```sql
WHERE salary IN (
    SELECT DISTINCT salary
    ...
)
```

### Top 3 ranks including ties

```sql
RANK() <= 3
```

These are **not necessarily the same result**.

---

# 16. Second-highest salary

One of the most common interview questions.

### Method 1 — `MAX`

```sql
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```

Suppose:

```text
150000
120000
120000
100000
90000
```

Result:

```text
120000
```

---

# 17. Second-highest salary with `DISTINCT`

Another simple approach:

```sql
SELECT salary
FROM (
    SELECT DISTINCT salary
    FROM employees
    ORDER BY salary DESC
    LIMIT 2
) x
ORDER BY salary
LIMIT 1;
```

Or PostgreSQL/MySQL-style:

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

Result:

```text
120000
```

---

# 18. Second-highest using `DENSE_RANK`

```sql
SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
) x
WHERE rnk = 2;
```

If you only select salary, duplicate salary rows may still appear.

Use:

```sql
SELECT DISTINCT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
) x
WHERE rnk = 2;
```

---

# 19. Second-highest employee(s)

If you want all employees tied for second-highest:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        DENSE_RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
) x
WHERE rnk = 2;
```

Output:

| name  | salary |
| ----- | -----: |
| Alice | 120000 |
| Frank | 120000 |
| Grace | 120000 |

---

# 20. `OFFSET`

For global ranking, another useful method is:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

Meaning:

```text
LIMIT 1
→ return one row

OFFSET 1
→ skip the first row
```

So:

```text
OFFSET 0 → highest
OFFSET 1 → second
OFFSET 2 → third
```

---

# 21. Nth highest salary

For exactly the Nth row:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET N-1;
```

For example, third row:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 2;
```

But this is **row-based**, not distinct-salary-based.

---

# 22. Nth distinct highest salary

Use:

```sql
SELECT salary
FROM (
    SELECT DISTINCT salary
    FROM employees
    ORDER BY salary DESC
) x
LIMIT 1 OFFSET 2;
```

Or:

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 2;
```

This gives the third distinct salary.

---

# 23. Better general solution: `DENSE_RANK`

For interview questions involving:

> Nth highest **distinct** salary

use:

```sql
SELECT DISTINCT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
) x
WHERE rnk = N;
```

Replace `N`.

For third:

```sql
WHERE rnk = 3
```

---

# 24. Top-N per group

This is where Top-N questions become much more important.

Question:

> Find the top 3 highest-paid employees in **each department**.

You cannot simply do:

```sql
ORDER BY salary DESC
LIMIT 3;
```

That gives top 3 across the entire company.

You need:

```text
PARTITION BY department_id
```

---

# 25. Top 3 per department using `ROW_NUMBER`

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
) x
WHERE rn <= 3;
```

This means:

```text
For every department:

    sort salaries descending
    number them 1, 2, 3...
    keep 1, 2, 3
```

---

# 26. Example

Department 10:

| name    | salary |
| ------- | -----: |
| Alice   | 120000 |
| Bob     | 100000 |
| Charlie |  90000 |
| David   |  80000 |

Department 20:

| name  | salary |
| ----- | -----: |
| Emma  | 150000 |
| Frank | 120000 |
| Grace | 110000 |
| Henry |  80000 |

After:

```sql
ROW_NUMBER() OVER (
    PARTITION BY department_id
    ORDER BY salary DESC
)
```

we get:

| name    | dept | salary |  rn |
| ------- | ---: | -----: | --: |
| Alice   |   10 | 120000 |   1 |
| Bob     |   10 | 100000 |   2 |
| Charlie |   10 |  90000 |   3 |
| David   |   10 |  80000 |   4 |
| Emma    |   20 | 150000 |   1 |
| Frank   |   20 | 120000 |   2 |
| Grace   |   20 | 110000 |   3 |
| Henry   |   20 |  80000 |   4 |

Then:

```sql
WHERE rn <= 3
```

returns top 3 from **each department**.

---

# 27. Top 3 per group with ties

If ties should be included:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        RANK() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
) x
WHERE rnk <= 3;
```

This may return more than 3 employees in a department.

That's intentional.

---

# 28. `ROW_NUMBER` vs `RANK` for per-group Top-N

Suppose department 10:

| employee | salary |
| -------- | -----: |
| A        | 120000 |
| B        | 100000 |
| C        | 100000 |
| D        | 100000 |
| E        |  90000 |

### `ROW_NUMBER`

```text
1
2
3
4
5
```

Top 2:

```text
A
B
```

Exactly two.

### `RANK`

```text
1
2
2
2
5
```

Top 2:

```text
A
B
C
D
```

All ties are included.

---

# 29. `DENSE_RANK` for Top-N per group

Suppose:

```text
120000
100000
100000
90000
80000
```

DENSE_RANK:

```text
1
2
2
3
4
```

So:

```sql
WHERE dense_rank <= 2
```

returns:

```text
120000
100000
100000
```

This means:

> Top 2 distinct salary levels in each department.

---

# 30. The three Top-N meanings

Memorize this table:

| Requirement                    | Function       |
| ------------------------------ | -------------- |
| Exactly N rows                 | `ROW_NUMBER()` |
| Top N positions including ties | `RANK()`       |
| Top N distinct values          | `DENSE_RANK()` |

This is one of the most useful SQL interview rules.

---

# 31. Bottom-N

Everything works in reverse.

### Bottom 3 globally

```sql
SELECT *
FROM employees
ORDER BY salary ASC
LIMIT 3;
```

### Bottom 3 per department

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary ASC
        ) AS rn
    FROM employees e
) x
WHERE rn <= 3;
```

---

# 32. Bottom 3 with ties

```sql
SELECT *
FROM (
    SELECT
        e.*,
        RANK() OVER (
            PARTITION BY department_id
            ORDER BY salary ASC
        ) AS rnk
    FROM employees e
) x
WHERE rnk <= 3;
```

Just change:

```text
DESC
```

to:

```text
ASC
```

---

# 33. Top-N based on an aggregate

Top-N doesn't have to be based on an existing column.

Question:

> Find the top 3 customers by total spending.

Suppose:

### `orders`

| order_id | customer_id | amount |
| -------: | ----------: | -----: |
|        1 |         101 |    500 |
|        2 |         101 |    300 |
|        3 |         102 |   1000 |
|        4 |         103 |    700 |
|        5 |         103 |    800 |

First:

```sql
SELECT
    customer_id,
    SUM(amount) AS total_spending
FROM orders
GROUP BY customer_id;
```

Output:

| customer_id | total_spending |
| ----------: | -------------: |
|         101 |            800 |
|         102 |           1000 |
|         103 |           1500 |

Then:

```sql
SELECT
    customer_id,
    SUM(amount) AS total_spending
FROM orders
GROUP BY customer_id
ORDER BY total_spending DESC
LIMIT 3;
```

---

# 34. Top-N groups

This is an important pattern:

```sql
SELECT
    group_column,
    AGG(value) AS metric
FROM table
GROUP BY group_column
ORDER BY metric DESC
LIMIT N;
```

Examples:

```text
Top customers by revenue
Top products by sales
Top departments by average salary
Top stores by number of orders
Top cities by users
```

---

# 35. Top 3 products by sales

```sql
SELECT
    product_id,
    SUM(quantity) AS units_sold
FROM order_items
GROUP BY product_id
ORDER BY units_sold DESC
LIMIT 3;
```

---

# 36. Top 3 products by revenue

Don't confuse:

```text
units sold
```

with:

```text
revenue
```

Revenue might be:

```sql
SELECT
    product_id,
    SUM(quantity * price) AS revenue
FROM order_items
GROUP BY product_id
ORDER BY revenue DESC
LIMIT 3;
```

Always identify the **metric**.

---

# 37. Top 3 departments by average salary

```sql
SELECT
    department_id,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
ORDER BY avg_salary DESC
LIMIT 3;
```

---

# 38. Top-N with a minimum threshold

Question:

> Find top 5 customers by spending, but only customers with at least 3 orders.

```sql
SELECT
    customer_id,
    COUNT(*) AS order_count,
    SUM(amount) AS total_spending
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 3
ORDER BY total_spending DESC
LIMIT 5;
```

Important order:

```text
FROM
→ GROUP BY
→ HAVING
→ ORDER BY
→ LIMIT
```

---

# 39. Top-N after filtering

Question:

> Find the top 5 employees earning more than 80000.

```sql
SELECT *
FROM employees
WHERE salary > 80000
ORDER BY salary DESC
LIMIT 5;
```

The filter happens before the ranking/order/limit conceptually.

---

# 40. Top-N after grouping

Question:

> Among departments with at least 5 employees, find the top 3 departments by average salary.

```sql
SELECT
    department_id,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING COUNT(*) >= 5
ORDER BY avg_salary DESC
LIMIT 3;
```

---

# 41. Top-N per group + aggregate

Question:

> Find the top 2 customers by revenue in each city.

First calculate revenue:

```sql
WITH customer_revenue AS (
    SELECT
        city,
        customer_id,
        SUM(amount) AS revenue
    FROM orders
    GROUP BY city, customer_id
)
SELECT *
FROM (
    SELECT
        cr.*,
        ROW_NUMBER() OVER (
            PARTITION BY city
            ORDER BY revenue DESC
        ) AS rn
    FROM customer_revenue cr
) x
WHERE rn <= 2;
```

This is a **very common advanced interview pattern**.

---

# 42. Top-N per group — the universal template

Memorize this:

```sql
SELECT *
FROM (
    SELECT
        t.*,
        ROW_NUMBER() OVER (
            PARTITION BY group_column
            ORDER BY metric DESC
        ) AS rn
    FROM table t
) x
WHERE rn <= N;
```

For ties:

```sql
RANK()
```

For distinct values:

```sql
DENSE_RANK()
```

This is one of the most valuable templates in SQL interviews.

---

# 43. Top 1 per group

Question:

> Find the highest-paid employee in each department.

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
) x
WHERE rn = 1;
```

---

# 44. Top 1 per group with ties

If two employees are both highest-paid:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        RANK() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
) x
WHERE rnk = 1;
```

Both are returned.

---

# 45. Top 2 per group

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
) x
WHERE rn <= 2;
```

---

# 46. Bottom 2 per group

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary ASC
        ) AS rn
    FROM employees e
) x
WHERE rn <= 2;
```

---

# 47. Top-N with a tie-breaking column

This is an extremely important real-world trick.

Suppose:

```text
Alice 120000
Bob   120000
Charlie 120000
```

If you use:

```sql
ROW_NUMBER() OVER (
    ORDER BY salary DESC
)
```

the ordering among equal salaries may be nondeterministic.

Add a tie-breaker:

```sql
ROW_NUMBER() OVER (
    ORDER BY salary DESC, emp_id ASC
)
```

Now:

```text
salary DESC
then
emp_id ASC
```

gives deterministic results.

---

# 48. Always think about deterministic ordering

Bad:

```sql
ORDER BY salary DESC
LIMIT 3;
```

if multiple rows have the same salary and you care exactly which rows are selected.

Better:

```sql
ORDER BY salary DESC, emp_id ASC
LIMIT 3;
```

This tells SQL exactly how to break ties.

---

# 49. Top-N with `NULL`

Suppose salaries contain NULL:

| name    | salary |
| ------- | -----: |
| Alice   | 120000 |
| Bob     |   NULL |
| Charlie |  90000 |

Sorting NULLs can differ in behavior between MySQL and PostgreSQL depending on `ASC`/`DESC`.

For predictable behavior, explicitly control NULL ordering.

PostgreSQL:

```sql
ORDER BY salary DESC NULLS LAST;
```

PostgreSQL supports:

```text
NULLS FIRST
NULLS LAST
```

MySQL doesn't use that exact syntax in the same way, so a portable trick is:

```sql
ORDER BY
    salary IS NULL,
    salary DESC
```

for putting non-NULL values first.

---

# 50. PostgreSQL `FETCH FIRST`

PostgreSQL also supports standard-style:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
FETCH FIRST 3 ROWS ONLY;
```

This is equivalent conceptually to:

```sql
LIMIT 3
```

---

# 51. PostgreSQL `FETCH ... WITH TIES`

PostgreSQL supports:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
FETCH FIRST 3 ROWS WITH TIES;
```

This can return more than 3 rows if the ordering value ties at the boundary.

This is a very useful PostgreSQL feature for tie-aware global Top-N.

---

# 52. MySQL `LIMIT ... OFFSET`

MySQL and PostgreSQL both support:

```sql
LIMIT 3 OFFSET 5;
```

Meaning:

```text
skip first 5
return next 3
```

Useful for pagination.

---

# 53. Pagination is basically Top-N

For page 1:

```sql
LIMIT 10 OFFSET 0
```

Page 2:

```sql
LIMIT 10 OFFSET 10
```

Page 3:

```sql
LIMIT 10 OFFSET 20
```

General:

```text
OFFSET = (page_number - 1) * page_size
```

---

# 54. But OFFSET pagination can become expensive

For example:

```sql
LIMIT 20 OFFSET 1000000;
```

The database may have to process/skip a large number of rows.

For very large datasets, **keyset/seek pagination** is often better.

---

# 55. Keyset pagination

Suppose we sort by:

```sql
ORDER BY salary DESC, emp_id DESC
```

If the last row from the previous page was:

```text
salary = 90000
emp_id = 50
```

Next page can use:

```sql
WHERE
    salary < 90000
    OR (
        salary = 90000
        AND emp_id < 50
    )
ORDER BY salary DESC, emp_id DESC
LIMIT 20;
```

This is an advanced but very valuable real-world pattern.

---

# 56. Top-N date/time problems

Top-N isn't limited to salaries.

Example:

> Find the latest 5 orders.

```sql
SELECT *
FROM orders
ORDER BY order_date DESC
LIMIT 5;
```

Oldest 5:

```sql
SELECT *
FROM orders
ORDER BY order_date ASC
LIMIT 5;
```

---

# 57. Latest order per customer

Global Top-N:

```sql
ORDER BY order_date DESC
LIMIT 1
```

Latest **per customer** requires partitioning:

```sql
SELECT *
FROM (
    SELECT
        o.*,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY order_date DESC, order_id DESC
        ) AS rn
    FROM orders o
) x
WHERE rn = 1;
```

This is one of the most frequently asked SQL problems.

---

# 58. Top 3 latest orders per customer

```sql
SELECT *
FROM (
    SELECT
        o.*,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY order_date DESC, order_id DESC
        ) AS rn
    FROM orders o
) x
WHERE rn <= 3;
```

Same pattern.

Only the:

```text
metric
```

changes.

---

# 59. Top-N by frequency

Question:

> Find the top 3 most frequently purchased products.

```sql
SELECT
    product_id,
    COUNT(*) AS purchase_count
FROM order_items
GROUP BY product_id
ORDER BY purchase_count DESC
LIMIT 3;
```

The pattern is:

```text
GROUP BY
+
COUNT
+
ORDER BY DESC
+
LIMIT
```

---

# 60. Bottom-N by frequency

```sql
SELECT
    product_id,
    COUNT(*) AS purchase_count
FROM order_items
GROUP BY product_id
ORDER BY purchase_count ASC
LIMIT 3;
```

---

# 61. Top-N by ratio/percentage

Suppose:

> Find the top 5 products by conversion rate.

You might calculate:

```sql
SELECT
    product_id,
    SUM(purchases) / NULLIF(SUM(views), 0) AS conversion_rate
FROM product_stats
GROUP BY product_id
ORDER BY conversion_rate DESC
LIMIT 5;
```

The important idea:

> Top-N is based on a **calculated metric**, not necessarily a stored column.

---

# 62. Top-N after JOIN

Question:

> Find the top 3 customers by total order amount, including customer names.

```sql
SELECT
    c.customer_id,
    c.name,
    SUM(o.amount) AS total_spending
FROM customers c
JOIN orders o
    ON o.customer_id = c.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_spending DESC
LIMIT 3;
```

Very common.

---

# 63. Top-N with `LEFT JOIN`

Question:

> Find the top 5 customers by spending, including customers who spent nothing.

```sql
SELECT
    c.customer_id,
    c.name,
    COALESCE(SUM(o.amount), 0) AS total_spending
FROM customers c
LEFT JOIN orders o
    ON o.customer_id = c.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_spending DESC
LIMIT 5;
```

Important:

```text
LEFT JOIN
+
COALESCE
```

ensures customers without orders can still appear with 0.

---

# 64. Top-N with ties after aggregation

Question:

> Find all customers tied for the top total spending.

First calculate totals, then rank:

```sql
WITH customer_totals AS (
    SELECT
        customer_id,
        SUM(amount) AS total_spending
    FROM orders
    GROUP BY customer_id
)
SELECT *
FROM (
    SELECT
        ct.*,
        RANK() OVER (
            ORDER BY total_spending DESC
        ) AS rnk
    FROM customer_totals ct
) x
WHERE rnk = 1;
```

---

# 65. Top 3 customers with ties

```sql
WITH customer_totals AS (
    SELECT
        customer_id,
        SUM(amount) AS total_spending
    FROM orders
    GROUP BY customer_id
)
SELECT *
FROM (
    SELECT
        ct.*,
        RANK() OVER (
            ORDER BY total_spending DESC
        ) AS rnk
    FROM customer_totals ct
) x
WHERE rnk <= 3;
```

---

# 66. Top 3 distinct revenue values

```sql
WITH customer_totals AS (
    SELECT
        customer_id,
        SUM(amount) AS total_spending
    FROM orders
    GROUP BY customer_id
)
SELECT DISTINCT total_spending
FROM (
    SELECT
        total_spending,
        DENSE_RANK() OVER (
            ORDER BY total_spending DESC
        ) AS rnk
    FROM customer_totals
) x
WHERE rnk <= 3;
```

---

# 67. A powerful pattern: aggregate → rank → filter

This pattern appears constantly.

```text
Step 1:
GROUP BY

      ↓

Step 2:
calculate metric

      ↓

Step 3:
RANK / ROW_NUMBER

      ↓

Step 4:
filter rank
```

Example:

```sql
WITH customer_totals AS (
    SELECT
        customer_id,
        SUM(amount) AS total_spending
    FROM orders
    GROUP BY customer_id
)
SELECT *
FROM (
    SELECT
        customer_totals.*,
        ROW_NUMBER() OVER (
            ORDER BY total_spending DESC
        ) AS rn
    FROM customer_totals
) x
WHERE rn <= 3;
```

Memorize this architecture.

---

# 68. Why can't we directly use `WHERE rn <= 3`?

This won't work:

```sql
SELECT
    name,
    salary,
    ROW_NUMBER() OVER (
        ORDER BY salary DESC
    ) AS rn
FROM employees
WHERE rn <= 3;
```

Why?

Because window functions are calculated **after WHERE** in the logical query-processing order.

So the alias `rn` isn't available to `WHERE` at that stage.

Use a subquery/CTE:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
) x
WHERE rn <= 3;
```

This is a very common interview question.

---

# 69. CTE version

Often cleaner:

```sql
WITH ranked AS (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
)
SELECT *
FROM ranked
WHERE rn <= 3;
```

For complicated interview questions, this is usually easier to explain.

---

# 70. Top-N per group with CTE

```sql
WITH ranked AS (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
)
SELECT *
FROM ranked
WHERE rn <= 3;
```

This is arguably the **single most important Top-N template**.

---

# 71. Top-N using correlated subqueries

Before window functions, Top-N per group was often solved with correlated subqueries.

Example:

> Highest-paid employee per department.

```sql
SELECT *
FROM employees e
WHERE salary = (
    SELECT MAX(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

This works well for Top-1 with ties.

For Top-N, however, window functions are generally much cleaner.

---

# 72. Top-N using a count of higher values

Another advanced method:

> Find employees whose salary is among the top 3 distinct salaries.

Conceptually:

```sql
SELECT *
FROM employees e
WHERE 3 > (
    SELECT COUNT(DISTINCT e2.salary)
    FROM employees e2
    WHERE e2.salary > e.salary
);
```

Why?

For a salary to be top 3:

```text
number of distinct salaries above it < 3
```

This is a classic correlated-subquery technique.

---

# 73. Understanding that pattern

Suppose salaries:

```text
150000
120000
100000
90000
```

For salary 120000:

```text
distinct salaries greater than 120000
= 1
```

For salary 100000:

```text
distinct salaries greater than 100000
= 2
```

For salary 90000:

```text
distinct salaries greater than 90000
= 3
```

So:

```text
COUNT(DISTINCT greater salaries) < 3
```

selects:

```text
150000
120000
100000
```

---

# 74. Top-N per group using correlated count

Advanced:

> Find top 3 distinct salaries per department.

```sql
SELECT *
FROM employees e
WHERE 3 > (
    SELECT COUNT(DISTINCT e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
      AND e2.salary > e.salary
);
```

This is a powerful interview pattern.

But if window functions are available, this is usually easier:

```sql
DENSE_RANK() OVER (
    PARTITION BY department_id
    ORDER BY salary DESC
)
```

---

# 75. Bottom-N using the same logic

Change:

```sql
e2.salary > e.salary
```

to:

```sql
e2.salary < e.salary
```

Example:

```sql
SELECT *
FROM employees e
WHERE 3 > (
    SELECT COUNT(DISTINCT e2.salary)
    FROM employees e2
    WHERE e2.salary < e.salary
);
```

This identifies bottom 3 distinct salary levels.

---

# 76. Top-N and `DISTINCT`

Be careful where you put `DISTINCT`.

These are different:

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

versus:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

The first means:

> top 3 unique salary values.

The second means:

> top 3 employee rows.

---

# 77. Top-N by category

Suppose:

```text
category
product
sales
```

Question:

> Top 2 products in each category.

```sql
WITH product_sales AS (
    SELECT
        category,
        product_id,
        SUM(amount) AS total_sales
    FROM sales
    GROUP BY category, product_id
),
ranked AS (
    SELECT
        product_sales.*,
        ROW_NUMBER() OVER (
            PARTITION BY category
            ORDER BY total_sales DESC
        ) AS rn
    FROM product_sales
)
SELECT *
FROM ranked
WHERE rn <= 2;
```

This is an extremely reusable pattern.

---

# 78. Top-N per month

Question:

> Find top 3 products by sales in each month.

Conceptually:

```sql
WITH monthly_product_sales AS (
    SELECT
        DATE_TRUNC('month', sale_date) AS month,
        product_id,
        SUM(amount) AS sales
    FROM sales
    GROUP BY DATE_TRUNC('month', sale_date), product_id
)
SELECT *
FROM (
    SELECT
        monthly_product_sales.*,
        ROW_NUMBER() OVER (
            PARTITION BY month
            ORDER BY sales DESC
        ) AS rn
    FROM monthly_product_sales
) x
WHERE rn <= 3;
```

PostgreSQL uses `DATE_TRUNC`.

For MySQL, you might use:

```sql
DATE_FORMAT(sale_date, '%Y-%m-01')
```

or another month representation.

---

# 79. Top-N per year

Same architecture:

```text
GROUP BY year
→ calculate metric
→ ROW_NUMBER/RANK
→ filter
```

Example MySQL:

```sql
WITH yearly_sales AS (
    SELECT
        YEAR(order_date) AS year,
        product_id,
        SUM(amount) AS sales
    FROM orders
    GROUP BY YEAR(order_date), product_id
)
SELECT *
FROM (
    SELECT
        yearly_sales.*,
        ROW_NUMBER() OVER (
            PARTITION BY year
            ORDER BY sales DESC
        ) AS rn
    FROM yearly_sales
) x
WHERE rn <= 3;
```

---

# 80. Top-N in each date period

This is a broader pattern:

```text
Top 3 products per:
    day
    week
    month
    quarter
    year
    region
    department
    category
    store
    customer segment
```

The only thing changing is:

```sql
PARTITION BY ...
```

---

# 81. Top-N with multiple sorting criteria

Question:

> Top 5 employees by salary, with higher performance score breaking salary ties.

```sql
SELECT *
FROM employees
ORDER BY
    salary DESC,
    performance_score DESC
LIMIT 5;
```

With window:

```sql
ROW_NUMBER() OVER (
    ORDER BY salary DESC, performance_score DESC
)
```

---

# 82. Top-N with a calculated score

Suppose:

```text
sales
returns
rating
```

You can calculate:

```sql
ORDER BY
    sales * 0.7 + rating * 0.3 DESC
LIMIT 10;
```

Top-N isn't a special SQL feature.

It's simply:

```text
define metric
+
sort metric
+
take N
```

---

# 83. Top-N logic in plain English

Whenever you see:

> "top"

translate it into:

```text
What metric?
↓
Higher or lower?
↓
Global or per group?
↓
Ties or exactly N?
↓
Rows or distinct values?
```

For example:

> Find the top 3 customers by revenue in each region, including ties.

Translate:

```text
metric = SUM(revenue)
direction = DESC
group = region
N = 3
ties = YES
```

Then:

```text
GROUP BY region, customer
↓
SUM(revenue)
↓
RANK() PARTITION BY region
↓
WHERE rank <= 3
```

That's the entire thought process.

---

# 84. Common Top-N interview mistakes

## Mistake 1

Using:

```sql
LIMIT 3
```

for:

> top 3 in each department.

`LIMIT` is global.

Use:

```sql
PARTITION BY department_id
```

---

## Mistake 2

Using `ROW_NUMBER()` when ties should be included.

If:

```text
top 3 including ties
```

use:

```sql
RANK()
```

---

## Mistake 3

Using `RANK()` when exactly 3 rows are required.

Use:

```sql
ROW_NUMBER()
```

---

## Mistake 4

Forgetting `DISTINCT`.

If question says:

> top 3 distinct salaries

think:

```sql
DISTINCT
```

or:

```sql
DENSE_RANK()
```

---

# 85. Mistake 5 — Ranking before aggregation

Wrong logic:

```text
rank individual orders
then calculate customer totals
```

If question is:

> top customers by total spending

you need:

```text
orders
↓
GROUP BY customer
↓
SUM(amount)
↓
rank customers
```

Always determine the **grain** first.

---

# 86. The concept of "grain"

This is an advanced but extremely important SQL skill.

Ask:

> What does one row represent at this stage?

Example:

Raw orders:

```text
1 row = 1 order
```

After:

```sql
GROUP BY customer_id
```

you have:

```text
1 row = 1 customer
```

After:

```sql
GROUP BY region, customer_id
```

you have:

```text
1 row = 1 customer-region combination
```

Then rank.

This prevents many Top-N mistakes.

---

# 87. Top-N architecture

Think:

```text
RAW DATA
   ↓
FILTER
   ↓
JOIN
   ↓
GROUP BY
   ↓
CALCULATE METRIC
   ↓
RANK
   ↓
FILTER RANK
   ↓
FINAL ORDER
```

Not every problem needs every step, but this architecture is incredibly useful.

---

# 88. SQL logical order for Top-N

Remember:

```text
FROM
JOIN
WHERE
GROUP BY
HAVING
SELECT
WINDOW FUNCTIONS
ORDER BY
LIMIT
```

This explains many SQL behaviors.

For example:

```sql
SELECT
    name,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS rn
FROM employees
WHERE salary > 50000;
```

First filter employees:

```text
salary > 50000
```

then rank the remaining employees.

This is often exactly what you want.

---

# 89. Filter before ranking vs after ranking

This distinction is huge.

Question:

> Top 3 employees among employees earning over 50000.

```sql
WITH ranked AS (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
    WHERE salary > 50000
)
SELECT *
FROM ranked
WHERE rn <= 3;
```

Here:

```text
filter first
→ rank second
```

---

# 90. Different question

> Find employees who are globally top 3, then among those show employees earning over 50000.

Now:

```text
rank first
→ filter salary second
```

Those are different problems.

SQL query structure must reflect the English.

---

# 91. Top-N with JOIN duplicates

Suppose you join:

```text
customers
orders
```

and then rank customers before aggregating.

You can accidentally rank **orders**, not customers.

Correct:

```text
JOIN
↓
GROUP BY customer
↓
SUM
↓
rank
```

Again:

> Know your grain.

---

# 92. Top-N and `HAVING`

Question:

> Top 5 customers who placed at least 10 orders.

```sql
SELECT
    customer_id,
    COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 10
ORDER BY order_count DESC
LIMIT 5;
```

Notice:

```text
HAVING
```

filters groups **before** Top-N.

---

# 93. Top-N and `WHERE`

Question:

> Top 5 customers based only on orders placed in 2026.

```sql
SELECT
    customer_id,
    SUM(amount) AS total
FROM orders
WHERE order_date >= '2026-01-01'
  AND order_date < '2027-01-01'
GROUP BY customer_id
ORDER BY total DESC
LIMIT 5;
```

The date filter changes which orders participate in the calculation.

---

# 94. Top-N and `HAVING` vs `WHERE`

Use:

```text
WHERE
```

for row-level filtering.

Use:

```text
HAVING
```

for aggregate/group-level filtering.

Example:

```sql
WHERE amount > 100
```

versus:

```sql
HAVING SUM(amount) > 1000
```

---

# 95. MySQL-specific note

MySQL supports:

```sql
LIMIT N
```

and:

```sql
LIMIT offset, count
```

Example:

```sql
LIMIT 10, 20;
```

means:

```text
skip 10
take 20
```

For readability, many developers prefer:

```sql
LIMIT 20 OFFSET 10;
```

---

# 96. PostgreSQL-specific note

PostgreSQL supports:

```sql
LIMIT 10 OFFSET 20
```

and standard-style:

```sql
FETCH FIRST 10 ROWS ONLY
```

It also supports:

```sql
FETCH FIRST 10 ROWS WITH TIES
```

which is useful for tie-aware global Top-N.

---

# 97. Top-N optimization

If you only need:

```sql
ORDER BY salary DESC
LIMIT 10
```

an appropriate index can help significantly.

For example, depending on the workload/database:

```text
index on salary
```

can help the database retrieve the highest values efficiently.

But don't blindly add indexes.

Consider:

```text
WHERE columns
JOIN columns
ORDER BY columns
GROUP BY columns
```

and verify with:

```text
EXPLAIN
```

or:

```text
EXPLAIN ANALYZE
```

---

# 98. Top-N per group is harder to optimize

This:

```sql
ROW_NUMBER() OVER (
    PARTITION BY department_id
    ORDER BY salary DESC
)
```

is more complex than a simple global:

```sql
ORDER BY salary DESC
LIMIT 10
```

because the database must determine the ranking inside each group.

Indexes and optimizer behavior matter.

For interviews, focus first on **correctness and query structure**.

---

# 99. Top-N problem-solving checklist

When you see a Top-N question, ask:

### 1. What are we ranking?

```text
employees?
customers?
products?
orders?
departments?
```

### 2. What is the metric?

```text
salary?
SUM(amount)?
COUNT(*)?
AVG(score)?
latest date?
```

### 3. Direction?

```text
DESC = top/highest/latest
ASC = bottom/lowest/earliest
```

### 4. Global or per group?

```text
whole table
OR
PARTITION BY group
```

### 5. Ties?

```text
ROW_NUMBER
RANK
DENSE_RANK
```

### 6. Distinct values?

```text
DISTINCT
DENSE_RANK
```

### 7. Need exactly N rows?

```text
ROW_NUMBER / LIMIT
```

### 8. Need all ties?

```text
RANK / DENSE_RANK
```

---

# 100. Ultimate Top-N pattern library

## Pattern 1 — Global Top-N

```sql
SELECT *
FROM table
ORDER BY metric DESC
LIMIT N;
```

---

## Pattern 2 — Global Bottom-N

```sql
SELECT *
FROM table
ORDER BY metric ASC
LIMIT N;
```

---

## Pattern 3 — Top-N per group

```sql
WITH ranked AS (
    SELECT
        t.*,
        ROW_NUMBER() OVER (
            PARTITION BY group_column
            ORDER BY metric DESC
        ) AS rn
    FROM table t
)
SELECT *
FROM ranked
WHERE rn <= N;
```

---

## Pattern 4 — Top-N per group with ties

```sql
WITH ranked AS (
    SELECT
        t.*,
        RANK() OVER (
            PARTITION BY group_column
            ORDER BY metric DESC
        ) AS rnk
    FROM table t
)
SELECT *
FROM ranked
WHERE rnk <= N;
```

---

## Pattern 5 — Top-N distinct values

```sql
SELECT DISTINCT metric
FROM table
ORDER BY metric DESC
LIMIT N;
```

---

## Pattern 6 — Nth distinct value

```sql
SELECT metric
FROM (
    SELECT DISTINCT metric
    FROM table
    ORDER BY metric DESC
) x
LIMIT 1 OFFSET N - 1;
```

---

## Pattern 7 — Second-highest

```sql
SELECT MAX(metric)
FROM table
WHERE metric < (
    SELECT MAX(metric)
    FROM table
);
```

---

## Pattern 8 — Top-N aggregate groups

```sql
SELECT
    group_column,
    SUM(metric) AS total
FROM table
GROUP BY group_column
ORDER BY total DESC
LIMIT N;
```

---

## Pattern 9 — Top-N aggregate per group

```sql
WITH aggregated AS (
    SELECT
        group_column,
        item_column,
        SUM(metric) AS total
    FROM table
    GROUP BY group_column, item_column
),
ranked AS (
    SELECT
        aggregated.*,
        ROW_NUMBER() OVER (
            PARTITION BY group_column
            ORDER BY total DESC
        ) AS rn
    FROM aggregated
)
SELECT *
FROM ranked
WHERE rn <= N;
```

---

## Pattern 10 — Latest N per group

```sql
WITH ranked AS (
    SELECT
        t.*,
        ROW_NUMBER() OVER (
            PARTITION BY group_column
            ORDER BY date_column DESC
        ) AS rn
    FROM table t
)
SELECT *
FROM ranked
WHERE rn <= N;
```

---

# 101. Top-N interview cheat sheet

```text
========================================================
TOP-N / BOTTOM-N SQL CHEATSHEET
========================================================

GLOBAL TOP N
--------------------------------------------------------
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;


GLOBAL BOTTOM N
--------------------------------------------------------
SELECT *
FROM employees
ORDER BY salary ASC
LIMIT 3;


TOP N DISTINCT VALUES
--------------------------------------------------------
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 3;


NTH HIGHEST DISTINCT
--------------------------------------------------------
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET N-1;


SECOND HIGHEST
--------------------------------------------------------
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);


TOP N PER GROUP
--------------------------------------------------------
WITH ranked AS (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
)
SELECT *
FROM ranked
WHERE rn <= N;


TOP N PER GROUP + TIES
--------------------------------------------------------
RANK() OVER (
    PARTITION BY department_id
    ORDER BY salary DESC
)


TOP N DISTINCT PER GROUP
--------------------------------------------------------
DENSE_RANK() OVER (
    PARTITION BY department_id
    ORDER BY salary DESC
)


EXACTLY N ROWS
--------------------------------------------------------
ROW_NUMBER()


N POSITIONS INCLUDING TIES
--------------------------------------------------------
RANK()


N DISTINCT VALUES
--------------------------------------------------------
DENSE_RANK()


TOP CUSTOMER BY TOTAL SPENDING
--------------------------------------------------------
SELECT
    customer_id,
    SUM(amount) AS total
FROM orders
GROUP BY customer_id
ORDER BY total DESC
LIMIT N;


TOP PRODUCTS BY SALES
--------------------------------------------------------
SELECT
    product_id,
    SUM(amount) AS sales
FROM order_items
GROUP BY product_id
ORDER BY sales DESC
LIMIT N;


LATEST N PER CUSTOMER
--------------------------------------------------------
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY order_date DESC
)


BOTTOM N
--------------------------------------------------------
Change:

DESC → ASC


TIE BREAKER
--------------------------------------------------------
ORDER BY salary DESC, emp_id ASC


IMPORTANT QUESTION
--------------------------------------------------------
Always ask:

1. What is the metric?
2. Highest or lowest?
3. Global or per group?
4. Exactly N or include ties?
5. Rows or distinct values?
6. Filter before or after ranking?


CORE ARCHITECTURE
--------------------------------------------------------
FILTER
   ↓
JOIN
   ↓
GROUP BY
   ↓
AGGREGATE
   ↓
RANK
   ↓
FILTER RANK
   ↓
FINAL ORDER
========================================================
```

# 102. The 10 patterns I would memorize for interviews

If you're preparing specifically for SQL coding rounds, get these **10 patterns completely automatic**:

### ① Top N globally

```sql
ORDER BY metric DESC
LIMIT N
```

### ② Bottom N globally

```sql
ORDER BY metric ASC
LIMIT N
```

### ③ Nth highest distinct value

```sql
DENSE_RANK() OVER (ORDER BY metric DESC)
```

### ④ Exactly N rows per group

```sql
ROW_NUMBER() OVER (
    PARTITION BY group_col
    ORDER BY metric DESC
)
```

### ⑤ Top N per group with ties

```sql
RANK() OVER (
    PARTITION BY group_col
    ORDER BY metric DESC
)
```

### ⑥ Top N distinct values per group

```sql
DENSE_RANK() OVER (
    PARTITION BY group_col
    ORDER BY metric DESC
)
```

### ⑦ Top customers/products

```sql
GROUP BY entity
ORDER BY SUM(...) DESC
LIMIT N
```

### ⑧ Top N after aggregation

```text
GROUP BY
→ aggregate
→ rank/order
→ LIMIT
```

### ⑨ Latest N per group

```sql
ROW_NUMBER() OVER (
    PARTITION BY group_col
    ORDER BY date_col DESC
)
```

### ⑩ Top N with ties + aggregation

```text
GROUP BY
→ calculate metric
→ RANK()
→ WHERE rank <= N
```

The **single most important thing** to internalize is this:

> **Top-N is not one SQL problem. First identify the grain, metric, grouping, and tie requirement. Then choose `LIMIT`, `ROW_NUMBER()`, `RANK()`, or `DENSE_RANK()`.**

Once you can do that translation from English, almost every Top-N/Bottom-N variation becomes a modification of one of these templates.
