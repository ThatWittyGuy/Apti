# SQL Subqueries & Correlated Subqueries

## From Scratch → English-to-SQL Translation → Every Pattern → MySQL + PostgreSQL → Interview Questions → Traps → Cheatsheet

Subqueries are one of the **most important SQL interview topics**.

If Joins answer:

> **"How do I combine tables?"**

then subqueries answer:

> **"How do I use the result of one query inside another query?"**

Once you understand subqueries, many difficult interview questions become much easier.

---

# 1. What is a subquery?

A **subquery** is a SQL query inside another SQL query.

Example:

> Find employees whose salary is greater than the average salary.

First, think about the English:

```text
employee salary
       >
average employee salary
```

We need two operations:

### Query 1

Find average salary:

```sql
SELECT AVG(salary)
FROM employees;
```

Suppose output is:

|   avg |
| ----: |
| 70000 |

### Query 2

Find employees above 70000:

```sql
SELECT *
FROM employees
WHERE salary > 70000;
```

A subquery combines them:

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

The inner query:

```sql
(
    SELECT AVG(salary)
    FROM employees
)
```

is the **subquery**.

---

# 2. The fundamental mental model

Think:

```text
OUTER QUERY
    │
    │ needs some value/set
    ↓
SUBQUERY
    │
    │ calculates it
    ↓
result
    │
    ↓
OUTER QUERY continues
```

Example:

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Conceptually:

```text
1. Calculate AVG(salary)
2. Compare every employee's salary to that result
3. Return matching employees
```

---

# 3. The three biggest types

You should first classify subqueries into:

```text
1. Scalar subquery
2. Multi-row subquery
3. Correlated subquery
```

There are also different **locations** where a subquery can appear:

```text
WHERE
FROM
SELECT
HAVING
```

---

# 4. Scalar subquery

A scalar subquery returns **one value**.

Example:

```sql
SELECT AVG(salary)
FROM employees;
```

Output:

|   avg |
| ----: |
| 70000 |

One value.

Therefore:

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

The subquery is scalar.

---

# 5. Scalar subquery examples

### Maximum salary

```sql
SELECT MAX(salary)
FROM employees;
```

Use it:

```sql
SELECT *
FROM employees
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
);
```

---

### Minimum salary

```sql
SELECT *
FROM employees
WHERE salary = (
    SELECT MIN(salary)
    FROM employees
);
```

---

### Average salary

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

---

# 6. Sample database we'll use

Let's use:

### `employees`

| emp_id | name    | department_id | salary | manager_id |
| -----: | ------- | ------------: | -----: | ---------: |
|      1 | Alice   |            10 |  90000 |       NULL |
|      2 | Bob     |            10 |  70000 |          1 |
|      3 | Charlie |            10 |  60000 |          1 |
|      4 | David   |            20 | 120000 |       NULL |
|      5 | Emma    |            20 |  80000 |          4 |
|      6 | Frank   |            20 |  75000 |          4 |
|      7 | Grace   |            30 |  95000 |       NULL |
|      8 | Henry   |            30 |  65000 |          7 |

### `departments`

| department_id | department_name |
| ------------: | --------------- |
|            10 | Engineering     |
|            20 | Sales           |
|            30 | HR              |
|            40 | Marketing       |

### `orders`

| order_id | customer_id | amount |
| -------: | ----------: | -----: |
|      101 |           1 |    500 |
|      102 |           1 |    700 |
|      103 |           2 |    300 |
|      104 |           3 |   1000 |
|      105 |           3 |    200 |

---

# 7. Subquery in `WHERE`

This is the most common type.

Pattern:

```sql
SELECT ...
FROM ...
WHERE column operator (
    SELECT ...
);
```

Example:

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

English translation:

> Find employees whose salary is greater than the overall average salary.

---

# 8. Subquery with `=`

Question:

> Find employees earning the maximum salary.

```sql
SELECT *
FROM employees
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
);
```

Output:

| emp_id | name  | salary |
| -----: | ----- | -----: |
|      4 | David | 120000 |

This is a very common pattern.

---

# 9. Subquery with `>`

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

---

# 10. Subquery with `<`

```sql
SELECT *
FROM employees
WHERE salary < (
    SELECT AVG(salary)
    FROM employees
);
```

---

# 11. Subquery with `>=`

```sql
SELECT *
FROM employees
WHERE salary >= (
    SELECT AVG(salary)
    FROM employees
);
```

---

# 12. Multiple conditions

Question:

> Find employees earning more than average and working in department 10.

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
)
AND department_id = 10;
```

You can combine subqueries with normal predicates.

---

# 13. Multi-row subqueries

Now the important difference.

Suppose:

> Find employees who work in departments with an employee earning over 100000.

A subquery might return:

```text
20
```

But it could return:

```text
10
20
```

Now we have **multiple rows**.

You cannot normally do:

```sql
WHERE department_id = (
    SELECT department_id
    FROM employees
    WHERE salary > 100000
)
```

if the inner query returns multiple rows.

You need:

```sql
WHERE department_id IN (
    SELECT department_id
    FROM employees
    WHERE salary > 100000
);
```

---

# 14. `IN` with subqueries

This is one of the most important patterns.

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM employees
    WHERE salary > 100000
);
```

Inner query:

```sql
SELECT department_id
FROM employees
WHERE salary > 100000;
```

Output:

| department_id |
| ------------: |
|            20 |

Outer query:

```text
department_id IN (20)
```

returns everyone in department 20.

Output:

