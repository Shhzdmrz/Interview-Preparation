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
