Absolutely. Here’s the **from-scratch → interview → coding-question** master note for **SQL JOINS**, with the same “translate English question → think in SQL → write query” approach.

# SQL JOINS — Complete Guide

### MySQL + PostgreSQL | Interviews + Coding Questions

Joins are one of the **most important SQL topics**. If you become comfortable with joins, a huge portion of SQL interview questions becomes much easier.

---

# 1. First: What does a JOIN actually mean?

Suppose we have two tables:

### `employees`

| employee_id | name    | dept_id | salary |
| ----------: | ------- | ------: | -----: |
|           1 | Alice   |      10 |  70000 |
|           2 | Bob     |      20 |  60000 |
|           3 | Charlie |      10 |  80000 |
|           4 | David   |      30 |  50000 |
|           5 | Eve     |    NULL |  55000 |

### `departments`

| dept_id | dept_name   |
| ------: | ----------- |
|      10 | Engineering |
|      20 | Sales       |
|      30 | HR          |
|      40 | Finance     |

The relationship is:

```text
employees.dept_id
       ↓
departments.dept_id
```

A JOIN lets us combine information from these tables.

For example:

> "Show each employee's name and department name."

The English translation is:

```text
employee information
        +
department information
        ↓
match using dept_id
```

SQL:

```sql
SELECT
    e.name,
    d.dept_name
FROM employees e
JOIN departments d
    ON e.dept_id = d.dept_id;
```

Output:

| name    | dept_name   |
| ------- | ----------- |
| Alice   | Engineering |
| Bob     | Sales       |
| Charlie | Engineering |
| David   | HR          |

Notice:

**Eve disappears.**

Why?

Because Eve has:

```text
dept_id = NULL
```

and we're using an `INNER JOIN`.

---

# 2. The fundamental JOIN mental model

Before writing any JOIN, ask:

### Question 1 — What tables do I need?

Example:

> Find employees and their department names.

Need:

```text
employees
departments
```

### Question 2 — How are they connected?

```text
employees.dept_id = departments.dept_id
```

### Question 3 — What should happen when there is NO match?

This determines the JOIN type.

```text
Need only matching rows?
        ↓
INNER JOIN

Need all rows from left table?
        ↓
LEFT JOIN

Need all rows from right table?
        ↓
RIGHT JOIN

Need everything from both?
        ↓
FULL OUTER JOIN

Need every possible combination?
        ↓
CROSS JOIN
```

This is the single most useful JOIN decision tree.

---

# 3. JOIN syntax

The general pattern:

```sql
SELECT columns
FROM table1
JOIN table2
    ON table1.key = table2.key;
```

Better with aliases:

```sql
SELECT
    e.name,
    d.dept_name
FROM employees AS e
JOIN departments AS d
    ON e.dept_id = d.dept_id;
```

Think:

```text
FROM
  ↓
Start with table

JOIN
  ↓
Bring another table

ON
  ↓
Tell SQL how rows are related

SELECT
  ↓
Choose what to display
```

---

# 4. INNER JOIN

## Meaning

> Give me rows where a match exists in BOTH tables.

Syntax:

```sql
SELECT ...
FROM A
INNER JOIN B
    ON A.key = B.key;
```

`INNER` is optional:

```sql
JOIN
```

means:

```sql
INNER JOIN
```

in normal SQL usage.

---

# 5. INNER JOIN example

### Employees

|  id | name    | dept_id |
| --: | ------- | ------: |
|   1 | Alice   |      10 |
|   2 | Bob     |      20 |
|   3 | Charlie |      10 |
|   4 | David   |      30 |
|   5 | Eve     |    NULL |

### Departments

| dept_id | department  |
| ------: | ----------- |
|      10 | Engineering |
|      20 | Sales       |
|      30 | HR          |
|      40 | Finance     |

Query:

```sql
SELECT
    e.name,
    d.department
FROM employees e
INNER JOIN departments d
    ON e.dept_id = d.dept_id;
```

Output:

| name    | department  |
| ------- | ----------- |
| Alice   | Engineering |
| Bob     | Sales       |
| Charlie | Engineering |
| David   | HR          |

Excluded:

```text
Eve
Finance
```

because neither has a matching row on the other side.

---

# 6. English → INNER JOIN translation

Whenever you see:

> employees **who have** a department

Think:

```text
matching employees
+
matching departments
```

→ `INNER JOIN`

---

### Common phrases

| English wording              | Likely JOIN |
| ---------------------------- | ----------- |
| employees with departments   | INNER JOIN  |
| customers who placed orders  | INNER JOIN  |
| products that have reviews   | INNER JOIN  |
| students enrolled in courses | INNER JOIN  |
| users who made purchases     | INNER JOIN  |

Example:

> Find customers who placed at least one order.

```sql
SELECT
    c.customer_id,
    c.name,
    o.order_id
FROM customers c
INNER JOIN orders o
    ON c.customer_id = o.customer_id;
```

---

# 7. LEFT JOIN — the most important JOIN

If you learn only one JOIN deeply:

**Learn LEFT JOIN.**

Meaning:

> Keep ALL rows from the left table, even if there is no match on the right.

Syntax:

```sql
SELECT ...
FROM A
LEFT JOIN B
    ON A.key = B.key;
```

Think:

```text
LEFT table = protected

RIGHT table = optional
```

---

# 8. LEFT JOIN example

Employees:

|  id | name    | dept_id |
| --: | ------- | ------: |
|   1 | Alice   |      10 |
|   2 | Bob     |      20 |
|   3 | Charlie |      10 |
|   4 | David   |      30 |
|   5 | Eve     |    NULL |

Departments:

| dept_id | department  |
| ------: | ----------- |
|      10 | Engineering |
|      20 | Sales       |
|      30 | HR          |
|      40 | Finance     |

Query:

```sql
SELECT
    e.name,
    d.department
FROM employees e
LEFT JOIN departments d
    ON e.dept_id = d.dept_id;
```

Output:

| name    | department  |
| ------- | ----------- |
| Alice   | Engineering |
| Bob     | Sales       |
| Charlie | Engineering |
| David   | HR          |
| Eve     | NULL        |

Eve stays.

Why?

Because employees is the **left table**.

---

# 9. The golden LEFT JOIN rule

Memorize this:

> **LEFT JOIN = keep everything from the left.**

For:

```sql
FROM employees e
LEFT JOIN departments d
```

think:

```text
employees → KEEP ALL
departments → match if possible
```

---

# 10. LEFT JOIN is extremely important for "missing" questions

Consider:

> Find employees who don't belong to any department.

This is a classic interview question.

Start:

```sql
FROM employees e
LEFT JOIN departments d
    ON e.dept_id = d.dept_id
```

Because we want to preserve every employee.