| name  | department_id | salary |
| ----- | ------------: | -----: |
| David |            20 | 120000 |
| Emma  |            20 |  80000 |
| Frank |            20 |  75000 |

---

# 15. `NOT IN`

Question:

> Find employees who are not in departments containing an employee earning over 100000.

```sql
SELECT *
FROM employees
WHERE department_id NOT IN (
    SELECT department_id
    FROM employees
    WHERE salary > 100000
);
```

But **STOP**.

This is where one of the biggest SQL traps appears:

# `NOT IN` + NULL

We'll cover this in depth later.

---

# 16. `ANY`

`ANY` compares a value with **at least one** value returned by the subquery.

Example:

```sql
SELECT *
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```

Suppose department 10 salaries are:

```text
90000
70000
60000
```

Then:

```text
salary > ANY (90000, 70000, 60000)
```

means:

> salary is greater than at least one of those values.

Therefore this is effectively:

```text
salary > 60000
```

because 60000 is the smallest.

---

# 17. `ALL`

`ALL` means:

> condition must be true against every value returned.

Example:

```sql
SELECT *
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```

Department 10:

```text
90000
70000
60000
```

So:

```text
salary > 90000
```

because you need to beat **all** values.

David:

```text
120000 > 90000
```

Yes.

---

# 18. `ANY` vs `ALL`

Memorize:

```text
ANY
→ at least one

ALL
→ every one
```

For:

```text
salary > ANY(...)
```

think approximately:

```text
salary > minimum
```

For:

```text
salary > ALL(...)
```

think approximately:

```text
salary > maximum
```

And:

```text
salary < ANY(...)
```

roughly relates to:

```text
salary < maximum
```

while:

```text
salary < ALL(...)
```

relates to:

```text
salary < minimum
```

This is a great interview shortcut, although NULL/empty-set semantics still matter.

---

# 19. `EXISTS`

Now we move to one of the most important subquery patterns.

`EXISTS` asks:

> Does the subquery return at least one row?

Example:

> Find customers who have at least one order.

```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

You don't care what the subquery returns.

You only care:

```text
Does a matching row exist?
```

---

# 20. Why `SELECT 1`?

You'll often see:

```sql
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE ...
)
```

Why `1`?

Because `EXISTS` only cares whether a row exists.

These are conceptually equivalent:

```sql
SELECT 1
```

```sql
SELECT *
```

```sql
SELECT o.order_id
```

inside `EXISTS`.

`SELECT 1` communicates the intent clearly:

> "I only care whether a row exists."

---

# 21. `NOT EXISTS`

Question:

> Find customers who have never placed an order.

```sql
SELECT *
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

This is one of the most useful SQL interview patterns.

---

# 22. `IN` vs `EXISTS`

This is a common interview question.

### `IN`

Think:

> Is this value inside this set?

```sql
WHERE department_id IN (
    SELECT department_id
    ...
)
```

### `EXISTS`

Think:

> Does a matching row exist?

```sql
WHERE EXISTS (
    SELECT 1
    ...
)
```

Mental model:

```text
IN
→ value-based

EXISTS
→ row-existence-based
```

---

# 23. Correlated subquery

Now we reach the important part.

A **correlated subquery** is a subquery that refers to a column from the outer query.

Example:

> Find employees who earn more than the average salary in their own department.

This is a classic interview problem.

For Alice:

```text
Alice's department = 10
Department 10 average = 73333.33
Alice salary = 90000
```

For David:

```text
David's department = 20
Department 20 average = 91666.67
David salary = 120000
```

The average changes depending on the employee.

---

# 24. Correlated query

```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

Look carefully:

```sql
e2.department_id = e.department_id
```

The inner query references:

```text
e.department_id
```

from the outer query.

That makes it **correlated**.

---

# 25. How to mentally execute it

Imagine the database processes Alice.

Outer row:

```text
Alice
department = 10
salary = 90000
```

Inner query becomes:

```sql
SELECT AVG(salary)
FROM employees
WHERE department_id = 10;
```

Result:

```text
73333.33
```

Compare:

```text
90000 > 73333.33
```

TRUE.

---

Now Bob:

```text
Bob
department = 10
salary = 70000
```

Inner query:

```sql
AVG(salary WHERE department_id = 10)
```

Result:

```text
73333.33
```

Compare:

```text
70000 > 73333.33
```

FALSE.

And so on.

---

# 26. Correlated vs non-correlated

### Non-correlated

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Inner query does not depend on outer row.

```text
same result for every employee
```

### Correlated

```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

Inner query depends on:

```text
current outer employee
```

Therefore:

```text
result can change for every outer row
```

---

# 27. The simplest way to recognize correlation

Look inside the subquery.

Ask:

> Does it reference an alias from the outer query?

If yes:

```text
CORRELATED
```

Example:

```sql
WHERE e2.department_id = e.department_id
                    ↑
            outer query alias
```

---

# 28. Correlated `EXISTS`

This is probably the most important correlated-subquery pattern.

Question:

> Find customers who have at least one order.

```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

This is correlated because:

```sql
o.customer_id = c.customer_id
```

references outer `c`.

---

# 29. Correlated `NOT EXISTS`

Question:

> Find customers with no orders.

```sql
SELECT *
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

Extremely important pattern.

---

# 30. Correlated subquery + aggregate

Question:

> Find employees whose salary is greater than their department's average.

