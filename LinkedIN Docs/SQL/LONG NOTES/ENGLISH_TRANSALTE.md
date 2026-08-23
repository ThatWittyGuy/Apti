# English → SQL Interview Translation Notes

## From English question → identify the pattern → build the query

The most important SQL interview skill is **not memorizing syntax**.

It is learning to translate:

> **English → requirement → SQL operation → query structure**

For example:

> "Find customers who placed at least 5 orders."

Don't immediately think about syntax.

Translate it:

```text
customers
    ↓
"placed orders"
    ↓
orders table
    ↓
"per customer"
    ↓
GROUP BY customer
    ↓
"at least 5"
    ↓
COUNT(*) >= 5
    ↓
this is a group condition
    ↓
HAVING
```

Final:

```sql
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 5;
```

This chapter teaches that translation process **from scratch**, including the major English patterns you'll encounter in SQL interviews.

---

# 1. First principle: SQL questions are usually asking you to identify 5 things

When you read an interview question, extract these:

```text
1. WHAT should be returned?
2. FROM WHICH table(s)?
3. AT WHAT LEVEL should results be calculated?
4. WHAT calculation is required?
5. WHAT conditions apply before/after calculation?
```

Let's use:

> Find departments with more than 5 employees whose average salary is above 70,000.

Translate:

```text
WHAT?
→ department

LEVEL?
→ one row per department

CALCULATION?
→ COUNT employees
→ AVG salary

ROW CONDITION?
→ employees whose ... [if specified]

GROUP CONDITION?
→ COUNT > 5
→ AVG > 70000
```

Query:

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING COUNT(*) > 5
   AND AVG(salary) > 70000;
```

---

# 2. The master translation dictionary

Start with this mental dictionary.

| English phrase    | Usually translates to                           |
| ----------------- | ----------------------------------------------- |
| per               | `GROUP BY`                                      |
| each              | `GROUP BY`                                      |
| for every         | `GROUP BY`                                      |
| by                | `GROUP BY`                                      |
| total             | `SUM()`                                         |
| sum               | `SUM()`                                         |
| number of         | `COUNT()`                                       |
| how many          | `COUNT()`                                       |
| average           | `AVG()`                                         |
| mean              | `AVG()`                                         |
| highest           | `MAX()`                                         |
| maximum           | `MAX()`                                         |
| lowest            | `MIN()`                                         |
| minimum           | `MIN()`                                         |
| unique            | `DISTINCT`                                      |
| different         | `DISTINCT`                                      |
| at least          | `>=`                                            |
| at most           | `<=`                                            |
| more than         | `>`                                             |
| less than         | `<`                                             |
| greater than      | `>`                                             |
| equal to          | `=`                                             |
| not equal         | `<>` / `!=`                                     |
| between           | `BETWEEN`                                       |
| either/or         | `OR`                                            |
| both              | `AND`                                           |
| not               | `NOT` / `<>`                                    |
| contains          | `LIKE '%x%'`                                    |
| starts with       | `LIKE 'x%'`                                     |
| ends with         | `LIKE '%x'`                                     |
| before            | `<`                                             |
| after             | `>`                                             |
| on or before      | `<=`                                            |
| on or after       | `>=`                                            |
| duplicate         | `GROUP BY ... HAVING COUNT(*) > 1`              |
| no duplicates     | `GROUP BY ... HAVING COUNT(*) = 1`              |
| has               | often `HAVING` after grouping                   |
| whose             | depends on whether condition is row/group level |
| top               | `ORDER BY ... DESC` + `LIMIT` / ranking         |
| bottom            | `ORDER BY ... ASC` + `LIMIT`                    |
| latest            | `MAX(date)` / ranking / `ORDER BY DESC`         |
| earliest          | `MIN(date)` / ranking / `ORDER BY ASC`          |
| percentage        | `numerator / denominator`                       |
| ratio             | division                                        |
| consecutive       | often window functions / date logic             |
| first             | `ROW_NUMBER()` / `MIN()` depending on question  |
| second highest    | ranking / `MAX()` subquery                      |
| each customer's   | `GROUP BY customer_id`                          |
| each department's | `GROUP BY department`                           |

But this table is only the beginning.

The **context** determines which SQL construct you need.

---

# 3. The most important word: "per"

When you see:

> per customer

think:

```sql
GROUP BY customer_id
```

When you see:

> per department

think:

```sql
GROUP BY department
```

When you see:

> per product

think:

```sql
GROUP BY product_id
```

When you see:

> per month

think:

```sql
GROUP BY month
```

---

# 4. Why does "per" mean `GROUP BY`?

Suppose the question says:

> Find total sales **per customer**.

"Per customer" means:

> I want a separate answer for every customer.

So:

```text
Customer 101 → total
Customer 102 → total
Customer 103 → total
```

That's exactly what `GROUP BY` does.

```sql
SELECT
    customer_id,
    SUM(amount) AS total_sales
FROM orders
GROUP BY customer_id;
```

### English → SQL

```text
total
  ↓
SUM(amount)

per customer
  ↓
GROUP BY customer_id
```

---

# 5. "Each" = usually `GROUP BY`

Question:

> Find the total revenue for each product.

Translation:

```text
total revenue
→ SUM(revenue)

each product
→ GROUP BY product_id
```

```sql
SELECT
    product_id,
    SUM(revenue) AS total_revenue
FROM sales
GROUP BY product_id;
```

---

# 6. "For every" = `GROUP BY`

> Find the average salary for every department.

Translation:

```text
average salary
→ AVG(salary)

every department
→ GROUP BY department
```

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

---

# 7. "By" = often `GROUP BY`

Be careful: "by" can have different meanings, but in analytical SQL it frequently means grouping.

> Sales by region

```sql
GROUP BY region
```

> Orders by customer

```sql
GROUP BY customer_id
```

> Revenue by month

```sql
GROUP BY month
```

> Employees by department

```sql
GROUP BY department
```

---

# 8. The "one row per..." rule

This is probably the **most powerful SQL interview translation trick**.

If the question says:

> Return one row per customer.

Immediately think:

```sql
GROUP BY customer_id
```

If:

> Return one row per department.

Think:

```sql
GROUP BY department
```

If:

> Return one row per customer per month.

Think:

```sql
GROUP BY customer_id, month
```

If:

> Return one row per product per country.

Think:

```sql
GROUP BY product_id, country
```

### Mental rule

> **"One row per X" → `GROUP BY X`**

---

# 9. "Total" → `SUM()`

Question:

> Find total sales per customer.

Translate:

```text
total
→ SUM()

per customer
→ GROUP BY customer_id
```

```sql
SELECT
    customer_id,
    SUM(amount) AS total_sales
FROM orders
GROUP BY customer_id;
```

---

# 10. "Total amount"

Usually:

```sql
SUM(amount)
```

Question:

> Calculate the total order amount for each customer.

```sql
SELECT
    customer_id,
    SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id;
```

---

# 11. "Total revenue"

```text
total revenue
→ SUM(revenue)
```

```sql
SELECT
    product_id,
    SUM(revenue) AS total_revenue
FROM sales
GROUP BY product_id;
```

---

# 12. "Number of" → `COUNT()`

Question:

> Find the number of employees in each department.

Translate:

```text
number of employees
→ COUNT(*)

each department
→ GROUP BY department
```

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

---

# 13. "How many" → `COUNT()`

> How many orders did each customer place?

```sql
SELECT
    customer_id,
    COUNT(*) AS order_count
FROM orders
GROUP BY customer_id;
```

Translation:

```text
how many orders
→ COUNT(*)

