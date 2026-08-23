Absolutely. Let’s build **SQL `GROUP BY` from zero → interview level**, including mental models, execution order, patterns, `HAVING`, joins, `CASE`, NULLs, multiple columns, common mistakes, advanced patterns, and lots of examples with expected outputs.

---

# 1. What is `GROUP BY`?

`GROUP BY` is used when you want to **combine rows that have the same value(s)** and then perform calculations on each group.

Typical questions:

- How many employees are in each department?
- What is the average salary per department?
- What is the maximum salary in each department?
- How many orders did each customer make?
- What is the total revenue per month?
- Which departments have more than 5 employees?

The key idea:

> **`GROUP BY` turns individual rows into groups.**

---

# 2. Start with a simple table

Suppose we have:

### `employees`

| emp_id | name    | department | salary |
| -----: | ------- | ---------- | -----: |
|      1 | Alice   | IT         |  60000 |
|      2 | Bob     | IT         |  70000 |
|      3 | Charlie | HR         |  50000 |
|      4 | David   | HR         |  55000 |
|      5 | Eva     | Sales      |  45000 |
|      6 | Frank   | Sales      |  65000 |
|      7 | Grace   | IT         |  80000 |

Without `GROUP BY`:

```sql
SELECT *
FROM employees;
```

Output:

| emp_id | name    | department | salary |
| -----: | ------- | ---------- | -----: |
|      1 | Alice   | IT         |  60000 |
|      2 | Bob     | IT         |  70000 |
|      3 | Charlie | HR         |  50000 |
|      4 | David   | HR         |  55000 |
|      5 | Eva     | Sales      |  45000 |
|      6 | Frank   | Sales      |  65000 |
|      7 | Grace   | IT         |  80000 |

Now:

```sql
SELECT department
FROM employees
GROUP BY department;
```

Output:

| department |
| ---------- |
| IT         |
| HR         |
| Sales      |

The rows have been divided into three groups:

```text
IT
 ├── Alice
 ├── Bob
 └── Grace

HR
 ├── Charlie
 └── David

Sales
 ├── Eva
 └── Frank
```

---

# 3. Why do we need aggregate functions?

Usually, `GROUP BY` is used with **aggregate functions**.

Important aggregates:

| Function  | Meaning |
| --------- | ------- |
| `COUNT()` | Count   |
| `SUM()`   | Total   |
| `AVG()`   | Average |
| `MIN()`   | Minimum |
| `MAX()`   | Maximum |

For example:

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

Output:

| department | employee_count |
| ---------- | -------------: |
| HR         |              2 |
| IT         |              3 |
| Sales      |              2 |

Conceptually:

```text
GROUP BY department

HR    → 2 rows → COUNT = 2
IT    → 3 rows → COUNT = 3
Sales → 2 rows → COUNT = 2
```

---

# 4. The fundamental pattern

Memorize this:

```sql
SELECT
    grouping_column,
    AGGREGATE_FUNCTION(column)
FROM table
GROUP BY grouping_column;
```

Examples:

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

```sql
SELECT department, SUM(salary)
FROM employees
GROUP BY department;
```

```sql
SELECT department, AVG(salary)
FROM employees
GROUP BY department;
```

```sql
SELECT department, MAX(salary)
FROM employees
GROUP BY department;
```

```sql
SELECT department, MIN(salary)
FROM employees
GROUP BY department;
```

---

# 5. `GROUP BY` + `COUNT()`

One of the most important interview patterns.

### Question

> How many employees are in each department?

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

Output:

| department | employee_count |
| ---------- | -------------: |
| HR         |              2 |
| IT         |              3 |
| Sales      |              2 |

---

# 6. `COUNT(*)` vs `COUNT(column)`

This is **very important for interviews**.

Suppose:

### `employees`

| emp_id | name    | department | manager_id |
| -----: | ------- | ---------- | ---------: |
|      1 | Alice   | IT         |         10 |
|      2 | Bob     | IT         |         10 |
|      3 | Charlie | HR         |       NULL |
|      4 | David   | HR         |         20 |

### `COUNT(*)`

```sql
SELECT
    department,
    COUNT(*) AS total
FROM employees
GROUP BY department;
```

Output:

| department | total |
| ---------- | ----: |
| HR         |     2 |
| IT         |     2 |

`COUNT(*)` counts **rows**.

### `COUNT(manager_id)`

```sql
SELECT
    department,
    COUNT(manager_id) AS managers_assigned
FROM employees
GROUP BY department;
```

Output:

| department | managers_assigned |
| ---------- | ----------------: |
| HR         |                 1 |
| IT         |                 2 |

Why?

`COUNT(column)` ignores `NULL`.

```text
HR:

Charlie → NULL
David   → 20

COUNT(manager_id) = 1
```

### Interview rule

Remember:

```text
COUNT(*)        → counts rows
COUNT(column)   → counts non-NULL values
COUNT(DISTINCT column) → counts unique non-NULL values
```

---

# 7. `SUM()`

Question:

> What is the total salary for each department?

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

Output:

| department | total_salary |
| ---------- | -----------: |
| HR         |       105000 |
| IT         |       210000 |
| Sales      |       110000 |

Calculation:

```text
IT:
60000 + 70000 + 80000 = 210000

HR:
50000 + 55000 = 105000

Sales:
45000 + 65000 = 110000
```

---

# 8. `AVG()`

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

Output:

| department | average_salary |
| ---------- | -------------: |
| HR         |          52500 |
| IT         |          70000 |
| Sales      |          55000 |

