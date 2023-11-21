# Dependency Inversion Principle (DIP)

The **`Dependency Inversion Principle (DIP)`** is one of the SOLID principles in C#, which states that high-level modules should not depend on low-level modules, but both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions. In essence, it promotes loose coupling and encourages the use of abstractions (interfaces or abstract classes) to decouple components in a way that makes the software more flexible and maintainable.

Here's an example in C# to illustrate the Dependency Inversion Principle:

Suppose you have a simple notification system that sends notifications to different channels (e.g., email and SMS). Initially, you might have a high-level module that directly depends on low-level modules:

```csharp
// Low-level module for sending email notifications
public class EmailNotifier
{
    public void SendEmail(string to, string message)
    {
        // Send an email
    }
}

// Low-level module for sending SMS notifications
public class SmsNotifier
{
    public void SendSms(string to, string message)
    {
        // Send an SMS
    }
}

// High-level module that depends on low-level modules
public class NotificationService
{
    private EmailNotifier emailNotifier = new EmailNotifier();
    private SmsNotifier smsNotifier = new SmsNotifier();

    public void SendNotification(string to, string message)
    {
        // Send notifications using email and SMS
        emailNotifier.SendEmail(to, message);
        smsNotifier.SendSms(to, message);
    }
}
```

In this design, the **`NotificationService`** class depends directly on the low-level modules, **`EmailNotifier`** and **`SmsNotifier`**. This tightly couples the high-level module to specific implementations, making it difficult to extend or change the notification system.

To apply the Dependency Inversion Principle, you can introduce abstractions (interfaces) and depend on these abstractions rather than concrete implementations:

```csharp
// Abstraction for notifiers
public interface INotifier
{
    void SendNotification(string to, string message);
}

// Concrete implementation for email notifications
public class EmailNotifier : INotifier
{
    public void SendNotification(string to, string message)
    {
        // Send an email
    }
}

// Concrete implementation for SMS notifications
public class SmsNotifier : INotifier
{
    public void SendNotification(string to, string message)
    {
        // Send an SMS
    }
}

// High-level module that depends on abstractions
public class NotificationService
{
    private readonly INotifier emailNotifier;
    private readonly INotifier smsNotifier;

    public NotificationService(INotifier emailNotifier, INotifier smsNotifier)
    {
        this.emailNotifier = emailNotifier;
        this.smsNotifier = smsNotifier;
    }

    public void SendNotification(string to, string message)
    {
        // Send notifications using abstractions
        emailNotifier.SendNotification(to, message);
        smsNotifier.SendNotification(to, message);
    }
}
```

In this refactored code, the **`NotificationService`** class depends on the **`INotifier`** interface, which abstracts the concept of sending notifications. The high-level module no longer depends on low-level modules but instead depends on the abstractions. This adheres to the Dependency Inversion Principle, allowing for flexibility in extending the system and swapping out implementations without modifying the high-level code.
