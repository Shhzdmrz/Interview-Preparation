# Entity Framework Core Revision Notes

## One-to-Many Relationship

# Question

Explain the One-to-Many relationship in Entity Framework.

# Senior-Level Answer

A One-to-Many relationship means one record in a principal entity is related to many records in a dependent entity.

For example, one `Customer` can have many `Orders`, but each `Order` belongs to one `Customer`. In Entity Framework Core, this is represented using navigation properties and a foreign key.

The principal entity usually has a collection navigation property, and the dependent entity has a reference navigation property plus a foreign key property.

# Example

```csharp
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public ICollection<Order> Orders { get; set; } = new List<Order>();
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public Customer Customer { get; set; }
}
```

This means one customer can have multiple orders.

# 15-Second Version

One-to-Many means one parent record relates to many child records. In EF Core, the parent has a collection navigation, and the child has a foreign key and reference navigation.

---

## One-to-Many Database Representation

# Question

How is a One-to-Many relationship represented in the database?

# Senior-Level Answer

In the database, a One-to-Many relationship is represented using a foreign key column in the child table.

The parent table has a primary key, and the child table stores that value as a foreign key. For example, the `Customers` table has `Id` as the primary key, and the `Orders` table has `CustomerId` as a foreign key.

This allows multiple order rows to reference the same customer row.

# Example

```sql
CREATE TABLE Customers
(
    Id INT PRIMARY KEY,
    Name NVARCHAR(100)
);

CREATE TABLE Orders
(
    Id INT PRIMARY KEY,
    CustomerId INT NOT NULL,
    CONSTRAINT FK_Orders_Customers
        FOREIGN KEY (CustomerId) REFERENCES Customers(Id)
);
```

# 15-Second Version

The child table contains a foreign key pointing to the parent table primary key. Many child rows can reference the same parent row.

---

## Lazy Loading vs Eager Loading

# Question

What is the difference between Lazy Loading and Eager Loading?

# Senior-Level Answer

Eager Loading loads related data as part of the original query, usually using `Include()` and `ThenInclude()`. It is useful when you know you need related data immediately.

Lazy Loading loads related data only when the navigation property is accessed. It can make code look simpler, but it can also cause hidden database queries and the N+1 query problem.

For most APIs, Eager Loading or explicit projection is preferred because it makes database queries predictable and easier to optimize.

# Example

```csharp
// Eager loading
var customers = await _db.Customers
    .Include(c => c.Orders)
    .ToListAsync();

// Projection is often better for APIs
var result = await _db.Customers
    .Select(c => new CustomerDto
    {
        Id = c.Id,
        Name = c.Name,
        OrderCount = c.Orders.Count
    })
    .ToListAsync();
```

# 15-Second Version

Eager Loading loads related data with the original query using `Include()`. Lazy Loading loads related data when the navigation property is accessed, but it can cause hidden queries and N+1 performance issues.