each customer
→ GROUP BY customer_id
```

---

# 14. "How many different..." → `COUNT(DISTINCT ...)`

This distinction is extremely important.

Question:

> How many customers purchased each product?

Potentially:

```sql
COUNT(customer_id)
```

But question:

> How many **different customers** purchased each product?

Now:

```sql
COUNT(DISTINCT customer_id)
```

Full query:

```sql
SELECT
    product_id,
    COUNT(DISTINCT customer_id) AS customer_count
FROM orders
GROUP BY product_id;
```

### Translation

```text
how many
→ COUNT

different customers
→ DISTINCT customer_id

each product
→ GROUP BY product_id
```

---

# 15. "Unique" → `DISTINCT`

Question:

> Find the number of unique customers per region.

```sql
SELECT
    region,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM orders
GROUP BY region;
```

Translation:

```text
unique customers
→ COUNT(DISTINCT customer_id)

per region
→ GROUP BY region
```

---

# 16. "Different" → usually `DISTINCT`

> How many different products did each customer buy?

```sql
SELECT
    customer_id,
    COUNT(DISTINCT product_id) AS products_bought
FROM orders
GROUP BY customer_id;
```

---

# 17. "Average" → `AVG()`

> What is the average salary in each department?

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

Translation:

```text
average
→ AVG()

each department
→ GROUP BY department
```

---

# 18. "Highest" → `MAX()`

> Find the highest salary in each department.

```sql
SELECT
    department,
    MAX(salary) AS highest_salary
FROM employees
GROUP BY department;
```

Translation:

```text
highest
→ MAX()

each department
→ GROUP BY department
```

---

# 19. "Lowest" → `MIN()`

> Find the lowest salary in each department.

```sql
SELECT
    department,
    MIN(salary) AS lowest_salary
FROM employees
GROUP BY department;
```

---

# 20. Aggregate translation table

Whenever you see:

| English      | SQL                   |
| ------------ | --------------------- |
| total        | `SUM()`               |
| sum          | `SUM()`               |
| number       | `COUNT()`             |
| how many     | `COUNT()`             |
| average      | `AVG()`               |
| mean         | `AVG()`               |
| highest      | `MAX()`               |
| maximum      | `MAX()`               |
| lowest       | `MIN()`               |
| minimum      | `MIN()`               |
| unique count | `COUNT(DISTINCT ...)` |

---

# 21. "More than" — first ask: rows or groups?

This is one of the **most important interview concepts**.

Question:

> Find employees with salary greater than 50,000.

This is about individual employees.

```sql
WHERE salary > 50000
```

But:

> Find departments with more than 5 employees.

This is about groups.

```sql
HAVING COUNT(*) > 5
```

### The key question

Ask yourself:

> **Am I filtering individual rows or the result of grouping?**

---

# 22. "More than X" for individual rows

> Find orders greater than $1,000.

```sql
SELECT *
FROM orders
WHERE amount > 1000;
```

Translation:

```text
orders
→ individual rows

greater than 1000
→ WHERE amount > 1000
```

---

# 23. "More than X" for groups

> Find customers who placed more than 5 orders.

```sql
SELECT
    customer_id,
    COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 5;
```

Translation:

```text
more than 5 orders
→ COUNT(*) > 5

customer
→ GROUP BY customer_id

condition on count
→ HAVING
```

---

# 24. "At least" → `>=`

> Customers with at least 5 orders.

```sql
HAVING COUNT(*) >= 5
```

Not:

```sql
HAVING COUNT(*) > 5
```

Because:

```text
at least 5
= 5 or more
= >= 5
```

---

# 25. "At most" → `<=`

> Find customers with at most 3 orders.

```sql
HAVING COUNT(*) <= 3
```

Translation:

```text
at most 3
= 3 or fewer
= <= 3
```

---

# 26. "Fewer than" → `<`

> Departments with fewer than 10 employees.

```sql
HAVING COUNT(*) < 10
```

---

# 27. "Less than or equal to" → `<=`

> Products with sales less than or equal to 1000.

```sql
WHERE sales <= 1000
```

---

# 28. "Greater than or equal to" → `>=`

> Employees earning at least 50,000.

```sql
WHERE salary >= 50000
```

---

# 29. The comparison dictionary

```text
more than       → >
greater than    → >

at least        → >=
minimum of      → >=

less than       → <
fewer than      → <

at most         → <=
maximum of      → <=

equal to        → =

not equal to    → <>
```

---

# 30. "Duplicate" → `GROUP BY + HAVING`

This is a classic interview pattern.

Question:

> Find duplicate emails.

Translation:

```text
same email
→ GROUP BY email

appears more than once
→ COUNT(*) > 1

group condition
→ HAVING
```

```sql
SELECT
    email,
    COUNT(*) AS occurrences
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

---

# 31. "Find values that occur more than once"

Same pattern:

```sql
SELECT
    column_name,
    COUNT(*) AS occurrences
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 1;
```

Memorize this.

---

# 32. "Find values that occur exactly once"

```sql
SELECT
    column_name
FROM table_name
GROUP BY column_name
HAVING COUNT(*) = 1;
```

Translation:

```text
same value
→ GROUP BY

exactly once
→ COUNT(*) = 1
```

---

# 33. "No duplicates"

If you mean values that occur exactly once:

```sql
GROUP BY column
HAVING COUNT(*) = 1
```

But be careful: "no duplicate rows" can mean something else depending on the question.

---

# 34. "Where" in English vs SQL `WHERE`

This is a subtle point.

Question:

> Find employees where salary is greater than 50,000.

Here "where" is natural English.

It means:

```sql
WHERE salary > 50000
```

But:

> Find departments where more than 5 employees work.

The condition is now on a group:

```sql
HAVING COUNT(*) > 5
```

So don't translate the English word "where" mechanically.

Instead ask:

> **What is being filtered?**

---

# 35. "Whose" → identify what is being filtered

Question:

> Find customers whose total spending is greater than 10,000.

"whose total spending" is a **group-level** condition.

```sql
SELECT
    customer_id,
    SUM(amount) AS total_spending
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 10000;
```

---

# 36. "Whose salary is greater than..." → row-level

> Find employees whose salary is greater than 50,000.

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

Compare:

```text
whose salary > 50000
→ WHERE

whose total spending > 10000
→ HAVING
```

The word "total" changes everything.

---

# 37. "Contains" → `LIKE`

> Find customers whose name contains "son".

```sql
SELECT *
FROM customers
WHERE name LIKE '%son%';
```

Translation:

```text
contains "son"
→ LIKE '%son%'
```

---

# 38. "Starts with"

> Find names starting with A.

```sql
WHERE name LIKE 'A%'
```

Remember:

```text
A%
```

means:

> starts with A, followed by anything.

---

# 39. "Ends with"

> Find names ending in "son".

```sql
WHERE name LIKE '%son'
```

---

# 40. LIKE translation cheat sheet

```text
contains X
→ LIKE '%X%'

starts with X
→ LIKE 'X%'

ends with X
→ LIKE '%X'

exactly X
→ = 'X'
```

---

# 41. "Between" → `BETWEEN`

> Find employees with salary between 50,000 and 80,000.

```sql
WHERE salary BETWEEN 50000 AND 80000
```

Usually `BETWEEN` is inclusive at both ends.

Conceptually:

```text
50000 <= salary <= 80000
```

---

# 42. "Between dates"

> Find orders between January 1 and January 31.

Be careful with timestamps.

For date-only columns:

```sql
WHERE order_date BETWEEN '2026-01-01' AND '2026-01-31'
```

For timestamp columns, a safer pattern is often:

```sql
WHERE order_date >= '2026-01-01'
  AND order_date <  '2026-02-01'
```

This avoids accidentally excluding times on January 31.

---

# 43. "In" → `IN`