---

# 9. `MIN()` and `MAX()`

### Lowest salary per department

```sql
SELECT
    department,
    MIN(salary) AS minimum_salary
FROM employees
GROUP BY department;
```

Output:

| department | minimum_salary |
| ---------- | -------------: |
| HR         |          50000 |
| IT         |          60000 |
| Sales      |          45000 |

### Highest salary per department

```sql
SELECT
    department,
    MAX(salary) AS maximum_salary
FROM employees
GROUP BY department;
```

Output:

| department | maximum_salary |
| ---------- | -------------: |
| HR         |          55000 |
| IT         |          80000 |
| Sales      |          65000 |

---

# 10. Multiple aggregate functions

You can use several aggregates together.

```sql
SELECT
    department,
    COUNT(*) AS employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS avg_salary,
    MIN(salary) AS min_salary,
    MAX(salary) AS max_salary
FROM employees
GROUP BY department;
```

Output:

| department | employees | total_salary | avg_salary | min_salary | max_salary |
| ---------- | --------: | -----------: | ---------: | ---------: | ---------: |
| HR         |         2 |       105000 |      52500 |      50000 |      55000 |
| IT         |         3 |       210000 |      70000 |      60000 |      80000 |
| Sales      |         2 |       110000 |      55000 |      45000 |      65000 |

This is an extremely common interview pattern.

---

# 11. The most important rule of `GROUP BY`

Suppose:

```sql
SELECT department, name, COUNT(*)
FROM employees
GROUP BY department;
```

This is generally **invalid**.

Why?

Because after grouping by `department`, SQL has one result row per department.

But which `name` should SQL return for IT?

```text
Alice?
Bob?
Grace?
```

There isn't one correct answer.

Therefore:

> Every selected column must generally either be in `GROUP BY` or be aggregated.

Correct:

```sql
SELECT
    department,
    COUNT(*)
FROM employees
GROUP BY department;
```

Correct:

```sql
SELECT
    department,
    MAX(salary)
FROM employees
GROUP BY department;
```

Correct:

```sql
SELECT
    department,
    AVG(salary)
FROM employees
GROUP BY department;
```

Incorrect:

```sql
SELECT
    department,
    name,
    AVG(salary)
FROM employees
GROUP BY department;
```

---

# 12. `GROUP BY` multiple columns

You can group by more than one column.

Suppose:

### `sales`

| sale_id | country | city     | amount |
| ------: | ------- | -------- | -----: |
|       1 | India   | Pune     |    100 |
|       2 | India   | Pune     |    200 |
|       3 | India   | Mumbai   |    300 |
|       4 | India   | Mumbai   |    100 |
|       5 | USA     | New York |    500 |
|       6 | USA     | New York |    200 |

Question:

> Total sales by country and city?

```sql
SELECT
    country,
    city,
    SUM(amount) AS total_sales
FROM sales
GROUP BY country, city;
```

Output:

| country | city     | total_sales |
| ------- | -------- | ----------: |
| India   | Mumbai   |         400 |
| India   | Pune     |         300 |
| USA     | New York |         700 |

Think of:

```sql
GROUP BY country, city
```

as grouping by the **combination**:

```text
(India, Pune)
(India, Mumbai)
(USA, New York)
```

Not:

```text
country separately
+
city separately
```

---

# 13. Grouping hierarchy

Imagine:

```text
Country
   ↓
City
   ↓
Department
```

For example:

```sql
GROUP BY country, city, department
```

means:

> Create one group for every unique `(country, city, department)` combination.

This pattern appears constantly in business/data-analysis SQL.

---

# 14. `GROUP BY` with expressions

You don't necessarily have to group by a physical column.

You can group by an expression.

For example:

```sql
SELECT
    YEAR(order_date) AS order_year,
    SUM(amount) AS revenue
FROM orders
GROUP BY YEAR(order_date);
```

Output:

| order_year | revenue |
| ---------: | ------: |
|       2024 |  500000 |
|       2025 |  750000 |
|       2026 |  900000 |

Depending on the database, date functions differ:

### MySQL

```sql
YEAR(order_date)
```

### PostgreSQL

```sql
EXTRACT(YEAR FROM order_date)
```

### SQL Server

```sql
YEAR(order_date)
```

---

# 15. Grouping by month

Suppose:

| order_id | order_date | amount |
| -------: | ---------- | -----: |
|        1 | 2026-01-05 |    100 |
|        2 | 2026-01-10 |    200 |
|        3 | 2026-02-03 |    300 |
|        4 | 2026-02-20 |    400 |

MySQL:

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

Output:

| year | month | revenue |
| ---: | ----: | ------: |
| 2026 |     1 |     300 |
| 2026 |     2 |     700 |

---

# 16. `WHERE` vs `GROUP BY`

This is a **major interview topic**.

`WHERE` filters **rows before grouping**.

Example:

> Calculate average salary for IT employees only.

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
WHERE department = 'IT'
GROUP BY department;
```

Processing:

```text
employees
   ↓
WHERE department = 'IT'
   ↓
IT rows
   ↓
GROUP BY department
   ↓
AVG(salary)
```

---

# 17. `HAVING`

`HAVING` filters **groups after grouping**.

Suppose:

> Find departments with more than 2 employees.

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

Output:

| department | employee_count |
| ---------- | -------------: |
| IT         |              3 |

Why not:

```sql
WHERE COUNT(*) > 2
```

?

Because `COUNT(*)` is calculated **after rows have been grouped**.

So:

```text
WHERE  → filter rows
GROUP BY → create groups
HAVING → filter groups
```

---

# 18. `WHERE` + `GROUP BY` + `HAVING`

This is one of the most useful patterns.

Question:

> Among employees earning more than 50,000, find departments having at least 2 such employees.

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
WHERE salary > 50000
GROUP BY department
HAVING COUNT(*) >= 2;
```

