# SQL Pattern Matching — `LIKE`, Wildcards & Regex

## From Scratch → Interview Patterns → Coding Questions → MySQL vs PostgreSQL → Cheatsheet

Pattern matching is one of the most useful SQL skills for **filtering text**.

Typical interview questions:

- Find names starting with `A`
- Find emails ending with `.com`
- Find products containing `"phone"`
- Find strings with exactly 5 characters
- Find names where the second character is `a`
- Find emails matching a particular structure
- Find rows containing only digits
- Find duplicate-looking records based on a pattern
- Validate phone numbers / email formats
- Extract or replace text using regex
- Solve "starts with / ends with / contains / does not contain" problems

The key idea is:

> **Translate the English requirement into a string pattern, then choose `LIKE` or Regex depending on how complicated the pattern is.**

---

# 1. First: Understand the problem in English

Suppose we have:

### `employees`

| employee_id | name    | email                                             | phone      |
| ----------: | ------- | ------------------------------------------------- | ---------- |
|           1 | Alice   | [alice@gmail.com](mailto:alice@gmail.com)         | 9876543210 |
|           2 | Bob     | [bob@yahoo.com](mailto:bob@yahoo.com)             | 9123456780 |
|           3 | Anita   | [anita@gmail.com](mailto:anita@gmail.com)         | 9988776655 |
|           4 | Charlie | [charlie@company.org](mailto:charlie@company.org) | 8877665544 |
|           5 | David   | [david@gmail.com](mailto:david@gmail.com)         | 9876501234 |
|           6 | Anna    | [anna@hotmail.com](mailto:anna@hotmail.com)       | 9123400000 |

When an interviewer says:

> Find employees whose name starts with `A`.

Think:

```text
starts with A
       ↓
A + anything
       ↓
'A%'
```

SQL:

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%';
```

Output:

| employee_id | name  |
| ----------: | ----- |
|           1 | Alice |
|           3 | Anita |
|           6 | Anna  |

---

# 2. The most important translation table

Memorize this.

| English requirement          | Pattern            |
| ---------------------------- | ------------------ |
| starts with `A`              | `'A%'`             |
| ends with `A`                | `'%A'`             |
| contains `A`                 | `'%A%'`            |
| exactly one character        | `'_'`              |
| exactly 3 characters         | `'___'`            |
| starts with A, total 5 chars | `'A____'`          |
| second character is A        | `'_A%'`            |
| third character is A         | `'__A%'`           |
| second-last character is A   | `'%A_'`            |
| ends in `.com`               | `'%.com'`          |
| contains `sql`               | `'%sql%'`          |
| doesn't contain `sql`        | `NOT LIKE '%sql%'` |

This translation skill solves a huge percentage of basic SQL pattern questions.

---

# 3. `LIKE` from scratch

Basic syntax:

```sql
SELECT *
FROM table_name
WHERE column_name LIKE pattern;
```

Example:

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%';
```

`LIKE` compares a string against a pattern.

There are two fundamental wildcards:

```text
%   → zero or more characters
_   → exactly one character
```

That's it.

But combining them gives many patterns.

---

# 4. `%` — zero or more characters

Suppose:

```text
name
---------
Alice
Anita
Anna
Bob
Aaron
```

Query:

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%';
```

Meaning:

```text
A
+
zero or more characters
```

Matches:

```text
A
Alice
Anita
Anna
Aaron
```

It does **not** mean "at least one character after A."

Technically:

```text
'A%'
```

matches even:

```text
'A'
```

---

# 5. Starts with

Question:

> Find customers whose name starts with `A`.

```sql
WHERE name LIKE 'A%'
```

Examples matched:

```text
Alice
Anita
Anna
Aaron
```

Not matched:

```text
Bob
Charlie
```

### Pattern

```text
A%
│└── anything after A
└── must start with A
```

---

# 6. Ends with

Question:

> Find emails ending in `.com`.

```sql
SELECT *
FROM employees
WHERE email LIKE '%.com';
```

Pattern:

```text
%.com
```

Meaning:

```text
anything + .com
```

Matches:

```text
alice@gmail.com
bob@yahoo.com
```

Doesn't match:

```text
charlie@company.org
```

---

# 7. Contains

Question:

> Find employees whose email contains `gmail`.

```sql
SELECT *
FROM employees
WHERE email LIKE '%gmail%';
```

Meaning:

```text
anything
+
gmail
+
anything
```

Matches:

```text
alice@gmail.com
anita@gmail.com
david@gmail.com
```

---

# 8. Does NOT contain

Question:

> Find employees whose email doesn't contain `gmail`.

```sql
SELECT *
FROM employees
WHERE email NOT LIKE '%gmail%';
```

Output:

```text
bob@yahoo.com
charlie@company.org
anna@hotmail.com
```

---

# 9. `_` — exactly one character

This is where many interview candidates make mistakes.

Suppose:

```text
name
------
Bob
Ben
Bill
Alice
```

Query:

```sql
SELECT *
FROM employees
WHERE name LIKE 'B_b';
```

Pattern:

```text
B _ b
```

Meaning:

```text
B
+
exactly ONE character
+
b
```

Potential matches:

```text
Bob
Bxb
B1b
```

But:

```text
Bill
```

doesn't match.

---

# 10. `%` vs `_`

Very important.

### `%`

```text
zero or more characters
```

### `_`

```text
exactly one character
```

Example:

```sql
LIKE 'A%'
```

matches:

```text
A
AB
ABC
Alice
Anita
```

But:

```sql
LIKE 'A_'
```

matches only:

```text
AB
AC
A1
```

Two-character strings beginning with A.

---

# 11. Exactly N characters

This is a classic coding question.

> Find names having exactly 5 characters.

Using `_`:

```sql
SELECT *
FROM employees
WHERE name LIKE '_____';
```

Five underscores:

```text
_ _ _ _ _
```

Each `_` = one character.

Therefore:

```text
_____ → exactly 5 characters
```

Depending on the database/collation and data, spaces or special characters can introduce edge cases, so for pure length logic, `CHAR_LENGTH()` is often clearer:

### MySQL

```sql
WHERE CHAR_LENGTH(name) = 5
```

### PostgreSQL

```sql
WHERE LENGTH(name) = 5
```

---

# 12. Exactly N characters starting with X

Question:

> Find names with exactly 5 characters starting with `A`.

```sql
WHERE name LIKE 'A____'
```

Break it down:

```text
A _ _ _ _
│ └──────┘
│ 4 arbitrary characters
└ starts with A
```

Total:

```text
1 + 4 = 5
```

---

# 13. Second character

Question:

> Find names where the second character is `a`.

```sql
WHERE name LIKE '_a%'
```

Breakdown:

```text
_ a %
│ │ └── anything after
│ └──── second character = a
└────── first character = anything
```

Matches:

```text
Sam
David
Karen
```

---

# 14. Third character

Question:

> Find names where the third character is `a`.

```sql
WHERE name LIKE '__a%'
```

Breakdown:

```text
_ _ a %
│ │ │ └── anything
│ │ └──── third character
│ └────── second
└──────── first
```

---

# 15. Last character

Question:

> Find names ending with `n`.

```sql
WHERE name LIKE '%n'
```

---

# 16. Second-last character

Question:

> Find names whose second-last character is `a`.

```sql
WHERE name LIKE '%a_'
```

Example:

```text
Sara
  a ← second-last?