> Find employees in IT, HR, or Sales.

```sql
WHERE department IN ('IT', 'HR', 'Sales')
```

Instead of:

```sql
WHERE department = 'IT'
   OR department = 'HR'
   OR department = 'Sales'
```

Both express the same basic idea.

---

# 44. "Not in" → `NOT IN`

> Find employees who are not in IT or HR.

```sql
WHERE department NOT IN ('IT', 'HR')
```

Be aware that `NULL` can make `NOT IN` behave unexpectedly when the subquery/list contains NULLs.

---

# 45. "Either ... or ..." → `OR`

> Find employees who work in IT or HR.

```sql
WHERE department = 'IT'
   OR department = 'HR'
```

---

# 46. "Both ... and ..." → `AND`

> Find employees in IT with salary above 70,000.

```sql
WHERE department = 'IT'
  AND salary > 70000
```

---

# 47. "Not" → `NOT`, `<>`, etc.

> Find employees who are not in IT.

```sql
WHERE department <> 'IT'
```

or:

```sql
WHERE NOT department = 'IT'
```

---

# 48. "Is missing" / "has no value" → `IS NULL`

Never write:

```sql
WHERE manager_id = NULL
```

Correct:

```sql
WHERE manager_id IS NULL
```

Translation:

```text
missing
→ IS NULL
```

---

# 49. "Has a value" → `IS NOT NULL`

> Find employees who have a manager.

```sql
WHERE manager_id IS NOT NULL
```

---

# 50. "Without" / "no" — important interview pattern

> Find customers with no orders.

This is usually not solved with simple `GROUP BY` alone.

Common solution:

```sql
SELECT
    c.customer_id,
    c.customer_name
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

Translation:

```text
customers
→ customers table

with no orders
→ LEFT JOIN orders

unmatched order
→ o.order_id IS NULL
```

Another method is `NOT EXISTS`:

```sql
SELECT
    c.customer_id,
    c.customer_name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

---

# 51. "Include customers with zero orders"

This phrase is a huge clue.

Think:

```text
include zero
→ LEFT JOIN
```

Then:

```sql
COUNT(child.id)
```

Example:

```sql
SELECT
    c.customer_id,
    COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id;
```

---

# 52. "Only customers who have orders"

Think:

```text
only matching records
→ INNER JOIN
```

```sql
SELECT
    c.customer_id,
    COUNT(o.order_id)
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id;
```

---

# 53. "Per month"

This is a date-grouping problem.

Question:

> Find total revenue per month.

Translation:

```text
total revenue
→ SUM(revenue)

per month
→ GROUP BY month
```

But you must extract the month from the date.

For example, MySQL:

```sql
SELECT
    YEAR(order_date) AS year,
    MONTH(order_date) AS month,
    SUM(amount) AS revenue
FROM orders
GROUP BY
    YEAR(order_date),
    MONTH(order_date);
```

---

# 54. "Per year"

MySQL:

```sql
SELECT
    YEAR(order_date) AS year,
    SUM(amount) AS revenue
FROM orders
GROUP BY YEAR(order_date);
```

PostgreSQL:

```sql
SELECT
    EXTRACT(YEAR FROM order_date) AS year,
    SUM(amount) AS revenue
FROM orders
GROUP BY EXTRACT(YEAR FROM order_date);
```

---

# 55. "Per day"

Depending on the database and whether timestamps are involved, you may need to extract/cast the date.

Conceptually:

```text
timestamp
→ date
→ GROUP BY date
```

For example:

```sql
SELECT
    CAST(order_timestamp AS DATE) AS order_day,
    COUNT(*) AS orders
FROM orders
GROUP BY CAST(order_timestamp AS DATE);
```

---

# 56. "Monthly average"

Be careful about what "monthly average" means.

It could mean:

### Average order amount per month

```sql
SELECT
    month,
    AVG(amount)
FROM orders
GROUP BY month;
```

Or:

### Average monthly revenue across all months

That requires another level of aggregation:

```text
orders
→ revenue per month
→ average of monthly revenues
```

This distinction appears in harder interviews.

---

# 57. "Top" → usually sorting + limiting

> Find the top 5 customers by total spending.

Translation:

```text
total spending
→ SUM(amount)

per customer
→ GROUP BY customer_id

top 5
→ ORDER BY total_spending DESC
→ LIMIT 5
```

```sql
SELECT
    customer_id,
    SUM(amount) AS total_spending
FROM orders
GROUP BY customer_id
ORDER BY total_spending DESC
LIMIT 5;
```

---

# 58. "Top 1"

```sql
ORDER BY total_spending DESC
LIMIT 1
```

But if the question says:

> Find **all customers tied for** highest spending

`LIMIT 1` is insufficient.

You may need `RANK()` or another approach.

This is an important interview distinction.

---

# 59. "Highest in each group"

Question:

> Find the highest-paid employee in each department.

Don't automatically do:

```sql
GROUP BY department
```

because that only gives:

```text
department + maximum salary
```

If you need the **employee row**, you need a different pattern.

Modern SQL:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
) x
WHERE rnk = 1;
```

Translation:

```text
highest
→ ORDER BY salary DESC

in each department
→ PARTITION BY department

return employee
→ window function + filtering
```

---

# 60. "Second highest"

This phrase should immediately trigger a question:

> Second highest overall or second highest per group?

### Overall second highest

Possible approach:

```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```

Or use ranking/window functions.

### Second highest per department

```sql
RANK() OVER (
    PARTITION BY department
    ORDER BY salary DESC
)
```

then:

```sql
WHERE rnk = 2
```

---

# 61. "First" / "latest" / "earliest"

These are often **ordering problems**, not `GROUP BY` problems.

Question:

> Find the latest order for each customer.

Translation:

```text
for each customer
→ PARTITION BY customer

latest
→ ORDER BY order_date DESC

one row
→ ROW_NUMBER()
```

```sql
SELECT *
FROM (
    SELECT
        o.*,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY order_date DESC
        ) AS rn
    FROM orders o
) x
WHERE rn = 1;
```

This is different from:

```sql
MAX(order_date)
```

if you need the entire order row.

---

# 62. "Latest date per customer"

If you only need the date:

```sql
SELECT
    customer_id,
    MAX(order_date) AS latest_order_date
FROM orders
GROUP BY customer_id;
```

Translation:

```text
latest date
→ MAX(order_date)

per customer
→ GROUP BY customer_id
```

If you need the complete order:

```text
latest order
→ window function
```

---

# 63. "Earliest"

If only the date is required:

```sql
MIN(order_date)
```

If the entire row is required:

```sql
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY order_date
)
```

---

# 64. "Percentage"

Percentage questions are extremely common.

Suppose:

> What percentage of orders are completed?

Translation:

```text
completed orders
→ numerator

all orders
→ denominator

percentage
→ numerator / denominator * 100
```

```sql
SELECT
    100.0 *
    SUM(CASE WHEN status = 'Completed' THEN 1 ELSE 0 END)
    / COUNT(*) AS completed_percentage
FROM orders;
```

---

# 65. "Percentage per department"

> What percentage of employees in each department earn more than 70,000?

Translation:

```text
per department
→ GROUP BY department

employees earning > 70000
→ numerator

all employees
→ denominator
```

```sql
SELECT
    department,
    100.0 *
    SUM(
        CASE
            WHEN salary > 70000 THEN 1
            ELSE 0
        END
    ) / COUNT(*) AS percentage_high_earners
