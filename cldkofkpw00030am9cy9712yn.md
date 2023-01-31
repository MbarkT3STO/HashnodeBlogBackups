# Interface vs Abstract Class in C#

In object-oriented programming, a class is a blueprint for creating objects (a particular data structure), providing initial values for state (member variables or attributes), and implementations of behavior (member functions or methods).

In C#, there are two ways to define a class that cannot be instantiated on its own, but can only be inherited by other classes: interfaces and abstract classes. Both interfaces and abstract classes provide a way to define a contract for derived classes, but they have some important differences that affect how they can be used.

## **Interface**

An interface defines a contract for a class, specifying a set of methods and properties that a class must implement. An interface can be thought of as a blueprint for a class, specifying what methods and properties the class must have, but not providing any implementation.

For example, the following interface defines a contract for a class that can be used to draw shapes:

```csharp
interface IDrawable
{
    void Draw();
}
```

A class that implements the IDrawable interface must provide an implementation of the Draw() method.

```csharp
class Circle : IDrawable
{
    public void Draw()
    {
        Console.WriteLine("Drawing a circle");
    }
}
```

An interface can be implemented by multiple classes, and a class can implement multiple interfaces.

## **Abstract Class**

An abstract class is a base class that cannot be instantiated on its own, but can be inherited by derived classes. Like an interface, an abstract class can define abstract methods and properties that must be implemented by derived classes. However, an abstract class can also provide a default implementation for some or all of its methods and properties.

For example, the following abstract class defines a base class for a shape:

```csharp
abstract class Shape
{
    public abstract void Draw();
}
```

A class that inherits from the Shape class must provide an implementation of the Draw() method.

```csharp
class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle");
    }
}
```

An abstract class in C# can be inherited by multiple classes, and a class can only inherit from a single abstract class.

## **Differences**

The main differences between interfaces and abstract classes are:

* An interface defines a contract, but an abstract class can provide a default implementation.
    
* A class can implement multiple interfaces, but can only inherit from a single abstract class.
    
* An interface cannot contain any implementation, while an abstract class can contain some implementation.
    
* An interface cannot contain any fields, while an abstract class can contain fields.
    

Both interfaces and abstract classes have their own use cases, and the choice between them depends on the specific requirements of your project. In general, you should use an interface when you want to define a contract that can be implemented by multiple classes, and an abstract class when you want to provide a common implementation for multiple derived classes.

## **Real-world Examples**

### **Custom Interface Example: ITransportable**

Consider a scenario where you want to define a contract for a class that can be transported. The contract could be defined using an interface named `ITransportable`.

```csharp
interface ITransportable
{
    void Load();
    void Unload();
}
```

Now any class that implements the `ITransportable` interface must provide implementations for the `Load` and `Unload` methods.

```csharp
class Car : ITransportable
{
    public void Load()
    {
        Console.WriteLine("Loading the car");
    }
    public void Unload()
    {
        Console.WriteLine("Unloading the car");
    }
}

class Bicycle : ITransportable
{
    public void Load()
    {
        Console.WriteLine("Loading the bicycle");
    }
    public void Unload()
    {
        Console.WriteLine("Unloading the bicycle");
    }
}
```

### **Custom Abstract Class Example: Vehicle**

Now consider a scenario where you want to define a base class for vehicles. The base class could be defined as an abstract class named `Vehicle`.

```csharp
abstract class Vehicle
{
    public abstract void Start();
    public abstract void Stop();

    public void Honk()
    {
        Console.WriteLine("Beep Beep");
    }
}
```

Now, any class that inherits from the `Vehicle` abstract class must provide implementations for the `Start` and `Stop` methods.

```csharp
class Car : Vehicle
{
    public override void Start()
    {
        Console.WriteLine("Starting the car");
    }
    public override void Stop()
    {
        Console.WriteLine("Stopping the car");
    }
}

class Bicycle : Vehicle
{
    public override void Start()
    {
        Console.WriteLine("Starting the bicycle");
    }
    public override void Stop()
    {
        Console.WriteLine("Stopping the bicycle");
    }
}
```

In this example, the `Vehicle` abstract class provides a default implementation of the `Honk` method that can be used by any class that inherits from it.

Note that the `Vehicle` abstract class can also implement the `ITransportable` interface, making it a transportable vehicle.

## **When to use Interfaces and Abstract Classes?**

### **Interfaces**

* ### Use an interface when you want to define a contract that multiple classes can implement.
    
* Use an interface when you want to define a contract for a class that doesn't have any implementation.
    
* Use an interface when you want to define a contract for a class that can be transported, serialized, or compared.
    
* Use an interface when you want to group related methods and properties that can be used by multiple classes.
    

### **Abstract Classes**

* Use an abstract class when you want to provide a base class with shared implementation.
    
* Use an abstract class when you want to enforce a common behavior for derived classes.
    
* Use an abstract class when you want to provide default implementations for some methods.
    
* Use an abstract class when you want to define a base class that can be extended but not instantiated on its own.
    

It's important to note that an abstract class can also implement interfaces and derive from other abstract classes, whereas an interface can inherit from multiple interfaces but cannot provide implementation. In general, use interfaces when defining contracts and abstract classes when defining base classes.