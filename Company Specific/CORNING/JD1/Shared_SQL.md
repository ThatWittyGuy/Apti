# Shared — SQL MCQs
### Applies to: All JDs mentioning SQL / databases / data analytics
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What does SQL stand for?
- A) Structured Query Logic
- B) Structured Query Language
- C) Sequential Query Language
- D) Standard Query List

**Answer: B**

---

**Q2.** 🟢 Which SQL command is used to retrieve data from a table?
- A) `GET`
- B) `FETCH`
- C) `SELECT`
- D) `PULL`

**Answer: C**

---

**Q3.** 🟢 Which clause filters rows BEFORE aggregation?
- A) `HAVING`
- B) `WHERE`
- C) `GROUP BY`
- D) `ORDER BY`

**Answer: B**

---

**Q4.** 🟢 Which clause filters rows AFTER aggregation?
- A) `WHERE`
- B) `HAVING`
- C) `FILTER`
- D) `LIMIT`

**Answer: B**

---

**Q5.** 🟢 What is a PRIMARY KEY?
- A) The first column in every table
- B) A column (or combination) that uniquely identifies each row in a table and cannot be NULL
- C) A key used to encrypt data
- D) The most frequently queried column

**Answer: B**

---

**Q6.** 🟢 What is a FOREIGN KEY?
- A) A key from a different database vendor
- B) A column that references the PRIMARY KEY of another table, establishing a relationship between tables
- C) An encrypted key for secure queries
- D) A unique identifier in an external system

**Answer: B**

---

**Q7.** 🟢 What does `SELECT DISTINCT col FROM table` do?
- A) Selects the first unique row only
- B) Returns only unique (non-duplicate) values of `col`
- C) Selects columns with distinct data types
- D) Sorts the column in distinct order

**Answer: B**

---

**Q8.** 🟢 Which aggregate function returns the number of rows?
- A) `SUM()`
- B) `COUNT()`
- C) `MAX()`
- D) `AVG()`

**Answer: B**

---

**Q9.** 🟡 What is the output of the following query if `employees` has 10 rows, 3 of which have NULL salary?
```sql
SELECT COUNT(salary) FROM employees;
```
- A) `10`
- B) `7`
- C) `3`
- D) `NULL`

**Answer: B** — `COUNT(column)` ignores NULLs; `COUNT(*)` counts all rows.

---

**Q10.** 🟡 What is the difference between `INNER JOIN` and `LEFT JOIN`?
- A) INNER JOIN includes all left rows; LEFT JOIN includes only matches
- B) INNER JOIN returns only rows with matches in both tables; LEFT JOIN returns all rows from the left table and matching rows from the right (NULLs for non-matches)
- C) They are identical except for syntax
- D) LEFT JOIN only works with two tables

**Answer: B**

---

**Q11.** 🟡 What does a `FULL OUTER JOIN` return?
- A) Only rows matching in both tables
- B) All rows from both tables — matched where possible, NULLs where there is no match on either side
- C) All rows from the left table only
- D) All rows from the right table only

**Answer: B**

---

**Q12.** 🟡 What will this query return?
```sql
SELECT department, COUNT(*) as emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5
ORDER BY emp_count DESC;
```
- A) All departments sorted by name
- B) Departments with more than 5 employees, sorted by employee count descending
- C) The top 5 departments
- D) All employees in departments with more than 5 people

**Answer: B**

---

**Q13.** 🟡 What is a SQL subquery?
- A) A query that runs on a subset of servers
- B) A query nested inside another query, used in SELECT, FROM, or WHERE clauses to provide intermediate results
- C) A stored procedure with parameters
- D) A partial SQL statement saved as a template

**Answer: B**

---

**Q14.** 🟡 What is the difference between `UNION` and `UNION ALL`?
- A) `UNION ALL` removes duplicates; `UNION` keeps them
- B) `UNION` removes duplicate rows from the combined result; `UNION ALL` includes all rows including duplicates (and is faster)
- C) `UNION` works only with same-structure tables; `UNION ALL` works with any tables
- D) They are identical

