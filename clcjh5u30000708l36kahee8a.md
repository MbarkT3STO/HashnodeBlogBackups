# Default Interface Method in C#

C# 8.0 introduced a new feature called default interface methods, also known as "interface members." This feature allows developers to add non-abstract methods to interfaces and provide a default implementation for those methods.

Prior to this feature, an interface could only contain abstract methods, which must be implemented by any class that implements the interface. This can be useful for ensuring that certain behavior is implemented in classes that implement the interface, but it can also be limiting. Default interface methods allow for more flexibility by providing a way for interfaces to specify optional behavior.

Here is an example of an interface with a default method:

```csharp
interface IShape
{
    double Area { get; }

    double Circumference()
    {
        return 2 * Math.PI * Math.Sqrt(Area / Math.PI);
    }
}
```

Any class that implements `IShape` can choose to use the default implementation of `Circumference` or provide its own implementation.

```csharp
class Circle : IShape
{
    public double Radius { get; set; }

    public double Area => Math.PI * Math.Pow(Radius, 2);
}
```

In this example, the `Circle` class is using the default implementation of `Circumference` provided by the `IShape` interface.

## **Overriding Default Interface Methods**

A class can also choose to override the default implementation of a default interface method by providing its own implementation.

```csharp
class Rectangle : IShape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public double Area => Width * Height;

    public double Circumference()
    {
        return 2 * (Width + Height);
    }
}
```

In this example, the `Rectangle` class is providing its own implementation of the `Circumference` method, which is different from the default implementation provided by the `IShape` interface.

## **Using Default Interface Methods with Inheritance**

Default interface methods can also be used in combination with inheritance. A class that implements an interface can inherit from a base class and override the default interface method in the base class.

```csharp
abstract class Shape
{
    public abstract double Area { get; }

    public virtual double Circumference()
    {
        return 2 * Math.PI * Math.Sqrt(Area / Math.PI);
    }
}

class Square : Shape, IShape
{
    public double SideLength { get; set; }

    public override double Area => Math.Pow(SideLength, 2);
}
```

In this example, the `Square` class inherits from the `Shape` class and implements the `IShape` interface. It overrides the `Circumference` method inherited from the `Shape` class and provides its own implementation.

## **Conclusion**

Default interface methods provide a way to add optional behavior to interfaces in C#. They can be used to specify default behavior that can be overridden by implementing classes, and they can also be used in combination with inheritance. This feature allows for more flexibility in designing interfaces and can make it easier to add new behavior to existing interfaces.