Then:

```sql
WHERE d.dept_id IS NULL
```

Full query:

```sql
SELECT
    e.employee_id,
    e.name
FROM employees e
LEFT JOIN departments d
    ON e.dept_id = d.dept_id
WHERE d.dept_id IS NULL;
```

Output:

| employee_id | name |
| ----------: | ---- |
|           5 | Eve  |

---

# 11. The "find missing" pattern

This is one of the most important SQL patterns.

```sql
SELECT ...
FROM A
LEFT JOIN B
    ON A.key = B.key
WHERE B.key IS NULL;
```

Meaning:

> Give me rows from A for which no matching row exists in B.

Examples:

### Customers who never ordered

```sql
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

### Products never sold

```sql
SELECT p.product_id, p.name
FROM products p
LEFT JOIN order_items oi
    ON p.product_id = oi.product_id
WHERE oi.product_id IS NULL;
```

### Employees without projects

```sql
SELECT e.employee_id, e.name
FROM employees e
LEFT JOIN employee_projects ep
    ON e.employee_id = ep.employee_id
WHERE ep.employee_id IS NULL;
```

This pattern appears **everywhere in interviews**.

---

# 12. RIGHT JOIN

`RIGHT JOIN` is essentially the mirror image of `LEFT JOIN`.

```sql
SELECT ...
FROM A
RIGHT JOIN B
    ON A.key = B.key;
```

Meaning:

> Keep ALL rows from the right table.

Example:

```sql
SELECT
    e.name,
    d.department
FROM employees e
RIGHT JOIN departments d
    ON e.dept_id = d.dept_id;
```

Output:

| name    | department  |
| ------- | ----------- |
| Alice   | Engineering |
| Charlie | Engineering |
| Bob     | Sales       |
| David   | HR          |
| NULL    | Finance     |

Finance remains even though no employee belongs to it.

---

# 13. LEFT JOIN vs RIGHT JOIN

These are equivalent logically if you swap table positions.

This:

```sql
FROM employees e
LEFT JOIN departments d
    ON e.dept_id = d.dept_id
```

is equivalent to:

```sql
FROM departments d
RIGHT JOIN employees e
    ON e.dept_id = d.dept_id
```

Because:

```text
LEFT JOIN
    ↓
preserve left table

RIGHT JOIN
    ↓
preserve right table
```

### Interview tip

Most developers prefer `LEFT JOIN` because it is easier to read.

Instead of:

```sql
A RIGHT JOIN B
```

you can usually rewrite:

```sql
B LEFT JOIN A
```

---

# 14. FULL OUTER JOIN

Meaning:

> Keep everything from BOTH tables.

Conceptually:

```text
matching rows
+
unmatched rows from A
+
unmatched rows from B
```

Syntax in PostgreSQL:

```sql
SELECT ...
FROM A
FULL OUTER JOIN B
    ON A.key = B.key;
```

Example:

```sql
SELECT
    e.name,
    d.department
FROM employees e
FULL OUTER JOIN departments d
    ON e.dept_id = d.dept_id;
```

Output:

| name    | department  |
| ------- | ----------- |
| Alice   | Engineering |
| Charlie | Engineering |
| Bob     | Sales       |
| David   | HR          |
| Eve     | NULL        |
| NULL    | Finance     |

So:

```text
Alice     → matched
Bob       → matched
Charlie   → matched
David     → matched
Eve       → employee only
Finance   → department only
```

---

# 15. VERY IMPORTANT: MySQL and FULL OUTER JOIN

PostgreSQL supports:

```sql
FULL OUTER JOIN
```

MySQL does **not** support native `FULL OUTER JOIN`.

In MySQL, commonly emulate it using `UNION`:

```sql
SELECT
    e.name,
    d.department
FROM employees e
LEFT JOIN departments d
    ON e.dept_id = d.dept_id

UNION

SELECT
    e.name,
    d.department
FROM employees e
RIGHT JOIN departments d
    ON e.dept_id = d.dept_id;
```

This gives the conceptual FULL OUTER JOIN result.

---

# 16. CROSS JOIN

This is completely different.

A `CROSS JOIN` creates:

> Every possible combination of rows.

Suppose:

### colors

| color |
| ----- |
| Red   |
| Blue  |

### sizes

| size |
| ---- |
| S    |
| M    |
| L    |

Query:

```sql
SELECT
    c.color,
    s.size
FROM colors c
CROSS JOIN sizes s;
```

Output:

| color | size |
| ----- | ---- |
| Red   | S    |
| Red   | M    |
| Red   | L    |
| Blue  | S    |
| Blue  | M    |
| Blue  | L    |

2 colors × 3 sizes =

```text
6 rows
```

---

# 17. CROSS JOIN formula

If:

```text
A has m rows
B has n rows
```

Then:

```text
CROSS JOIN result = m × n
```

Example:

```text
5 employees
4 shifts
```

Result:

```text
5 × 4 = 20 combinations
```

---

# 18. When would you actually use CROSS JOIN?

Useful for generating combinations.

For example:

> Generate every combination of product and store.

```sql
SELECT
    p.product_name,
    s.store_name
FROM products p
CROSS JOIN stores s;
```

Another classic:

> Generate every date × employee combination.

```sql
SELECT
    d.date,
    e.employee_id
FROM calendar d
CROSS JOIN employees e;
```

Then you can LEFT JOIN actual attendance onto it.

---

# 19. Self JOIN

A self join means:

> Join a table to itself.

Very common for hierarchical data.

Example:

### employees

| employee_id | name    | manager_id |
| ----------: | ------- | ---------: |
|           1 | Alice   |       NULL |
|           2 | Bob     |          1 |
|           3 | Charlie |          1 |
|           4 | David   |          2 |
|           5 | Eve     |          2 |

Here:

```text
Alice
 ├── Bob
 │    ├── David
 │    └── Eve
 └── Charlie
```

`manager_id` points back to `employee_id`.

---

# 20. Self JOIN syntax

We use two aliases:

```sql
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
```

Think:

```text
e = employee
m = manager
```

Query:

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

Output:

| employee | manager |
| -------- | ------- |
| Alice    | NULL    |
| Bob      | Alice   |
| Charlie  | Alice   |
| David    | Bob     |
| Eve      | Bob     |

---

# 21. Why LEFT JOIN for self joins?

Because Alice has no manager.

If we use:

```sql
INNER JOIN
```

Alice disappears.

If we want everyone:

```sql
LEFT JOIN
```

---

# 22. Common self-join interview question

> Find employees who earn more than their manager.

Data:

| employee | salary | manager |
| -------- | -----: | ------- |
| Alice    | 100000 | NULL    |
| Bob      |  80000 | Alice   |
| Charlie  | 110000 | Alice   |
| David    |  90000 | Bob     |

Query:

```sql
SELECT
    e.name AS employee,
    e.salary AS employee_salary,
    m.name AS manager,
    m.salary AS manager_salary
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

