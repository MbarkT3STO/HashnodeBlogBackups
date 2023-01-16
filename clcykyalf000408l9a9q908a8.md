# Decorator Design Pattern in C#

The Decorator Design Pattern is a structural pattern that allows for the dynamic addition of behavior to an individual object, without affecting the behavior of other objects from the same class. In C#, this pattern can be implemented using a combination of interfaces, abstract classes, and concrete classes.

## **The Problem**

Suppose we have a simple `Beverage` class that represents a drink, with a `Description` property and a `Cost()` method that calculates the cost of the drink. We want to add different types of beverages, such as coffee and tea, as well as various toppings, such as milk and sugar. However, we do not want to create a new class for each combination of beverage and topping.

## **The Solution**

To solve this problem, we can use the Decorator Design Pattern. First, we define an `IBeverage` interface that has a `Description` property and a `Cost()` method. Then, we create an abstract `BeverageDecorator` class that implements this interface and has a protected `IBeverage` field. This class serves as a base for all the decorator classes.

Next, we create concrete classes for the different types of beverages, such as `Coffee` and `Tea`, that implement the `IBeverage` interface. Finally, we create decorator classes for each topping, such as `Milk` and `Sugar`, that inherit from the `BeverageDecorator` class and add their respective behavior to the `IBeverage` object they decorate.

## **The Code**

```csharp
interface IBeverage
{
    string Description { get; }
    double Cost();
}

abstract class BeverageDecorator : IBeverage
{
    protected IBeverage beverage;

    public BeverageDecorator(IBeverage beverage)
    {
        this.beverage = beverage;
    }

    public virtual string Description
    {
        get { return beverage.Description; }
    }

    public virtual double Cost()
    {
        return beverage.Cost();
    }
}

class Coffee : IBeverage
{
    public string Description
    {
        get { return "Coffee"; }
    }

    public double Cost()
    {
        return 1.5;
    }
}

class Tea : IBeverage
{
    public string Description
    {
        get { return "Tea"; }
    }

    public double Cost()
    {
        return 1.0;
    }
}

class Milk : BeverageDecorator
{
    public Milk(IBeverage beverage) : base(beverage) { }

    public override string Description
    {
        get { return beverage.Description + ", Milk"; }
    }

    public override double Cost()
    {
        return beverage.Cost() + 0.5;
    }
}

class Sugar : BeverageDecorator
{
    public Sugar(IBeverage beverage) : base(beverage) { }

    public override string Description
    {
        get { return beverage.Description + ", Sugar"; }
    }

    public override double Cost()
    {
        return beverage.Cost() + 0.3;
    }
}
```

## **Using the Code**

Now, we can create various combinations of beverages and toppings, by creating instances of the various classes and decorating them with the desired toppings. For example,

```csharp
IBeverage coffee = new Coffee();
coffee = new Milk(coffee);
coffee = new Sugar(coffee);
Console.WriteLine(coffee.Description); // Output: "Coffee, Milk, Sugar"
Console.WriteLine(coffee.Cost()); // Output: 2.3
```

This will create a coffee with milk and sugar, and print out the description and cost of the resulting object.

Alternatively, you could also chain the decoration methods together in one line.

```csharp
IBeverage tea = new Tea().WithMilk().WithSugar();
Console.WriteLine(tea.Description); // Output: "Tea, Milk, Sugar"
Console.WriteLine(tea.Cost()); // Output: 1.8
```

It's important to note that the decorator pattern does not affect the behavior of other objects from the same class, so if you create another instance of Coffee, it will not have Milk or Sugar as Toppings, unless you explicitly decorate it.

## Real-World Example

A real-world example of using the Decorator Design Pattern in C# could be creating an e-commerce application where users can customize a product with different options and accessories.

For example, let's consider a scenario where a user can purchase a laptop and can add different options like RAM, Hard Drive, and Warranty.

```csharp
interface ILaptop
{
    string Description { get; }
    double Price();
}

abstract class LaptopDecorator : ILaptop
{
    protected ILaptop laptop;

    public LaptopDecorator(ILaptop laptop)
    {
        this.laptop = laptop;
    }

    public virtual string Description
    {
        get { return laptop.Description; }
    }

    public virtual double Price()
    {
        return laptop.Price();
    }
}

class MacBook : ILaptop
{
    public string Description
    {
        get { return "MacBook"; }
    }

    public double Price()
    {
        return 999.99;
    }
}

class RAM : LaptopDecorator
{
    private int _gb;
    public RAM(ILaptop laptop, int gb) : base(laptop) 
    {
        _gb = gb;
    }

    public override string Description
    {
        get { return $"{laptop.Description}, {_gb}GB RAM"; }
    }

    public override double Price()
    {
        return laptop.Price() + (_gb * 50);
    }
}

class HardDrive : LaptopDecorator
{
    private int _gb;
    public HardDrive(ILaptop laptop, int gb) : base(laptop) 
    {
        _gb = gb;
    }

    public override string Description
    {
        get { return $"{laptop.Description}, {_gb}GB Hard Drive"; }
    }

    public override double Price()
    {
        return laptop.Price() + (_gb * 20);
    }
}

class Warranty : LaptopDecorator
{
    private int _year;
    public Warranty(ILaptop laptop, int year) : base(laptop) 
    {
        _year = year;
    }

    public override string Description
    {
        get { return $"{laptop.Description}, {_year} Year Warranty"; }
    }

    public override double Price()
    {
        return laptop.Price() + (_year * 100);
    }
}
```

