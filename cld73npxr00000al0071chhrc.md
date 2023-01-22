# Using the Arg.Is() Matcher with FakeItEasy

One of the key features of FakeItEasy is its support for argument matchers, which allow developers to specify how the fake object should respond to different method calls based on the arguments passed in.

One of the most commonly used argument matchers in FakeItEasy is the [`Arg.Is`](http://Arg.Is)`()` matcher. This matcher allows developers to specify that a certain method should be called only when a specific argument is passed in. For example, imagine that you are testing a class that depends on an interface called `ILogger` to log messages. You can use FakeItEasy to create a fake implementation of this interface, and use the [`Arg.Is`](http://Arg.Is)`()` matcher to specify that the `Log()` method should only be called when a specific string is passed in as the message argument.

Here's an example of how you might use the [`Arg.Is`](http://Arg.Is)`()` matcher in a unit test:

```csharp
[Test]
public void TestLogging()
{
    // Create a fake implementation of ILogger
    var fakeLogger = A.Fake<ILogger>();
    // Configure the fake to call a callback when Log("Important message") is called
    A.CallTo(() => fakeLogger.Log("Important message")).Invokes(() => Console.WriteLine("Important message logged"));
    // Create an instance of the class under test
    var myClass = new MyClass(fakeLogger);

    // Act
    myClass.DoWork();

    // Assert
    A.CallTo(() => fakeLogger.Log(Arg.Is("Important message"))).MustHaveHappened();
}
```

In this example, the `TestLogging()` method creates a fake implementation of the `ILogger` interface using the `A.Fake<T>()` method. It then configures the fake to call a callback when the `Log()` method is called with the argument "Important message" using the `A.CallTo()` method. The [`Arg.Is`](http://Arg.Is)`()` matcher is used in the `A.CallTo()` method to specify that the callback should be called only when the `Log()` method is called with the argument "Important message".

The test then creates an instance of the class under test, which is assumed to take an `ILogger` in its constructor and call the `Log()` method on it, and calls the `DoWork()` method on it.

The `Assert` section of the test verifies that the Log("Important message") method was called using the `A.CallTo(() => fakeLogger.Log(`[`Arg.Is`](http://Arg.Is)`("Important message"))).MustHaveHappened()` which throws an exception if the Log("Important message") method was not called.

It is also worth noting that [`Arg.Is`](http://Arg.Is)`()` matcher can be used with other FakeItEasy methods such as `A.CallToSet(() => fakeLogger.IsEnabled =` [`Arg.Is`](http://Arg.Is)`(true))` and `A.CallTo(() => fakeLogger.Log(`[`Arg.Is`](http://Arg.Is)`<string>(x => x.StartsWith("Error"))))`.

Overall, the [`Arg.Is`](http://Arg.Is)`()` matcher is a powerful tool that can be used to make your tests more expressive and maintainable. By using argument matchers like [`Arg.Is`](http://Arg.Is)`()`, you can ensure that your fake objects respond in the way that you expect, without having to hard-code specific arguments in your tests.

# **Real-world example**

Let's consider a scenario where we are testing a `PaymentService` class that depends on a `IPaymentGateway` to process payments. We want to test that the `PaymentService` calls the `ProcessPayment` method on the `IPaymentGateway` with the correct payment details.

```csharp
[Test]
public void TestPaymentService_ShouldProcessPayment()
{
    //Arrange
    var fakePaymentGateway = A.Fake<IPaymentGateway>();
    var paymentService = new PaymentService(fakePaymentGateway);
    var paymentDetails = new PaymentDetails { Amount = 100, CardNumber = "1234-5678-1234-5678" };

    //Act
    paymentService.ProcessPayment(paymentDetails);

    //Assert
    A.CallTo(() => fakePaymentGateway.ProcessPayment(Arg.Is<PaymentDetails>(x => x.Amount == 100 && x.CardNumber == "1234-5678-1234-5678"))).MustHaveHappened();
}
```

In this example, we are using [`Arg.Is`](http://Arg.Is) matcher in the `A.CallTo(() => fakePaymentGateway.ProcessPayment(`[`Arg.Is`](http://Arg.Is)`<PaymentDetails>(x => x.Amount == 100 && x.CardNumber == "1234-5678-1234-5678"))).MustHaveHappened()` to check that the `ProcessPayment` method was called on the `fakePaymentGateway` with the correct payment details.

Here is an example of how the `PaymentService`, `IPaymentGateway`, and `PaymentDetails` classes might be defined:

```csharp
public class PaymentDetails
{
    public decimal Amount { get; set; }
    public string CardNumber { get; set; }
}

public interface IPaymentGateway
{
    void ProcessPayment(PaymentDetails paymentDetails);
}

public class PaymentService
{
    private readonly IPaymentGateway _paymentGateway;

    public PaymentService(IPaymentGateway paymentGateway)
    {
        _paymentGateway = paymentGateway;
    }

    public void ProcessPayment(PaymentDetails paymentDetails)
    {
        _paymentGateway.ProcessPayment(paymentDetails);
    }
}
```

Here, the `PaymentDetails` class represents the information needed to process a payment, such as the amount and card number. The `IPaymentGateway` interface defines a single method, `ProcessPayment`, that takes a `PaymentDetails` object as an argument. The `PaymentService` class is a class that takes an implementation of `IPaymentGateway` in its constructor and calls the `ProcessPayment` method on it when the `ProcessPayment` method is called.

In the test, we are creating a fake implementation of `IPaymentGateway` using `A.Fake<IPaymentGateway>()` and passing it to the `PaymentService` class. We are then testing that the `PaymentService` calls the `ProcessPayment` method on the `IPaymentGateway` with the correct payment details using `A.CallTo(() => fakePaymentGateway.ProcessPayment(`[`Arg.Is`](http://Arg.Is)`<PaymentDetails>(x => x.Amount == 100 && x.CardNumber == "1234-5678-1234-5678"))).MustHaveHappened()`

## **Conclusion**

In this article, we've discussed how to use the [`Arg.Is`](http://Arg.Is)`()` matcher with the FakeItEasy library to create powerful and expressive tests for your .NET code. By using argument matchers like [`Arg.Is`](http://Arg.Is)`()`, you can ensure that your fake objects respond in the way that you expect, making your tests more maintainable and reliable. With this knowledge, you should be able to create more robust tests for your own projects, making it easier to catch bugs and improve the quality of your code.