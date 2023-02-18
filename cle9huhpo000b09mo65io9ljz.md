# Strategy Design Pattern

## **Introduction**

One of the most popular design patterns is the Strategy Design Pattern. This pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable at runtime. In this article, we will discuss the Strategy Design Pattern in C#.

## **What is Strategy Design Pattern?**

The Strategy Design Pattern is a behavioral pattern that allows selecting an algorithm at runtime. The pattern defines a set of algorithms that can be used interchangeably. Each algorithm is encapsulated in a separate class that implements a common interface. The client code can then select the desired algorithm dynamically, without being aware of the implementation details.

## **How to implement the Strategy Design Pattern in C#?**

To implement the Strategy Design Pattern in C#, we need to follow the following steps:

1. Define the Strategy interface: This interface should define the method(s) that will be used by the context object to execute the algorithm.
    
2. Implement the concrete strategies: Each concrete strategy should implement the Strategy interface and provide its own implementation of the algorithm.
    
3. Define the context object: The context object should have a reference to the Strategy interface and use it to execute the algorithm.
    

## **Example**

Let's say we have a payment system that supports multiple payment methods such as PayPal, Credit Card, and Bitcoin. We want to implement the payment system using the Strategy Design Pattern in C#. Here is how we can do it:

### **Step 1: Define the Strategy interface**

```csharp
public interface IPaymentMethod
{
    void ProcessPayment(double amount);
}
```

The IPaymentMethod interface defines a method for processing payments.

### **Step 2: Implement the concrete strategies**

```csharp
public class PayPalPayment : IPaymentMethod
{
    public void ProcessPayment(double amount)
    {
        Console.WriteLine($"Processing payment of {amount} using PayPal.");
    }
}

public class CreditCardPayment : IPaymentMethod
{
    public void ProcessPayment(double amount)
    {
        Console.WriteLine($"Processing payment of {amount} using Credit Card.");
    }
}

public class BitcoinPayment : IPaymentMethod
{
    public void ProcessPayment(double amount)
    {
        Console.WriteLine($"Processing payment of {amount} using Bitcoin.");
    }
}
```

The concrete strategies implement the IPaymentMethod interface and provide their own implementation of the ProcessPayment method.

### **Step 3: Define the context object**

```csharp
public class PaymentContext
{
    private IPaymentMethod _paymentMethod;

    public PaymentContext(IPaymentMethod paymentMethod)
    {
        _paymentMethod = paymentMethod;
    }

    public void ProcessPayment(double amount)
    {
        _paymentMethod.ProcessPayment(amount);
    }

    public void SetPaymentMethod(IPaymentMethod paymentMethod)
    {
        _paymentMethod = paymentMethod;
    }
}
```

The PaymentContext class has a reference to the IPaymentMethod interface and uses it to execute the algorithm. The SetPaymentMethod method allows changing the payment method at runtime.

### **Usage**

Now, let's see how we can use the payment system using the Strategy Design Pattern.

```csharp
static void Main(string[] args)
{
    // PayPal payment
    var paymentContext = new PaymentContext(new PayPalPayment());
    paymentContext.ProcessPayment(100);

    // Credit Card payment
    paymentContext.SetPaymentMethod(new CreditCardPayment());
    paymentContext.ProcessPayment(200);

    // Bitcoin payment
    paymentContext.SetPaymentMethod(new BitcoinPayment());
    paymentContext.ProcessPayment(300);
}
```

In the Main method, we create a PaymentContext object and pass it a concrete strategy (PayPalPayment). We then use the ProcessPayment method to process a payment of 100. We then change the payment method to CreditCard Payment using the SetPaymentMethod method and process a payment of 200. Finally, we change the payment method to BitcoinPayment and process a payment of 300.

## **Conclusion**

The Strategy Design Pattern is a powerful tool for selecting an algorithm at runtime. It provides a flexible and extensible way to encapsulate algorithms and makes them interchangeable. In this article, we discussed how to implement the Strategy Design Pattern in C# using a payment system example. We defined the Strategy interface, implemented the concrete strategies, and defined the context object. We also showed how to use the payment system by creating a PaymentContext object and dynamically changing the payment method at runtime.