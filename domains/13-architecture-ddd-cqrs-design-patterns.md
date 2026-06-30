# Architecture, DDD, CQRS & Design Patterns Revision Notes

## Factory Design Pattern

# Question

Explain the Factory Design Pattern.

# Senior-Level Answer

The Factory Design Pattern centralizes object creation logic instead of spreading `new` statements and conditional creation logic across the application.

It is useful when the exact concrete type to create depends on runtime input, configuration, business rules, provider type, or environment. The calling code asks the factory for an abstraction and does not need to know the concrete implementation details.

This improves maintainability, reduces coupling, and supports the Open/Closed Principle because new implementations can be added with less change to existing business logic.

# Example

```csharp
public interface INotificationSender
{
    Task SendAsync(string message);
}

public class EmailSender : INotificationSender
{
    public Task SendAsync(string message) => Task.CompletedTask;
}

public class SmsSender : INotificationSender
{
    public Task SendAsync(string message) => Task.CompletedTask;
}

public class NotificationSenderFactory
{
    public INotificationSender Create(string channel)
    {
        return channel switch
        {
            "email" => new EmailSender(),
            "sms" => new SmsSender(),
            _ => throw new NotSupportedException($"Unsupported channel: {channel}")
        };
    }
}
```

The caller asks the factory for a sender and works with the `INotificationSender` abstraction.

# 15-Second Version

The Factory Pattern centralizes object creation. It hides concrete class creation from the caller and returns an abstraction based on input, configuration, or business rules.

---

## Factory Pattern in Large Applications

# Question

How is the Factory Pattern used in large applications?

# Senior-Level Answer

In large applications, the Factory Pattern is commonly used when there are multiple implementations of the same abstraction and the correct one must be selected dynamically.

Examples include selecting a payment provider, notification channel, report generator, file parser, integration client, authentication provider, or AI provider based on tenant, configuration, request type, or business rules.

In modern .NET applications, factories are often combined with dependency injection. Instead of manually constructing every dependency using `new`, the factory can resolve registered services or use a dictionary of strategies. This keeps business code clean and avoids long `if-else` blocks in application services.

# Example

```csharp
public interface IPaymentProcessor
{
    Task ProcessAsync(decimal amount);
}

public class PaymentProcessorFactory
{
    private readonly IServiceProvider _serviceProvider;

    public PaymentProcessorFactory(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    public IPaymentProcessor Create(string provider)
    {
        return provider switch
        {
            "stripe" => _serviceProvider.GetRequiredService<StripePaymentProcessor>(),
            "paytabs" => _serviceProvider.GetRequiredService<PaytabsPaymentProcessor>(),
            _ => throw new NotSupportedException($"Unsupported provider: {provider}")
        };
    }
}
```

# 15-Second Version

In large systems, factories select the correct implementation, such as payment provider, report generator, parser, or integration client. They reduce coupling and keep business services free from object-creation logic.