Execution:

### Original

```text
Alice    IT      60000
Bob      IT      70000
Charlie  HR      50000
David    HR      55000
Eva      Sales   45000
Frank    Sales   65000
Grace    IT      80000
```

### After `WHERE salary > 50000`

```text
Alice    IT      60000
Bob      IT      70000
David    HR      55000
Frank    Sales   65000
Grace    IT      80000
```

### Group

```text
IT    → Alice, Bob, Grace → 3
HR    → David             → 1
Sales → Frank             → 1
```

### HAVING

```text
COUNT >= 2
```

Result:

| department | employee_count |
| ---------- | -------------: |
| IT         |              3 |

---

# 19. SQL logical execution order

You should know this extremely well for interviews.

A simplified logical order is:

```text
FROM
  ↓
JOIN
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
DISTINCT
  ↓
ORDER BY
  ↓
LIMIT
```

For example:

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
WHERE salary > 40000
GROUP BY department
HAVING AVG(salary) > 60000
ORDER BY avg_salary DESC;
```

Conceptually:

```text
1. FROM employees

2. WHERE salary > 40000

3. GROUP BY department

4. Calculate AVG(salary)

5. HAVING AVG(salary) > 60000

6. SELECT department, avg_salary

7. ORDER BY avg_salary DESC
```

---

# 20. `GROUP BY` + `ORDER BY`

Question:

> Show departments from highest average salary to lowest.

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC;
```

Output:

| department | avg_salary |
| ---------- | ---------: |
| IT         |      70000 |
| Sales      |      55000 |
| HR         |      52500 |

You can also write:

```sql
ORDER BY AVG(salary) DESC;
```

---

# 21. `GROUP BY` + `HAVING` + `ORDER BY`

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING COUNT(*) >= 2
ORDER BY avg_salary DESC;
```

This is a very common interview query.

---

# 22. `COUNT(DISTINCT ...)`

Suppose:

### `orders`

| order_id | customer_id | product_id |
| -------: | ----------: | ---------: |
|        1 |         101 |          A |
|        2 |         101 |          B |
|        3 |         101 |          A |
|        4 |         102 |          A |
|        5 |         103 |          C |

Question:

> How many unique products did each customer purchase?

```sql
SELECT
    customer_id,
    COUNT(DISTINCT product_id) AS unique_products
FROM orders
GROUP BY customer_id;
```

Output:

| customer_id | unique_products |
| ----------: | --------------: |
|         101 |               2 |
|         102 |               1 |
|         103 |               1 |

Customer 101 purchased:

```text
A
B
A
```

Distinct:

```text
A
B
```

Therefore:

```text
COUNT(DISTINCT product_id) = 2
```

---

# 23. Very common interview pattern: duplicate detection

Question:

> Find duplicate email addresses.

Table:

### `users`

|  id | email                             |
| --: | --------------------------------- |
|   1 | [a@gmail.com](mailto:a@gmail.com) |
|   2 | [b@gmail.com](mailto:b@gmail.com) |
|   3 | [a@gmail.com](mailto:a@gmail.com) |
|   4 | [c@gmail.com](mailto:c@gmail.com) |
|   5 | [b@gmail.com](mailto:b@gmail.com) |

Query:

```sql
SELECT
    email,
    COUNT(*) AS occurrences
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

Output:

| email                             | occurrences |
| --------------------------------- | ----------: |
| [a@gmail.com](mailto:a@gmail.com) |           2 |
| [b@gmail.com](mailto:b@gmail.com) |           2 |

### Pattern to memorize

```sql
GROUP BY column
HAVING COUNT(*) > 1
```

This is the classic **duplicate finder**.

---

# 24. Find values occurring exactly once

Opposite:

```sql
SELECT
    email
FROM users
GROUP BY email
HAVING COUNT(*) = 1;
```

Output:

| email                             |
| --------------------------------- |
| [c@gmail.com](mailto:c@gmail.com) |

---

# 25. Find the department with the highest average salary

First calculate:

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

Then sort:

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC
LIMIT 1;
```

Output:

| department | avg_salary |
| ---------- | ---------: |
| IT         |      70000 |

This is a common solution.

---

# 26. Find departments where maximum salary exceeds 70,000

```sql
SELECT
    department,
    MAX(salary) AS max_salary
FROM employees
GROUP BY department
HAVING MAX(salary) > 70000;
```

Output:

| department | max_salary |
| ---------- | ---------: |
| IT         |      80000 |

Notice:

```sql
MAX(salary) > 70000
```

belongs in `HAVING`, because `MAX()` is an aggregate.

---

# 27. Conditional aggregation

This is **extremely important** for SQL interviews.

Suppose:

| employee | department | salary |
| -------- | ---------- | -----: |
| Alice    | IT         |  60000 |
| Bob      | IT         |  90000 |
| Charlie  | HR         |  50000 |
| David    | HR         |  80000 |

Question:

> Count employees earning more than 70,000 per department.

One approach:

```sql
SELECT
    department,
    SUM(
        CASE
            WHEN salary > 70000 THEN 1
            ELSE 0
        END
    ) AS high_salary_employees