```

Let's carefully count:

```text
S a r a
    ↑
second-last = r
```

So Sara does **not** match `%a_`.

For:

```text
S a m
  ↑
second-last = a
```

`Sam` matches:

```sql
LIKE '%a_'
```

This pattern is worth remembering.

---

# 17. Character position patterns

| Requirement      | Pattern |
| ---------------- | ------- |
| first = A        | `A%`    |
| second = A       | `_A%`   |
| third = A        | `__A%`  |
| fourth = A       | `___A%` |
| last = A         | `%A`    |
| second-last = A  | `%A_`   |
| exactly 3 chars  | `___`   |
| exactly 5 chars  | `_____` |
| 5 chars, first A | `A____` |
| 5 chars, last A  | `____A` |

This is extremely useful for interview questions.

---

# 18. Combining patterns

You can combine multiple conditions.

Question:

> Find names starting with A and ending with n.

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%'
  AND name LIKE '%n';
```

For example:

```text
An
Anita       ❌
Anjan       ✅
Aaron       ❌
```

---

# 19. OR patterns

Question:

> Find emails from Gmail or Yahoo.

```sql
SELECT *
FROM employees
WHERE email LIKE '%gmail%'
   OR email LIKE '%yahoo%';
```

---

# 20. Multiple allowed prefixes

Question:

> Find customers whose name starts with A or B.

```sql
WHERE name LIKE 'A%'
   OR name LIKE 'B%'
```

You cannot generally write:

```sql
WHERE name LIKE 'A%' OR 'B%'
```

That is a common mistake.

---

# 21. `LIKE` with `AND`

Question:

> Find names starting with A and containing `n`.

```sql
WHERE name LIKE 'A%'
  AND name LIKE '%n%';
```

This means both conditions must be true.

---

# 22. `NOT LIKE`

Question:

> Find names that don't start with A.

```sql
WHERE name NOT LIKE 'A%';
```

Question:

> Find emails that don't end with `.com`.

```sql
WHERE email NOT LIKE '%.com';
```

---

# 23. Multiple `NOT LIKE`

Question:

> Find emails that are neither Gmail nor Yahoo.

```sql
WHERE email NOT LIKE '%gmail%'
  AND email NOT LIKE '%yahoo%';
```

Important:

```sql
NOT LIKE A
AND NOT LIKE B
```

is usually what you want.

---

# 24. `LIKE` + `IN` misconception

You cannot normally do:

```sql
WHERE name LIKE IN ('A%', 'B%')
```

That's invalid syntax.

Instead:

```sql
WHERE name LIKE 'A%'
   OR name LIKE 'B%';
```

For many regex-style alternatives, Regex becomes more convenient.

---

# 25. Case sensitivity

This is an important **MySQL vs PostgreSQL** difference.

Suppose:

```text
Alice
alice
ALICE
```

---

# 26. MySQL `LIKE`

MySQL's case sensitivity depends heavily on the **collation** of the column/expression.

With a common case-insensitive collation:

```sql
WHERE name LIKE 'a%'
```

may match:

```text
Alice
alice
ALICE
```

With a binary/case-sensitive collation, behavior differs.

So don't blindly assume:

> MySQL LIKE is always case-sensitive.

It isn't.

You should know the column's collation.

---

# 27. PostgreSQL `LIKE`

PostgreSQL:

```sql
LIKE
```

is generally case-sensitive.

Example:

```sql
WHERE name LIKE 'a%'
```

matches:

```text
alice
```

but not:

```text
Alice
```

PostgreSQL has a very useful operator:

```sql
ILIKE
```

which is case-insensitive.

```sql
WHERE name ILIKE 'a%';
```

Now:

```text
Alice
alice
ALICE
```

can match.

### Interview memory trick

```text
PostgreSQL:

LIKE  → case-sensitive
ILIKE → case-insensitive
```

---

# 28. MySQL case-insensitive search

One common approach is:

```sql
WHERE LOWER(name) LIKE 'a%';
```

or:

```sql
WHERE LOWER(name) LIKE '%john%';
```

But remember:

> Applying a function to a column can affect index usage.

If performance matters, collation/index design is often better than blindly applying `LOWER()`.

---

# 29. Escaping `%` and `_`

Suppose the actual data contains:

```text
50%
100%
A_B
```

What if you want to find a literal `%`?

You need to escape it.

Example:

```sql
WHERE description LIKE '%50\%%'
```

Conceptually:

```text
%       → wildcard
\%      → literal %
```

Similarly:

```sql
LIKE '%A\_B%'
```

can search for literal:

```text
A_B
```

The exact escape behavior can depend on SQL/database settings, so for interview questions, understand the concept:

> **Wildcard characters need escaping when you mean them literally.**

---

# 30. Now Regex

`LIKE` is excellent for simple patterns.

But eventually you need:

```text
OR
character ranges
repetition counts
optional characters
start/end anchors
digit classes
complex validation
```

That's where **regular expressions** come in.

Think:

```text
LIKE
 ↓
simple pattern matching

REGEX
 ↓
complex pattern matching
```

---

# 31. Basic Regex vocabulary

These are the most important symbols:

| Regex    | Meaning                |     |
| -------- | ---------------------- | --- |
| `.`      | any single character   |     |
| `^`      | beginning of string    |     |
| `$`      | end of string          |     |
| `[abc]`  | a, b, or c             |     |
| `[^abc]` | anything except a,b,c  |     |
| `[a-z]`  | lowercase letter range |     |
| `[A-Z]`  | uppercase letter range |     |
| `[0-9]`  | digit                  |     |
| `*`      | zero or more           |     |
| `+`      | one or more            |     |
| `?`      | zero or one            |     |
| `{n}`    | exactly n              |     |
| `{n,m}`  | between n and m        |     |
| `        | `                      | OR  |
| `()`     | grouping               |     |

These unlock most SQL regex interview questions.

---

# 32. Regex vs LIKE mental model

Suppose:

> Name starts with A.

### LIKE

```sql
name LIKE 'A%'
```

### Regex