**Answer: B**

---

**Q15.** 🟡 What is a SQL INDEX and why is it used?
- A) A numbered list of all tables in a database
- B) A database object that speeds up data retrieval by creating a separate lookup structure on one or more columns, at the cost of additional write overhead
- C) A constraint that enforces unique values
- D) A list of all queries run on the database

**Answer: B**

---

**Q16.** 🟡 What is the difference between `DELETE` and `TRUNCATE`?
- A) DELETE removes table structure; TRUNCATE removes only data
- B) DELETE removes specific rows (logged, supports WHERE, can be rolled back); TRUNCATE removes all rows quickly with minimal logging and cannot use WHERE
- C) TRUNCATE supports a WHERE clause; DELETE does not
- D) They are identical for practical purposes

**Answer: B**

---

**Q17.** 🟡 What is a VIEW in SQL?
- A) A read-only snapshot of table data stored on disk
- B) A named, saved SQL query that acts like a virtual table — queried like a real table but doesn't store data itself (usually)
- C) A user interface for database management
- D) A dashboard report generated from SQL

**Answer: B**

---

**Q18.** 🟡 What does `COALESCE(col1, col2, 0)` do?
- A) Combines two columns into one
- B) Returns the first non-NULL value in the list — `col1` if not NULL, else `col2`, else `0`
- C) Calculates the average of col1 and col2
- D) Converts NULL values to empty strings

**Answer: B**

---

**Q19.** 🟡 What is a stored procedure in SQL?
- A) A SQL query saved in a text file
- B) A named, precompiled set of SQL statements stored in the database and executed as a unit, optionally accepting parameters
- C) A procedure that stores database backups
- D) An automated scheduled query

**Answer: B**

---

**Q20.** 🟡 What does `GROUP BY ROLLUP(a, b)` do compared to regular `GROUP BY a, b`?
- A) It is the same as `GROUP BY a, b`
- B) It generates subtotals and a grand total in addition to the regular group-level aggregates — producing multiple levels of grouping in one query
- C) It rolls up data from child to parent tables
- D) It groups data in reverse order

**Answer: B**

---

**Q21.** 🔴 What is a Window Function in SQL? Give an example.
- A) A function applied to a visible portion of the screen; example: `SCREEN_TOP()`
- B) A function that performs calculations across a defined set of rows related to the current row without collapsing them into one output row; example: `ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC)`
- C) A function that creates a sliding time window for joins; example: `TIME_WINDOW()`
- D) A performance optimization function; example: `CACHE_WINDOW()`

**Answer: B**

---

**Q22.** 🔴 What is the difference between `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()`?
- A) They are identical
- B) `ROW_NUMBER()` assigns unique sequential numbers; `RANK()` gives tied rows the same rank and skips the next number(s); `DENSE_RANK()` gives tied rows the same rank without skipping
- C) `RANK()` assigns unique numbers; `ROW_NUMBER()` handles ties
- D) `DENSE_RANK()` is for dates; others are for numbers

**Answer: B**

---

**Q23.** 🔴 What is a CTE (Common Table Expression)?
- A) A permanent table created from a SELECT result
- B) A temporary named result set defined with `WITH` at the start of a query, improving readability and enabling recursive queries
- C) A compressed table for large datasets
- D) A cached temporary table stored in memory permanently

**Answer: B**

---

**Q24.** 🔴 What is a recursive CTE used for?
- A) Running the same query multiple times for performance testing
- B) Querying hierarchical or tree-structured data (e.g., org charts, bill of materials) by having the CTE reference itself
- C) Joining a table to itself
- D) Generating random data for testing

**Answer: B**

---

**Q25.** 🔴 What is database normalization?
- A) Converting all column names to lowercase
- B) Organizing a database to reduce data redundancy and improve integrity by dividing data into related tables and defining relationships
- C) Normalizing numeric values to a 0–1 scale
- D) Indexing all columns for faster queries