FROM employees
GROUP BY department;
```

Output:

| department | high_salary_employees |
| ---------- | --------------------: |
| HR         |                     1 |
| IT         |                     1 |

---

# 28. Multiple conditional counts

You can calculate several metrics at once.

```sql
SELECT
    department,

    SUM(CASE
        WHEN salary >= 70000 THEN 1
        ELSE 0
    END) AS high_salary,

    SUM(CASE
        WHEN salary < 70000 THEN 1
        ELSE 0
    END) AS low_salary

FROM employees
GROUP BY department;
```

Output:

| department | high_salary | low_salary |
| ---------- | ----------: | ---------: |
| HR         |           1 |          1 |
| IT         |           1 |          1 |

This technique is called:

> **Conditional aggregation**

You will see it constantly in interviews.

---

# 29. Conditional `COUNT`

Depending on database:

```sql
COUNT(CASE WHEN salary > 70000 THEN 1 END)
```

Example:

```sql
SELECT
    department,
    COUNT(CASE WHEN salary > 70000 THEN 1 END) AS high_earners
FROM employees
GROUP BY department;
```

Why does this work?

Because:

```sql
COUNT(...)
```

ignores `NULL`.

So:

```text
salary = 60000 → NULL
salary = 90000 → 1
salary = 50000 → NULL
```

Only the `1` gets counted.

---

# 30. `SUM(CASE...)` vs `COUNT(CASE...)`

Both can solve conditional counting.

### Method 1

```sql
SUM(
    CASE
        WHEN salary > 70000 THEN 1
        ELSE 0
    END
)
```

### Method 2

```sql
COUNT(
    CASE
        WHEN salary > 70000 THEN 1
    END
)
```

Both are useful.

For interviews, I recommend being comfortable with **both**.

---

# 31. Conditional percentage

Question:

> What percentage of employees in each department earn more than 70,000?

A common pattern:

```sql
SELECT
    department,
    100.0 *
    SUM(CASE WHEN salary > 70000 THEN 1 ELSE 0 END)
    / COUNT(*) AS percentage_high_salary
FROM employees
GROUP BY department;
```

The `100.0` is often useful to avoid integer division in databases where integer division would otherwise occur.

---

# 32. `GROUP BY` after a `JOIN`

This is another major interview area.

Suppose:

### `customers`

| customer_id | customer_name |
| ----------: | ------------- |
|           1 | Alice         |
|           2 | Bob           |
|           3 | Charlie       |

### `orders`

| order_id | customer_id | amount |
| -------: | ----------: | -----: |
|      101 |           1 |    100 |
|      102 |           1 |    200 |
|      103 |           2 |    300 |
|      104 |           2 |    100 |

Question:

> Calculate total spending for each customer.

```sql
SELECT
    c.customer_id,
    c.customer_name,
    SUM(o.amount) AS total_spent
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id,
    c.customer_name;
```

Output:

| customer_id | customer_name | total_spent |
| ----------: | ------------- | ----------: |
|           1 | Alice         |         300 |
|           2 | Bob           |         400 |

Charlie doesn't appear because an `INNER JOIN` removes customers without orders.

---

# 33. Include customers with zero orders

Use `LEFT JOIN`.

```sql
SELECT
    c.customer_id,
    c.customer_name,
    COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id,
    c.customer_name;
```

Output:

| customer_id | customer_name | order_count |
| ----------: | ------------- | ----------: |
|           1 | Alice         |           2 |
|           2 | Bob           |           2 |
|           3 | Charlie       |           0 |

### Important interview trap

Don't write:

```sql
COUNT(*)
```

if you want customers with zero matching orders.

Because `LEFT JOIN` still produces a row for Charlie.

So:

```sql
COUNT(*)
```

would count that generated row.

Instead:

```sql
COUNT(o.order_id)
```

counts only non-NULL order IDs.

---

# 34. `LEFT JOIN` + `GROUP BY` pattern

Memorize:

```sql
SELECT
    parent.id,
    COUNT(child.id)
FROM parent
LEFT JOIN child
    ON parent.id = child.parent_id
GROUP BY parent.id;
```

This pattern solves:

- customers with number of orders
- departments with number of employees
- authors with number of books
- users with number of posts
- products with number of reviews

---

# 35. Find customers with more than 3 orders

```sql
SELECT
    customer_id,
    COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 3;
```

This is a classic.

---

# 36. Find customers whose total spending exceeds 10,000

```sql
SELECT
    customer_id,
    SUM(amount) AS total_spent
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 10000;
```

Notice the difference:

```sql
WHERE amount > 10000
```

means:

> Remove individual orders below/equal to 10,000.

But:

```sql
HAVING SUM(amount) > 10000
```

means:

> Keep customers whose combined orders exceed 10,000.

Huge conceptual difference.

---

# 37. Find products purchased by at least 100 customers

```sql
SELECT
    product_id,
    COUNT(DISTINCT customer_id) AS customers
FROM orders
GROUP BY product_id
HAVING COUNT(DISTINCT customer_id) >= 100;
```

This combines:

```text
GROUP BY
+
COUNT(DISTINCT)
+
HAVING
```

Very common interview pattern.

---

# 38. Find departments with average salary > company average

This introduces a **subquery + GROUP BY**.

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) >
       (SELECT AVG(salary)
        FROM employees);
```

Conceptually:

```text
Company average
       ↓
     62,857

Department averages
       ↓
HR     52,500
IT     70,000
Sales  55,000

Keep > 62,857

IT
```