```sql
SELECT *
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

This pattern appears constantly in interviews.

---

# 31. Another classic: maximum salary per department

### Correlated subquery approach

```sql
SELECT *
FROM employees e
WHERE salary = (
    SELECT MAX(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

Output:

| name  | department | salary |
| ----- | ---------: | -----: |
| Alice |         10 |  90000 |
| David |         20 | 120000 |
| Grace |         30 |  95000 |

This is an excellent pattern to know.

---

# 32. But there's another solution

Using a window function:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        MAX(salary) OVER (
            PARTITION BY department_id
        ) AS max_dept_salary
    FROM employees e
) x
WHERE salary = max_dept_salary;
```

Or:

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

This is important because interviewers often ask:

> "Can you solve this without a subquery?"

---

# 33. Subquery vs JOIN vs Window Function

A major SQL interview skill is knowing that many problems have multiple solutions.

Example:

> Find employees earning above their department average.

### Correlated subquery

```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

### JOIN

```sql
SELECT e.*
FROM employees e
JOIN (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) d
ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

### Window function

```sql
SELECT *
FROM (
    SELECT
        e.*,
        AVG(salary) OVER (
            PARTITION BY department_id
        ) AS avg_salary
    FROM employees e
) x
WHERE salary > avg_salary;
```

Three solutions.

---

# 34. Interview insight

If asked:

> "How would you solve this?"

Don't just give one query.

A strong answer is:

> "I can solve this using a correlated subquery, a grouped derived table joined back to the employees table, or a window function. For readability and performance, I'd choose based on the database and data size."

That's a much stronger interview response.

---

# 35. Subquery in `FROM`

A subquery can appear in the `FROM` clause.

This is called a:

```text
derived table
```

Example:

```sql
SELECT *
FROM (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) d;
```

Output:

| department_id | avg_salary |
| ------------: | ---------: |
|            10 |   73333.33 |
|            20 |   91666.67 |
|            30 |      80000 |

The subquery creates a temporary result set for the outer query.

---

# 36. Derived table + JOIN

This is extremely useful.

```sql
SELECT
    e.name,
    e.salary,
    d.avg_salary
FROM employees e
JOIN (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) d
ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

This is often a great alternative to a correlated subquery.

---

# 37. Subquery in `SELECT`

A subquery can also appear in the SELECT list.

Example:

```sql
SELECT
    name,
    salary,
    (SELECT AVG(salary) FROM employees) AS company_avg
FROM employees;
```

Output:

| name    | salary | company_avg |
| ------- | -----: | ----------: |
| Alice   |  90000 |       81875 |
| Bob     |  70000 |       81875 |
| Charlie |  60000 |       81875 |
| David   | 120000 |       81875 |

Every row gets the same company average.

---

# 38. Correlated subquery in `SELECT`

You can also correlate it.

Question:

> Show each employee's salary and their department average.

```sql
SELECT
    e.name,
    e.salary,
    (
        SELECT AVG(e2.salary)
        FROM employees e2
        WHERE e2.department_id = e.department_id
    ) AS department_avg
FROM employees e;
```

Output:

| name    | salary | department_avg |
| ------- | -----: | -------------: |
| Alice   |  90000 |          73333 |
| Bob     |  70000 |          73333 |
| Charlie |  60000 |          73333 |
| David   | 120000 |          91667 |
| Emma    |  80000 |          91667 |
| Frank   |  75000 |          91667 |
| Grace   |  95000 |          80000 |
| Henry   |  65000 |          80000 |

---

# 39. But window functions are often cleaner

Same result:

```sql
SELECT
    name,
    salary,
    AVG(salary) OVER (
        PARTITION BY department_id
    ) AS department_avg
FROM employees;
```

This is usually much more elegant.

---

# 40. Subquery in `HAVING`

Yes, you can use one there too.

Question:

> Find departments whose average salary is greater than the overall average salary.

```sql
SELECT
    department_id,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > (
    SELECT AVG(salary)
    FROM employees
);
```

This is a very good interview pattern.

---

# 41. Subquery with `COUNT`

Question:

> Find customers who have more orders than the average customer.

Conceptually:

```sql
SELECT ...
WHERE order_count > (
    SELECT AVG(order_count)
    ...
);
```

Usually this requires a derived table/CTE because you need to first calculate order counts per customer.

Example:

```sql
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > (
    SELECT AVG(order_count)
    FROM (
        SELECT customer_id, COUNT(*) AS order_count
        FROM orders
        GROUP BY customer_id
    ) x
);
```

This is an important **nested subquery** pattern.

---

# 42. Nested subqueries

A subquery can contain another subquery.

Example:

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department_id IN (
        SELECT department_id
        FROM departments
        WHERE department_name IN ('Engineering', 'Sales')
    )
);
```

You don't want to overuse deeply nested queries, but you should be able to read them.

---

# 43. Multiple levels of thinking

When you see:

```sql
SELECT ...
FROM ...
WHERE x > (
    SELECT ...
    FROM ...
    WHERE y IN (
        SELECT ...
    )
);
```

Don't try to understand everything simultaneously.

Go **inside-out**:

```text
innermost query
      ↓
middle query
      ↓
outer query
```

This is one of the best techniques for reading difficult SQL.

---

# 44. English → subquery translation

This is one of the most useful sections for your interview preparation.

Look for phrases like:

### "greater than the average"

```text
salary > (SELECT AVG(...))
```

### "equal to the maximum"

```text
salary = (SELECT MAX(...))
```

### "in departments where..."

```text
IN (SELECT department_id ...)
```

### "has at least one"

```text
EXISTS (...)
```

### "has none"

```text
NOT EXISTS (...)
```

### "higher than their department average"

```text
correlated subquery
```

### "highest in each department"

```text
MAX + correlated subquery
```

or:

```text
RANK/DENSE_RANK
```

---

# 45. The phrase "their" is a huge clue

Interview questions often say:

> Find employees whose salary is greater than **their department's** average.

The word:

```text
their
```

is a clue that the comparison depends on the current row.

Think:

```text
CORRELATED SUBQUERY
```

Example:

```sql
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
)
```

---

# 46. The phrase "overall" is another clue

> Find employees whose salary is greater than the **overall average**.

No correlation:

```sql
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
)
```

Compare:

```text
overall average
→ non-correlated