Output:

| employee | employee_salary | manager | manager_salary |
| -------- | --------------: | ------- | -------------: |
| Charlie  |          110000 | Alice   |         100000 |
| David    |           90000 | Bob     |          80000 |

This is a **classic interview pattern**.

---

# 23. The most important JOIN pattern: 3 tables

Real-world SQL usually involves more than two tables.

Suppose:

### customers

| customer_id | name  |
| ----------: | ----- |
|           1 | Alice |
|           2 | Bob   |

### orders

| order_id | customer_id | order_date |
| -------: | ----------: | ---------- |
|      101 |           1 | 2026-01-10 |
|      102 |           1 | 2026-01-15 |
|      103 |           2 | 2026-01-20 |

### order_items

| order_id | product_id | quantity |
| -------: | ---------: | -------: |
|      101 |         10 |        2 |
|      101 |         20 |        1 |
|      102 |         10 |        3 |
|      103 |         30 |        2 |

Question:

> Show customer name, order ID, product ID and quantity.

Translation:

```text
customer
   ↓
order
   ↓
order_item
```

Query:

```sql
SELECT
    c.name,
    o.order_id,
    oi.product_id,
    oi.quantity
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN order_items oi
    ON o.order_id = oi.order_id;
```

Output:

| name  | order_id | product_id | quantity |
| ----- | -------: | ---------: | -------: |
| Alice |      101 |         10 |        2 |
| Alice |      101 |         20 |        1 |
| Alice |      102 |         10 |        3 |
| Bob   |      103 |         30 |        2 |

---

# 24. How to think about multi-table joins

Don't think:

> "How do I write a complicated JOIN?"

Instead draw the relationship:

```text
customers
    |
    | customer_id
    ↓
orders
    |
    | order_id
    ↓
order_items
    |
    | product_id
    ↓
products
```

Then join one connection at a time.

```sql
FROM customers c

JOIN orders o
  ON c.customer_id = o.customer_id

JOIN order_items oi
  ON o.order_id = oi.order_id

JOIN products p
  ON oi.product_id = p.product_id
```

This mental model makes complex queries much easier.

---

# 25. JOIN + WHERE

Example:

> Find employees in Engineering.

```sql
SELECT
    e.name
FROM employees e
JOIN departments d
    ON e.dept_id = d.dept_id
WHERE d.department = 'Engineering';
```

Logical thinking:

```text
1. Connect employees to departments
2. Keep Engineering
3. Display employee names
```

---

# 26. JOIN + GROUP BY

Extremely common interview pattern.

Question:

> Find the number of employees in each department.

```sql
SELECT
    d.department,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.department;
```

Why `LEFT JOIN`?

Because we want departments with **zero employees** too.

Output:

| department  | employee_count |
| ----------- | -------------: |
| Engineering |              2 |
| Sales       |              1 |
| HR          |              1 |
| Finance     |              0 |

---

# 27. Why COUNT(column) matters with LEFT JOIN

Consider:

```sql
COUNT(*)
```

versus:

```sql
COUNT(e.employee_id)
```

With:

```sql
LEFT JOIN
```

a department with no employee still produces a row:

```text
Finance | NULL
```

Therefore:

```sql
COUNT(*)
```

counts that row.

But:

```sql
COUNT(e.employee_id)
```

doesn't count NULL.

So:

```sql
COUNT(e.employee_id)
```

is generally the correct choice when counting matching records after a LEFT JOIN.

---

# 28. JOIN + HAVING

Question:

> Find departments having more than 5 employees.

```sql
SELECT
    d.department,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.department
HAVING COUNT(e.employee_id) > 5;
```

Remember:

```text
WHERE
 ↓
filters rows

GROUP BY
 ↓
creates groups

HAVING
 ↓
filters groups
```

---

# 29. JOIN + SUM

Question:

> Find total sales per customer.

Tables:

```text
customers
orders
order_items
```

Suppose:

### order_items

| order_id | product  | amount |
| -------: | -------- | -----: |
|      101 | Laptop   |   1000 |
|      101 | Mouse    |     50 |
|      102 | Keyboard |    100 |
|      103 | Monitor  |    500 |

Query:

```sql
SELECT
    c.customer_id,
    c.name,
    SUM(oi.amount) AS total_spent
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN order_items oi
    ON o.order_id = oi.order_id
GROUP BY
    c.customer_id,
    c.name;
```

---

# 30. JOIN + ORDER BY

Question:

> Show departments ordered by employee count descending.

```sql
SELECT
    d.department,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.department
ORDER BY employee_count DESC;
```

---

# 31. JOIN + DISTINCT

Question:

> Find customers who bought a laptop.

```sql
SELECT DISTINCT
    c.customer_id,
    c.name
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN order_items oi
    ON o.order_id = oi.order_id
JOIN products p
    ON oi.product_id = p.product_id
WHERE p.name = 'Laptop';
```

Why `DISTINCT`?

A customer might buy the laptop multiple times.

Without `DISTINCT`:

```text
Alice
Alice
Alice
```

With:

```sql
DISTINCT
```

we get:

```text
Alice
```

---

# 32. A HUGE interview concept: JOIN multiplication

This is one of the biggest sources of SQL mistakes.

Suppose:

### customers

| customer_id | name  |
| ----------: | ----- |
|           1 | Alice |

### orders

| order_id | customer_id |
| -------: | ----------: |
|      101 |           1 |
|      102 |           1 |

### order_items

| order_id | product  |
| -------: | -------- |
|      101 | Laptop   |
|      101 | Mouse    |
|      102 | Keyboard |

After joining:

```text
Alice
  ↓
2 orders
  ↓
3 order items
```

Result:

| customer | order | product  |
| -------- | ----: | -------- |
| Alice    |   101 | Laptop   |
| Alice    |   101 | Mouse    |
| Alice    |   102 | Keyboard |

This is correct.

But if you weren't expecting multiple rows per customer, you may think your JOIN is "duplicating" data.

It isn't.

The JOIN is exposing the underlying one-to-many relationship.

---

# 33. The JOIN cardinality concept

You should know these:

```text
1 → 1
1 → many
many → 1
many → many
```

Example:

### One customer → many orders

```text
customer
   |
   +---- order 1
   +---- order 2
   +---- order 3
```

So joining customer to orders produces multiple rows for the customer.

---

# 34. Many-to-many relationships

Suppose:

```text
students
courses
```

A student can take many courses.

A course can have many students.

You usually need a bridge table:

```text
students
   |
   ↓
student_courses
   ↑
   |
courses
```