Output:

| department | avg_salary |
| ---------- | ---------: |
| IT         |      70000 |

---

# 39. Find departments whose total salary is highest

One approach:

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
ORDER BY total_salary DESC
LIMIT 1;
```

Another interview-style approach uses a subquery if you need more control.

---

# 40. Find the highest salary in each department

Be careful.

This:

```sql
SELECT
    department,
    MAX(salary)
FROM employees
GROUP BY department;
```

gives you the **salary**, but not the employee.

Output:

| department | max_salary |
| ---------- | ---------: |
| HR         |      55000 |
| IT         |      80000 |
| Sales      |      65000 |

If the question is:

> Find the employee who earns the highest salary in each department.

That's a different problem.

You need another technique, such as:

- window functions
- correlated subquery
- join with aggregate result

For example:

```sql
SELECT e.*
FROM employees e
JOIN (
    SELECT
        department,
        MAX(salary) AS max_salary
    FROM employees
    GROUP BY department
) x
    ON e.department = x.department
   AND e.salary = x.max_salary;
```

This handles ties too.

---

# 41. `GROUP BY` vs window functions

This distinction is **critical**.

### `GROUP BY`

Reduces rows.

```sql
SELECT
    department,
    AVG(salary)
FROM employees
GROUP BY department;
```

Output:

```text
HR     52500
IT     70000
Sales  55000
```

Three rows.

### Window function

Keeps individual rows.

```sql
SELECT
    name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS dept_avg
FROM employees;
```

Output conceptually:

| name    | department | salary | dept_avg |
| ------- | ---------- | -----: | -------: |
| Alice   | IT         |  60000 |    70000 |
| Bob     | IT         |  70000 |    70000 |
| Grace   | IT         |  80000 |    70000 |
| Charlie | HR         |  50000 |    52500 |
| David   | HR         |  55000 |    52500 |
| Eva     | Sales      |  45000 |    55000 |
| Frank   | Sales      |  65000 |    55000 |

### Mental model

```text
GROUP BY
→ collapse rows

WINDOW FUNCTION
→ calculate across rows without collapsing them
```

---

# 42. The biggest `GROUP BY` interview patterns

Learn these patterns by heart.

## Pattern 1 — Count per category

```sql
SELECT category, COUNT(*)
FROM table
GROUP BY category;
```

---

## Pattern 2 — Sum per category

```sql
SELECT category, SUM(amount)
FROM table
GROUP BY category;
```

---

## Pattern 3 — Average per category

```sql
SELECT category, AVG(amount)
FROM table
GROUP BY category;
```

---

## Pattern 4 — Duplicate detection

```sql
SELECT column, COUNT(*)
FROM table
GROUP BY column
HAVING COUNT(*) > 1;
```

---

## Pattern 5 — Exactly one occurrence

```sql
SELECT column, COUNT(*)
FROM table
GROUP BY column
HAVING COUNT(*) = 1;
```

---

## Pattern 6 — Minimum/maximum per category

```sql
SELECT
    category,
    MAX(value)
FROM table
GROUP BY category;
```

---

## Pattern 7 — Filter rows before grouping

```sql
SELECT category, COUNT(*)
FROM table
WHERE condition
GROUP BY category;
```

---

## Pattern 8 — Filter groups

```sql
SELECT category, COUNT(*)
FROM table
GROUP BY category
HAVING COUNT(*) > 5;
```

---

## Pattern 9 — Multiple grouping columns

```sql
SELECT
    category,
    region,
    COUNT(*)
FROM table
GROUP BY category, region;
```

---

## Pattern 10 — Unique values per group

```sql
SELECT
    category,
    COUNT(DISTINCT user_id)
FROM table
GROUP BY category;
```

---

## Pattern 11 — Conditional count

```sql
SELECT
    category,
    SUM(
        CASE
            WHEN condition THEN 1
            ELSE 0
        END
    )
FROM table
GROUP BY category;
```

---

## Pattern 12 — Parent with zero children

```sql
SELECT
    p.id,
    COUNT(c.id)
FROM parent p
LEFT JOIN child c
    ON p.id = c.parent_id
GROUP BY p.id;
```

---

# 43. A powerful interview framework

When you see a SQL question, ask:

### Question 1

**What should one output row represent?**

For example:

> "Number of orders per customer"

One output row = **one customer**.

Therefore:

```sql
GROUP BY customer_id
```

---

### Question 2

**What calculation is required?**

"Number of orders"

```sql
COUNT(*)
```

"Total amount"

```sql
SUM(amount)
```

"Average amount"

```sql
AVG(amount)
```

"Highest amount"

```sql
MAX(amount)
```

---

### Question 3

**Do I filter rows or groups?**

Filter individual rows:

```sql
WHERE
```

Filter aggregated groups:

```sql
HAVING
```

---

### Question 4

**Do I need unique values?**

Use:

```sql
COUNT(DISTINCT ...)
```

---

### Question 5

**Do I need records with no matching child rows?**

Use:

```sql
LEFT JOIN
```

---

# 44. Translate English → SQL

This is one of the best ways to prepare for interviews.

### "per"

Usually means:

```sql
GROUP BY
```

Examples:

> Sales **per** customer

```sql
GROUP BY customer_id
```

> Average salary **per** department

```sql
GROUP BY department
```

> Orders **per** month

```sql
GROUP BY month
```

---

### "each"

Usually means grouping.

> Total revenue for each product.

```sql
GROUP BY product_id
```

---

### "more than X"

Ask:

**Are we filtering rows or groups?**

> Employees with salary > 50,000

```sql
WHERE salary > 50000
```

> Departments with more than 5 employees

```sql
HAVING COUNT(*) > 5
```

---

### "unique"

Usually:

```sql
DISTINCT
```

---

### "at least"

Usually:

```sql
>=
```

> Customers with at least 5 orders

```sql
HAVING COUNT(*) >= 5
```

---

### "duplicate"

Usually:

```sql
GROUP BY
HAVING COUNT(*) > 1
```

---

# 45. Common mistakes

## Mistake 1: Using `WHERE` with aggregate

Wrong:

```sql
SELECT department, COUNT(*)
FROM employees
WHERE COUNT(*) > 2
GROUP BY department;
```

Correct:

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

---

# 46. Mistake 2: Selecting a non-grouped column

Wrong:

```sql
SELECT
    department,
    name,
    COUNT(*)
