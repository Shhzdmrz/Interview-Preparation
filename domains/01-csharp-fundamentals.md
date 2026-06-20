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