```text
^A
```

Why?

```text
^ → beginning
A → must start with A
```

---

# 33. Regex: starts with

Conceptually:

```regex
^A
```

Matches:

```text
Alice
Anita
Anna
```

Doesn't match:

```text
Bob
Charlie
```

---

# 34. Regex: ends with

```regex
A$
```

Meaning:

```text
A
+
end of string
```

Matches:

```text
Anna
Sara
```

if they end with `a`.

---

# 35. Contains

Regex:

```regex
A
```

can mean the character `A` appears somewhere, depending on the regex function/operator being used.

For explicit substring matching:

```regex
.*A.*
```

Conceptually:

```text
anything
+
A
+
anything
```

---

# 36. Important: `.*` vs SQL `%`

These are analogous:

```text
LIKE:
%

Regex:
.*
```

But they are **not the same syntax**.

### LIKE

```sql
'%abc%'
```

### Regex

```regex
.*abc.*
```

---

# 37. Digits

Regex:

```regex
[0-9]
```

means exactly one digit.

```text
0
1
2
...
9
```

One or more digits:

```regex
[0-9]+
```

Exactly 10 digits:

```regex
^[0-9]{10}$
```

This is a classic interview pattern.

---

# 38. Exactly 10 digits

English:

> Find phone numbers containing exactly 10 digits.

Regex:

```regex
^[0-9]{10}$
```

Break it down:

```text
^
│
start

[0-9]
│
any digit

{10}
│
exactly 10 times

$
│
end
```

Therefore:

```text
9876543210     ✅
123456789      ❌
12345678901    ❌
98765abc10     ❌
```

---

# 39. Why `^` and `$` matter

Suppose:

```regex
[0-9]{10}
```

This could find a 10-digit substring inside:

```text
abc1234567890xyz
```

But:

```regex
^[0-9]{10}$
```

requires the **entire string** to consist of exactly 10 digits.

This distinction is extremely important.

---

# 40. Letters only

Requirement:

> Find names containing only alphabetic characters.

Basic ASCII-style regex:

```regex
^[A-Za-z]+$
```

Meaning:

```text
start
+
one or more letters
+
end
```

Matches:

```text
Alice
John
David
```

Doesn't match:

```text
John123
John_Doe
John Doe
```

Note: international Unicode letters require more careful handling than `[A-Za-z]`.

---

# 41. Alphanumeric only

Requirement:

> Only letters and numbers.

```regex
^[A-Za-z0-9]+$
```

Matches:

```text
ABC123
John99
12345
```

Doesn't match:

```text
ABC-123
John_Doe
ABC 123
```

---

# 42. Exactly 5 alphanumeric characters

```regex
^[A-Za-z0-9]{5}$
```

Matches:

```text
AB123
A1234
12345
```

Doesn't:

```text
ABC123
AB-12
```

---

# 43. Character ranges

You can specify ranges:

```regex
[a-z]
```

lowercase letters.

```regex
[A-Z]
```

uppercase letters.

```regex
[0-9]
```

digits.

Combined:

```regex
[A-Za-z0-9]
```

---

# 44. OR with `|`

Requirement:

> Find strings containing either `cat` or `dog`.

Regex:

```regex
cat|dog
```

Meaning:

```text
cat OR dog
```

For grouped alternatives:

```regex
^(cat|dog)$
```

means the entire string must be exactly:

```text
cat
```

or:

```text
dog
```

---

# 45. Optional character `?`

Regex:

```regex
colou?r
```

matches:

```text
color
colour
```

Because:

```text
u?
```

means:

```text
zero or one u
```

---

# 46. `+` — one or more

```regex
[0-9]+
```

matches:

```text
1
12
123
12345
```

But not:

```text
''
```

because at least one digit is required.

---

# 47. `*` — zero or more

```regex
[0-9]*
```

matches:

```text
''
1
12
12345
```

This is an important difference:

```text
+ → at least one
* → zero or more
```

---

# 48. `{n}` — exactly N

```regex
[0-9]{5}
```

exactly five digits.

```regex
[A-Z]{3}
```

exactly three uppercase letters.

---

# 49. `{n,m}`

```regex
[0-9]{5,10}
```

means:

```text
at least 5
at most 10
```

digits.

---

# 50. Common Regex Cheat Table

| Requirement             | Regex            |        |
| ----------------------- | ---------------- | ------ |
| starts A                | `^A`             |        |
| ends A                  | `A$`             |        |
| contains A              | `A`              |        |
| only digits             | `^[0-9]+$`       |        |
| exactly 10 digits       | `^[0-9]{10}$`    |        |
| only letters            | `^[A-Za-z]+$`    |        |
| only alphanumeric       | `^[A-Za-z0-9]+$` |        |
| 5 digits                | `^[0-9]{5}$`     |        |
| 5–10 digits             | `^[0-9]{5,10}$`  |        |
| contains digit          | `[0-9]`          |        |
| contains uppercase      | `[A-Z]`          |        |
| contains lowercase      | `[a-z]`          |        |
| contains either cat/dog | `cat             | dog`   |
| exactly cat or dog      | `^(cat           | dog)$` |
| optional `u`            | `u?`             |        |
| one or more digits      | `[0-9]+`         |        |
| zero or more digits     | `[0-9]*`         |        |

---

# 51. MySQL Regex

This is where database-specific syntax matters.

Modern MySQL uses:

```sql
REGEXP_LIKE()
```

You can also encounter:

```sql
REGEXP
```

and:

```sql
RLIKE
```

for regex matching.

For interview purposes, know the modern form:

```sql
WHERE REGEXP_LIKE(column_name, 'pattern')
```

Example:

```sql
SELECT *
FROM employees
WHERE REGEXP_LIKE(name, '^A');
```

Meaning:

> Name starts with A.

---

# 52. MySQL: digits only

```sql
SELECT *
FROM employees
WHERE REGEXP_LIKE(phone, '^[0-9]+$');
```

---

# 53. MySQL: exactly 10 digits

```sql
SELECT *
FROM employees
WHERE REGEXP_LIKE(phone, '^[0-9]{10}$');
```

---

# 54. MySQL: letters only

```sql
SELECT *
FROM employees
WHERE REGEXP_LIKE(name, '^[A-Za-z]+$');
```

---

# 55. MySQL: email-ish pattern

A simple interview-style pattern:

```sql
SELECT *
FROM employees
WHERE REGEXP_LIKE(
    email,
    '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$'
);
```

This is useful for demonstrating regex.

But don't treat it as a complete RFC-compliant email validator. Real-world email syntax is much more complicated.

---

# 56. PostgreSQL Regex

PostgreSQL has very convenient operators.

### Case-sensitive regex

```sql
~
```

Example:

```sql
SELECT *
FROM employees
WHERE name ~ '^A';
```

### Case-insensitive regex

```sql
~*
```

Example:

```sql
SELECT *
FROM employees
WHERE name ~* '^a';
```

---

# 57. PostgreSQL regex NOT

```sql
!~
```

means:

> does not match regex

Case-insensitive:

```sql
!~*
```

Example:

```sql
SELECT *
FROM employees
WHERE name !~ '^A';
```

---

# 58. PostgreSQL regex examples

### Starts with A

```sql
WHERE name ~ '^A'
```

### Ends with n

```sql
WHERE name ~ 'n$'
```

### Exactly 10 digits

```sql
WHERE phone ~ '^[0-9]{10}$'
```

### Letters only

```sql
WHERE name ~ '^[A-Za-z]+$'
```

### Case-insensitive starts with A

```sql
WHERE name ~* '^a'
```

---

# 59. MySQL vs PostgreSQL

This is worth putting directly into your interview notes.

| Task                        | MySQL                                        | PostgreSQL                                      |
| --------------------------- | -------------------------------------------- | ----------------------------------------------- |
| Simple matching             | `LIKE`                                       | `LIKE`                                          |
| Case-insensitive LIKE       | usually collation-dependent                  | `ILIKE`                                         |
| Regex match                 | `REGEXP_LIKE()`                              | `~`                                             |
| Regex case-insensitive      | regex options / collation behavior           | `~*`                                            |
| Regex NOT                   | `NOT REGEXP_LIKE()`                          | `!~`                                            |
| Regex NOT, case-insensitive | `NOT REGEXP_LIKE()` with appropriate options | `!~*`                                           |
| Regex replacement           | `REGEXP_REPLACE()`                           | `REGEXP_REPLACE()`                              |
| Regex substring             | `REGEXP_SUBSTR()` in modern MySQL            | `substring(... from pattern)` / regex functions |

---

# 60. `LIKE` vs Regex: when should I use which?

This is one of the most important interview decisions.

## Use `LIKE` when:

The requirement is simple:

```text
starts with
ends with
contains
doesn't contain
fixed position
simple wildcard
```

Examples:

```sql
WHERE name LIKE 'A%'
```

```sql
WHERE email LIKE '%.com'
```

```sql
WHERE product_name LIKE '%phone%'
```

---

## Use Regex when:

You need:

```text
OR alternatives
character classes
repetition
exact digit counts
complex structure
validation
optional characters
multiple rules
```

Example:

```regex
^[0-9]{10}$
```

is much cleaner than trying to construct a complicated `LIKE` pattern.

---

# 61. Interview decision tree

When you see a question:

### Step 1

Ask:

> Is this just starts/ends/contains?

Use:

```sql
LIKE
```

### Step 2

Ask:

> Do I need exactly N characters?

Could use:

```sql
LIKE '_____'
```

or preferably:

```sql
LENGTH(...)
```

depending on the question.

### Step 3

Ask:

> Do I need character classes / repetition / OR?

Use:

```text
REGEX
```

### Step 4

Ask:

> Does case matter?

PostgreSQL:

```sql
LIKE
ILIKE
~
~*
```

MySQL:

> Check collation and use appropriate regex/case-handling behavior.

---

# 62. Classic interview problem: names starting with A

### MySQL

```sql
SELECT name
FROM employees
WHERE name LIKE 'A%';
```

### PostgreSQL

```sql
SELECT name
FROM employees
WHERE name LIKE 'A%';
```

If PostgreSQL should ignore case:

```sql
SELECT name
FROM employees
WHERE name ILIKE 'a%';
```

---

# 63. Classic problem: emails ending `.com`

```sql
SELECT *
FROM employees
WHERE email LIKE '%.com';
```

This is better than Regex because the requirement is simple.

---

# 64. Classic problem: email contains Gmail

```sql
SELECT *
FROM employees
WHERE email LIKE '%@gmail.com';
```

This is more precise than:

```sql
LIKE '%gmail%'
```

because you're explicitly looking for the domain.

---

# 65. Classic problem: phone has exactly 10 digits

### MySQL

```sql
SELECT *
FROM employees
WHERE REGEXP_LIKE(phone, '^[0-9]{10}$');
```

### PostgreSQL

```sql
SELECT *
FROM employees
WHERE phone ~ '^[0-9]{10}$';
```

---

# 66. Classic problem: phone starts with 9

Simple:

```sql
WHERE phone LIKE '9%'
```

No regex necessary.

---

# 67. Classic problem: phone starts with 98 and exactly 10 digits

Regex:

```regex
^98[0-9]{8}$
```

Why 8?

```text
98
+
8 more digits
=
10 digits
```

### MySQL

```sql
WHERE REGEXP_LIKE(phone, '^98[0-9]{8}$')
```

### PostgreSQL

```sql
WHERE phone ~ '^98[0-9]{8}$'
```

---

# 68. Classic problem: PIN code

Suppose:

> Find exactly 6-digit PIN codes.

```regex
^[0-9]{6}$
```

MySQL:

```sql
SELECT *
FROM users
WHERE REGEXP_LIKE(pin, '^[0-9]{6}$');
```

PostgreSQL:

```sql
SELECT *
FROM users
WHERE pin ~ '^[0-9]{6}$';
```

---

# 69. Classic problem: product code

Suppose valid codes look like:

```text
AB123
CD999
XY456
```

Requirement:

```text
2 uppercase letters
+
3 digits
```

Regex:

```regex
^[A-Z]{2}[0-9]{3}$
```

---

# 70. Classic problem: employee IDs

Suppose:

```text
EMP001
EMP002
EMP999
```

Pattern:

```regex
^EMP[0-9]{3}$
```

MySQL:

```sql
WHERE REGEXP_LIKE(employee_code, '^EMP[0-9]{3}$')
```

PostgreSQL:

```sql
WHERE employee_code ~ '^EMP[0-9]{3}$'
```

---

# 71. Classic problem: order IDs

Suppose:

```text
ORD-1001
ORD-2005
ORD-9999
```

Regex:

```regex
^ORD-[0-9]{4}$
```

---

# 72. Classic problem: username

Suppose username must:

```text
start with a letter
then contain letters, numbers, _
length 5–15
```

A simplified pattern:

```regex
^[A-Za-z][A-Za-z0-9_]{4,14}$
```

Why?

First character:

```regex
[A-Za-z]
```

Remaining:

```regex
[A-Za-z0-9_]{4,14}
```

Total:

```text
5–15 characters
```

---

# 73. Negated character class

Suppose:

> Find strings containing a character that isn't a digit.

You can use:

```regex
[^0-9]
```

Meaning:

> one character that is NOT 0–9.

