# Short Response: JOIN Queries and Connecting to Postgres

Answer each question below. Write in complete sentences (3–5 per answer).

---

## Question 1

What is the difference between `INNER JOIN` and `LEFT JOIN`? Give a concrete example of when you would use each.

**Your answer:**

`INNER JOIN` returns rows from two tables where the join condition is true, rows without a match are excluded. This is useful when you only want to see data that has a match in both tables, such as finding students who are enrolled in math class. `LEFT JOIN` returns all rows from the left table regardless of if they have a match in the right table, rows with no match return `NULL`. This is useful when you want to find the students that are and aren’t enrolled in math class.

## Question 2

Look at this query. What will it return, and why do users with zero bookmarks still appear in the results?

```sql
SELECT users.username, COUNT(bookmarks.bookmark_id) AS total_bookmarks
FROM users
LEFT JOIN bookmarks ON users.user_id = bookmarks.user_id
GROUP BY users.user_id;
```

**Your answer:**

This will return a table with a ***username*** column and a ***total_bookmarks*** column. Each row will show the number of bookmarks each user has, including users that have no bookmarks.
Users with zero bookmarks still appear because we used` LEFT JOIN`. `LEFT JOIN` includes all users regardless of if they have bookmarks or not because `LEFT JOIN` return rows with no match as NULL, instead of excluding the row from the final result like `INNER JOIN`.

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