# C# Fundamentals Revision Notes

## Value Type vs Reference Type

# Question

What is the output?

```csharp
int x = 10;
int y = x;
y = 20;
Console.WriteLine(x);
```

# Senior-Level Answer

The output is `10`.

`int` is a value type. When `y = x` is executed, the value of `x` is copied into `y`. After that, `x` and `y` are independent variables. Changing `y` does not change `x`.

Value types store the actual value directly. Common examples are `int`, `bool`, `double`, `DateTime`, `decimal`, and `struct`.

Reference types store a reference to an object. Common examples are `class`, `string`, arrays, and collections.

# Example

```csharp
int x = 10;
int y = x;
y = 20;

Console.WriteLine(x); // 10
Console.WriteLine(y); // 20
```

# 15-Second Version

`int` is a value type, so assignment copies the value. Changing `y` does not affect `x`. The output is `10`.

---

## String Interning

# Question

What is the output?

```csharp
string s1 = "abc";
string s2 = "abc";
Console.WriteLine(object.ReferenceEquals(s1, s2));
```

# Senior-Level Answer

The output is `True`.

String literals are interned by the .NET runtime. This means identical string literals can point to the same memory reference to save memory and improve performance.

Even though `string` is a reference type, it is immutable. When a string changes, a new string is created instead of modifying the existing one.

# Example

```csharp
string s1 = "abc";
string s2 = "abc";

Console.WriteLine(object.ReferenceEquals(s1, s2)); // True
```

# 15-Second Version

The output is `True` because .NET interns identical string literals, so both variables can reference the same string object.

---

## Task.Wait / Result vs await

# Question

What is the difference between `await`, `Task.Wait()`, and `.Result`?

# Senior-Level Answer

`await` asynchronously waits for a task to complete without blocking the current thread.

`Task.Wait()` and `.Result` block the current thread until the task completes. This is called sync-over-async and can cause scalability problems in ASP.NET applications because blocked threads cannot process other requests.

In older ASP.NET Framework, WPF, and WinForms applications, `.Result` and `.Wait()` can also cause deadlocks because of synchronization context behavior.

In ASP.NET Core, the deadlock risk is lower, but blocking threads is still bad for scalability. Prefer `await` all the way.

# Example

```csharp
// Good
var users = await userService.GetUsersAsync();

// Avoid
var users = userService.GetUsersAsync().Result;

// Avoid
userService.GetUsersAsync().Wait();
```

# 15-Second Version

`await` waits asynchronously and does not block the thread. `.Result` and `.Wait()` block the thread and can hurt scalability or cause deadlocks. Prefer `await` all the way.

---

## int vs Int32

# Question

What is the difference between `int` and `Int32` in C#?

# Senior-Level Answer

There is no functional difference. `int` is the C# keyword alias for `System.Int32`, which is the underlying .NET type.

They compile to the same type and are interchangeable. The same pattern applies to other built-in aliases such as `long` and `System.Int64`, `bool` and `System.Boolean`, and `string` and `System.String`.

The usual C# convention is to use `int` when declaring variables and parameters. `Int32` is sometimes used when accessing static members such as `Int32.Parse()` or `Int32.MaxValue`, although `int.Parse()` and `int.MaxValue` are equally valid.

# Example

```csharp
int first = 10;
Int32 second = 20;

Console.WriteLine(first.GetType() == second.GetType()); // True
Console.WriteLine(int.MaxValue == Int32.MaxValue);      // True
```

# 15-Second Version

There is no functional difference. `int` is a C# alias for `System.Int32`; both compile to the same .NET type. Use `int` by convention in normal C# declarations.

---

## virtual, abstract, and override

# Question

When overriding a method, what keyword is used in the base class: `virtual` or `abstract`?

# Senior-Level Answer

The base class can use either `virtual` or `abstract`, depending on the design.

A `virtual` method has a default implementation that a derived class may override. An `abstract` method has no implementation, must be declared inside an abstract class, and must be overridden by a non-abstract derived class.

The derived class uses the `override` keyword in both cases.

Quick recall: base class uses `virtual` or `abstract`; derived class uses `override`.

# Example

```csharp
public class NotificationService
{
    public virtual void Send()
    {
        Console.WriteLine("Default notification");
    }
}

public abstract class Report
{
    public abstract void Generate();
}

public class EmailNotificationService : NotificationService
{
    public override void Send()
    {
        Console.WriteLine("Email notification");
    }
}

public class SalesReport : Report
{
    public override void Generate()
    {
        Console.WriteLine("Sales report generated");
    }
}
```

# 15-Second Version

The base method can be `virtual` if it has a default implementation or `abstract` if it has none. The derived class uses `override` in both cases.

---

## Metadata in .NET

# Question

What is metadata in .NET?

# Senior-Level Answer

Metadata is data stored in a .NET assembly that describes the code contained in that assembly.

A compiled assembly contains Intermediate Language, or IL, together with metadata describing types, methods, properties, fields, parameters, accessibility, attributes, and references to other assemblies.