Important:

```regex
[^abc]
```

does **not** mean "not the entire string abc."

It means:

> one character that is not a, b, or c.

---

# 74. `^` has two meanings

This is a classic regex trap.

At the **beginning**:

```regex
^abc
```

means:

> starts with abc

Inside brackets:

```regex
[^abc]
```

means:

> a character that is NOT a, b, or c

Same symbol, different meaning.

---

# 75. Regex `.`

`.` means:

> any single character

Example:

```regex
a.c
```

can match:

```text
abc
a1c
a-c
a c
```

because the middle character can be any character.

If you actually need a literal period, escape it:

```regex
\.
```

This matters for email patterns.

---

# 76. Regex email dot

Don't confuse:

```regex
.
```

with:

```regex
\.
```

This:

```regex
gmail.com
```

allows any character between `gmail` and `com`.

This:

```regex
gmail\.com
```

means an actual period.

---

# 77. SQL pattern matching with NULL

This is a very important SQL concept.

Suppose:

```text
email
----------------
alice@gmail.com
NULL
bob@yahoo.com
```

Then:

```sql
WHERE email LIKE '%gmail%'
```

does **not** return the NULL row.

Why?

Because:

```sql
NULL LIKE '%gmail%'
```

is:

```text
UNKNOWN
```

not:

```text
TRUE
```

SQL `WHERE` keeps only rows where the condition evaluates to TRUE.

So:

```sql
WHERE email LIKE '%gmail%'
```

naturally excludes NULL.

---

# 78. If you explicitly care about NULL

Suppose:

> Find rows where email either contains Gmail OR is NULL.

```sql
WHERE email LIKE '%gmail%'
   OR email IS NULL;
```

This is a very common interview trick.

---

# 79. Pattern matching with `CASE`

You can classify records.

Example:

```sql
SELECT
    name,
    CASE
        WHEN email LIKE '%@gmail.com' THEN 'Gmail'
        WHEN email LIKE '%@yahoo.com' THEN 'Yahoo'
        ELSE 'Other'
    END AS email_type
FROM employees;
```

Output:

| name    | email_type |
| ------- | ---------- |
| Alice   | Gmail      |
| Bob     | Yahoo      |
| Anita   | Gmail      |
| Charlie | Other      |

---

# 80. Pattern matching + aggregation

Now we combine concepts.

Question:

> Count Gmail users.

```sql
SELECT COUNT(*) AS gmail_users
FROM employees
WHERE email LIKE '%@gmail.com';
```

Or:

```sql
SELECT
    COUNT(*) AS gmail_users
FROM employees
WHERE email LIKE '%gmail%';
```

The first is usually more precise.

---

# 81. Conditional aggregation

Question:

> Count Gmail and Yahoo users separately.

```sql
SELECT
    SUM(CASE WHEN email LIKE '%@gmail.com' THEN 1 ELSE 0 END) AS gmail_users,
    SUM(CASE WHEN email LIKE '%@yahoo.com' THEN 1 ELSE 0 END) AS yahoo_users
FROM employees;
```

This is a powerful interview pattern.

---

# 82. Pattern matching + GROUP BY

Question:

> Count users by email domain.

A simple approach:

```sql
SELECT
    CASE
        WHEN email LIKE '%@gmail.com' THEN 'Gmail'
        WHEN email LIKE '%@yahoo.com' THEN 'Yahoo'
        ELSE 'Other'
    END AS domain_type,
    COUNT(*) AS users
FROM employees
GROUP BY
    CASE
        WHEN email LIKE '%@gmail.com' THEN 'Gmail'
        WHEN email LIKE '%@yahoo.com' THEN 'Yahoo'
        ELSE 'Other'
    END;
```

Output:

| domain_type | users |
| ----------- | ----: |
| Gmail       |     3 |
| Yahoo       |     1 |
| Other       |     2 |

---

# 83. `LIKE` with dates/numbers — be careful

Suppose:

```sql
WHERE order_date LIKE '2026%'
```

This may be possible if you're dealing with textual representation, but it is usually **not the correct approach for DATE/TIMESTAMP columns**.

For dates, use date functions/ranges:

```sql
WHERE order_date >= '2026-01-01'
  AND order_date < '2027-01-01'
```

Don't use pattern matching just because something visually looks like text.

---

# 84. `LIKE` with numeric columns

Bad:

```sql
WHERE salary LIKE '10%'
```

if `salary` is numeric.

Better:

```sql
WHERE salary >= 10000
  AND salary < 11000;
```

Pattern matching is primarily for textual data.

---

# 85. Performance: the big `LIKE` rule

This is extremely important for interviews.

Compare:

```sql
WHERE name LIKE 'John%'
```

with:

```sql
WHERE name LIKE '%John%'
```

The first has a **fixed prefix**.

The second begins with a wildcard.

In many database/index configurations:

```text
'John%'
```

can potentially use an index efficiently.

But:

```text
'%John%'
```

usually cannot use an ordinary B-tree index for a normal prefix lookup because the beginning is unknown.

Interview answer:

> **Leading `%` often prevents efficient use of a normal B-tree index.**

---

# 86. Prefix search

Good for indexes:

```sql
WHERE name LIKE 'John%'
```

Because the database knows:

```text
starts somewhere in the John... range
```

---

# 87. Contains search

Potentially expensive:

```sql
WHERE name LIKE '%John%'
```

because John could occur anywhere:

```text
John
BigJohn
Johnny
MrJohnSmith
```

For large datasets, specialized text-search/trigram/full-text indexes may be more appropriate depending on the database and query.

---

# 88. PostgreSQL: `pg_trgm`

For PostgreSQL, substring/fuzzy text searches can often benefit from the `pg_trgm` extension and appropriate indexes.

Conceptually:

```sql
CREATE EXTENSION IF NOT EXISTS pg_trgm;
```

Then a trigram index can support many `LIKE '%something%'` searches.

For example:

```sql
CREATE INDEX idx_users_name_trgm
ON users
USING gin (name gin_trgm_ops);
```

Then:

```sql
WHERE name ILIKE '%john%'
```

can be dramatically more suitable for large datasets than relying on a plain B-tree index.

This is an **advanced performance topic**, not something you need for basic SQL interviews.

---

# 89. MySQL Regex performance

Regex can be much more computationally expensive than a simple prefix `LIKE`.

So don't automatically do:

```sql
REGEXP_LIKE(name, '^John')
```

when:

```sql
name LIKE 'John%'
```

is sufficient.

General principle:

> Use the simplest predicate that accurately expresses the requirement.

---

# 90. A very important interview distinction

Question:

> Find names starting with "A".

Possible:

```sql
LIKE 'A%'
```

or:

```regex
^A
```

Which is better?