### student_courses

| student_id | course_id |
| ---------: | --------: |
|          1 |       101 |
|          1 |       102 |
|          2 |       101 |
|          3 |       103 |

Query:

> Find all courses taken by Alice.

```sql
SELECT
    c.course_name
FROM students s
JOIN student_courses sc
    ON s.student_id = sc.student_id
JOIN courses c
    ON sc.course_id = c.course_id
WHERE s.name = 'Alice';
```

---

# 35. The "bridge table" pattern

Whenever you see:

```text
many-to-many
```

look for an intermediate table.

Examples:

```text
students ↔ courses
users ↔ roles
products ↔ categories
doctors ↔ patients
employees ↔ projects
```

Usually:

```text
A
 ↓
bridge
 ↓
B
```

---

# 36. JOIN conditions with multiple columns

Sometimes one column isn't enough.

Suppose:

### sales

| store_id | product_id | sale_date  |
| -------: | ---------: | ---------- |
|        1 |         10 | 2026-01-01 |

### prices

| store_id | product_id | price |
| -------: | ---------: | ----: |
|        1 |         10 |   100 |
|        2 |         10 |   120 |

You need both:

```sql
SELECT ...
FROM sales s
JOIN prices p
    ON s.store_id = p.store_id
   AND s.product_id = p.product_id;
```

Pattern:

```sql
ON A.key1 = B.key1
AND A.key2 = B.key2
```

---

# 37. JOIN on non-equality conditions

JOIN does not have to use `=`.

Example:

> Match employees to salary bands.

### salary_bands

| band   | min_salary | max_salary |
| ------ | ---------: | ---------: |
| Junior |          0 |      50000 |
| Mid    |      50001 |     100000 |
| Senior |     100001 |     200000 |

Query:

```sql
SELECT
    e.name,
    e.salary,
    s.band
FROM employees e
JOIN salary_bands s
    ON e.salary BETWEEN s.min_salary AND s.max_salary;
```

This is called a **range/non-equi join**.

---

# 38. Another range JOIN example

Question:

> Match each order to the discount applicable on its amount.

```sql
SELECT
    o.order_id,
    o.amount,
    d.discount_percent
FROM orders o
JOIN discounts d
    ON o.amount BETWEEN d.min_amount AND d.max_amount;
```

Very useful interview pattern.

---

# 39. JOIN conditions using `<`, `>`, etc.

Example:

> Find employees whose salary is greater than their manager's.

```sql
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
WHERE e.salary > m.salary
```

The comparison itself happens in `WHERE`.

You can also have more complex conditions in `ON`.

---

# 40. ON vs WHERE — one of the most important concepts

Compare:

### Query A

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
    ON e.dept_id = d.dept_id
WHERE d.department = 'Engineering';
```

### Query B

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
    ON e.dept_id = d.dept_id
   AND d.department = 'Engineering';
```

These are **NOT equivalent**.

---

# 41. Understanding the difference

Query A:

```sql
WHERE d.department = 'Engineering'
```

After the LEFT JOIN, rows where `d.department` is NULL are removed.

So the LEFT JOIN effectively behaves like an INNER JOIN for this condition.

Query B:

```sql
ON ...
AND d.department = 'Engineering'
```

still preserves every employee.

Employees not in Engineering simply get:

```text
NULL
```

for department columns.

---

# 42. Very important interview pattern

Suppose:

> Find all customers and their orders placed in 2026, including customers who placed no 2026 orders.

Correct:

```sql
SELECT
    c.customer_id,
    c.name,
    o.order_id
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
   AND EXTRACT(YEAR FROM o.order_date) = 2026;
```

If you write:

```sql
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE EXTRACT(YEAR FROM o.order_date) = 2026;
```

customers with no orders become NULL and get removed.

That's a common interview trap.

---

# 43. JOIN + NULL

Remember:

```sql
NULL = NULL
```

does **not** evaluate to TRUE.

It evaluates to UNKNOWN.

Therefore:

```sql
ON a.id = b.id
```

doesn't match two NULL values.

If you specifically need NULL-safe comparison, database syntax differs.

---

# 44. MySQL NULL-safe equality

MySQL has:

```sql
a <=> b
```

This is NULL-safe equality.

Example:

```sql
ON a.value <=> b.value
```

It treats:

```text
NULL <=> NULL
```

as true.

---

# 45. PostgreSQL NULL-safe comparison

PostgreSQL supports:

```sql
IS NOT DISTINCT FROM
```

Example:

```sql
ON a.value IS NOT DISTINCT FROM b.value
```

This treats two NULLs as equivalent for comparison.

---

# 46. JOIN aliases — strongly recommended

Instead of:

```sql
SELECT employees.name,
       departments.department
FROM employees
JOIN departments
ON employees.dept_id = departments.dept_id;
```

write:

```sql
SELECT
    e.name,
    d.department
FROM employees e
JOIN departments d
    ON e.dept_id = d.dept_id;
```

For 4–5 tables, aliases become essential.

---

# 47. How to choose the LEFT table

This is a major skill.

Question:

> Find all departments, including departments with no employees.

Start with:

```sql
FROM departments d
LEFT JOIN employees e
```

because:

```text
departments = things we MUST keep
```

Question:

> Find all employees, including employees without departments.

Start with:

```sql
FROM employees e
LEFT JOIN departments d
```

because:

```text
employees = things we MUST keep
```

---

# 48. The "preserve this entity" trick

When reading a question, ask:

> **What entity must appear even if there is no matching data?**

That table should usually be your LEFT table.

Examples:

> Show all customers, even those with no orders.

```sql
FROM customers c
LEFT JOIN orders o
```

> Show all products, even those never sold.

```sql
FROM products p
LEFT JOIN order_items oi
```

> Show all departments, even empty departments.

```sql
FROM departments d
LEFT JOIN employees e
```

This one trick solves many JOIN questions.

---

# 49. JOIN vs WHERE EXISTS

Another important interview distinction.

Question:

> Find customers who have at least one order.

Using JOIN:

```sql
SELECT DISTINCT
    c.customer_id,
    c.name
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id;
```

Using `EXISTS`:

```sql
SELECT
    c.customer_id,
    c.name
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

The second expresses:

> Does a matching row exist?

This is often cleaner when you don't need columns from the joined table.

---

# 50. JOIN vs EXISTS — mental distinction

### JOIN

Think:

> "Bring matching rows/data."

### EXISTS

Think:

> "Tell me whether a match exists."

Example:

```text
Need order_id?
     ↓
JOIN

Only need to know whether customer ordered?
     ↓
EXISTS
```

---

# 51. NOT EXISTS — another powerful pattern

Question:

> Find customers who never placed an order.

```sql
SELECT
    c.customer_id,
    c.name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