FROM employees
GROUP BY department;
```

Correct:

```sql
SELECT
    department,
    COUNT(*)
FROM employees
GROUP BY department;
```

Or if you genuinely need some aggregate:

```sql
SELECT
    department,
    MAX(salary)
FROM employees
GROUP BY department;
```

---

# 47. Mistake 3: Confusing `WHERE` and `HAVING`

Question:

> Find departments with more than 3 employees whose salary is above 50,000.

Correct:

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
WHERE salary > 50000
GROUP BY department
HAVING COUNT(*) > 3;
```

Think:

```text
salary > 50000
       ↓
     WHERE

COUNT(*) > 3
       ↓
    HAVING
```

---

# 48. Mistake 4: Using `COUNT(*)` after `LEFT JOIN`

Suppose:

```sql
customers
LEFT JOIN orders
```

If you want customers with zero orders:

Potentially wrong:

```sql
COUNT(*)
```

Better:

```sql
COUNT(o.order_id)
```

because unmatched rows have:

```text
o.order_id = NULL
```

and `COUNT(o.order_id)` ignores NULL.

---

# 49. Mistake 5: Forgetting `DISTINCT`

Question:

> Number of unique customers who purchased each product.

Wrong:

```sql
COUNT(customer_id)
```

If the same customer bought the product multiple times, they are counted multiple times.

Correct:

```sql
COUNT(DISTINCT customer_id)
```

---

# 50. Mistake 6: `AVG()` and NULL

Suppose salaries are:

```text
50000
60000
NULL
```

Then:

```sql
AVG(salary)
```

calculates:

```text
(50000 + 60000) / 2
= 55000
```

It does **not** treat NULL as zero.

This is important.

If you explicitly want NULL to behave as zero:

```sql
AVG(COALESCE(salary, 0))
```

But be careful: that changes the business meaning.

---

# 51. `GROUP BY` and NULL

Suppose:

| employee | department |
| -------- | ---------- |
| Alice    | IT         |
| Bob      | IT         |
| Charlie  | NULL       |
| David    | HR         |

Query:

```sql
SELECT
    department,
    COUNT(*)
FROM employees
GROUP BY department;
```

Output conceptually:

| department | count |
| ---------- | ----: |
| NULL       |     1 |
| HR         |     1 |
| IT         |     2 |

All NULL values belong to the same group.

---

# 52. Grouping with `CASE`

You can create categories dynamically.

Suppose:

| employee | salary |
| -------- | -----: |
| Alice    |  40000 |
| Bob      |  60000 |
| Charlie  |  90000 |
| David    | 120000 |

Create salary bands:

```sql
SELECT
    CASE
        WHEN salary < 50000 THEN 'Low'
        WHEN salary < 100000 THEN 'Medium'
        ELSE 'High'
    END AS salary_band,
    COUNT(*) AS employees
FROM employees
GROUP BY
    CASE
        WHEN salary < 50000 THEN 'Low'
        WHEN salary < 100000 THEN 'Medium'
        ELSE 'High'
    END;
```

Output:

| salary_band | employees |
| ----------- | --------: |
| Low         |         1 |
| Medium      |         2 |
| High        |         1 |

This pattern is useful for:

- age groups
- salary bands
- price ranges
- customer segments
- order sizes

---

# 53. Grouping by age ranges

```sql
SELECT
    CASE
        WHEN age < 18 THEN 'Under 18'
        WHEN age BETWEEN 18 AND 30 THEN '18-30'
        WHEN age BETWEEN 31 AND 50 THEN '31-50'
        ELSE '50+'
    END AS age_group,
    COUNT(*) AS people
FROM users
GROUP BY
    CASE
        WHEN age < 18 THEN 'Under 18'
        WHEN age BETWEEN 18 AND 30 THEN '18-30'
        WHEN age BETWEEN 31 AND 50 THEN '31-50'
        ELSE '50+'
    END;
```

---

# 54. A very common interview problem

### Table

`orders`

| order_id | customer_id | amount | status    |
| -------: | ----------: | -----: | --------- |
|        1 |         101 |    100 | Completed |
|        2 |         101 |    200 | Cancelled |
|        3 |         101 |    300 | Completed |
|        4 |         102 |    500 | Completed |
|        5 |         102 |    100 | Completed |
|        6 |         103 |     50 | Cancelled |

Question:

> Find customers whose completed-order revenue exceeds 300.

Solution:

```sql
SELECT
    customer_id,
    SUM(amount) AS completed_revenue
FROM orders
WHERE status = 'Completed'
GROUP BY customer_id
HAVING SUM(amount) > 300;
```

Output:

| customer_id | completed_revenue |
| ----------: | ----------------: |
|         102 |               600 |

Notice the sequence:

```text
WHERE status = Completed
        ↓
GROUP BY customer
        ↓
SUM(amount)
        ↓
HAVING SUM(amount) > 300
```

---

# 55. Another interview problem: active users

Table:

### `logins`

| user_id | login_date |
| ------: | ---------- |
|       1 | 2026-01-01 |
|       1 | 2026-01-02 |
|       2 | 2026-01-01 |
|       3 | 2026-01-05 |
|       3 | 2026-01-06 |

Question:

> Find users who logged in at least twice.

```sql
SELECT
    user_id,
    COUNT(*) AS login_count
FROM logins
GROUP BY user_id
HAVING COUNT(*) >= 2;
```

Output:

| user_id | login_count |
| ------: | ----------: |
|       1 |           2 |
|       3 |           2 |

---

# 56. Another: products sold in multiple orders

```sql
SELECT
    product_id,
    COUNT(*) AS order_count
FROM order_items
GROUP BY product_id
HAVING COUNT(*) > 1;
```

If the question instead says:

> Products purchased by at least 5 different customers

then:

```sql
SELECT
    product_id,
    COUNT(DISTINCT customer_id) AS customer_count
FROM orders
GROUP BY product_id
HAVING COUNT(DISTINCT customer_id) >= 5;
```

The phrase **"different customers"** is your signal for `DISTINCT`.

---

# 57. Grouping after filtering

Suppose:

> Find the average salary of employees in IT earning more than 60,000.

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
WHERE department = 'IT'
  AND salary > 60000
GROUP BY department;
```

Rows considered:

```text
Bob    70000
Grace  80000
```

Result:

```text
75000
```

---

# 58. `HAVING` without `WHERE`

Totally valid:

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```

You don't need `WHERE`.

---

# 59. `GROUP BY` without aggregate

Technically:

```sql
SELECT department
FROM employees
GROUP BY department;
```

works.

But conceptually, if you simply need unique values, this is usually clearer:

```sql
SELECT DISTINCT department
FROM employees;
```

So:

```text
GROUP BY → usually for aggregation
DISTINCT → usually for removing duplicates
```

There are cases where they can be interchangeable, but they communicate different intent.

---

# 60. `DISTINCT` vs `GROUP BY`

### Distinct departments

```sql
SELECT DISTINCT department
FROM employees;
```

### Group departments

```sql
SELECT department
FROM employees
GROUP BY department;
```

Both can return:

| department |
| ---------- |
| HR         |
| IT         |
| Sales      |

But:

```sql
GROUP BY
```

becomes powerful when you need:

```sql
COUNT()
SUM()
AVG()
MIN()
MAX()
HAVING
```

---

# 61. `GROUP BY` with `JOIN` and `HAVING`

Question:

> Find customers who have placed at least 3 orders and spent more than $500.

```sql
SELECT
    c.customer_id,
    c.customer_name,
    COUNT(o.order_id) AS order_count,
    SUM(o.amount) AS total_spent
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id,
    c.customer_name
HAVING COUNT(o.order_id) >= 3
   AND SUM(o.amount) > 500;
```

This is a very realistic interview query.

---

# 62. The "one row per X" technique

When an interviewer says:

> "Return one row per..."

immediately think:

```sql
GROUP BY X
```

Examples:

> One row per customer

```sql
GROUP BY customer_id
```

> One row per department

```sql
GROUP BY department
```

> One row per product and month

```sql
GROUP BY product_id, month
```

> One row per country and city

```sql
GROUP BY country, city
```

This is probably the single most useful mental shortcut.

---

# 63. The "per" rule

When you see:

```text
per customer
per department
per product
per month
per country
per category
```

think:

```sql
GROUP BY
```

Then ask:

> What aggregate should I calculate?

```text
number       → COUNT
total        → SUM
average      → AVG
highest      → MAX
lowest       → MIN
unique count → COUNT(DISTINCT ...)
```

---

# 64. The "more than" rule

If:

```text
more than X rows
at least X rows
fewer than X rows
```

and you're talking about a **group**, use:

```sql
HAVING
```

Example:

> Departments with more than 10 employees.

```sql
GROUP BY department
HAVING COUNT(*) > 10
```

---

# 65. The "before grouping" rule

If the condition describes individual records:

```text
salary > 50000
status = 'Completed'
country = 'India'
date >= '2026-01-01'
```

use:

```sql
WHERE
```

Example:

```sql
WHERE status = 'Completed'
```

Then group.

---

# 66. The "after grouping" rule

If the condition describes the aggregate:

```text
count > 5
average > 100
total > 10000
maximum > 500
```

use:

```sql
HAVING
```

Example:

```sql
HAVING SUM(amount) > 10000
```

---

# 67. Interview cheat sheet

| English              | SQL                                    |
| -------------------- | -------------------------------------- |
| each department      | `GROUP BY department`                  |
| per customer         | `GROUP BY customer_id`                 |
| total                | `SUM()`                                |
| number of rows       | `COUNT(*)`                             |
| unique number        | `COUNT(DISTINCT x)`                    |
| average              | `AVG()`                                |
| highest              | `MAX()`                                |
| lowest               | `MIN()`                                |
| filter rows          | `WHERE`                                |
| filter groups        | `HAVING`                               |
| duplicates           | `GROUP BY ... HAVING COUNT(*) > 1`     |
| at least 5           | `HAVING COUNT(*) >= 5`                 |
| unique customers     | `COUNT(DISTINCT customer_id)`          |
| include zero matches | `LEFT JOIN`                            |
| conditional count    | `SUM(CASE WHEN ... THEN 1 ELSE 0 END)` |
| one row per X        | `GROUP BY X`                           |