their department average
→ correlated
```

Very useful distinction.

---

# 47. The phrase "at least one"

Usually think:

```sql
EXISTS
```

Example:

> Customers who have at least one order over $1000.

```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
      AND o.amount > 1000
);
```

---

# 48. The phrase "none"

Think:

```sql
NOT EXISTS
```

Example:

> Customers who have no orders above $1000.

```sql
SELECT *
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
      AND o.amount > 1000
);
```

Notice:

This means:

> There does not exist an order above $1000.

It does **not** necessarily mean:

> The customer has no orders at all.

That's an important logical distinction.

---

# 49. `NOT EXISTS` vs `LEFT JOIN`

These two often solve the same problem.

### NOT EXISTS

```sql
SELECT c.*
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

### LEFT JOIN

```sql
SELECT c.*
FROM customers c
LEFT JOIN orders o
    ON o.customer_id = c.customer_id
WHERE o.customer_id IS NULL;
```

Both can find customers without orders.

---

# 50. Why `NOT EXISTS` is often safer than `NOT IN`

Suppose:

```text
departments
10
20
NULL
```

Query:

```sql
WHERE department_id NOT IN (
    SELECT department_id
    FROM departments
)
```

NULL introduces three-valued logic complications.

`NOT EXISTS` is usually safer when the subquery can contain NULLs:

```sql
WHERE NOT EXISTS (
    SELECT 1
    FROM departments d
    WHERE d.department_id = e.department_id
)
```

### Interview rule

When asked:

> Find rows that don't have a matching row.

A very strong default pattern is:

```sql
NOT EXISTS
```

---

# 51. The `NOT IN` NULL trap

Suppose:

```text
A table:

id
---
1
2
3

B table:

id
---
2
NULL
```

You might write:

```sql
SELECT *
FROM A
WHERE id NOT IN (
    SELECT id
    FROM B
);
```

Intuitively you expect:

```text
1
3
```

But the NULL can make the comparison evaluate to UNKNOWN in cases you didn't expect.

This is why:

```sql
NOT EXISTS
```

is often preferable.

---

# 52. `EXISTS` does not care about duplicates

Suppose customer 1 has 500 orders.

This:

```sql
WHERE EXISTS (
    SELECT 1
    FROM orders
    WHERE customer_id = c.customer_id
)
```

still just produces:

```text
TRUE
```

for customer 1.

It doesn't matter whether there is:

```text
1 order
```

or:

```text
500 orders
```

This is one reason `EXISTS` is elegant for existence questions.

---

# 53. `JOIN` can multiply rows

Compare:

```sql
SELECT c.*
FROM customers c
JOIN orders o
    ON o.customer_id = c.customer_id;
```

If customer 1 has 5 orders, customer 1 can appear 5 times.

But:

```sql
SELECT c.*
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

returns customer 1 once.

This is a very important distinction.

---

# 54. `EXISTS` for filtering vs JOIN for retrieving

Good mental model:

```text
EXISTS
→ "Does a related row exist?"

JOIN
→ "I need columns from the related table."
```

If you only need to test existence, `EXISTS` is often clearer.

---

# 55. Subquery for second-highest salary

Classic interview question.

### Method 1: `MAX`

```sql
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```

This returns the second distinct-highest salary.

---

# 56. Second-highest with duplicates

Suppose salaries:

```text
120000
120000
90000
80000
```

The query:

```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```

returns:

```text
90000
```

Correct.

This is why using `< MAX()` is a useful pattern.

---

# 57. Third-highest salary

You can nest:

```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
    WHERE salary < (
        SELECT MAX(salary)
        FROM employees
    )
);
```

It works, but becomes ugly quickly.

Better:

```text
DENSE_RANK()
```

or:

```text
ORDER BY DISTINCT salary
LIMIT/OFFSET
```

depending on the question/database.

---

# 58. Subquery for "highest salary in each department"

```sql
SELECT *
FROM employees e
WHERE salary = (
    SELECT MAX(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

This returns all ties.

If two employees earn 120000 in the same department, both are returned.

---

# 59. Highest-paid employee per department — exactly one

If the interviewer says:

> Return exactly one employee per department.

Then `ROW_NUMBER()` may be more appropriate:

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC, emp_id
        ) AS rn
    FROM employees e
) x
WHERE rn = 1;
```

The difference is:

```text
MAX / RANK / DENSE_RANK
→ can return ties

ROW_NUMBER
→ exactly one row
```

---

# 60. Subqueries with `DISTINCT`

Question:

> Find employees who work in departments that have at least one employee earning over 100000.

You might write:

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT DISTINCT department_id
    FROM employees
    WHERE salary > 100000
);
```

`DISTINCT` isn't always necessary with `IN`, because membership doesn't care about duplicate values.

This is also valid:

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM employees
    WHERE salary > 100000
);
```

This is a nice interview observation.

---

# 61. Subquery + `HAVING`

Question:

> Find departments whose employee count is greater than the average department employee count.

First calculate counts:

```sql
SELECT
    department_id,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department_id;