Equivalent common pattern:

```sql
SELECT
    c.customer_id,
    c.name
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.customer_id IS NULL;
```

Both are important to know.

---

# 52. A common interview trap: LEFT JOIN + WHERE

Question:

> Find all customers and their orders, but only show orders above $100, while retaining customers with no qualifying orders.

Correct:

```sql
SELECT
    c.name,
    o.order_id,
    o.amount
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
   AND o.amount > 100;
```

Incorrect if you need customers with no qualifying orders:

```sql
SELECT
    c.name,
    o.order_id,
    o.amount
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.amount > 100;
```

Because `WHERE` removes NULL rows.

---

# 53. JOIN + conditional aggregation

Very common in coding interviews.

Question:

> Show each customer and how many orders they placed.

```sql
SELECT
    c.customer_id,
    c.name,
    COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name;
```

---

# 54. Multiple conditional counts

Question:

> For every customer, show total orders and completed orders.

```sql
SELECT
    c.customer_id,
    c.name,
    COUNT(o.order_id) AS total_orders,
    COUNT(
        CASE
            WHEN o.status = 'completed'
            THEN 1
        END
    ) AS completed_orders
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name;
```

PostgreSQL/MySQL also support the modern `FILTER` syntax in PostgreSQL, while MySQL commonly uses conditional aggregation.

PostgreSQL:

```sql
COUNT(*) FILTER (
    WHERE o.status = 'completed'
)
```

---

# 55. JOIN + CASE

Question:

> Label employees based on their department.

```sql
SELECT
    e.name,
    d.department,
    CASE
        WHEN d.department = 'Engineering'
            THEN 'TECH'
        WHEN d.department = 'Sales'
            THEN 'BUSINESS'
        ELSE 'OTHER'
    END AS category
FROM employees e
LEFT JOIN departments d
    ON e.dept_id = d.dept_id;
```

---

# 56. JOIN + window functions

Very common advanced interview combination.

Question:

> Find each employee's salary and their department's average salary.

```sql
SELECT
    e.name,
    d.department,
    e.salary,
    AVG(e.salary) OVER (
        PARTITION BY e.dept_id
    ) AS dept_avg_salary
FROM employees e
JOIN departments d
    ON e.dept_id = d.dept_id;
```

Output conceptually:

| name    | department  | salary | dept_avg_salary |
| ------- | ----------- | -----: | --------------: |
| Alice   | Engineering |  70000 |           75000 |
| Charlie | Engineering |  80000 |           75000 |
| Bob     | Sales       |  60000 |           60000 |

---

# 57. JOIN + ranking

Question:

> Find the highest-paid employee in each department.

One approach:

```sql
WITH ranked AS (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY e.dept_id
            ORDER BY e.salary DESC
        ) AS rn
    FROM employees e
)
SELECT
    r.name,
    d.department,
    r.salary
FROM ranked r
JOIN departments d
    ON r.dept_id = d.dept_id
WHERE r.rn = 1;
```

This combines:

```text
JOIN
+
CTE
+
WINDOW FUNCTION
+
ROW_NUMBER
```

Very common in SQL interviews.

---

# 58. Joining aggregated results

Another advanced pattern.

Question:

> Find customers whose spending is greater than the average customer spending.

First calculate customer spending:

```sql
WITH customer_totals AS (
    SELECT
        c.customer_id,
        c.name,
        SUM(o.amount) AS total_spent
    FROM customers c
    JOIN orders o
        ON c.customer_id = o.customer_id
    GROUP BY c.customer_id, c.name
)
SELECT *
FROM customer_totals
WHERE total_spent > (
    SELECT AVG(total_spent)
    FROM customer_totals
);
```

This is often easier than trying to do everything in one huge JOIN.

---

# 59. Four essential JOIN diagrams

## INNER JOIN

```text
A       B
 ███████
   ↑
matching area only
```

Keep:

```text
A ∩ B
```

---

## LEFT JOIN

```text
A       B
████████
  █████
```

Keep:

```text
ALL A
+
matching B
```

---

## RIGHT JOIN

Keep:

```text
matching A
+
ALL B
```

---

## FULL OUTER JOIN

Keep:

```text
ALL A
+
ALL B
```

---

# 60. JOIN cheat table

| JOIN  | Keeps unmatched left? | Keeps unmatched right? |
| ----- | --------------------: | ---------------------: |
| INNER |                    ❌ |                     ❌ |
| LEFT  |                    ✅ |                     ❌ |
| RIGHT |                    ❌ |                     ✅ |
| FULL  |                    ✅ |                     ✅ |
| CROSS |                   N/A |                    N/A |
| SELF  |               depends |                depends |

---

# 61. English → JOIN translation cheat sheet

This is the part I'd memorize for coding questions.

| Question wording                       | Think                         |
| -------------------------------------- | ----------------------------- |
| "who have..."                          | INNER JOIN                    |
| "with matching..."                     | INNER JOIN                    |
| "all customers, even those without..." | LEFT JOIN                     |
| "including customers with no..."       | LEFT JOIN                     |
| "find customers without..."            | LEFT JOIN + `IS NULL`         |
| "find customers who never..."          | `NOT EXISTS` or LEFT JOIN     |
| "all departments, including empty..."  | LEFT JOIN                     |
| "all records from both..."             | FULL OUTER JOIN               |
| "every possible combination..."        | CROSS JOIN                    |
| "employee and manager"                 | SELF JOIN                     |
| "employee earns more than manager"     | SELF JOIN + comparison        |
| "students and courses"                 | bridge table + JOIN           |
| "at least one..."                      | EXISTS / JOIN                 |
| "no matching..."                       | NOT EXISTS / LEFT JOIN + NULL |
| "within a range"                       | range JOIN                    |

---

# 62. The JOIN question-solving algorithm

When you see a SQL problem, don't immediately write code.

Use this sequence.

## Step 1 — Identify the final entity

Ask:

> What should one output row represent?

Examples:

```text
one employee
one customer
one order
one department
one customer + order
```

This is extremely important.

---

## Step 2 — Identify required tables

Example:

> Show customer name, product name and order date.

Need:

```text
customers
orders
order_items
products
```

---

## Step 3 — Draw the relationship

```text
customers
   ↓ customer_id
orders
   ↓ order_id
order_items
   ↓ product_id
products
```

---

## Step 4 — Identify mandatory table

Question:

> Should entities without matches remain?

If yes:

```text
LEFT JOIN
```

---

## Step 5 — Write only the FROM/JOIN section

Before SELECT:

```sql
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN order_items oi
    ON o.order_id = oi.order_id
JOIN products p
    ON oi.product_id = p.product_id
```

---

## Step 6 — Add filters

```sql
WHERE ...
```