FROM employees
GROUP BY department;
```

---

# 66. "Ratio"

Question:

> Find the ratio of completed to cancelled orders.

Think:

```text
completed count
÷
cancelled count
```

Conditional aggregation:

```sql
SELECT
    SUM(CASE WHEN status = 'Completed' THEN 1 ELSE 0 END) * 1.0
    /
    NULLIF(
        SUM(CASE WHEN status = 'Cancelled' THEN 1 ELSE 0 END),
        0
    ) AS completed_cancelled_ratio
FROM orders;
```

`NULLIF(..., 0)` can protect against division by zero.

---

# 67. "Conditional" / "only when" → `CASE`

Question:

> Count completed orders for each customer.

Think:

```text
per customer
→ GROUP BY customer_id

only completed
→ CASE WHEN status = 'Completed'
```

```sql
SELECT
    customer_id,
    SUM(
        CASE
            WHEN status = 'Completed' THEN 1
            ELSE 0
        END
    ) AS completed_orders
FROM orders
GROUP BY customer_id;
```

---

# 68. Multiple categories in one query

Question:

> For each customer, show completed, cancelled, and pending order counts.

Use conditional aggregation:

```sql
SELECT
    customer_id,

    SUM(CASE
        WHEN status = 'Completed' THEN 1
        ELSE 0
    END) AS completed_orders,

    SUM(CASE
        WHEN status = 'Cancelled' THEN 1
        ELSE 0
    END) AS cancelled_orders,

    SUM(CASE
        WHEN status = 'Pending' THEN 1
        ELSE 0
    END) AS pending_orders

FROM orders
GROUP BY customer_id;
```

This pattern is incredibly useful.

---

# 69. "In each category, count condition X"

This is the general template:

```sql
SELECT
    grouping_column,
    SUM(
        CASE
            WHEN condition THEN 1
            ELSE 0
        END
    ) AS conditional_count
FROM table
GROUP BY grouping_column;
```

Examples:

```text
per department → GROUP BY department
per customer   → GROUP BY customer_id
per product    → GROUP BY product_id
per country    → GROUP BY country
```

---

# 70. "Count X where condition"

Sometimes:

```sql
COUNT(
    CASE WHEN condition THEN 1 END
)
```

Example:

```sql
SELECT
    department,
    COUNT(CASE WHEN salary > 70000 THEN 1 END) AS high_earners
FROM employees
GROUP BY department;
```

Equivalent conceptual pattern:

```sql
SUM(CASE WHEN salary > 70000 THEN 1 ELSE 0 END)
```

Know both.

---

# 71. "Sum only when..."

Question:

> Find each customer's completed-order revenue.

```sql
SELECT
    customer_id,
    SUM(
        CASE
            WHEN status = 'Completed' THEN amount
            ELSE 0
        END
    ) AS completed_revenue
FROM orders
GROUP BY customer_id;
```

Translation:

```text
each customer
→ GROUP BY customer_id

revenue
→ SUM(amount)

completed only
→ CASE WHEN status = 'Completed'
```

---

# 72. "Average only when..."

Same idea:

```sql
AVG(
    CASE
        WHEN status = 'Completed'
        THEN amount
    END
)
```

Because `AVG()` ignores NULL, rows failing the condition don't contribute.

---

# 73. "Find groups where..." → `HAVING`

Whenever you see:

> Find departments **where the number of employees is greater than 10**.

Think:

```sql
GROUP BY department
HAVING COUNT(*) > 10
```

---

# 74. "Groups with..."

This is another clue.

> Find customers with total spending above 50,000.

```sql
GROUP BY customer_id
HAVING SUM(amount) > 50000
```

> Find products with more than 100 orders.

```sql
GROUP BY product_id
HAVING COUNT(*) > 100
```

> Find departments with average salary above 80,000.

```sql
GROUP BY department
HAVING AVG(salary) > 80000
```

---

# 75. English → `WHERE` vs `HAVING`

Use this test:

### Is the condition about one original row?

```text
salary > 50000
status = 'Completed'
country = 'India'
age >= 18
```

→ `WHERE`

### Is the condition about a group?

```text
COUNT(*) > 5
SUM(amount) > 10000
AVG(salary) > 70000
MAX(score) >= 90
```

→ `HAVING`

---

# 76. Multiple `WHERE` conditions

Question:

> Find completed orders above 500 made in India.

```sql
SELECT *
FROM orders
WHERE status = 'Completed'
  AND amount > 500
  AND country = 'India';
```

Translation:

```text
completed
→ status = 'Completed'

above 500
→ amount > 500

India
→ country = 'India'

all conditions required
→ AND
```

---

# 77. "Either condition"

> Find orders that are either completed or pending.

```sql
WHERE status = 'Completed'
   OR status = 'Pending'
```

Or:

```sql
WHERE status IN ('Completed', 'Pending')
```

---

# 78. "Not completed"

```sql
WHERE status <> 'Completed'
```

But if NULL status matters, remember:

```sql
status <> 'Completed'
```

doesn't return NULL rows.

If the requirement is "anything except completed, including missing status", you may need:

```sql
WHERE status <> 'Completed'
   OR status IS NULL
```

---

# 79. "All", "any", "exists" — advanced translation

Some questions use:

> Find customers who have **at least one** completed order.

This can be:

```sql
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
      AND o.status = 'Completed'
)
```

Translation:

```text
at least one matching row
→ EXISTS
```

---

# 80. "No matching rows" → `NOT EXISTS`

> Find customers who have never placed an order.

```sql
SELECT c.*
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

Translation:

```text
never
→ NOT EXISTS
```

---

# 81. "At least one"

Common translations:

```text
at least one
→ EXISTS
```

or sometimes:

```sql
GROUP BY ...
HAVING COUNT(*) >= 1
```

`EXISTS` is often more natural when you don't actually need the count.

---

# 82. "Exactly N"

Question:

> Find customers who placed exactly 3 orders.

```sql
SELECT
    customer_id
FROM orders
GROUP BY customer_id
HAVING COUNT(*) = 3;
```

Translation:

```text
exactly 3
→ = 3
```

---

# 83. "At least N"

> Customers with at least 3 orders.

```sql
HAVING COUNT(*) >= 3
```

---

# 84. "At most N"

> Customers with at most 3 orders.

```sql
HAVING COUNT(*) <= 3
```

---

# 85. "Between N and M"

> Customers with between 3 and 10 orders.

```sql
HAVING COUNT(*) BETWEEN 3 AND 10
```

---

# 86. "More than one"

> Products bought more than once.

```sql
GROUP BY product_id
HAVING COUNT(*) > 1
```

---

# 87. "Only one"

> Customers who placed exactly one order.

```sql
GROUP BY customer_id
HAVING COUNT(*) = 1
```

---

# 88. "Most"

"Most" usually means:

> Calculate metric → sort descending → take top result.

Example:

> Which customer spent the most?

```sql
SELECT
    customer_id,
    SUM(amount) AS total_spent
FROM orders
GROUP BY customer_id
ORDER BY total_spent DESC
LIMIT 1;
```

---

# 89. "Least"

> Which customer spent the least?

```sql
SELECT
    customer_id,
    SUM(amount) AS total_spent
FROM orders
GROUP BY customer_id
ORDER BY total_spent ASC
LIMIT 1;
```

Again, if ties matter, use ranking instead of `LIMIT 1`.

---

# 90. "Top N per group" is different

Question:

> Find the top 3 highest-paid employees in each department.

This is **not**:

```sql
GROUP BY department
ORDER BY salary DESC
LIMIT 3
```

That gives only 3 employees overall.

Instead:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
) x
WHERE rn <= 3;
```

Translation:

```text
top 3
→ ROW_NUMBER()

in each department
→ PARTITION BY department

highest salary
→ ORDER BY salary DESC
```

---

# 91. "Rank"

If question says:

> Rank employees by salary within each department.

Think:

```sql
RANK() OVER (
    PARTITION BY department
    ORDER BY salary DESC
)
```

Important distinction:

```text
GROUP BY
→ collapse rows