```

Then compare to average count:

```sql
SELECT
    department_id,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) > (
    SELECT AVG(employee_count)
    FROM (
        SELECT
            department_id,
            COUNT(*) AS employee_count
        FROM employees
        GROUP BY department_id
    ) x
);
```

This is an advanced nested-subquery problem.

---

# 62. A common mistake: aggregate inside aggregate

People sometimes try:

```sql
AVG(COUNT(*))
```

directly.

Usually you can't aggregate an aggregate at the same query level like that.

Instead:

```text
Step 1:
GROUP BY → get COUNT per group

Step 2:
outer query → AVG those counts
```

So:

```sql
SELECT AVG(employee_count)
FROM (
    SELECT
        department_id,
        COUNT(*) AS employee_count
    FROM employees
    GROUP BY department_id
) x;
```

This is a fundamental derived-table pattern.

---

# 63. Subquery vs CTE

These can often express the same logic.

### Subquery

```sql
SELECT *
FROM (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) x
WHERE avg_salary > 80000;
```

### CTE

```sql
WITH dept_avg AS (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM dept_avg
WHERE avg_salary > 80000;
```

CTE is often easier to read, especially with multiple steps.

---

# 64. When should you use a CTE instead?

Use a CTE when:

```text
query is becoming difficult to read
you need multiple logical steps
you want to reuse an intermediate result
you need recursion
```

Use a simple subquery when:

```text
logic is short
the intermediate result is used only once
```

---

# 65. Correlated subquery vs window function

Another major interview comparison.

Question:

> Show employees and department average.

Correlated:

```sql
SELECT
    e.name,
    e.salary,
    (
        SELECT AVG(e2.salary)
        FROM employees e2
        WHERE e2.department_id = e.department_id
    ) AS dept_avg
FROM employees e;
```

Window:

```sql
SELECT
    name,
    salary,
    AVG(salary) OVER (
        PARTITION BY department_id
    ) AS dept_avg
FROM employees;
```

The window function is usually simpler for this type of "compare each row to a group statistic" problem.

---

# 66. Performance of correlated subqueries

Historically, people often say:

> "Correlated subqueries are always slow."

That's too simplistic.

A correlated subquery **can** be expensive because conceptually the inner query is evaluated in relation to outer rows.

For example:

```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

With a huge table, repeatedly calculating department averages may be less efficient than calculating them once.

A join/CTE/window solution may be better.

But modern optimizers can rewrite some correlated queries, so:

> **Never assume performance purely from syntax. Check the execution plan.**

---

# 67. Better alternative: precompute once

Instead of repeatedly calculating department averages:

```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

calculate averages once:

```sql
WITH dept_avg AS (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT e.*
FROM employees e
JOIN dept_avg d
    ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

This is often easier to reason about.

---

# 68. Important optimization principle

Think:

```text
Can I calculate this once?
```

If the answer is yes, consider:

```text
CTE
derived table
window function
JOIN
```

instead of repeatedly recalculating it.

---

# 69. Subquery with `ORDER BY` + `LIMIT`

Useful for top-N questions.

Example:

> Find employees earning the second-highest salary.

One approach:

```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```

Another approach depends on whether duplicates matter.

For PostgreSQL:

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

MySQL supports the same general `LIMIT` concept.

---

# 70. A subtle point: scalar subquery must return one row

This:

```sql
WHERE salary = (
    SELECT salary
    FROM employees
);
```

is dangerous/wrong because the subquery may return many rows.

A scalar context expects one value.

You need something like:

```sql
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
);
```

which returns one value.

---

# 71. What happens if scalar subquery returns zero rows?

This depends on context and SQL semantics, but a scalar subquery with no row generally behaves like `NULL`.

For example:

```sql
salary > (
    SELECT salary
    FROM employees
    WHERE emp_id = 999999
)
```

If no employee exists, the scalar result is effectively NULL.

Then:

```text
salary > NULL
```

becomes:

```text
UNKNOWN
```

and the row isn't returned.

Another reason to understand NULL.

---

# 72. Subquery returning multiple columns

A normal scalar comparison like:

```sql
WHERE salary = (
    SELECT salary, name
    FROM employees
)
```

is invalid because the subquery returns multiple columns.

A scalar subquery needs:

```text
one row
one column
```

when used as a single scalar value.

---

# 73. Row-value comparisons

Advanced SQL can compare multiple columns together.

For example:

```sql
WHERE (department_id, salary) IN (
    SELECT department_id, MAX(salary)
    FROM employees
    GROUP BY department_id
);
```

This is a useful advanced pattern.

Conceptually:

```text
(department, salary)
```

must match a pair returned by the subquery.

Support and syntax details can vary somewhat by database, but both MySQL and PostgreSQL support row constructors.

---

# 74. Subquery in `UPDATE`

Subqueries aren't only for SELECT.

Example:

> Give employees in the lowest-paid department a 10% raise.

You can use a subquery in an update:

```sql
UPDATE employees
SET salary = salary * 1.10
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE department_name = 'HR'
);
```

Be careful when the subquery can return multiple rows.

---

# 75. Subquery in `DELETE`

Example:

> Delete customers who have never placed an order.

A `NOT EXISTS` approach:

```sql
DELETE FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

Exact syntax/aliasing details can differ by MySQL/PostgreSQL, so check the database when writing production DML.

---

# 76. Subquery in `INSERT`

You can insert the result of another query:

```sql
INSERT INTO high_salary_employees (
    emp_id,
    name,
    salary
)
SELECT emp_id, name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

This is extremely useful in ETL/data-processing tasks.

---

# 77. Common interview pattern: customers above average order amount

```sql
SELECT *
FROM customers c
WHERE (
    SELECT AVG(o.amount)
    FROM orders o
    WHERE o.customer_id = c.customer_id
) > 500;
```

This is correlated.

The question is:

> For each customer, calculate their average order amount, then keep customers whose average is above 500.

---

# 78. Common interview pattern: customers with more than 3 orders

Using correlated subquery:

```sql
SELECT *
FROM customers c
WHERE (
    SELECT COUNT(*)
    FROM orders o
    WHERE o.customer_id = c.customer_id
) > 3;
```

This is easy to understand:

```text
for each customer
    count their orders
    if count > 3
        return customer
```

Alternative:

```sql
SELECT c.*
FROM customers c
JOIN orders o
    ON o.customer_id = c.customer_id
GROUP BY c.customer_id
HAVING COUNT(*) > 3;
```

---

# 79. Common interview pattern: employees whose manager earns more

Suppose:

```text
emp_id | name | manager_id | salary
```

Question:

> Find employees whose salary is less than their manager's salary.

This can be solved with a self-join:

```sql
SELECT e.*
FROM employees e
JOIN employees m
    ON e.manager_id = m.emp_id
WHERE e.salary < m.salary;
```

But you can also think of it as a related-row comparison.

This is a good reminder:

> **Not every multi-step problem should use a subquery.**

Sometimes a self-join is clearer.

---

# 80. The big comparison table

| Problem                                 | Good tool                           |
| --------------------------------------- | ----------------------------------- |
| Compare to overall average              | Scalar subquery                     |
| Compare to group average                | Window / join / correlated subquery |
| Check whether related row exists        | `EXISTS`                            |
| Check whether related row doesn't exist | `NOT EXISTS`                        |
| Value belongs to set                    | `IN`                                |
| Value doesn't belong to set             | `NOT IN` / preferably carefully     |
| Maximum/minimum                         | Aggregate subquery                  |
| Top N per group                         | Window functions                    |
| Complex multi-step logic                | CTE                                 |
| Hierarchy                               | Recursive CTE                       |
| Combine related columns                 | JOIN                                |
| Compare each row with previous/next     | Window functions                    |

---

# 81. The "which method should I use?" decision tree

When you see a question:

### "Greater than the overall average?"

```text
Scalar subquery
```

### "Greater than their department average?"

```text
Correlated subquery
OR
Window function
OR
JOIN + GROUP BY
```

### "Has at least one?"

```text
EXISTS
```

### "Has none?"

```text
NOT EXISTS
```

### "Is in this list?"

```text
IN
```

### "Not in this list?"

```text
NOT IN
```

but watch NULL.

### "Top 3 per department?"

```text
ROW_NUMBER / RANK / DENSE_RANK
```

### "Need multiple intermediate steps?"

```text
CTE
```

---

# 82. Subquery interview traps

## Trap 1: `=` with multiple rows

Wrong:

```sql
WHERE department_id = (
    SELECT department_id
    FROM employees
    WHERE salary > 50000
);
```

If multiple departments qualify, scalar `=` fails.

Use:

```sql
WHERE department_id IN (
    SELECT department_id
    FROM employees
    WHERE salary > 50000
);
```

---

## Trap 2: `NOT IN` + NULL

Potentially dangerous:

```sql
WHERE id NOT IN (...)
```

Prefer:

```sql
WHERE NOT EXISTS (...)
```

when NULL is possible.

---

## Trap 3: Forgetting correlation

Question:

> Above their department average.

Wrong:

```sql
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

That compares against the **company average**.

Correct:

```sql
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

---

# 83. Trap 4: Correlation on the wrong column

Suppose:

```sql
WHERE e2.department_id = e.department_id
```

Correct.

But accidentally:

```sql
WHERE e2.department_id = e2.department_id
```

is basically always true for non-NULL values.

That changes the query completely.

Always identify:

```text
outer alias
inner alias
join/correlation condition
```

---

# 84. Trap 5: Missing aliases

When querying the same table twice:

```sql
employees e
employees e2
```

you almost always want clear aliases.

Good:

```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

Much easier to read than ambiguous column references.

---

# 85. Trap 6: `EXISTS` doesn't need `DISTINCT`

Don't write:

```sql
WHERE EXISTS (
    SELECT DISTINCT customer_id
    FROM orders
    WHERE ...
)
```

usually.

You only need to know whether a row exists.

Simpler:

```sql
WHERE EXISTS (
    SELECT 1
    FROM orders
    WHERE ...
)
```

---

# 86. Trap 7: JOIN accidentally duplicates rows

Question:

> Find customers who have at least one order.

This:

```sql
SELECT c.*
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id;
```

may return duplicates.

You might add:

```sql
DISTINCT
```

but often the cleaner expression is:

```sql
WHERE EXISTS (...)
```

because your actual requirement is existence.

---

# 87. Trap 8: Overusing correlated subqueries

If you need:

> calculate the same group statistic for every row

consider:

```sql
AVG(...) OVER (PARTITION BY ...)
```

or:

```sql
GROUP BY + JOIN
```

instead of a correlated subquery.

---

# 88. How to read a difficult subquery

Use this method:

## Step 1

Find the **innermost query**.

## Step 2

Determine:

```text
What does it return?
one value?
many values?
rows?
columns?
```

## Step 3

Move outward.

## Step 4

For every outer reference, mark:

```text
CORRELATED
```

## Step 5

Translate the outer condition into English.

---

# 89. Example of reading a difficult query

```sql
SELECT *
FROM employees e
WHERE department_id IN (
    SELECT department_id
    FROM employees
    GROUP BY department_id
    HAVING AVG(salary) > (
        SELECT AVG(salary)
        FROM employees
    )
);
```

Read inside-out.

### Inner:

```sql
SELECT AVG(salary)
FROM employees
```

→ overall average.

### Middle:

```sql
GROUP BY department_id
HAVING AVG(salary) > overall_average
```

→ departments whose average salary exceeds company average.

### Outer:

```sql
department_id IN (...)
```

→ employees belonging to those departments.

English:

> Find employees who work in departments whose average salary is greater than the overall company average.

This is exactly the kind of translation skill you want.

---

# 90. MySQL vs PostgreSQL

The **core subquery syntax is very similar** between MySQL and PostgreSQL.

These work conceptually in both:

```sql
WHERE x IN (SELECT ...)
```

```sql
WHERE EXISTS (SELECT ...)
```

```sql
WHERE x > (SELECT AVG(...))
```

```sql
FROM (SELECT ...) x
```

```sql
WITH x AS (...)
SELECT ...
```

The differences become more noticeable in:

- DML with subqueries
- `UPDATE`/`DELETE` restrictions and syntax
- optimizer behavior
- CTE materialization behavior
- lateral queries
- database-specific functions

---

# 91. Advanced: `LATERAL`

Once you're comfortable with correlated subqueries, the next advanced concept is:

```text
LATERAL
```

PostgreSQL is particularly powerful here.

It allows a subquery in `FROM` to reference columns from a preceding table expression.

Conceptually:

```sql
SELECT ...
FROM customers c
CROSS JOIN LATERAL (
    SELECT ...
    FROM orders o
    WHERE o.customer_id = c.customer_id
    ORDER BY ...
    LIMIT 1
) x;
```

This is extremely useful for problems like:

> Get the latest order for every customer.

You don't need this for beginner SQL interviews, but it's a very useful advanced topic after correlated subqueries.

---

# 92. Latest order per customer — correlated approach

One approach:

```sql
SELECT *
FROM orders o
WHERE order_date = (
    SELECT MAX(o2.order_date)
    FROM orders o2
    WHERE o2.customer_id = o.customer_id
);
```

This is correlated.

But if two orders have exactly the same latest timestamp, it can return both.

If you need exactly one row, use:

```text
ROW_NUMBER()
```

or PostgreSQL-specific approaches such as `DISTINCT ON`.

---

# 93. The most important subquery patterns to memorize

### Pattern 1 — Compare with overall aggregate

```sql
WHERE value > (
    SELECT AVG(value)
    FROM table
)
```

---

### Pattern 2 — Compare with maximum

```sql
WHERE value = (
    SELECT MAX(value)
    FROM table
)
```

---

### Pattern 3 — Membership

```sql
WHERE id IN (
    SELECT id
    FROM ...
)
```

---

### Pattern 4 — Existence

```sql
WHERE EXISTS (
    SELECT 1
    FROM ...
    WHERE ...
)
```

---

### Pattern 5 — Non-existence

```sql
WHERE NOT EXISTS (
    SELECT 1
    FROM ...
    WHERE ...
)
```

---

### Pattern 6 — Group-specific comparison

```sql
WHERE e.value > (
    SELECT AVG(e2.value)
    FROM table e2
    WHERE e2.group_id = e.group_id
)
```

---

### Pattern 7 — Maximum per group

```sql
WHERE value = (
    SELECT MAX(e2.value)
    FROM table e2
    WHERE e2.group_id = e.group_id
)
```

---

### Pattern 8 — Derived table

```sql
FROM (
    SELECT ...
    FROM ...
) x
```

---

### Pattern 9 — Nested subquery

```sql
WHERE x IN (
    SELECT x
    FROM ...
    WHERE y > (
        SELECT ...
    )
)
```

---

### Pattern 10 — Group result compared to overall result

```sql
GROUP BY group_id
HAVING AVG(value) > (
    SELECT AVG(value)
    FROM table
)
```

---

# 94. Interview question set

You should practice these in order.

### Beginner

1. Find employees earning above average.
2. Find employee with highest salary.
3. Find employee with lowest salary.
4. Find employees earning below average.
5. Find employees in departments where salary > 100k.
6. Find customers who have orders.
7. Find customers with no orders.

### Intermediate

8. Find employees earning above their department average.
9. Find highest-paid employee in each department.
10. Find employees earning the second-highest salary.
11. Find departments whose average salary exceeds company average.
12. Find customers with more than 3 orders.
13. Find customers whose average order amount exceeds 500.
14. Find products purchased by at least one customer.
15. Find products never purchased.

### Advanced

16. Find the second-highest salary per department.
17. Find the top 3 salaries per department.
18. Find employees whose salary exceeds their manager's salary.
19. Find customers whose latest order exceeds their average order.
20. Find departments whose employee count is above the average department size.
21. Find customers who bought every product in a category.
22. Find employees who earn more than all employees in another department.
23. Find employees who earn more than at least one employee in another department.
24. Find customers who have orders in every month of a year.
25. Find the latest record per group.

---

# 95. Subqueries vs Window Functions vs Joins

This is perhaps the most important high-level comparison.

| Requirement                | Subquery        | JOIN           | Window        |
| -------------------------- | --------------- | -------------- | ------------- |
| Overall average comparison | ✅              |                |               |
| Existence                  | ✅ `EXISTS`     | ✅             |               |
| Missing relationship       | ✅ `NOT EXISTS` | ✅ `LEFT JOIN` |               |
| Group average comparison   | ✅              | ✅             | ⭐            |
| Max per group              | ✅              | ✅             | ⭐            |
| Ranking                    |                 |                | ⭐⭐⭐        |
| Running total              |                 |                | ⭐⭐⭐        |
| Previous row               |                 |                | ⭐⭐⭐        |
| Combine table columns      |                 | ⭐⭐⭐         |               |
| Hierarchy                  |                 |                | Recursive CTE |
| Multi-step query           |                 |                | CTE           |

---

# 96. The biggest conceptual distinction

Remember these four questions:

```text
"What is ONE value?"
        ↓
Scalar subquery

"What is a SET of values?"
        ↓
IN / ANY / ALL

"Does a matching ROW exist?"
        ↓
EXISTS

"Does the answer depend on the CURRENT outer row?"
        ↓
Correlated subquery
```

If you internalize this, subqueries become much easier.

---

# 97. Ultimate English → SQL cheat sheet

| English                         | Think                            |
| ------------------------------- | -------------------------------- |
| greater than average            | `> (SELECT AVG(...))`            |
| equal to maximum                | `= (SELECT MAX(...))`            |
| equal to minimum                | `= (SELECT MIN(...))`            |
| belongs to these groups         | `IN (SELECT ...)`                |
| does not belong                 | `NOT IN` / `NOT EXISTS`          |
| at least one                    | `EXISTS`                         |
| none                            | `NOT EXISTS`                     |
| every                           | `ALL`                            |
| at least one of them            | `ANY`                            |
| their department average        | correlated subquery              |
| their manager                   | correlated/self-join             |
| highest in each group           | correlated `MAX` / window        |
| latest per group                | correlated `MAX` / window        |
| more than 3 related records     | correlated `COUNT` / GROUP BY    |
| group average > overall average | `HAVING` + scalar subquery       |
| multi-step logic                | CTE / derived table              |
| intermediate result             | derived table                    |
| same calculation per row        | window function often preferable |

---

# 98. One-page cheatsheet

```text
========================================================
SQL SUBQUERIES CHEATSHEET
========================================================

SUBQUERY
--------------------------------------------------------
Query inside another query.

Example:
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
)


SCALAR SUBQUERY
--------------------------------------------------------
Returns ONE value.

MAX:
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
)