---

## Step 7 — Add aggregation if required

```sql
GROUP BY ...
HAVING ...
```

---

## Step 8 — Add sorting

```sql
ORDER BY ...
```

---

# 63. The SQL structure to memorize

For most JOIN interview questions:

```sql
SELECT
    ...
FROM table_a a
JOIN table_b b
    ON ...
LEFT JOIN table_c c
    ON ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...;
```

You don't need every clause every time.

Think:

```text
SELECT
FROM
JOIN
ON
WHERE
GROUP BY
HAVING
ORDER BY
```

---

# 64. SQL logical execution order

Very useful for understanding JOIN behavior.

Although we write:

```sql
SELECT
FROM
JOIN
WHERE
GROUP BY
HAVING
ORDER BY
```

conceptually SQL processes roughly:

```text
FROM
 ↓
JOIN
 ↓
ON
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
```

This explains many JOIN problems.

---

# 65. The biggest JOIN mistake: wrong JOIN type

Question:

> Find all products and the number sold, including products never sold.

Wrong:

```sql
FROM products p
JOIN order_items oi
```

Products with no sales disappear.

Correct:

```sql
FROM products p
LEFT JOIN order_items oi
```

The word:

> **including products never sold**

is the signal.

---

# 66. The second biggest mistake: wrong table on the left

Compare:

```sql
FROM customers c
LEFT JOIN orders o
```

versus:

```sql
FROM orders o
LEFT JOIN customers c
```

They preserve different entities.

Ask:

> What must survive even without a match?

That table goes on the left.

---

# 67. The third biggest mistake: accidental row multiplication

Suppose:

```text
customer → 5 orders
order → 3 items each
```

Joining them produces potentially:

```text
15 rows
```

for that customer's order/item combinations.

Don't automatically use:

```sql
DISTINCT
```

to hide duplicates.

First ask:

> Is this actually a duplicate, or is this a legitimate one-to-many relationship?

---

# 68. DISTINCT is not a JOIN fix

Bad approach:

```sql
SELECT DISTINCT ...
```

just because JOIN produced more rows.

Instead understand:

```text
What is the grain of the result?
```

For example:

```text
one row = one customer
```

Then aggregate:

```sql
GROUP BY customer_id
```

if necessary.

---

# 69. "Grain" — extremely important interview concept

**Grain = what one row represents.**

Example:

Before JOIN:

```text
employees
1 row = 1 employee
```

After:

```sql
employees
JOIN orders
```

potentially:

```text
1 row = 1 employee-order
```

After:

```sql
employees
JOIN orders
JOIN order_items
```

potentially:

```text
1 row = 1 employee-order-item
```

Always ask:

> What does one row represent at this stage?

This prevents many SQL errors.

---

# 70. Classic Interview Question #1

### Find customers who have never placed an order.

### Method 1 — LEFT JOIN

```sql
SELECT
    c.customer_id,
    c.name
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

### Method 2 — NOT EXISTS

```sql
SELECT
    c.customer_id,
    c.name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

Both are excellent patterns.

---

# 71. Classic Interview Question #2

### Find departments with no employees.

```sql
SELECT
    d.dept_id,
    d.department
FROM departments d
LEFT JOIN employees e
    ON d.dept_id = e.dept_id
WHERE e.employee_id IS NULL;
```

Pattern:

```text
parent without child
```

---

# 72. Classic Interview Question #3

### Find employees who don't have a manager.

```sql
SELECT
    e.employee_id,
    e.name
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id
WHERE m.employee_id IS NULL;
```

But if `manager_id IS NULL` itself is sufficient:

```sql
SELECT
    employee_id,
    name
FROM employees
WHERE manager_id IS NULL;
```

Interview tip:

**Use the simplest query that correctly expresses the requirement.**

---

# 73. Classic Interview Question #4

### Find employees who earn more than their manager.

```sql
SELECT
    e.name AS employee,
    e.salary AS employee_salary,
    m.name AS manager,
    m.salary AS manager_salary
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

Pattern:

```text
same table
+
two roles
=
SELF JOIN
```

---

# 74. Classic Interview Question #5

### Find customers with at least one order over $1,000.

JOIN version:

```sql
SELECT DISTINCT
    c.customer_id,
    c.name
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.amount > 1000;
```

EXISTS version:

```sql
SELECT
    c.customer_id,
    c.name
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
      AND o.amount > 1000
);
```

---

# 75. Classic Interview Question #6

### Find products that were never ordered.

```sql
SELECT
    p.product_id,
    p.name
FROM products p
LEFT JOIN order_items oi
    ON p.product_id = oi.product_id
WHERE oi.product_id IS NULL;
```

Memorize this structure:

```sql
FROM parent
LEFT JOIN child
WHERE child.id IS NULL
```

---

# 76. Classic Interview Question #7

### Find the number of orders for every customer, including customers with zero orders.

```sql
SELECT
    c.customer_id,
    c.name,
    COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id,
    c.name;
```

Key:

```text
LEFT JOIN
+
COUNT(child.id)
```

---

# 77. Classic Interview Question #8

### Find customers with more than 5 orders.

```sql
SELECT
    c.customer_id,
    c.name,
    COUNT(o.order_id) AS order_count
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id,
    c.name
HAVING COUNT(o.order_id) > 5;
```

Could also use LEFT JOIN if you conceptually want to preserve zero-order customers before filtering, but because `> 5` excludes them anyway, INNER JOIN is sufficient.

---

# 78. Classic Interview Question #9

### Find the department with the highest average salary.

```sql
SELECT
    d.department,
    AVG(e.salary) AS avg_salary
FROM departments d
JOIN employees e
    ON d.dept_id = e.dept_id
GROUP BY
    d.dept_id,
    d.department
ORDER BY avg_salary DESC
LIMIT 1;
```

MySQL + PostgreSQL:

```sql
LIMIT 1
```

works in both.

---

# 79. Classic Interview Question #10

### Find employees whose salary is greater than the average salary of their department.

Window-function solution:

```sql
SELECT
    e.employee_id,
    e.name,
    e.salary
