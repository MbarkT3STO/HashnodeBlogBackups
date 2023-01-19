# Factory Design Pattern in C#

The factory design pattern is a creational design pattern that provides a way to create objects without specifying the exact class of object that will be created. This pattern is used in scenarios where a class cannot anticipate the type of objects it must create, or when a class wants to delegate responsibility for object creation to its subclasses.

## **How it works**

The factory pattern consists of a factory class and one or more product classes. The factory class is responsible for creating objects of the product classes, while the product classes define the interface for the objects that the factory class creates.

## **Example**

Let's take an example of a factory class that creates different types of vehicles:

```csharp
interface IVehicle
{
    void Drive();
}

class Car : IVehicle
{
    public void Drive()
    {
        Console.WriteLine("Driving a car");
    }
}

class Bike : IVehicle
{
    public void Drive()
    {
        Console.WriteLine("Driving a bike");
    }
}

class VehicleFactory
{
    public IVehicle GetVehicle(string vehicleType)
    {
        if (vehicleType == "car")
            return new Car();
        else if (vehicleType == "bike")
            return new Bike();
        else
            return null;
    }
}
```

In this example, the `VehicleFactory` class is the factory class that creates objects of the `Car` and `Bike` classes, which implement the `IVehicle` interface. The `GetVehicle` method of the factory class takes a string input, and based on its value, it creates and returns an object of the corresponding class.

```csharp
VehicleFactory factory = new VehicleFactory();
IVehicle vehicle = factory.GetVehicle("car");
vehicle.Drive();
// Output: Driving a car
```

We can use the factory class to create different types of vehicles without knowing the exact class of the object that will be created. This decouples the client code from the classes that are being instantiated.

## **Advantages**

* The factory pattern provides a way to create objects without specifying the exact class of object that will be created.
    
* The factory pattern promotes loose coupling by eliminating the need for the client code to know the exact class of object that will be created.
    
* The factory pattern allows for the creation of objects that belong to a particular family of classes, without specifying the exact class of object that will be created.
    

## Real-World Examples

### E-Commere App

Imagine you are building an e-commerce application and want to provide multiple payment options for customers, such as credit card, PayPal, and bank transfer. Instead of creating separate functions for each payment method, a factory class can be used to create objects of the appropriate classes for each payment method.

```csharp
interface IPayment
{
    void MakePayment();
}

class CreditCardPayment : IPayment
{
    public void MakePayment()
    {
        Console.WriteLine("Making payment using credit card");
    }
}

class PayPalPayment : IPayment
{
    public void MakePayment()
    {
        Console.WriteLine("Making payment using PayPal");
    }
}

class BankTransferPayment : IPayment
{
    public void MakePayment()
    {
        Console.WriteLine("Making payment using bank transfer");
    }
}

class PaymentFactory
{
    public IPayment GetPaymentMethod(string paymentMethod)
    {
        if (paymentMethod == "credit card")
            return new CreditCardPayment();
        else if (paymentMethod == "paypal")
            return new PayPalPayment();
        else if (paymentMethod == "bank transfer")
            return new BankTransferPayment();
        else
            return null;
    }
}
```

In this example, the `PaymentFactory` class is responsible for creating objects of the `CreditCardPayment`, `PayPalPayment`, and `BankTransferPayment` classes, which all implement the `IPayment` interface. The `GetPaymentMethod` method of the factory class takes a string input and creates and returns an object of the corresponding class.

```csharp
PaymentFactory factory = new PaymentFactory();
IPayment payment = factory.GetPaymentMethod("credit card");
payment.MakePayment();
// Output: Making payment using credit card
```

By using the factory pattern, the e-commerce application can add or remove payment options without affecting the rest of the code. The factory pattern also allows the client code to create payment objects without knowing the exact class of the object that will be created, promoting loose coupling and flexibility in the code.

### Word Processing App

Another example of the factory design pattern in action is in the creation of document objects in a word processing application.

```csharp
interface IDocument
{
    void Open();
    void Close();
}

class WordDocument : IDocument
{
    public void Open()
    {
        Console.WriteLine("Opening a Word document");
    }
    public void Close()
    {
        Console.WriteLine("Closing a Word document");
    }
}

class PDFDocument : IDocument
{
    public void Open()
    {
        Console.WriteLine("Opening a PDF document");
    }
    public void Close()
    {
        Console.WriteLine("Closing a PDF document");
    }
}

class DocumentFactory
{
    public IDocument GetDocument(string documentType)
    {
        if (documentType == "word")
            return new WordDocument();
        else if (documentType == "pdf")
            return new PDFDocument();
        else
            return null;
    }
}
```

In this example, the `DocumentFactory` class is responsible for creating objects of the `WordDocument` and `PDFDocument` classes, which implement the `IDocument` interface. The `GetDocument` method of the factory class takes a string input and creates and returns an object of the corresponding class.

```csharp
DocumentFactory factory = new DocumentFactory();
IDocument document = factory.GetDocument("pdf");
document.Open();
// Output: Opening a PDF document
document.Close();
// Output: Closing a PDF document
```

By using the factory pattern, the word processing application can add or remove document types without affecting the rest of the code. The factory pattern also allows the client code to create document objects without knowing the exact class of the object that will be created, promoting loose coupling and flexibility in the code.

## Using Generics

The factory design pattern can also be implemented using generics in C#. This allows for the creation of factory classes that can work with any type of object, rather than being specific to a particular class or interface.

```csharp
class GenericFactory<T> where T : new()
{
    public T CreateInstance()
    {
        return new T();
    }
}
```

In this example, the `GenericFactory` class is a generic factory class that creates objects of the type specified by the type parameter `T`. The `new()` constraint is used to ensure that the type parameter has a public parameterless constructor.

```csharp
GenericFactory<Vehicle> factory = new GenericFactory<Vehicle>();
Vehicle vehicle = factory.CreateInstance();
```

This can be useful in scenarios where a factory class needs to work with multiple types of objects, or when the types of objects that the factory creates may change at runtime. With generics, the factory class can be written once, and can be reused for different types without modification.

This generic factory can be used in the previous examples as well, for example:

```csharp
GenericFactory<WordDocument> factory = new GenericFactory<WordDocument>();
IDocument document = factory.CreateInstance();
```

The advantage of this approach is that the factory class is not tightly coupled to the specific types it creates, which allows for more flexibility and maintainability in the code.

## Using Factory Method

Another way to implement the factory pattern using generics is to use a factory method that takes a single type parameter, and returns an instance of that type.

```csharp
class Factory
{
    public static T Create<T>() where T : new()
    {
        return new T();
    }
}
```

This factory method can be called like this:

```csharp
Vehicle vehicle = Factory.Create<Vehicle>();
```

This is a more concise and simplified way of creating factory classes, as it eliminates the need to create a separate factory class for each type.

It's also worth noting that a factory pattern could use a combination of both the factory class and the factory method, where the factory class can have multiple factory methods, each one with a specific type or set of types, that way it can be more reusable and adaptable to different scenarios.

## **Conclusion**

The factory design pattern is a powerful creational pattern that provides a way to create objects without specifying the exact class of object that will be created. It promotes loose coupling and allows for the creation of objects that belong to a particular family of classes. This pattern can be useful in a wide variety of scenarios and is a great tool for creating flexible and maintainable code.