---

# 68. A complete interview example

Suppose:

### `orders`

| order_id | customer_id | product | amount | status    |
| -------: | ----------: | ------- | -----: | --------- |
|        1 |         101 | Laptop  |   1000 | Completed |
|        2 |         101 | Mouse   |    100 | Completed |
|        3 |         101 | Laptop  |   1000 | Cancelled |
|        4 |         102 | Phone   |    800 | Completed |
|        5 |         102 | Case    |    100 | Completed |
|        6 |         102 | Phone   |    800 | Completed |
|        7 |         103 | Mouse   |    100 | Cancelled |

Question:

> Find customers who have at least 2 completed orders and spent more than 500 on completed orders.

Step 1: only completed orders:

```sql
WHERE status = 'Completed'
```

Step 2: one result per customer:

```sql
GROUP BY customer_id
```

Step 3: count orders:

```sql
COUNT(*)
```

Step 4: calculate revenue:

```sql
SUM(amount)
```

Step 5: filter groups:

```sql
HAVING COUNT(*) >= 2
   AND SUM(amount) > 500
```

Final:

```sql
SELECT
    customer_id,
    COUNT(*) AS completed_orders,
    SUM(amount) AS total_spent
FROM orders
WHERE status = 'Completed'
GROUP BY customer_id
HAVING COUNT(*) >= 2
   AND SUM(amount) > 500;
```

Output:

| customer_id | completed_orders | total_spent |
| ----------: | ---------------: | ----------: |
|         101 |                2 |        1100 |
|         102 |                3 |        1700 |

That's the entire thought process an interviewer wants to see.

---

# 69. Advanced mental model

Think of SQL as progressively transforming a table:

```text
RAW TABLE
   ↓
FROM / JOIN
   ↓
ROW FILTERING
   ↓
WHERE
   ↓
GROUPING
   ↓
GROUP BY
   ↓
AGGREGATION
   ↓
COUNT / SUM / AVG / MIN / MAX
   ↓
GROUP FILTERING
   ↓
HAVING
   ↓
SORT
   ↓
ORDER BY
   ↓
TOP N
   ↓
LIMIT
```

For example:

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM employees
WHERE salary >= 50000
GROUP BY department
HAVING COUNT(*) >= 2
ORDER BY avg_salary DESC;
```

Think:

```text
employees
    ↓
salary >= 50000
    ↓
rows remaining
    ↓
group by department
    ↓
count + average
    ↓
keep groups with count >= 2
    ↓
sort by average salary
```

---

# 70. The 10 patterns I would master for interviews

If you're preparing specifically for **SQL interviews**, don't just memorize syntax. Master these ten patterns:

### Pattern 1

**Count per group**

```sql
SELECT x, COUNT(*)
FROM t
GROUP BY x;
```

### Pattern 2

**Aggregate per group**

```sql
SELECT x, SUM(value)
FROM t
GROUP BY x;
```

### Pattern 3

**Duplicate detection**

```sql
SELECT x, COUNT(*)
FROM t
GROUP BY x
HAVING COUNT(*) > 1;
```

### Pattern 4

**Group filtering**

```sql
GROUP BY x
HAVING aggregate_condition;
```

### Pattern 5

**Row filtering + group filtering**

```sql
WHERE row_condition
GROUP BY x
HAVING aggregate_condition;
```

### Pattern 6

**Distinct counting**

```sql
COUNT(DISTINCT user_id)
```

### Pattern 7

**Conditional aggregation**

```sql
SUM(
    CASE WHEN condition THEN 1 ELSE 0 END
)
```

### Pattern 8

**Parent + zero children**

```sql
LEFT JOIN
GROUP BY
COUNT(child.id)
```

### Pattern 9

**Multiple dimensions**

```sql
GROUP BY country, city, category
```

### Pattern 10

**Aggregate result compared with another aggregate**

```sql
HAVING AVG(x) >
       (SELECT AVG(x) FROM table)
```

---

# 71. Final mental map

When you encounter a SQL interview question, mentally convert it like this:

```text
"per / each"
       ↓
    GROUP BY

"number of"
       ↓
    COUNT

"unique number of"
       ↓
COUNT(DISTINCT)

"total"
       ↓
     SUM

"average"
       ↓
     AVG

"highest"
       ↓
     MAX

"lowest"
       ↓
     MIN

"where individual row satisfies..."
       ↓
     WHERE

"groups having..."
       ↓
    HAVING

"duplicate"
       ↓
GROUP BY + HAVING COUNT(*) > 1

"include those with no matching records"
       ↓
    LEFT JOIN

"conditional count"
       ↓
SUM(CASE WHEN ... THEN 1 ELSE 0 END)

"one row per customer"
       ↓
GROUP BY customer_id
```

### The single most important distinction

```text
WHERE
  ↓
filters ROWS

GROUP BY
  ↓
creates GROUPS

HAVING
  ↓
filters GROUPS
```

If you truly understand that sequence, most `GROUP BY` questions become much easier.

If you're preparing for interviews, the next step after this is to practice **20–30 `GROUP BY` problems in increasing difficulty**, especially problems combining **`JOIN + GROUP BY + HAVING + CASE + COUNT(DISTINCT)`**, because that's where interview questions usually become challenging.