FROM (
    SELECT
        e.*,
        AVG(salary) OVER (
            PARTITION BY dept_id
        ) AS dept_avg
    FROM employees e
) e
WHERE e.salary > e.dept_avg;
```

This doesn't technically require a JOIN to departments unless you need department names.

---

# 80. MySQL vs PostgreSQL JOIN differences

Most JOIN syntax is the same.

| Feature                       | MySQL                    | PostgreSQL |
| ----------------------------- | ------------------------ | ---------- |
| INNER JOIN                    | ✅                       | ✅         |
| LEFT JOIN                     | ✅                       | ✅         |
| RIGHT JOIN                    | ✅                       | ✅         |
| FULL OUTER JOIN               | ❌ native                | ✅         |
| CROSS JOIN                    | ✅                       | ✅         |
| SELF JOIN                     | ✅                       | ✅         |
| JOIN with multiple conditions | ✅                       | ✅         |
| Range JOIN                    | ✅                       | ✅         |
| `USING`                       | ✅                       | ✅         |
| `NATURAL JOIN`                | ✅                       | ✅         |
| `LATERAL`                     | Limited/different syntax | ✅         |

For interviews, the biggest difference to remember:

> **PostgreSQL has FULL OUTER JOIN; MySQL doesn't natively.**

---

# 81. USING vs ON

Instead of:

```sql
JOIN departments d
    ON e.dept_id = d.dept_id
```

you can sometimes write:

```sql
JOIN departments d
    USING (dept_id)
```

This requires the join columns to have the same name.

Example:

```sql
SELECT
    e.name,
    d.department
FROM employees e
JOIN departments d
USING (dept_id);
```

---

# 82. Why `ON` is usually safer for learning/interviews

`ON` makes the relationship explicit:

```sql
ON e.dept_id = d.dept_id
```

It's especially useful when:

```text
column names differ
```

Example:

```sql
ON e.department_id = d.dept_id
```

For interview explanations, I'd generally prefer `ON`.

---

# 83. NATURAL JOIN — know it, but be careful

You may encounter:

```sql
NATURAL JOIN
```

It automatically joins using columns with the same names.

Example:

```sql
SELECT *
FROM employees
NATURAL JOIN departments;
```

This can be dangerous because changing table schemas can unexpectedly change the JOIN behavior.

For production/interview SQL, explicit:

```sql
ON ...
```

is usually much clearer.

---

# 84. Advanced pattern: JOIN against the same table twice

Example:

> Find employees and their manager's department.

```sql
SELECT
    e.name AS employee,
    d1.department AS employee_department,
    m.name AS manager,
    d2.department AS manager_department
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id
LEFT JOIN departments d1
    ON e.dept_id = d1.dept_id
LEFT JOIN departments d2
    ON m.dept_id = d2.dept_id;
```

Here:

```text
employees e = employee
employees m = manager

departments d1 = employee's department
departments d2 = manager's department
```

Same physical table, different logical roles.

---

# 85. Advanced pattern: find pairs of employees

Self joins can compare rows against each other.

Question:

> Find pairs of employees working in the same department.

```sql
SELECT
    e1.name AS employee1,
    e2.name AS employee2
FROM employees e1
JOIN employees e2
    ON e1.dept_id = e2.dept_id
   AND e1.employee_id < e2.employee_id;
```

Why:

```sql
e1.employee_id < e2.employee_id
```

?

To prevent:

```text
Alice - Bob
Bob - Alice
```

and:

```text
Alice - Alice
```

You only want:

```text
Alice - Bob
```

---

# 86. This is a powerful self-JOIN pattern

Whenever you see:

> Compare every row with another row from the same table

think:

```sql
FROM table a
JOIN table b
```

Then add a condition to prevent duplicate/reversed pairs:

```sql
a.id < b.id
```

---

# 87. Example: employees with same salary

```sql
SELECT
    e1.name AS employee1,
    e2.name AS employee2,
    e1.salary
FROM employees e1
JOIN employees e2
    ON e1.salary = e2.salary
   AND e1.employee_id < e2.employee_id;
```

---

# 88. JOIN order matters conceptually

Consider:

```sql
A
JOIN B
JOIN C
```

You should understand the relationships:

```text
A → B
B → C
```

Don't blindly connect every table to every other table.

For example:

```sql
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN products p
    ON o.product_id = p.product_id
```

might be wrong if `orders` doesn't contain `product_id`.

Maybe you need:

```text
orders → order_items → products
```

So:

```sql
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN order_items oi
    ON o.order_id = oi.order_id
JOIN products p
    ON oi.product_id = p.product_id
```

---

# 89. The database schema is your JOIN map

When solving coding questions, identify:

### Primary key

Usually:

```text
customer_id
order_id
product_id
```

### Foreign key

Usually:

```text
orders.customer_id
order_items.order_id
order_items.product_id
```

Then connect:

```text
PK ←→ FK
```

Typical pattern:

```sql
ON parent.primary_key = child.foreign_key
```

---

# 90. JOIN performance basics

Interviewers may ask:

> How can you optimize a JOIN?

Basic answer:

### Index JOIN columns

For example:

```sql
employees.dept_id
departments.dept_id
```

Usually the primary key is already indexed.

Foreign-key columns may need an index depending on database/schema design.

---

# 91. Avoid unnecessary columns

Instead of:

```sql
SELECT *
```

prefer:

```sql
SELECT
    c.customer_id,
    c.name,
    o.order_id
```

Benefits:

```text
less data
clearer intent
easier debugging
```

---

# 92. Filter early when appropriate

Instead of joining a huge set and filtering later, sometimes you can restrict the joined table:

```sql
SELECT ...
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
   AND o.status = 'completed';
```

The optimizer may transform queries itself, so don't obsess over manually rewriting every filter for performance. Correctness comes first; use `EXPLAIN`/`EXPLAIN ANALYZE` for actual performance diagnosis.

---

# 93. EXPLAIN

When investigating a JOIN's performance:

### MySQL

```sql
EXPLAIN
SELECT ...
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id;
```

### PostgreSQL

```sql
EXPLAIN ANALYZE
SELECT ...
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id;
```

You'll see information about things such as:

```text
scan method
join method
estimated rows
actual rows
cost
```

---

# 94. JOIN algorithms — interview awareness

At the database-engine level, common strategies include:

```text
Nested Loop Join
Hash Join
Merge Join
```

You normally don't manually choose them in basic SQL; the optimizer decides.

But interviewers may ask conceptually.

### Nested Loop

Concept:

```text
for each row in A:
    find matching rows in B
```

Can be effective with good indexes and small outer inputs.

### Hash Join

Concept:

```text
build hash structure for one side
probe it using the other side
```

Often useful for equality joins.

### Merge Join

Conceptually:

```text
sort/scan both sides
walk through them together
```

Can be efficient when inputs are suitably ordered.

---

# 95. Don't confuse JOIN with UNION

Very important.

### JOIN

Combines:

```text
columns horizontally
```

Example:

```text
customer + order information
```

### UNION

Combines:

```text
rows vertically
```

Example:

```text
employees_2025
+
employees_2026
```

Visual:

```text
JOIN:

A          B
──────     ──────
row 1  +   info
row 2  +   info


UNION:

A
────
row 1
row 2

+

