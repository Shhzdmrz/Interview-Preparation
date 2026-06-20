# ASP.NET Core Revision Notes

## Middleware Pipeline

# Question

What is middleware in ASP.NET Core?

# Senior-Level Answer

Middleware is a component in the ASP.NET Core request pipeline that can inspect, modify, handle, or short-circuit an HTTP request and response.

Each request passes through a sequence of middleware components before reaching the controller or endpoint. Middleware is commonly used for cross-cutting concerns such as exception handling, routing, authentication, authorization, logging, CORS, rate limiting, and response compression.

The order of middleware matters because each component can decide whether to pass the request to the next middleware or stop the pipeline.

# Example

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.UseExceptionHandler("/error");
app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();

app.MapControllers();

app.Run();
```

In this example, exception handling runs first, routing identifies the endpoint, authentication identifies the user, authorization checks permissions, and then the request reaches the controller.

# 15-Second Version

Middleware is a component in the ASP.NET Core request pipeline. It can inspect, modify, or stop a request before it reaches the controller. It is used for things like logging, exception handling, authentication, and authorization.

---

## Dependency Injection Lifetimes

# Question

What is the difference between Singleton, Scoped, and Transient lifetimes in ASP.NET Core dependency injection?

# Senior-Level Answer

Singleton creates one instance for the entire application lifetime. It is useful for thread-safe shared services such as configuration providers, logging services, memory cache, or a Redis connection multiplexer.

Scoped creates one instance per HTTP request. It is commonly used for `DbContext` because all operations during one request should share the same context instance.

Transient creates a new instance every time the service is requested. It is useful for lightweight stateless services.

A Singleton should not depend on a Scoped service because the scoped service belongs to a specific request and may be disposed after that request ends. This can cause incorrect behavior or runtime errors.

# Example

```csharp
builder.Services.AddSingleton<ICacheService, CacheService>();
builder.Services.AddScoped<IOrderRepository, OrderRepository>();
builder.Services.AddTransient<IEmailFormatter, EmailFormatter>();
```

`CacheService` lives for the app lifetime, `OrderRepository` lives for one request, and `EmailFormatter` is created whenever it is requested.

# 15-Second Version

Singleton is one instance for the whole app, Scoped is one instance per request, and Transient is a new instance every time. `DbContext` is usually Scoped. Singleton should not depend on Scoped.

---

## Async Method Without await

# Question

What is wrong with this code?

```csharp
public async Task<IActionResult> Get()
{
    var users = _db.Users.ToList();
    return Ok(users);
}
```

# Senior-Level Answer

The method is marked `async`, but it does not use `await`. This creates a compiler warning and adds unnecessary async state-machine overhead.

The better solution is to use the asynchronous EF Core method `ToListAsync()` and await it. This avoids blocking the request thread while the database operation is running.

If there is no asynchronous operation, remove `async` and return `IActionResult` directly. But for database calls in ASP.NET Core, prefer async EF Core methods.

# Example

```csharp
public async Task<IActionResult> Get()
{
    var users = await _db.Users.ToListAsync();
    return Ok(users);
}
```

# 15-Second Version

The method is marked `async` but has no `await`. Use `await _db.Users.ToListAsync()` for database calls, or remove `async` if there is no async work.