Now, we can create various combinations of Laptops with different options and accessories.

```csharp
ILaptop laptop = new MacBook();
laptop = new RAM(laptop, 8);
laptop = new HardDrive(laptop, 512);
laptop = new Warranty(laptop, 2);
Console.WriteLine(laptop.Description); // Output: "MacBook, 8GB RAM, 512GB Hard Drive, 2 Year Warranty"
Console.WriteLine(laptop.Price()); // Output: 1859.99
```

This way, the decorator pattern allows us to add different options and accessories to the MacBook without creating a new class for each combination.

In this example, we can see how the decorator pattern allows to add new functionality and flexibility to the product and also it makes it more maintainable Additionally, we can also chain the decoration methods together in one line.

```csharp
ILaptop laptop = new MacBook().WithRAM(8).WithHardDrive(512).WithWarranty(2);
Console.WriteLine(laptop.Description); // Output: "MacBook, 8GB RAM, 512GB Hard Drive, 2 Year Warranty"
Console.WriteLine(laptop.Price()); // Output: 1859.99
```

Another advantage of using the Decorator Design Pattern is that it promotes code reusability. We can create different decorators that can be used with different products and not just limited to laptops. For instance, we can create a `CameraDecorator` that can be used to add options and accessories to a camera.

## Chain the Decoration Methods Together

### Using Decorator Methods

To chain the decoration methods together, you need to define the decorator methods in the base class or the class that is being decorated, which in this case is the `ILaptop` or `MacBook` class.

Here is an example of how you can chain the decoration methods together:

```csharp
class MacBook : ILaptop
{
    public string Description
    {
        get { return "MacBook"; }
    }

    public double Price()
    {
        return 999.99;
    }

    public MacBook WithRAM(int gb)
    {
        return new RAM(this, gb);
    }

    public MacBook WithHardDrive(int gb)
    {
        return new HardDrive(this, gb);
    }

    public MacBook WithWarranty(int year)
    {
        return new Warranty(this, year);
    }
}
```

Here we have added three methods `WithRAM`, `WithHardDrive` and `WithWarranty` to the `MacBook` class which returns the same class with the specified decorator.

These methods can be called in a chain, and it allows you to create a new object with multiple decorators in a single line of code.

```csharp
ILaptop laptop = new MacBook().WithRAM(8).WithHardDrive(512).WithWarranty(2);
```

This way, the decorator pattern allows you to add new functionality to the product in a more readable and concise way, making the code more maintainable and easy to understand.

### Using Extension Methods

An alternative way to chain the decoration methods together is by using Extension Methods in C#. Extension methods allow you to add new methods to an existing class, without modifying its source code.

Here is an example of how you can chain the decoration methods together using Extension methods:

```csharp
static class LaptopExtensions
{
    public static ILaptop WithRAM(this ILaptop laptop, int gb)
    {
        return new RAM(laptop, gb);
    }

    public static ILaptop WithHardDrive(this ILaptop laptop, int gb)
    {
        return new HardDrive(laptop, gb);
    }

    public static ILaptop WithWarranty(this ILaptop laptop, int year)
    {
        return new Warranty(laptop, year);
    }
}
```

In this example, we have defined an extension class `LaptopExtensions` that contains three extension methods `WithRAM`, `WithHardDrive` and `WithWarranty`. These methods accept an object of the `ILaptop` interface and return the same object decorated with the specified decorator.

To use these extension methods, you need to include the namespace of the extension class in the file where you want to use it.

```csharp
using ExtensionMethods;
```

You can then create a new object with multiple decorators in a single line of code by chaining the extension methods together.

```csharp
ILaptop laptop = new MacBook().WithRAM(8).WithHardDrive(512).WithWarranty(2);
```

This way you can use the Decorator Design pattern in a more readable and concise way, making the code more maintainable and easy to understand, also in this way you don't need to modify the original class to add new functionality.