AVG:
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
)


MULTI-ROW SUBQUERY
--------------------------------------------------------
Returns multiple values.

IN:
WHERE department_id IN (
    SELECT department_id
    FROM employees
    WHERE salary > 100000
)


ANY
--------------------------------------------------------
At least one match.

salary > ANY (...)
≈ salary > minimum


ALL
--------------------------------------------------------
Every value must satisfy condition.

salary > ALL (...)
≈ salary > maximum


EXISTS
--------------------------------------------------------
Does at least one matching row exist?

WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
)


NOT EXISTS
--------------------------------------------------------
No matching row exists.

WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
)


CORRELATED SUBQUERY
--------------------------------------------------------
Inner query references outer query.

WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
)


MAX PER GROUP
--------------------------------------------------------
WHERE salary = (
    SELECT MAX(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
)


SUBQUERY IN FROM
--------------------------------------------------------
Derived table.

FROM (
    SELECT ...
    FROM ...
) x


SUBQUERY IN SELECT
--------------------------------------------------------
SELECT
    name,
    (SELECT AVG(salary) FROM employees)
FROM employees


SUBQUERY IN HAVING
--------------------------------------------------------
HAVING AVG(salary) > (
    SELECT AVG(salary)
    FROM employees
)


NESTED SUBQUERY
--------------------------------------------------------
Solve inside → outside.

innermost
   ↓
middle
   ↓
outer


IMPORTANT DIFFERENCES
--------------------------------------------------------
Overall average
    → non-correlated

Their department average
    → correlated

At least one
    → EXISTS

None
    → NOT EXISTS

Value in set
    → IN

Not in set
    → NOT IN (watch NULL)


NULL TRAP
--------------------------------------------------------
NOT IN + NULL can produce unexpected results.

Safer anti-match:
NOT EXISTS


COMMON ALTERNATIVES
--------------------------------------------------------
Subquery ↔ JOIN ↔ Window Function ↔ CTE

Always ask:
"Which is clearest and most efficient?"


PERFORMANCE
--------------------------------------------------------
Correlated subqueries CAN be expensive.

Consider:
- JOIN
- CTE
- Window Function

But don't assume.
Check EXPLAIN / execution plan.


INTERVIEW CLUES
--------------------------------------------------------
"overall"
    → scalar subquery

"their"
    → correlated subquery

"at least one"
    → EXISTS

"none"
    → NOT EXISTS

"among these"
    → IN

"every"
    → ALL

"top per group"
    → Window / correlated MAX

"multiple steps"
    → CTE


========================================================
```

# The 5 patterns you should know cold

If you're doing SQL coding interviews, make these automatic:

### 1. Above overall average

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

### 2. Highest salary

```sql
SELECT *
FROM employees
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
);
```

### 3. Has at least one related row

```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

### 4. Has no related row

```sql
SELECT *
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

### 5. Above **their group's** average

```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

The fifth one is the key transition from **ordinary subqueries → correlated subqueries**.

And the next conceptual step after mastering these is to learn **`CASE WHEN` + `NULL` + `EXISTS/NOT EXISTS` + CTEs**, because those four combined with joins, grouping, dates, and window functions form the majority of the patterns you'll encounter in SQL interview coding questions.