PARTITION BY
→ window calculation while keeping rows
```

---

# 92. "Compare each row with group average"

Question:

> Find employees earning more than their department's average salary.

This is a window-function pattern:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        AVG(salary) OVER (
            PARTITION BY department
        ) AS dept_avg
    FROM employees e
) x
WHERE salary > dept_avg;
```

Translation:

```text
department average
→ AVG() OVER(PARTITION BY department)

employee salary > department average
→ compare row with window result
```

---

# 93. "Compared to overall average"

Question:

> Find employees earning above the company average.

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Translation:

```text
company average
→ SELECT AVG(salary)

employee salary greater than it
→ WHERE salary > (...)
```

---

# 94. "Compare group average to overall average"

Question:

> Find departments whose average salary is above the company average.

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > (
    SELECT AVG(salary)
    FROM employees
);
```

This is:

```text
department average
→ GROUP BY + AVG

company average
→ subquery

compare
→ HAVING
```

---

# 95. "Per X per Y"

This is a very important pattern.

> Find total sales per customer per month.

Translation:

```text
per customer
→ customer_id

per month
→ month

therefore:
→ GROUP BY customer_id, month
```

```sql
SELECT
    customer_id,
    YEAR(order_date) AS year,
    MONTH(order_date) AS month,
    SUM(amount) AS total_sales
FROM orders
GROUP BY
    customer_id,
    YEAR(order_date),
    MONTH(order_date);
```

---

# 96. "By X and Y"

Same concept.

> Total sales by region and product.

```sql
SELECT
    region,
    product_id,
    SUM(amount) AS total_sales
FROM sales
GROUP BY region, product_id;
```

One result row represents:

```text
(region, product)
```

---

# 97. "By X, Y, and Z"

> Average salary by country, city, and department.

```sql
SELECT
    country,
    city,
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY
    country,
    city,
    department;
```

The grouping key is:

```text
(country, city, department)
```

---

# 98. "Total for each X, but only Y"

Question:

> Find total completed sales for each customer.

There are two common ways.

### Method 1 — filter before grouping

```sql
SELECT
    customer_id,
    SUM(amount) AS completed_sales
FROM orders
WHERE status = 'Completed'
GROUP BY customer_id;
```

### Method 2 — conditional aggregation

```sql
SELECT
    customer_id,
    SUM(
        CASE
            WHEN status = 'Completed'
            THEN amount
            ELSE 0
        END
    ) AS completed_sales
FROM orders
GROUP BY customer_id;
```

Which one should you use?

If you **only care about completed rows**, `WHERE` is usually simpler.

If you want **multiple categories in the same result**, conditional aggregation is better.

---

# 99. Example: multiple categories require conditional aggregation

Question:

> For every customer, show total completed sales and total cancelled sales.

You can't simply put one status in `WHERE`, because you'd lose the other status.

Use:

```sql
SELECT
    customer_id,

    SUM(CASE
        WHEN status = 'Completed'
        THEN amount
        ELSE 0
    END) AS completed_sales,

    SUM(CASE
        WHEN status = 'Cancelled'
        THEN amount
        ELSE 0
    END) AS cancelled_sales

FROM orders
GROUP BY customer_id;
```

---

# 100. "Only customers who..." vs "show all customers"

This distinction is extremely important.

Question:

> Show all customers and their order counts, including customers with zero orders.

```sql
FROM customers c
LEFT JOIN orders o
```

Question:

> Show customers who have orders.

```sql
FROM customers c
JOIN orders o
```

Question:

> Show customers with no orders.

Either:

```sql
LEFT JOIN
WHERE o.id IS NULL
```

or:

```sql
NOT EXISTS
```

---

# 101. "For each customer, including zero"

This phrase is an automatic signal:

```text
for each customer
+
including zero
```

→

```sql
LEFT JOIN
```

Example:

```sql
SELECT
    c.customer_id,
    COUNT(o.order_id) AS orders
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id;
```

---

# 102. "Customers with at least one order"

Could use:

```sql
SELECT
    c.customer_id
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

Or:

```sql
SELECT
    c.customer_id
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id;
```

The first communicates **existence** more directly.

---

# 103. "Customers with exactly N different products"

This combines multiple translations.

> Find customers who purchased exactly 3 different products.

Translate:

```text
customers
→ GROUP BY customer_id

different products
→ COUNT(DISTINCT product_id)

exactly 3
→ = 3

group condition
→ HAVING
```

```sql
SELECT
    customer_id
FROM orders
GROUP BY customer_id
HAVING COUNT(DISTINCT product_id) = 3;
```

---

# 104. "Products sold to at least N customers"

> Find products purchased by at least 10 different customers.

```sql
SELECT
    product_id
FROM orders
GROUP BY product_id
HAVING COUNT(DISTINCT customer_id) >= 10;
```

Translation:

```text
different customers
→ COUNT(DISTINCT customer_id)

at least 10
→ >= 10
```

---

# 105. "Customer's first order"

If only the first date:

```sql
SELECT
    customer_id,
    MIN(order_date) AS first_order_date
FROM orders
GROUP BY customer_id;
```

Translation:

```text
first date
→ MIN(order_date)

per customer
→ GROUP BY customer_id
```

But if you need the **complete first order**, use ranking:

```sql
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY order_date
)
```

---

# 106. "Customer's last order"

Only date:

```sql
SELECT
    customer_id,
    MAX(order_date) AS last_order_date
FROM orders
GROUP BY customer_id;
```

Entire row:

```sql
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY order_date DESC
)
```

---

# 107. "Consecutive"

This word is a warning:

> Consecutive days
> consecutive logins
> consecutive purchases

Usually this is **not a simple `GROUP BY` question**.

Expect techniques such as:

- `LAG()`
- `LEAD()`
- `ROW_NUMBER()`
- date arithmetic
- gaps-and-islands

Example concept:

```sql
LAG(login_date) OVER (
    PARTITION BY user_id
    ORDER BY login_date
)
```

Translation:

```text
previous row
→ LAG()

next row
→ LEAD()
```

---

# 108. "Previous" → `LAG()`

> Find each employee's previous salary record.

Think:

```sql
LAG(salary) OVER (
    PARTITION BY employee_id
    ORDER BY effective_date
)
```

---

# 109. "Next" → `LEAD()`

> Find the next order date for each customer.

```sql
LEAD(order_date) OVER (
    PARTITION BY customer_id
    ORDER BY order_date
)
```

---

# 110. "Running total"

Question:

> Calculate running revenue by date.

Think:

```sql
SUM(amount) OVER (
    ORDER BY order_date
)
```

Translation:

```text
running total
→ SUM() OVER (ORDER BY ...)
```

---

# 111. "Running total per customer"

```sql
SUM(amount) OVER (
    PARTITION BY customer_id
    ORDER BY order_date
)
```

Translation:

```text
running
→ window function

total
→ SUM

per customer
→ PARTITION BY customer_id

chronologically
→ ORDER BY order_date
```

---

# 112. "Cumulative"

"Cumulative" usually signals a running calculation.

```text
cumulative total
→ SUM() OVER (ORDER BY ...)

cumulative count
→ COUNT() OVER (ORDER BY ...)
```

---

# 113. "Moving average"

Question:

> Calculate a 7-day moving average.

Think:

```sql
AVG(amount) OVER (
    ORDER BY date
    ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
)
```

This is an advanced window-function pattern.

---

# 114. "Each row compared with previous row"

Think:

```sql
LAG()
```

> Find days where sales increased from the previous day.

Conceptually:

```sql
sales - LAG(sales) OVER (ORDER BY date)
```

Then filter the result.

---

# 115. "Change from previous"

Translation:

```text
previous value
→ LAG()

current - previous
→ arithmetic
```

Example:

```sql
SELECT
    date,
    sales,
    sales - LAG(sales) OVER (
        ORDER BY date
    ) AS change
FROM daily_sales;
```

---

# 116. "Percentage change"

```text
(current - previous) / previous * 100
```

SQL:

```sql
100.0 *
(
    sales - LAG(sales) OVER (ORDER BY date)
)
/
NULLIF(
    LAG(sales) OVER (ORDER BY date),
    0
)
```

---

# 117. "Category / bucket / range"

Words like:

- bucket
- band
- segment
- range
- category

often suggest:

```sql
CASE
    WHEN ...
    THEN ...
END
```

Example:

> Group customers into low, medium, and high spenders.

```sql
CASE
    WHEN total_spent < 1000 THEN 'Low'
    WHEN total_spent < 5000 THEN 'Medium'
    ELSE 'High'
END
```

---

# 118. "Classify"

> Classify employees as junior, mid, or senior based on experience.

Think:

```sql
CASE
    WHEN experience < 2 THEN 'Junior'
    WHEN experience < 5 THEN 'Mid'
    ELSE 'Senior'
END
```

---

# 119. "If"

"If" in analytical SQL often means `CASE`.

```text
if condition
→ CASE WHEN condition THEN ...
```

Example:

> If salary is above 100,000, classify as high earner.

```sql
CASE
    WHEN salary > 100000 THEN 'High'
    ELSE 'Normal'
END
```

---

# 120. "Count each type"

If the question says:

> Count completed, cancelled, and pending orders by customer.

Think:

```text
GROUP BY customer
+
conditional aggregation
```

```sql
SUM(CASE WHEN status = 'Completed' THEN 1 ELSE 0 END)
```

This pattern is worth memorizing.

---

# 121. "Pivot-like" questions

If an interviewer asks:

> Show one row per customer with separate columns for completed and cancelled orders.

This is essentially a manual pivot:

```sql
SELECT
    customer_id,
    SUM(CASE WHEN status = 'Completed' THEN 1 ELSE 0 END) AS completed,
    SUM(CASE WHEN status = 'Cancelled' THEN 1 ELSE 0 END) AS cancelled
FROM orders
GROUP BY customer_id;
```

---

# 122. "Compare two categories"

Question:

> Find customers whose completed spending is greater than cancelled spending.

Translation:

```text
per customer
→ GROUP BY customer_id

completed spending
→ conditional SUM

cancelled spending
→ conditional SUM

compare groups
→ HAVING
```

```sql
SELECT
    customer_id
FROM orders
GROUP BY customer_id
HAVING
    SUM(CASE WHEN status = 'Completed' THEN amount ELSE 0 END)
    >
    SUM(CASE WHEN status = 'Cancelled' THEN amount ELSE 0 END);
```

This is a very useful advanced pattern.

---

# 123. "All conditions must be true"

Use:

```sql
AND
```

> Employees in IT who earn above 70,000 and joined after 2020.

```sql
WHERE department = 'IT'
  AND salary > 70000
  AND join_date >= '2021-01-01'
```

---

# 124. "Any of these conditions"

Use:

```sql
OR
```

or `IN` when comparing the same column.

```sql
WHERE department IN ('IT', 'HR', 'Sales')
```

---

# 125. "Neither ... nor"

Usually:

```sql
WHERE department NOT IN ('IT', 'HR')
```

or:

```sql
WHERE department <> 'IT'
  AND department <> 'HR'
```

---

# 126. "Does not contain"

```sql
WHERE name NOT LIKE '%test%'
```

---

# 127. "Starts with A and ends with Z"

```sql
WHERE name LIKE 'A%'
  AND name LIKE '%Z'
```

---

# 128. "Date after"

> Orders after January 1, 2026.

```sql
WHERE order_date > '2026-01-01'
```

---

# 129. "Since"

"Since January 1" generally means inclusive:

```sql
WHERE order_date >= '2026-01-01'
```

---

# 130. "Before"

```sql
WHERE order_date < '2026-01-01'
```

---

# 131. "On or before"

```sql
WHERE order_date <= '2026-01-01'
```

---

# 132. "Latest per group" — two meanings

This is a classic trap.

### Meaning 1: latest date

> Latest order date per customer.

```sql
MAX(order_date)
```

### Meaning 2: complete latest record

> Latest order per customer.

Use:

```sql
ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY order_date DESC
)
```

Always ask yourself:

> Does the interviewer want **the value** or **the row containing that value**?

---

# 133. "Maximum salary per department" vs "employee with maximum salary"

These are different.

### Maximum salary

```sql
SELECT
    department,
    MAX(salary)
FROM employees
GROUP BY department;
```

### Employee with maximum salary

Use ranking/subquery/join.

This distinction is extremely important.

---

# 134. "Average of averages" — dangerous phrase

Suppose:

| department | employee_count | avg_salary |
| ---------- | -------------: | ---------: |
| A          |            100 |     50,000 |
| B          |              2 |    100,000 |

If you calculate:

```text
(50000 + 100000) / 2
= 75000
```

you've given both departments equal weight.

But the true company average must consider employee counts:

```text
total salaries / total employees
```

Therefore, whenever you see:

> average of group averages

stop and determine what the business actually means.

---

# 135. "Average per group"

This is straightforward:

```sql
AVG(value)
GROUP BY group_column
```

But:

> Average of the monthly averages

requires two aggregation levels.

This is a useful advanced interview concept.

---

# 136. Two-level aggregation

Question:

> Find the average monthly revenue.

Step 1:

```text
orders
→ revenue per month
```

Step 2:

```text
monthly revenues
→ AVG()
```

Conceptually:

```sql
SELECT AVG(monthly_revenue)
FROM (
    SELECT
        month,
        SUM(amount) AS monthly_revenue
    FROM orders
    GROUP BY month
) x;
```

This is called **aggregation over aggregation**.

---

# 137. "Grouped result compared with another grouped result"

When one aggregation isn't enough, think:

```text
subquery
CTE
window function
```

Example:

> Find departments whose total salary is greater than the average department total salary.

First:

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

Then average those totals:

```sql
SELECT AVG(total_salary)
FROM (...grouped query...) x;
```

Then compare.

---

# 138. CTE makes this easier to understand

```sql
WITH department_totals AS (
    SELECT
        department,
        SUM(salary) AS total_salary
    FROM employees
    GROUP BY department
)
SELECT *
FROM department_totals
WHERE total_salary > (
    SELECT AVG(total_salary)
    FROM department_totals
);
```

English translation:

```text
1. Calculate total salary per department.
2. Calculate average of those department totals.
3. Keep departments above that average.
```

---

# 139. The "question decomposition" method

For difficult SQL questions, literally write the English sentence into pieces.

Example:

> Find the top 3 products in each category by total revenue among completed orders, considering only products sold to at least 10 unique customers.

Break it down:

```text
TOP 3
→ ROW_NUMBER / RANK

IN EACH CATEGORY
→ PARTITION BY category

BY TOTAL REVENUE
→ SUM(revenue)

COMPLETED ORDERS
→ WHERE status = 'Completed'

AT LEAST 10 UNIQUE CUSTOMERS
→ COUNT(DISTINCT customer_id) >= 10

PRODUCTS
→ GROUP BY product
```

Now build in stages.

### Stage 1

```sql
SELECT
    category,
    product_id,
    SUM(revenue) AS total_revenue,
    COUNT(DISTINCT customer_id) AS customers
FROM sales
WHERE status = 'Completed'
GROUP BY category, product_id;
```

