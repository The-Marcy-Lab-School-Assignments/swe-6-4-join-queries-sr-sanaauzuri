# Short Response: JOIN Queries and Connecting to Postgres

Answer each question below. Write in complete sentences (3–5 per answer).

---

## Question 1

What is the difference between `INNER JOIN` and `LEFT JOIN`? Give a concrete example of when you would use each.

**Your answer:**

---

## Question 2

Look at this query. What will it return, and why do users with zero bookmarks still appear in the results?

```sql
SELECT users.username, COUNT(bookmarks.bookmark_id) AS total_bookmarks
FROM users
LEFT JOIN bookmarks ON users.user_id = bookmarks.user_id
GROUP BY users.user_id;
```

**Your answer:**

---

## Question 3

What is the `pg` library and why can't you write SQL directly in a `.js` file without it? And what is a connection pool?

**Your answer:**



## Question 4

What is a parameterized query and what problem does it solve? Rewrite the unsafe query below as a safe parameterized query using `pg`:

```js
// Unsafe — never do this!
pool.query(`SELECT * FROM users WHERE username = '${username}'`);
```

**Your answer:**

A **parameterized query** uses placeholders like `$NUM` instead of directly inserting user input into a SQL string. This solves **SQL injection**, a common and destructive web vulnerability.  **SQL injection** is when a malicious user can pass in something like `DROP TABLE - - `as a value, and without a parameterized query, this is executed and can delete a table from your database. Using placeholders keeps the SQL string and the values separate, so Postgres treats the input as a value to be compared against your table rather than as a command to run. This means even if a malicious user tries to pass in a destructive query, it won't execute.

```js
// Safe — parameterized query
pool.query(`SELECT * FROM users WHERE username = $1`,[username]);
```