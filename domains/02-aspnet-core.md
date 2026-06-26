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

---

## ASP.NET Core Application Lifecycle

# Question

Explain the ASP.NET Core application lifecycle.

# Senior-Level Answer

The application starts in `Program.cs`. The builder creates and configures the host, including configuration, logging, dependency injection, and the web server, which is normally Kestrel. Services are registered, the application is built, middleware and endpoints are configured, and `app.Run()` starts the host.

For each HTTP request, Kestrel receives the request and passes it through the middleware pipeline in the order the middleware was registered. Typical stages include exception handling, HTTPS redirection, routing, authentication, authorization, and endpoint execution.

Each middleware can perform work before and after the next component, call `next()` to continue, or short-circuit the request. Routing selects an endpoint. For an MVC controller endpoint, model binding and filters run, the action executes, and a result is produced. The response then travels back through the middleware pipeline in reverse order.

The key idea is that ASP.NET Core uses a configurable middleware pipeline with built-in dependency injection.

# Example

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

var app = builder.Build();

app.UseExceptionHandler("/error");
app.UseHttpsRedirection();
app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

The request enters at the first middleware, reaches the selected endpoint if no middleware short-circuits it, and the response returns through the pipeline in reverse order.

# 15-Second Version

ASP.NET Core starts in `Program.cs`, builds the host and DI container, and starts Kestrel. Each request flows through middleware in order, routing selects an endpoint, the endpoint executes, and the response returns through middleware in reverse order.

---

## ASP.NET Core Filters

# Question

What are filters in ASP.NET Core?

# Senior-Level Answer

Filters execute code at specific stages before or after MVC controller action execution.

They are used for cross-cutting concerns that are closely related to MVC actions, such as authorization, logging, validation, caching, exception handling, and modifying action results, without repeating the same logic in every controller action.

Filters are part of the MVC or Razor Pages execution pipeline. Middleware is broader and runs for HTTP requests before an endpoint is executed, while filters run inside the selected MVC endpoint.

# Example

```csharp
public class ExecutionLoggingFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        Console.WriteLine("Action is starting");
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        Console.WriteLine("Action has completed");
    }
}
```

# 15-Second Version

Filters run before or after specific stages of MVC action execution. They handle action-related cross-cutting concerns such as authorization, validation, logging, exception handling, and result processing.

---

## Types of ASP.NET Core Filters

# Question

What are the different types of filters in ASP.NET Core?

# Senior-Level Answer

The main filter types are:

1. **Authorization filters** run first and determine whether the request is authorized.
2. **Resource filters** run after authorization and wrap most of the remaining MVC pipeline. They can short-circuit processing and are useful for caching or resource setup.
3. **Action filters** run immediately before and after the controller action method.
4. **Exception filters** handle unhandled exceptions raised during action execution and related MVC processing.
5. **Result filters** run before and after the action result is executed, such as before and after a response is formatted.

Model binding occurs after resource filters begin and before action filters execute.

# Example

```csharp
[Authorize]
[ServiceFilter(typeof(ExecutionLoggingFilter))]
public IActionResult GetOrder(int id)
{
    return Ok();
}
```

`[Authorize]` performs authorization, while `ExecutionLoggingFilter` can run before and after the action.

# 15-Second Version

The filter types are Authorization, Resource, Action, Exception, and Result filters. They run at different stages around model binding, action execution, exception handling, and result execution.

---

## Creating and Applying Filters

# Question

How are filters created in ASP.NET Core, and which classes or interfaces are used?

# Senior-Level Answer

A filter can be created by implementing the interface for the required stage. Most filter types have synchronous and asynchronous versions:

- `IActionFilter` or `IAsyncActionFilter`
- `IAuthorizationFilter` or `IAsyncAuthorizationFilter`
- `IExceptionFilter` or `IAsyncExceptionFilter`
- `IResourceFilter` or `IAsyncResourceFilter`
- `IResultFilter` or `IAsyncResultFilter`

For attribute-based filters, a class can inherit from `ActionFilterAttribute`, which supports action and result stages, or from a more specific attribute class such as `ExceptionFilterAttribute`. Methods such as `OnActionExecuting()` and `OnActionExecuted()` can then be overridden.

Filters can be applied at three scopes:

1. Globally, to every controller action.
2. At controller level, to every action in a controller.
3. At action level, to one action.

When a filter requires services from dependency injection, it can be registered in the service container and applied with `ServiceFilterAttribute` or `TypeFilterAttribute`.

# Example

```csharp
public class ExecutionLoggingFilter : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        Console.WriteLine("Before action");
    }

    public override void OnActionExecuted(ActionExecutedContext context)
    {
        Console.WriteLine("After action");
    }
}

[ExecutionLoggingFilter]
public class OrdersController : ControllerBase
{
}
```

A global filter can be registered as follows:

```csharp
builder.Services.AddControllers(options =>
{
    options.Filters.Add<ExecutionLoggingFilter>();
});
```

# 15-Second Version

Create filters by implementing the matching sync or async filter interface, or inherit from an attribute such as `ActionFilterAttribute`. Apply them globally, at controller level, or at action level.