### Stage 2

Filter products:

```sql
HAVING COUNT(DISTINCT customer_id) >= 10
```

### Stage 3

Rank within category:

```sql
RANK() OVER (
    PARTITION BY category
    ORDER BY total_revenue DESC
)
```

This decomposition method is far more useful than trying to write the whole query at once.

---

# 140. The 7-question SQL translation method

For almost every interview question, ask these seven questions:

### 1. What is my output?

```text
customer?
department?
product?
employee?
date?
```

### 2. What does one row represent?

```text
one customer
one department
one customer + month
one product + category
```

### 3. What is my metric?

```text
COUNT
SUM
AVG
MIN
MAX
DISTINCT COUNT
```

### 4. What rows should be considered?

```text
WHERE
```

### 5. What groups should survive?

```text
HAVING
```

### 6. Do I need ranking/order?

```text
ORDER BY
RANK
ROW_NUMBER
DENSE_RANK
```

### 7. Do I need all parent records?

```text
LEFT JOIN
```

---

# 141. The "output grain" concept

This is an extremely important professional SQL concept.

**Grain** means:

> What does one row of the result represent?

Example:

```text
employee table
→ one row = one employee
```

After:

```sql
GROUP BY department
```

the result becomes:

```text
one row = one department
```

After:

```sql
GROUP BY customer_id, month
```

the result becomes:

```text
one row = one customer-month combination
```

If you understand **grain**, many SQL questions become much easier.

---

# 142. Grain translation table

| English                  | Result grain      |
| ------------------------ | ----------------- |
| each customer            | customer          |
| each department          | department        |
| each product             | product           |
| each customer per month  | customer + month  |
| each product per country | product + country |
| each employee per month  | employee + month  |
| each department and role | department + role |

Then:

```text
grain
→ GROUP BY columns
```

---

# 143. A complete translation example

Question:

> Find each customer's total completed spending, number of different products purchased, and customers who spent more than 5,000.

Break it down:

```text
each customer
→ GROUP BY customer_id

total completed spending
→ SUM(CASE WHEN status = 'Completed' THEN amount ELSE 0 END)

different products
→ COUNT(DISTINCT product_id)

spent more than 5000
→ HAVING total > 5000
```

Query:

```sql
SELECT
    customer_id,

    SUM(
        CASE
            WHEN status = 'Completed'
            THEN amount
            ELSE 0
        END
    ) AS completed_spending,

    COUNT(DISTINCT product_id) AS unique_products

FROM orders

GROUP BY customer_id

HAVING
    SUM(
        CASE
            WHEN status = 'Completed'
            THEN amount
            ELSE 0
        END
    ) > 5000;
```

---

# 144. A harder translation

Question:

> Find departments that have at least 5 employees, an average salary above 60,000, and at least 3 employees earning more than 80,000.

Translate each phrase separately:

```text
departments
→ GROUP BY department

at least 5 employees
→ COUNT(*) >= 5

average salary above 60000
→ AVG(salary) > 60000

at least 3 employees earning > 80000
→ SUM(CASE WHEN salary > 80000 THEN 1 ELSE 0 END) >= 3
```

Final:

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary,

    SUM(
        CASE
            WHEN salary > 80000 THEN 1
            ELSE 0
        END
    ) AS high_earners

FROM employees

GROUP BY department

HAVING COUNT(*) >= 5
   AND AVG(salary) > 60000
   AND SUM(
        CASE
            WHEN salary > 80000 THEN 1
            ELSE 0
       END
   ) >= 3;
```

This is exactly how you should approach complicated interview questions.

---

# 145. English keywords that should trigger specific SQL patterns

## Grouping signals

```text
per
each
every
by
for each
one row per
```

→

```sql
GROUP BY
```

---

## Aggregate signals

```text
total
sum
```

→ `SUM()`

```text
number
how many
count
```

→ `COUNT()`

```text
average
mean
```

→ `AVG()`

```text
highest
maximum
largest
```

→ `MAX()`

```text
lowest
minimum
smallest
```

→ `MIN()`

---

## Distinct signals

```text
unique
different
distinct
```

→

```sql
DISTINCT
```

Especially:

```text
number of unique customers
```

→

```sql
COUNT(DISTINCT customer_id)
```

---

## Group-filter signals

```text
departments with...
customers having...
products with...
groups where...
at least N per...
more than N per...
```

→

```sql
HAVING
```

---

## Row-filter signals

```text
where salary...
orders with status...
employees in...
records after...
rows before...
```

→

```sql
WHERE
```

---

## Existence signals

```text
at least one
has an order
has a matching record
exists
```

→

```sql
EXISTS
```

---

## Non-existence signals

```text
no orders
never ordered
without a matching record
doesn't have
```

→

```sql
NOT EXISTS
```

or:

```sql
LEFT JOIN ... IS NULL
```

---

## Ranking signals

```text
top
bottom
highest ranked
lowest ranked
first
second
third
Nth
```

→ often:

```sql
ROW_NUMBER()
RANK()
DENSE_RANK()
```

---

## Sequential signals

```text
previous
next
before this row
after this row
change from previous
```

→

```sql
LAG()
LEAD()
```

---

## Running calculation signals

```text
running total
cumulative
running average
```

→

```sql
SUM() OVER (...)
AVG() OVER (...)
```

---

## Classification signals

```text
bucket
band
segment
classify
if
category based on
range
```

→

```sql
CASE
```

---

# 146. The complete English → SQL pipeline

Think of SQL interview questions like this:

```text
                    ENGLISH QUESTION
                           │
                           ▼
                  What is the output?
                           │
                           ▼
                     What is the grain?
                           │
                           ▼
                    Which table(s)?
                           │
                           ▼
                 Need JOIN or not?
                           │
                           ▼
                Filter individual rows?
                           │
                          WHERE
                           │
                           ▼
                       GROUP BY?
                           │
                           ▼
                    Which aggregate?
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
            COUNT         SUM          AVG
              │            │            │
              └────────────┼────────────┘
                           ▼
                   Filter groups?
                           │
                         HAVING
                           │
                           ▼
                     Need ranking?
                           │
                   ┌───────┴───────┐
                   ▼               ▼
                ORDER BY      WINDOW FUNCTION
                   │
                   ▼
                 LIMIT
```

---

# 147. The biggest distinction: `WHERE` vs `HAVING`

Memorize this table.

| Question asks about | Use      |
| ------------------- | -------- |
| employee salary     | `WHERE`  |
| order amount        | `WHERE`  |
| customer country    | `WHERE`  |
| order status        | `WHERE`  |
| number of orders    | `HAVING` |
| total spending      | `HAVING` |
| average salary      | `HAVING` |
| maximum purchase    | `HAVING` |

Example:

> Employees earning over 50K:

```sql
WHERE salary > 50000
```

> Departments with more than 5 employees:

```sql
HAVING COUNT(*) > 5
```

> Customers whose total spending exceeds 10K:

```sql
HAVING SUM(amount) > 10000
```

---

# 148. The biggest distinction: `GROUP BY` vs window functions

| English requirement           | Usually         |
| ----------------------------- | --------------- |
| one row per department        | `GROUP BY`      |
| department total              | `GROUP BY`      |
| department average            | `GROUP BY`      |
| employee + department average | window function |
| top 3 per department          | window function |
| rank employees                | window function |
| previous employee/order       | `LAG()`         |
| next employee/order           | `LEAD()`        |
| running total                 | window function |

Mental model:

```text
GROUP BY
→ reduces rows

WINDOW FUNCTION
→ preserves rows
```

---

# 149. The biggest distinction: `COUNT(*)` vs `COUNT(column)`

```text
COUNT(*)
→ count rows

