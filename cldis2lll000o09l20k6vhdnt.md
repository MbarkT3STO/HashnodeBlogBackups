# Implementing Retry and Circuit Breaker Patterns in C# using Polly

Polly is a .NET library that makes it easier to implement the retry and circuit breaker patterns in C# applications. These patterns help to provide resilience and stability to applications by handling failures and preventing further failures from cascading and taking down the entire system.

## **Installing Polly**

Polly is available as a NuGet package, which makes it easy to add to your project. You can install it using the following steps:

1. Open the NuGet Package Manager Console in Visual Studio.
    
2. Run the following command: `Install-Package Polly`
    
3. Wait for the package to install.
    

After the installation is complete, you can start using Polly in your code by including the `Polly` namespace.

```csharp
using Polly;
```

That's it! You're now ready to start using the retry and circuit breaker patterns in your C# application using Polly.

## **Retry Pattern**

The retry pattern involves automatically retrying a failed operation a specified number of times before giving up. This can be useful in situations where the failure is temporary, such as when a service is temporarily unavailable or when there is a network issue.

To implement the retry pattern using Polly, you can use the `Retry` policy. Here's an example:

```csharp
var policy = Policy.Handle<Exception>()
    .Retry(3, (exception, retryCount) =>
    {
        Console.WriteLine($"Retrying due to {exception.GetType().Name}... Attempt {retryCount}");
    });

policy.Execute(() =>
{
    Console.WriteLine("Executing operation");
    throw new Exception();
});
```

In this example, the operation is executed within the `Execute` method, and the `Retry` policy is used to handle any exceptions that are thrown. The policy specifies that the operation should be retried three times, and includes a delegate that logs the retry count each time the operation is retried.

## **Circuit Breaker Pattern**

The circuit breaker pattern is used to prevent further failures from cascading by temporarily stopping execution of a failing operation after a specified number of failures. This allows the system to recover before continuing to execute the operation.

To implement the circuit breaker pattern using Polly, you can use the `CircuitBreaker` policy. Here's an example:

```csharp
var policy = Policy.Handle<Exception>()
    .CircuitBreaker(3, TimeSpan.FromSeconds(30),
        (exception, duration) =>
        {
            Console.WriteLine("Circuit breaker tripped");
        },
        () => Console.WriteLine("Circuit breaker reset"));

policy.Execute(() =>
{
    Console.WriteLine("Executing operation");
    throw new Exception();
});
```

In this example, the `CircuitBreaker` policy is used to handle exceptions thrown by the operation. The policy specifies that after three failures, the circuit breaker should trip for 30 seconds. A delegate is also provided to log when the circuit breaker is tripped, and another delegate is provided to log when the circuit breaker is reset.

## **Customizing the Retry and Circuit Breaker Policies**

Polly provides a range of options to customize the retry and circuit breaker policies to fit the specific needs of your application. Some of the options you can specify include:

* The number of retries or the number of failures before the circuit breaker trips
    
* The amount of time the circuit breaker should stay tripped
    
* The type of exceptions to handle
    
* Whether to wait between retries
    
* Whether to use an exponential backoff strategy between retries
    

Here's an example that shows how to customize a retry policy to use an exponential backoff strategy:

```csharp
var policy = Policy.Handle<Exception>()
    .WaitAndRetry(new[]
    {
        TimeSpan.FromSeconds(1),
        TimeSpan.FromSeconds(2),
        TimeSpan.FromSeconds(4)
    }, (exception, timeSpan, retryCount, context) =>
    {
        Console.WriteLine($"Retrying due to {exception.GetType().Name}... Attempt {retryCount}");
    });

policy.Execute(() =>
{
    Console.WriteLine("Executing operation");
    throw new Exception();
});
```

In this example, the `WaitAndRetry` policy is used to handle exceptions, and the `TimeSpan` array specifies the wait time between retries. This creates an exponential backoff strategy, where the wait time between retries increases with each subsequent retry.

Similarly, you can customize a circuit breaker policy to handle specific types of exceptions, or to trip the circuit breaker after a different number of failures.

## **Advanced Usage**

Polly provides additional features for advanced usage, such as the ability to execute multiple policies in a specific order, or to execute multiple policies for different parts of an operation. For example, you might want to first execute a retry policy, followed by a circuit breaker policy.

To achieve this, you can use the `Wrap` method to wrap multiple policies together. Here's an example:

```csharp
var retryPolicy = Policy.Handle<Exception>()
    .Retry(3);

var circuitBreakerPolicy = Policy.Handle<Exception>()
    .CircuitBreaker(3, TimeSpan.FromSeconds(30));

var policy = retryPolicy.Wrap(circuitBreakerPolicy);

policy.Execute(() =>
{
    Console.WriteLine("Executing operation");
    throw new Exception();
});
```

In this example, the retry policy and circuit breaker policy are both created, and then wrapped together using the `Wrap` method. The resulting policy will first execute the retry policy, and then, if the operation still fails, it will execute the circuit breaker policy.

## **Conclusion**

In conclusion, Polly is a powerful and flexible library that makes it easy to implement the retry and circuit breaker patterns in C# applications. With its range of customization options and advanced features, it provides a valuable tool for developers looking to improve the robustness of their applications. Whether you're building a new application or adding resilience to an existing one, Polly is a great choice for your needs.