Usually:

```sql
LIKE 'A%'
```

because:

- simpler
- clearer
- often more index-friendly
- communicates intent directly

Regex isn't automatically "more advanced = better."

---

# 91. Common mistake: `%` in Regex

Wrong:

```sql
REGEXP_LIKE(name, 'A%')
```

if you intend SQL `LIKE` semantics.

Regex `%` is generally just a literal percent character.

Regex equivalent of:

```sql
LIKE 'A%'
```

is:

```regex
^A
```

or, if you want to explicitly express anything afterward:

```regex
^A.*
```

---

# 92. Common mistake: `_` in Regex

In `LIKE`:

```sql
'A_'
```

means:

```text
A + exactly one character
```

But in Regex:

```regex
A_
```

the `_` is generally just a literal underscore.

Regex equivalent is:

```regex
^A.$
```

for exactly two characters where the first is A.

---

# 93. `LIKE` → Regex translation table

| SQL `LIKE` | Regex equivalent |
| ---------- | ---------------- |
| `'A%'`     | `^A` / `^A.*`    |
| `'%A'`     | `A$` / `.*A$`    |
| `'%A%'`    | `A` / `.*A.*`    |
| `'A_'`     | `^A.$`           |
| `'_A%'`    | `^.A`            |
| `'A____'`  | `^A....$`        |
| `'%.com'`  | `\.com$`         |
| `'___'`    | `^...$`          |

The exact regex depends on whether the regex API searches for a matching substring or requires the whole expression to match, so anchors are important.

---

# 94. Advanced: Regex replacement

Pattern matching isn't only about filtering.

You can also **replace** matching text.

Both MySQL and PostgreSQL support `REGEXP_REPLACE()` in modern versions.

Conceptually:

```sql
REGEXP_REPLACE(text, pattern, replacement)
```

Example:

```text
abc123xyz
```

Replace digits with `#`:

```regex
[0-9]+
```

Result:

```text
abc#xyz
```

---

# 95. PostgreSQL example

```sql
SELECT REGEXP_REPLACE(
    'abc123xyz',
    '[0-9]+',
    '#'
);
```

Result:

```text
abc#xyz
```

---

# 96. MySQL example

```sql
SELECT REGEXP_REPLACE(
    'abc123xyz',
    '[0-9]+',
    '#'
);
```

Result:

```text
abc#xyz
```

---

# 97. Extracting patterns

PostgreSQL has rich regex functionality for extracting matches.

For example:

```sql
SELECT regexp_matches(
    'Order ID: 12345',
    '[0-9]+'
);
```

can return the matching digit sequence.

PostgreSQL's regex functions are particularly powerful for text processing.

---

# 98. MySQL `REGEXP_SUBSTR`

Modern MySQL supports:

```sql
REGEXP_SUBSTR()
```

Conceptually:

```sql
REGEXP_SUBSTR(string, pattern)
```

Example:

```sql
SELECT REGEXP_SUBSTR(
    'Order ID: 12345',
    '[0-9]+'
);
```

Result:

```text
12345
```

---

# 99. Three levels of pattern matching

Think of SQL pattern matching as three levels:

### Level 1 — Exact

```sql
WHERE name = 'Alice'
```

### Level 2 — Simple pattern

```sql
WHERE name LIKE 'A%'
```

### Level 3 — Complex pattern

```sql
WHERE name ~ '^[A-Z][a-z]{4,10}$'
```

or MySQL:

```sql
WHERE REGEXP_LIKE(name, '^[A-Z][a-z]{4,10}$')
```

This hierarchy is useful in interviews.

---

# 100. A practical interview framework

When you get a pattern-matching question, do this:

### Step 1 — Identify the data type

Is it:

```text
VARCHAR/TEXT?
DATE?
NUMBER?
```

Pattern matching generally applies to text.

---

### Step 2 — Identify the English condition

Look for words:

```text
starts
ends
contains
exactly
only
either
optional
digits
letters
before
after
```

---

### Step 3 — Decide `LIKE` or Regex

Simple:

```text
starts / ends / contains
```

→ `LIKE`

Complex:

```text
digits
ranges
repetition
OR
validation
```

→ Regex

---

### Step 4 — Build the pattern

Example:

> Exactly 8 digits.

Translate:

```text
start
+
digit
× 8
+
end
```

→

```regex
^[0-9]{8}$
```

---

### Step 5 — Check edge cases

Ask:

```text
What about NULL?
What about empty string?
What about uppercase/lowercase?
What about spaces?
What about special characters?
```

---

# 101. Pattern-matching interview questions

Here are the patterns you should be able to solve immediately.

---

## Q1. Starts with A

```sql
WHERE name LIKE 'A%'
```

---

## Q2. Ends with A

```sql
WHERE name LIKE '%A'
```

---

## Q3. Contains A

```sql
WHERE name LIKE '%A%'
```

---

## Q4. Doesn't contain A

```sql
WHERE name NOT LIKE '%A%'
```

---

## Q5. Exactly 5 characters

```sql
WHERE name LIKE '_____'
```

or:

```sql
WHERE CHAR_LENGTH(name) = 5   -- MySQL
```

```sql
WHERE LENGTH(name) = 5        -- PostgreSQL
```

---

## Q6. Second character is A

```sql
WHERE name LIKE '_A%'
```

---

## Q7. Third character is A

```sql
WHERE name LIKE '__A%'
```

---

## Q8. Ends in `.com`

```sql
WHERE email LIKE '%.com'
```

---

## Q9. Gmail addresses

```sql
WHERE email LIKE '%@gmail.com'
```

---

## Q10. Exactly 10 digits

### MySQL

```sql
WHERE REGEXP_LIKE(phone, '^[0-9]{10}$')
```

### PostgreSQL

```sql
WHERE phone ~ '^[0-9]{10}$'
```

---

# 102. Q11. Only letters

### MySQL

```sql
WHERE REGEXP_LIKE(name, '^[A-Za-z]+$')
```

### PostgreSQL

```sql
WHERE name ~ '^[A-Za-z]+$'
```

---

# 103. Q12. Only alphanumeric

```regex
^[A-Za-z0-9]+$
```

MySQL:

```sql
WHERE REGEXP_LIKE(code, '^[A-Za-z0-9]+$')
```

PostgreSQL:

```sql
WHERE code ~ '^[A-Za-z0-9]+$'
```

---

# 104. Q13. Starts with A and ends with Z

```regex
^A.*Z$
```

---

# 105. Q14. Exactly 6 characters, first 2 letters, last 4 digits

```regex
^[A-Za-z]{2}[0-9]{4}$
```

---

# 106. Q15. Employee ID `EMP` + 4 digits

```regex
^EMP[0-9]{4}$
```

