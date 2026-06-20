# SQL Revision Notes

## SQL Index

# Question

What is an index in SQL?

# Senior-Level Answer

An index is a database structure that improves the speed of data retrieval operations on a table.

It works like an index in a book. Instead of scanning every row, the database can use the index to find matching rows faster.

An index does not automatically mean the values are unique. An index can be unique or non-unique. A unique index enforces uniqueness, but a normal index only improves lookup performance.

Indexes improve read performance but can slow down write operations because the database must update the index when rows are inserted, updated, or deleted.

# Example

```sql
CREATE INDEX IX_Users_Email
ON Users (Email);
```

This helps queries that search users by email.

```sql
SELECT *
FROM Users
WHERE Email = 'test@example.com';
```

# 15-Second Version

An index improves query performance by helping the database find rows faster. It is not always unique. Unique indexes enforce uniqueness; normal indexes just improve lookup speed.

---

## DELETE vs TRUNCATE

# Question

What is the difference between DELETE and TRUNCATE in SQL?

# Senior-Level Answer

`DELETE` removes rows from a table and supports a `WHERE` condition. It is logged row by row and can remove selected records.

`TRUNCATE` removes all rows from a table. It does not support a `WHERE` condition and is usually faster because it deallocates data pages instead of deleting rows one by one.

`TRUNCATE` usually resets the identity value, while `DELETE` usually does not. `TRUNCATE` may also fail if the table is referenced by a foreign key.

# Example

```sql
DELETE FROM Users
WHERE IsActive = 0;
```

Removes only inactive users.

```sql
TRUNCATE TABLE Users;
```

Removes all rows from the table.

# 15-Second Version

`DELETE` can remove selected rows using `WHERE`. `TRUNCATE` removes all rows, is usually faster, and often resets identity. Use `DELETE` when you need conditions.

---

## INNER JOIN vs LEFT JOIN

# Question

What is the difference between INNER JOIN and LEFT JOIN?

# Senior-Level Answer

`INNER JOIN` returns only rows that have matching records in both tables.

`LEFT JOIN` returns all rows from the left table and matching rows from the right table. If there is no match, the right table columns return `NULL`.

Use `INNER JOIN` when you only want matched records. Use `LEFT JOIN` when you want to keep all records from the main table even if related data is missing.

# Example

```sql
SELECT u.Name, o.Id
FROM Users u
INNER JOIN Orders o ON o.UserId = u.Id;
```

Returns only users who have orders.

```sql
SELECT u.Name, o.Id
FROM Users u
LEFT JOIN Orders o ON o.UserId = u.Id;
```

Returns all users, even users with no orders.

# 15-Second Version

`INNER JOIN` returns only matching rows from both tables. `LEFT JOIN` returns all rows from the left table and matching rows from the right table, with `NULL` if no match exists.
