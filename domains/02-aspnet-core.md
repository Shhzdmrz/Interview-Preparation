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