---

# 107. Q16. Product code: 3 uppercase letters + 3 digits

```regex
^[A-Z]{3}[0-9]{3}$
```

---

# 108. Q17. Contains either `sql` or `database`

Regex:

```regex
sql|database
```

PostgreSQL:

```sql
WHERE description ~ 'sql|database'
```

MySQL:

```sql
WHERE REGEXP_LIKE(description, 'sql|database')
```

---

# 109. Q18. Starts with A, B, or C

With `LIKE`:

```sql
WHERE name LIKE 'A%'
   OR name LIKE 'B%'
   OR name LIKE 'C%';
```

Regex:

```regex
^[ABC]
```

PostgreSQL:

```sql
WHERE name ~ '^[ABC]'
```

MySQL:

```sql
WHERE REGEXP_LIKE(name, '^[ABC]')
```

Regex is much cleaner here.

---

# 110. Q19. Doesn't start with A

Regex:

```regex
^[^A]
```

But be careful with empty strings.

For robust "first character is not A" logic, think about whether empty strings should qualify.

This is a good example of why **requirements matter more than blindly memorizing patterns**.

---

# 111. Q20. Contains at least one digit

Regex:

```regex
[0-9]
```

MySQL:

```sql
WHERE REGEXP_LIKE(code, '[0-9]')
```

PostgreSQL:

```sql
WHERE code ~ '[0-9]'
```

---

# 112. Q21. Contains no digits

MySQL:

```sql
WHERE NOT REGEXP_LIKE(code, '[0-9]')
```

PostgreSQL:

```sql
WHERE code !~ '[0-9]'
```

But again, if `NULL` matters:

```text
NULL
```

will not behave like a normal non-matching string.

---

# 113. Q22. Contains at least one uppercase letter

```regex
[A-Z]
```

---

# 114. Q23. Contains at least one lowercase letter

```regex
[a-z]
```

---

# 115. Q24. Contains at least one special character

A simplistic ASCII-style pattern might be:

```regex
[^A-Za-z0-9]
```

Meaning:

> contains at least one character outside letters and digits.

But "special character" must be defined by the problem.

---

# 116. Q25. Password-style validation

Suppose requirement:

```text
8+ characters
at least one uppercase
at least one lowercase
at least one digit
```

This is where regex becomes much more complex, especially because regex dialects differ in support for lookaheads and related constructs.

For SQL interviews, don't blindly memorize one universal password regex.

Instead, it can often be clearer to express the requirements separately:

```sql
WHERE LENGTH(password) >= 8
  AND password ~ '[A-Z]'
  AND password ~ '[a-z]'
  AND password ~ '[0-9]'
```

PostgreSQL example.

This is an excellent interview technique:

> **Break a complicated pattern into multiple simple predicates when that improves readability.**

---

# 117. Multiple Regex predicates

Instead of one enormous regex:

```sql
WHERE password ~ ...
```

you can do:

```sql
WHERE LENGTH(password) >= 8
  AND password ~ '[A-Z]'
  AND password ~ '[a-z]'
  AND password ~ '[0-9]';
```

This is often easier to explain and maintain.

---

# 118. This is a major SQL interview principle

Don't try to solve everything with one expression.

For example:

```sql
WHERE
    name LIKE 'A%'
    AND email LIKE '%@gmail.com'
    AND phone ~ '^[0-9]{10}$';
```

This is perfectly legitimate.

SQL allows you to combine:

```text
LIKE
+
REGEX
+
AND/OR
+
CASE
+
functions
```

---

# 119. Pattern matching + JOIN

Suppose:

```text
customers
customer_id | name | email
```

and:

```text
blocked_domains
domain
-----------
spam.com
fake.com
```

You could use pattern matching in a join:

```sql
SELECT c.*
FROM customers c
JOIN blocked_domains b
  ON c.email LIKE '%@' || b.domain;
```

In PostgreSQL, `||` concatenates strings.

MySQL uses:

```sql
CONCAT()
```

Example:

```sql
ON c.email LIKE CONCAT('%@', b.domain)
```

This is an advanced but realistic SQL pattern.

---

# 120. Pattern matching + CASE + GROUP BY

Suppose you want:

```text
Gmail
Yahoo
Other
```

and counts.

PostgreSQL/MySQL-style:

```sql
SELECT
    CASE
        WHEN email LIKE '%@gmail.com' THEN 'Gmail'
        WHEN email LIKE '%@yahoo.com' THEN 'Yahoo'
        ELSE 'Other'
    END AS provider,
    COUNT(*) AS total
FROM employees
GROUP BY
    CASE
        WHEN email LIKE '%@gmail.com' THEN 'Gmail'
        WHEN email LIKE '%@yahoo.com' THEN 'Yahoo'
        ELSE 'Other'
    END;
```

This combines three major SQL concepts:

```text
Pattern matching
+
CASE
+
GROUP BY
```

---

# 121. Common mistakes

## Mistake 1

Writing:

```sql
LIKE 'A'
```

when you mean starts with A.

Wrong because:

```sql
'A'
```

means exactly A.

Correct:

```sql
'A%'
```

---

## Mistake 2

Using:

```sql
LIKE '%A'
```

when you mean contains A.

That means:

> ends with A.

Contains:

```sql
LIKE '%A%'
```

---

## Mistake 3

Confusing `%` and `_`

```text
% → 0 or more
_ → exactly 1
```

---

## Mistake 4

Using LIKE syntax inside Regex

Wrong conceptually:

```regex
A%
```

Correct:

```regex
^A
```

---

## Mistake 5

Forgetting anchors

You want exactly 10 digits:

```regex
[0-9]{10}
```

Better:

```regex
^[0-9]{10}$
```

---

## Mistake 6

Forgetting PostgreSQL `ILIKE`

If case-insensitive text matching is required:

```sql
ILIKE
```

is often the simplest answer.

---

## Mistake 7

Ignoring NULL

Remember:

```sql
NULL LIKE '%x%'
```

is not TRUE.

---

## Mistake 8

Using regex when LIKE is enough

Prefer:

```sql
LIKE 'John%'
```

over unnecessarily complicated regex when the requirement is just prefix matching.

---

# 122. The ultimate mental model

Think about pattern matching as:

```text
                    PATTERN MATCHING
                           │
              ┌────────────┴────────────┐
              │                         │
            LIKE                      REGEX
              │                         │
       ┌──────┼──────┐          ┌───────┼────────┐
       │      │      │          │       │        │
    starts  ends  contains   classes repetition OR
       │      │      │          │       │        │
      A%    %A    %A%       [0-9]    {10}       |
```

---

# 123. The English → SQL translation method

This is the part I would memorize for interviews.

### "starts with"