This makes .NET assemblies self-describing. The CLR uses metadata to load and execute types, while tools and frameworks use it for features such as reflection, dependency injection, serialization, debugging, and IntelliSense.

# Example

```csharp
Type type = typeof(CustomerService);

Console.WriteLine(type.Name);

foreach (var method in type.GetMethods())
{
    Console.WriteLine(method.Name);
}
```

Reflection can inspect `CustomerService` because its type and member information is available through assembly metadata.

# 15-Second Version

Metadata describes the types and members inside a .NET assembly. It makes assemblies self-describing and enables the CLR, reflection, debugging, serialization, and tooling to understand the compiled code.

---

## .NET Standard

# Question

What is .NET Standard?

# Senior-Level Answer

.NET Standard is a formal specification of .NET APIs that different .NET implementations agreed to support.

It is a contract, not a runtime. A library targeting a particular .NET Standard version can run on any .NET implementation that supports that version, including compatible versions of .NET Framework, .NET Core, and Xamarin.

Higher .NET Standard versions expose more APIs but are supported by fewer older platforms. Since .NET 5 unified the modern .NET platform, .NET Standard is mainly relevant when creating libraries that must support both modern .NET and older .NET Framework applications.

# Example

```xml
<PropertyGroup>
  <TargetFramework>netstandard2.0</TargetFramework>
</PropertyGroup>
```

A library targeting `netstandard2.0` can be referenced by multiple compatible .NET implementations.

# 15-Second Version

.NET Standard is an API specification, not a runtime. It allowed libraries to run across compatible .NET Framework, .NET Core, and Xamarin implementations. Today it is mainly useful for libraries that still support older .NET Framework applications.

---

## How async/await works

# Question

How does `async`/`await` work in .NET applications?

# Senior-Level Answer

`async` and `await` allow asynchronous operations to be written in a readable, sequential style without blocking the current thread.

When an awaited operation is not complete, the method returns control to the caller and registers a continuation. The thread is freed to do other work. When the awaited task completes, the continuation resumes and the rest of the method executes.

In ASP.NET Core, this is especially useful for I/O-bound operations such as database calls, HTTP calls, file operations, and message processing. It improves scalability because request threads are not blocked while waiting for external resources.

`async`/`await` does not automatically make CPU-heavy code faster. For CPU-bound work, use background processing, queues, or dedicated worker services instead of blocking request threads.

# Example

```csharp
public async Task<UserDto> GetUserAsync(int id)
{
    var user = await _db.Users
        .AsNoTracking()
        .FirstOrDefaultAsync(x => x.Id == id);

    return MapToDto(user);
}
```

The thread is not blocked while the database query is running.

# 15-Second Version

`await` pauses the method without blocking the thread. The thread can handle other work, and the method continues when the task completes. It is mainly for I/O-bound work like database or HTTP calls.

---

## Code Reusability in C#

# Question

How do you achieve code reusability in C#?

# Senior-Level Answer

Code reusability in C# is achieved by separating common behavior into reusable types and abstractions instead of duplicating logic.

Common techniques include classes, methods, interfaces, abstract base classes, generics, extension methods, reusable services, shared libraries, and design patterns such as Strategy, Factory, and Decorator.

Good reuse should still follow SOLID principles. For example, reusable services should have clear responsibilities, depend on abstractions, and be injected using dependency injection. Reuse should not mean creating large helper classes that become hard to maintain.

# Example

```csharp
public interface INotificationSender
{
    Task SendAsync(string message);
}

public class EmailNotificationSender : INotificationSender
{
    public Task SendAsync(string message)
    {
        // send email
        return Task.CompletedTask;
    }
}
```

Any feature that needs notification behavior can depend on `INotificationSender` instead of duplicating email logic.

# 15-Second Version

Use reusable methods, classes, interfaces, generics, extension methods, services, shared libraries, and design patterns. The goal is to avoid duplication while keeping responsibilities clear and testable.

---

## Exception Handling in Projects

# Question

How do you handle exceptions in your project?

# Senior-Level Answer

I avoid scattering `try-catch` blocks everywhere. I use centralized exception handling for unexpected errors and local `try-catch` only when I can add value, such as retrying, translating an exception, compensating an operation, or adding context.

In ASP.NET Core, unexpected exceptions should be handled by middleware or `IExceptionHandler`, logged with correlation IDs, and returned as a consistent error response without exposing internal details.

For expected business failures, I prefer validation results, domain errors, or controlled responses instead of throwing exceptions for normal flow. I also make sure exceptions are logged with enough context to troubleshoot production issues.

# Example

```csharp
app.UseExceptionHandler("/error");
```

```csharp
try
{
    await _paymentGateway.CaptureAsync(request);
}
catch (PaymentGatewayException ex)
{
    _logger.LogError(ex, "Payment capture failed for OrderId {OrderId}", request.OrderId);
    throw;
}
```

# 15-Second Version

Use centralized exception handling for unexpected errors, local `try-catch` only when it adds value, log with context and correlation IDs, and return safe consistent error responses.