B
────
row 3
row 4
```

---

# 96. JOIN vs UNION interview question

If someone says:

> Combine customer details with order details

→ `JOIN`

If someone says:

> Combine rows from two similar tables

→ `UNION`

---

# 97. Common JOIN mistakes checklist

### Mistake 1

Forgetting `ON`:

```sql
FROM employees e
JOIN departments d;
```

This is effectively a Cartesian product in supported syntax contexts.

Usually you meant:

```sql
ON e.dept_id = d.dept_id
```

---

### Mistake 2

Using INNER JOIN when LEFT JOIN is required.

Question:

> Include customers with no orders.

Must consider:

```sql
LEFT JOIN
```

---

### Mistake 3

Putting a LEFT JOIN condition in WHERE.

Potentially changes semantics:

```sql
LEFT JOIN ...
WHERE right_table.condition
```

---

### Mistake 4

Using `COUNT(*)` with LEFT JOIN when you need count of matches.

Usually prefer:

```sql
COUNT(child.id)
```

---

### Mistake 5

Using DISTINCT to hide a wrong JOIN.

Understand cardinality first.

---

### Mistake 6

Joining on the wrong key.

Always verify:

```text
PK ↔ FK
```

---

### Mistake 7

Missing a bridge table.

For:

```text
many-to-many
```

look for:

```text
junction/bridge table
```

---

# 98. The most useful JOIN patterns to memorize

## Pattern 1 — Matching records

```sql
SELECT ...
FROM A
JOIN B
    ON A.id = B.a_id;
```

---

## Pattern 2 — Keep all A

```sql
SELECT ...
FROM A
LEFT JOIN B
    ON A.id = B.a_id;
```

---

## Pattern 3 — A without B

```sql
SELECT ...
FROM A
LEFT JOIN B
    ON A.id = B.a_id
WHERE B.a_id IS NULL;
```

---

## Pattern 4 — Existence

```sql
SELECT ...
FROM A
WHERE EXISTS (
    SELECT 1
    FROM B
    WHERE B.a_id = A.id
);
```

---

## Pattern 5 — Non-existence

```sql
SELECT ...
FROM A
WHERE NOT EXISTS (
    SELECT 1
    FROM B
    WHERE B.a_id = A.id
);
```

---

## Pattern 6 — Count children

```sql
SELECT
    A.id,
    COUNT(B.id)
FROM A
LEFT JOIN B
    ON A.id = B.a_id
GROUP BY A.id;
```

---

## Pattern 7 — Self JOIN

```sql
SELECT ...
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id;
```

---

## Pattern 8 — Many-to-many

```sql
SELECT ...
FROM A
JOIN bridge ab
    ON A.id = ab.a_id
JOIN B
    ON ab.b_id = B.id;
```

---

## Pattern 9 — All combinations

```sql
SELECT ...
FROM A
CROSS JOIN B;
```

---

## Pattern 10 — Range JOIN

```sql
SELECT ...
FROM A
JOIN B
    ON A.value BETWEEN B.min_value AND B.max_value;
```

---

# 99. A powerful English translation framework

When you read a coding question, translate it like this:

### Question

> Find all customers who have placed at least one order, including their total spending, and include customers with no orders.

Break it down:

### "customers"

```text
base table = customers
```

### "including customers with no orders"

```text
LEFT JOIN
```

### "orders"

```text
JOIN customers → orders
```

### "total spending"

```text
SUM()
```

### "for each customer"

```text
GROUP BY customer
```

Final:

```sql
SELECT
    c.customer_id,
    c.name,
    COALESCE(SUM(o.amount), 0) AS total_spending
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id,
    c.name;
```

This is exactly how you should translate English into SQL.

---

# 100. JOIN decision tree

When you see a JOIN problem, mentally run this:

```text
                Do I need another table?
                       |
                       YES
                       |
              How are rows related?
                       |
                    PK ↔ FK
                       |
          ┌────────────┼────────────┐
          ↓            ↓            ↓
    Only matches   Keep ALL A    Keep ALL B
          |            |            |
      INNER JOIN    LEFT JOIN    RIGHT JOIN
                       |
                       ↓
             Need rows missing in B?
                       |
                      YES
                       |
               B.key IS NULL
```

And:

```text
Need everything from both?
        ↓
FULL OUTER JOIN

Need every combination?
        ↓
CROSS JOIN

Same table, different roles?
        ↓
SELF JOIN
```

---

# 101. The ultimate JOIN interview cheat sheet

## INNER JOIN

```sql
FROM A
JOIN B
ON A.id = B.a_id
```

**Meaning:** only matches.

---

## LEFT JOIN

```sql
FROM A
LEFT JOIN B
ON A.id = B.a_id
```

**Meaning:** all A + matching B.

---

## RIGHT JOIN

```sql
FROM A
RIGHT JOIN B
ON A.id = B.a_id
```

**Meaning:** all B + matching A.

---

## FULL OUTER JOIN

PostgreSQL:

```sql
FROM A
FULL OUTER JOIN B
ON A.id = B.a_id
```

**Meaning:** everything from both.

MySQL:

```sql
LEFT JOIN
UNION
RIGHT JOIN
```

---

## CROSS JOIN

```sql
FROM A
CROSS JOIN B
```

**Meaning:** every A × every B.

---

## SELF JOIN

```sql
FROM employees e
JOIN employees m
ON e.manager_id = m.employee_id
```

**Meaning:** same table used in two roles.

---

# 102. Five patterns you should know cold

### 1. Has matching record

```sql
A JOIN B
```

### 2. Doesn't have matching record

```sql
A LEFT JOIN B
WHERE B.id IS NULL
```

### 3. Has at least one

```sql
WHERE EXISTS (...)
```

### 4. Has none

```sql
WHERE NOT EXISTS (...)
```

### 5. Compare rows in same table

```sql
A a
JOIN A b
ON ...
```

If you can recognize these five patterns quickly, many SQL interview questions become pattern-recognition exercises rather than starting from zero.

---

# 103. Final mental model

Don't memorize JOIN syntax as isolated commands.

Think in **questions**:

```text
"Give me matching things"
        ↓
INNER JOIN


"Give me everything from X"
        ↓
LEFT/RIGHT JOIN


"Give me X that has nothing in Y"
        ↓
LEFT JOIN + IS NULL
        OR
NOT EXISTS


"Give me everything from both"
        ↓
FULL OUTER JOIN


"Give me every possible pair"
        ↓
CROSS JOIN


"Compare rows within the same table"
        ↓
SELF JOIN


"Connect many-to-many entities"
        ↓
BRIDGE TABLE + JOIN
```

And before every JOIN, ask the three golden questions:

> **1. What should one output row represent?**
> **2. What tables contain the information I need?**
> **3. Which table's rows must survive even when there is no match?**

If you answer those three correctly, the JOIN usually becomes straightforward.
