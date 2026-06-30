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

---

## Index Benefits and Trade-Offs

# Question

How do indexes improve query performance, and what are their benefits?

# Senior-Level Answer

Indexes improve query performance by allowing the database engine to find rows without scanning the entire table.

They are especially useful for columns used in `WHERE`, `JOIN`, `ORDER BY`, and sometimes `GROUP BY` clauses. A good index can reduce I/O, speed up lookups, improve joins, and make sorting more efficient.

The trade-off is that indexes require extra storage and can slow down `INSERT`, `UPDATE`, and `DELETE` operations because the database must maintain the index when data changes.

Indexes should be based on real query patterns, not added blindly.

# Example

```sql
CREATE INDEX IX_Orders_CustomerId_CreatedDate
ON Orders (CustomerId, CreatedDate);
```

This can help queries that filter orders by customer and date.

# 15-Second Version

Indexes speed up reads by helping the database find rows faster, especially for filters, joins, and sorting. The trade-off is extra storage and slower writes.

---

## Clustered vs Non-Clustered Index

# Question

What is the difference between Clustered and Non-Clustered indexes?

# Senior-Level Answer

A clustered index defines the physical or logical order of data rows in a table. Because the table data itself is organized by the clustered index key, a table can have only one clustered index.

A non-clustered index is a separate structure that stores indexed key values with a pointer back to the actual data row. A table can have multiple non-clustered indexes.

Clustered indexes are commonly used on primary keys, but the best key depends on the workload. Non-clustered indexes are useful for frequent search, join, or sorting columns.

# Example

```sql
-- Clustered index, commonly on primary key
CREATE CLUSTERED INDEX IX_Orders_Id
ON Orders (Id);

-- Non-clustered index for search/filter
CREATE NONCLUSTERED INDEX IX_Orders_CustomerId
ON Orders (CustomerId);
```

# 15-Second Version

A clustered index controls the table row order, so there can be only one. A non-clustered index is a separate lookup structure, so a table can have many.

---

## Normalization

# Question

What is normalization, and what is its impact on database design?

# Senior-Level Answer

Normalization is the process of organizing relational database tables to reduce duplication and improve data integrity.

It usually involves splitting data into related tables and connecting them with primary keys and foreign keys. For example, instead of storing customer details repeatedly in every order row, customer data is stored once in a `Customers` table and referenced from `Orders`.

The benefits are less duplication, better consistency, easier updates, and stronger relational design. The trade-off is that highly normalized databases may require more joins, which can make some queries more complex or slower if not indexed properly.

# Example

```text
Before normalization:
Orders(OrderId, CustomerName, CustomerEmail, ProductName)

After normalization:
Customers(CustomerId, Name, Email)
Orders(OrderId, CustomerId)
Products(ProductId, Name)
OrderItems(OrderId, ProductId)
```

# 15-Second Version

Normalization reduces duplication by splitting data into related tables. It improves consistency and integrity, but can increase joins and query complexity.