COUNT(column)
→ count non-NULL values

COUNT(DISTINCT column)
→ count unique non-NULL values
```

Especially after `LEFT JOIN`:

```sql
COUNT(child.id)
```

is often what you want.

---

# 150. The biggest distinction: `MAX(value)` vs "row with maximum value"

```text
maximum salary
→ MAX(salary)

employee with maximum salary
→ ranking / join / subquery
```

Similarly:

```text
latest date
→ MAX(date)

latest order
→ ranking
```

---

# 151. The biggest distinction: "top overall" vs "top per group"

```text
top 5 overall
→ ORDER BY ... DESC LIMIT 5
```

But:

```text
top 5 in each department
→ ROW_NUMBER/RANK/DENSE_RANK
   PARTITION BY department
```

---

# 152. The biggest distinction: "unique" vs "group"

```text
unique departments
→ DISTINCT department
```

```text
average salary per department
→ GROUP BY department
```

---

# 153. The ultimate SQL interview checklist

Before writing a query, ask:

```text
□ What table contains the required data?

□ Do I need another table?

□ What does one output row represent?

□ What are my GROUP BY columns?

□ What aggregate do I need?

□ Do I need COUNT or COUNT(DISTINCT)?

□ Am I filtering rows?
   → WHERE

□ Am I filtering groups?
   → HAVING

□ Do I need NULL handling?

□ Do I need all parent records?
   → LEFT JOIN

□ Do I need conditional aggregation?
   → CASE

□ Do I need ranking?
   → RANK / DENSE_RANK / ROW_NUMBER

□ Do I need previous/next?
   → LAG / LEAD

□ Do I need a running calculation?
   → window function

□ Do I need sorting?
   → ORDER BY

□ Do I need only top N?
   → LIMIT / ranking

□ Are ties important?

□ Am I returning a value or the entire row?
```

---

# 154. Final one-page cheat sheet

## 🟦 GROUPING

```text
per X
each X
every X
by X
one row per X
```

↓

```sql
GROUP BY X
```

---

## 🟩 AGGREGATES

```text
total
sum
```

↓

```sql
SUM(x)
```

```text
number
how many
count
```

↓

```sql
COUNT(*)
```

```text
unique/different customers
```

↓

```sql
COUNT(DISTINCT customer_id)
```

```text
average
mean
```

↓

```sql
AVG(x)
```

```text
highest
maximum
```

↓

```sql
MAX(x)
```

```text
lowest
minimum
```

↓

```sql
MIN(x)
```

---

## 🟨 FILTERING

```text
individual row condition
```

↓

```sql
WHERE
```

```text
group condition
```

↓

```sql
HAVING
```

Remember:

```text
WHERE salary > 50000

HAVING AVG(salary) > 50000
```

---

## 🟧 COMPARISONS

```text
more than X       → > X
at least X        → >= X
less than X       → < X
at most X         → <= X
exactly X         → = X
not X             → <> X
between X and Y   → BETWEEN X AND Y
```

---

## 🟥 DUPLICATES

```text
duplicate
appears more than once
```

↓

```sql
GROUP BY column
HAVING COUNT(*) > 1
```

Exactly once:

```sql
GROUP BY column
HAVING COUNT(*) = 1
```

---

## 🟪 DISTINCT

```text
unique
different
distinct
```

↓

```sql
DISTINCT
```

Unique count:

```sql
COUNT(DISTINCT column)
```

---

## 🟫 CONDITIONAL AGGREGATION

```text
count only X
```

↓

```sql
SUM(CASE
    WHEN condition THEN 1
    ELSE 0
END)
```

```text
sum only X
```

↓

```sql
SUM(CASE
    WHEN condition THEN amount
    ELSE 0
END)
```

---

## 🟦 NULL

```text
missing
```

↓

```sql
IS NULL
```

```text
has a value
```

↓

```sql
IS NOT NULL
```

Never:

```sql
= NULL
```

---

## 🟩 JOIN CLUES

```text
include all customers
include zero orders
even if no matching record
```

↓

```sql
LEFT JOIN
```

```text
only matching records
```

↓

```sql
INNER JOIN
```

```text
no matching record
never ordered
without orders
```

↓

```sql
NOT EXISTS
```

or:

```sql
LEFT JOIN ... WHERE child.id IS NULL
```

---

## 🟨 TEXT

```text
contains X
→ LIKE '%X%'

starts with X
→ LIKE 'X%'

ends with X
→ LIKE '%X'

exactly X
→ = 'X'
```

---

## 🟧 MULTIPLE VALUES

```text
X, Y, Z
```

↓

```sql
IN (X, Y, Z)
```

Not:

```sql
NOT IN (...)
```

with care around NULLs.

---

## 🟥 TOP / BOTTOM

```text
top N overall
```

↓

```sql
ORDER BY metric DESC
LIMIT N
```

```text
bottom N overall
```

↓

```sql
ORDER BY metric ASC
LIMIT N
```

```text
top N per group
```

↓

```sql
ROW_NUMBER()
RANK()
DENSE_RANK()
OVER (PARTITION BY ...)
```

---

## 🟪 FIRST / LAST

```text
earliest date
→ MIN(date)

latest date
→ MAX(date)
```

But:

```text
earliest/latest row
→ ROW_NUMBER()
```

---

## 🟫 PREVIOUS / NEXT

```text
previous
→ LAG()

next
→ LEAD()
```

---

## 🟦 RUNNING

```text
running total
cumulative total
```

↓

```sql
SUM(x) OVER (
    ORDER BY date
)
```

Per group:

```sql
SUM(x) OVER (
    PARTITION BY customer_id
    ORDER BY date
)
```

---

## 🟩 CLASSIFICATION

```text
bucket
band
segment
classify
if
range
```

↓

```sql
CASE
    WHEN condition THEN result
    WHEN condition THEN result
    ELSE result
END
```

---

# 155. The final interview formula

When you get a SQL question, mentally say:

> **What is one output row?**

Then:

```text
ONE ROW PER X
        ↓
GROUP BY X
```

Then:

> **What am I calculating?**

```text
how many     → COUNT
unique how many → COUNT(DISTINCT)
total        → SUM
average      → AVG
highest      → MAX
lowest       → MIN
```

Then:

> **Am I filtering rows or groups?**

```text
rows
→ WHERE

groups
→ HAVING
```

Then:

> **Do I need conditional logic?**

```text
CASE
```

Then:

> **Do I need records that have no match?**

```text
LEFT JOIN
```

Then:

> **Do I need ranking/previous/next/running values?**

```text
ROW_NUMBER / RANK / DENSE_RANK
LAG / LEAD
SUM() OVER / AVG() OVER
```

And finally:

```text
ORDER BY
LIMIT
```

### The shortest possible mental map

```text
"per / each / by"
        ↓
    GROUP BY

"how many"
        ↓
      COUNT

"unique how many"
        ↓
COUNT(DISTINCT)

"total"
        ↓
       SUM

"average"
        ↓
       AVG

"highest / lowest"
        ↓
   MAX / MIN

"row condition"
        ↓
      WHERE

"group condition"
        ↓
     HAVING

"duplicate"
        ↓
GROUP BY + HAVING COUNT > 1

"only when"
        ↓
      CASE

"include zero"
        ↓
    LEFT JOIN

"top N per group"
        ↓
 WINDOW FUNCTION

"previous / next"
        ↓
   LAG / LEAD

"running"
        ↓
 WINDOW FUNCTION

"one row per X"
        ↓
   GROUP BY X
```

**If you train yourself to translate the English into this intermediate representation before writing SQL, you stop guessing queries and start systematically constructing them.**