**Answer: B**

---

**Q26.** 🔴 What is the difference between 2NF and 3NF in normalization?
- A) 2NF removes duplicate rows; 3NF removes duplicate columns
- B) 2NF removes partial dependencies (non-key columns must depend on the full primary key); 3NF additionally removes transitive dependencies (non-key columns must depend only on the primary key, not on other non-key columns)
- C) They are the same normal form
- D) 3NF is for numeric data; 2NF is for text data

**Answer: B**

---

**Q27.** 🔴 What is an ACID transaction in databases?
- A) A harmful database operation that corrupts data
- B) A transaction guaranteeing Atomicity (all-or-nothing), Consistency (valid state before and after), Isolation (concurrent transactions don't interfere), Durability (committed data persists)
- C) A transaction that runs on acidic (low-latency) hardware
- D) An advanced compression and indexing design

**Answer: B**

---

**Q28.** 🔴 What is the difference between OLTP and OLAP systems?
- A) OLTP is for analytics; OLAP is for transactions
- B) OLTP (Online Transaction Processing) handles high-volume, fast, granular transactional writes/reads (e.g., ERP); OLAP (Online Analytical Processing) handles complex queries over large datasets for analytics (e.g., data warehouses)
- C) They are the same type of system
- D) OLAP uses SQL; OLTP uses NoSQL

**Answer: B**

---

**Q29.** 🔴 What is a "star schema" in data warehousing?
- A) A schema shaped like a star in the ER diagram for aesthetics
- B) A data warehouse design with a central fact table connected to multiple dimension tables, optimized for query performance and simplicity
- C) A schema used only in SAP HANA
- D) A schema with multiple fact tables at the center

**Answer: B**

---

**Q30.** 🔴 What is the difference between a "fact table" and a "dimension table" in a star schema?
- A) Fact tables store text; dimension tables store numbers
- B) Fact tables store measurable, quantitative business events (e.g., sales transactions with amounts); dimension tables store descriptive attributes used to filter/group facts (e.g., product, customer, date)
- C) They are interchangeable terms
- D) Fact tables are in OLTP; dimension tables are in OLAP only

**Answer: B**

---

**Q31.** 🟡 What does `EXPLAIN` (or `EXPLAIN ANALYZE`) do in SQL?
- A) Generates documentation for a SQL query
- B) Shows the query execution plan — how the database engine will (or did) execute the query, including joins, indexes used, and cost estimates — useful for performance tuning
- C) Translates SQL into natural language
- D) Validates SQL syntax without executing it

**Answer: B**

---

**Q32.** 🟡 What is a self-join?
- A) A query that automatically joins all tables in a database
- B) Joining a table to itself using an alias, useful for hierarchical data or comparing rows within the same table
- C) A join that doesn't require a ON clause
- D) A join between two identical tables in different schemas

**Answer: B**

---

**Q33.** 🟡 What is the purpose of `CASE WHEN` in SQL?
- A) To create conditional loops in SQL
- B) To implement conditional logic within a query — returning different values based on specified conditions, similar to `if/else`
- C) To handle SQL exceptions
- D) To define CASE sensitivity for string comparisons

**Answer: B**

---

**Q34.** 🔴 What is a "covering index"?
- A) An index that covers all tables in a database
- B) An index that includes all columns needed by a query, allowing the database to satisfy the query entirely from the index without accessing the base table
- C) A partial index that covers specific rows
- D) An index that covers primary and foreign keys only

**Answer: B**

---

**Q35.** 🔴 What is the `LAG()` window function used for?
- A) Measuring query execution lag
- B) Accessing a value from a previous row within the result set partition without a self-join — e.g., comparing current row's value to the previous row's value
- C) Delaying query execution
- D) Calculating the lag between two timestamps

**Answer: B**

---
*35 Questions | Shared across all JDs | See JD-specific files for domain MCQs.*
