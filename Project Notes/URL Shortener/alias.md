It means that if your URL shortener allows users to choose their own short URL (called a **custom alias**), the feature seems straightforward at first but introduces design problems that are common interview questions.

For example, instead of generating:

```
https://short.ly/aB3xYz
```

a user wants:

```
https://short.ly/myblog
```

### The main edge cases

**1. Alias already exists (collision)**

Two users both want:

```
https://short.ly/sale
```

Only one can have it.

Questions interviewers may ask:

* Should the second user get an error?
* Should they be offered alternatives (`sale1`, `sale2026`)?
* Can aliases ever be reused?

---

**2. Race conditions**

Imagine two users click "Create" for the alias `promo` at almost the same time.

Without proper handling:

* Both systems check:

  ```
  promo doesn't exist
  ```
* Both try to insert it.

Now you've got a conflict.

The solution is usually:

* Put a **UNIQUE constraint** on the alias column in the database.
* Let only one insert succeed.
* The other request receives a "Alias already taken" response.

Interviewers like this because it tests whether you think about concurrency.

---

**3. Reserved words**

Users shouldn't be allowed to create aliases like:

```
/login
/admin
/api
/help
```

These may already be routes used by your application.

So you maintain a reserved-word list.

---

**4. Offensive or protected names**

Should users be allowed to claim:

```
google
amazon
microsoft
facebook
```

Or offensive words?

Many production systems block certain aliases.

---

**5. Case sensitivity**

Are these the same?

```
/John
/john
/JOHN
```

Most URL shorteners treat aliases as **case-insensitive** to avoid confusion.

---

**6. Updating aliases**

Suppose someone changes:

```
/myblog
```

to

```
/techblog
```

What happens to the old alias?

Possible choices:

* Redirect old → new
* Delete the old alias
* Keep both working

Each has trade-offs.

---

**7. Alias expiration**

If a user deletes:

```
/summer-sale
```

Can someone else immediately claim it?

Some systems:

* Never reuse aliases.
* Reuse them after a waiting period.
* Reuse them immediately (risking broken links).

---

### Why interviewers like this topic

It's a simple feature that reveals whether you think beyond the "happy path." They may ask questions such as:

* How do you prevent duplicate aliases?
* What happens if two users request the same alias simultaneously?
* Should aliases be case-sensitive?
* How do you handle reserved words?
* Can aliases be renamed or deleted?
* Can aliases be reused after deletion?

A strong answer is that you'd enforce uniqueness with a database unique index, validate aliases against reserved or prohibited names, define clear case-sensitivity rules, and handle concurrent requests by relying on the database's uniqueness guarantee rather than only checking in application code.