```text
A
↓
A%
```

### "ends with"

```text
A
↓
%A
```

### "contains"

```text
A
↓
%A%
```

### "exactly N characters"

```text
N underscores
```

### "second character is A"

```text
_A%
```

### "exactly N digits"

```text
^[0-9]{N}$
```

### "only letters"

```text
^[A-Za-z]+$
```

### "starts with A and ends with Z"

```text
^A.*Z$
```

### "A or B or C"

```text
^[ABC]
```

### "contains a digit"

```text
[0-9]
```

### "doesn't contain a digit"

```text
NOT ... [0-9]
```

---

# 124. MySQL mini-cheatsheet

### LIKE

```sql
WHERE name LIKE 'A%'
WHERE name LIKE '%A'
WHERE name LIKE '%A%'
WHERE name LIKE '_A%'
WHERE name LIKE '_____'
```

### NOT LIKE

```sql
WHERE name NOT LIKE '%A%'
```

### Regex

```sql
WHERE REGEXP_LIKE(name, '^A')
WHERE REGEXP_LIKE(name, 'A$')
WHERE REGEXP_LIKE(phone, '^[0-9]{10}$')
WHERE REGEXP_LIKE(code, '^[A-Z]{3}[0-9]{3}$')
```

### Regex replace

```sql
REGEXP_REPLACE(text, pattern, replacement)
```

### Regex extract

```sql
REGEXP_SUBSTR(text, pattern)
```

---

# 125. PostgreSQL mini-cheatsheet

### LIKE

```sql
WHERE name LIKE 'A%'
```

### Case-insensitive LIKE

```sql
WHERE name ILIKE 'a%'
```

### Regex

```sql
WHERE name ~ '^A'
```

### Case-insensitive regex

```sql
WHERE name ~* '^a'
```

### NOT regex

```sql
WHERE name !~ '^A'
```

### NOT case-insensitive regex

```sql
WHERE name !~* '^a'
```

### Regex replace

```sql
REGEXP_REPLACE(text, pattern, replacement)
```

### Regex extraction

```sql
regexp_matches(...)
```

---

# 126. One-page interview cheatsheet

```text
====================================================
SQL PATTERN MATCHING CHEATSHEET
====================================================

LIKE
----------------------------------------------------
% = zero or more characters
_ = exactly one character

Starts A:
    LIKE 'A%'

Ends A:
    LIKE '%A'

Contains A:
    LIKE '%A%'

Doesn't contain A:
    NOT LIKE '%A%'

Second character A:
    LIKE '_A%'

Third character A:
    LIKE '__A%'

Second-last character A:
    LIKE '%A_'

Exactly 3 chars:
    LIKE '___'

Exactly 5 chars:
    LIKE '_____'

5 chars starting A:
    LIKE 'A____'

Ends .com:
    LIKE '%.com'

Gmail:
    LIKE '%@gmail.com'


REGEX
----------------------------------------------------
.        any character
^        beginning
$        end
[]       character class
[^]      negated character class
+        one or more
*        zero or more
?        zero or one
{n}      exactly n
{n,m}    n to m
|        OR
()       grouping

Starts A:
    ^A

Ends A:
    A$

Contains digit:
    [0-9]

Only digits:
    ^[0-9]+$

Exactly 10 digits:
    ^[0-9]{10}$

Only letters:
    ^[A-Za-z]+$

Only alphanumeric:
    ^[A-Za-z0-9]+$

Exactly 5 digits:
    ^[0-9]{5}$

5-10 digits:
    ^[0-9]{5,10}$

3 uppercase + 3 digits:
    ^[A-Z]{3}[0-9]{3}$

Starts A and ends Z:
    ^A.*Z$

A/B/C:
    ^[ABC]

Cat OR dog:
    cat|dog

Exactly cat or dog:
    ^(cat|dog)$


MYSQL
----------------------------------------------------
LIKE:
    column LIKE 'A%'

Regex:
    REGEXP_LIKE(column, '^A')

Regex replacement:
    REGEXP_REPLACE()

Regex substring:
    REGEXP_SUBSTR()


POSTGRESQL
----------------------------------------------------
LIKE:
    column LIKE 'A%'

Case-insensitive LIKE:
    column ILIKE 'a%'

Regex:
    column ~ '^A'

Case-insensitive regex:
    column ~* '^a'

NOT regex:
    column !~ '^A'

NOT case-insensitive regex:
    column !~* '^a'


PERFORMANCE
----------------------------------------------------
Good prefix:
    LIKE 'John%'

Often expensive:
    LIKE '%John%'

Reason:
    Leading % makes ordinary B-tree prefix
    lookup much harder/impossible.

Use simplest pattern that solves problem.


NULL
----------------------------------------------------
NULL LIKE '%abc%'
        ↓
     UNKNOWN

WHERE keeps TRUE rows.

Need NULL too:
    WHERE email LIKE '%gmail%'
       OR email IS NULL


DECISION
----------------------------------------------------
Starts / ends / contains?
        ↓
      LIKE

Complex structure?
        ↓
      REGEX

Need case-insensitive PostgreSQL?
        ↓
      ILIKE / ~*

Need exactly N digits?
        ↓
      ^[0-9]{N}$

Need only letters?
        ↓
      ^[A-Za-z]+$
====================================================
```

## The 10 patterns I'd memorize first

If you're preparing for SQL interviews, get these **10** completely automatic:

```sql
-- 1. Starts with A
LIKE 'A%'

-- 2. Ends with A
LIKE '%A'

-- 3. Contains A
LIKE '%A%'

-- 4. Second character A
LIKE '_A%'

-- 5. Exactly 5 characters
LIKE '_____'

-- 6. Exactly 10 digits
^[0-9]{10}$

-- 7. Only letters
^[A-Za-z]+$

-- 8. Only alphanumeric
^[A-Za-z0-9]+$

-- 9. Starts A, ends Z
^A.*Z$

-- 10. A/B/C at beginning
^[ABC]
```

The **big interview trick** is not memorizing hundreds of regexes. It's learning to translate:

> **English → constraint → pattern → SQL predicate**

For example:

> "Find product codes with exactly 2 uppercase letters followed by 4 digits."

Immediately think:

```text
2 uppercase letters → [A-Z]{2}
4 digits            → [0-9]{4}
whole string        → ^ ... $
```

Therefore:

```regex
^[A-Z]{2}[0-9]{4}$
```

Then wrap it in the database-specific syntax:

**MySQL**

```sql
WHERE REGEXP_LIKE(product_code, '^[A-Z]{2}[0-9]{4}$')
```

**PostgreSQL**

```sql
WHERE product_code ~ '^[A-Z]{2}[0-9]{4}$'
```

That translation process is the real skill.
