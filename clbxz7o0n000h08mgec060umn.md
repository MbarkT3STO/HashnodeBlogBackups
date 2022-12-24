# Inheritance vs Composition in C#

In C# and other object-oriented programming languages, inheritance and composition are two design patterns that allow you to reuse code and create relationships between classes.

## **Inheritance**

Inheritance is a way to create a new class that is a modified version of an existing class. The new class, called the subclass, inherits the properties and methods of the existing class, called the superclass.

In C#, inheritance is implemented using the `:` operator and the `base` keyword.

For example, consider the following code:

```csharp
public class Animal
{
    public string Name { get; set; }

    public Animal(string name)
    {
        Name = name;
    }

    public virtual void MakeNoise()
    {
        Console.WriteLine("Some noise");
    }
}

public class Dog : Animal
{
    public Dog(string name) : base(name)
    {
    }

    public override void MakeNoise()
    {
        Console.WriteLine("Bark!");
    }
}
```

Here, we define a `Animal` class with a `Name` property and a `MakeNoise` method. We then define a `Dog` class that inherits from the `Animal` class and overrides the `MakeNoise` method to print "Bark!" to the console instead of the default "Some noise".

## **Composition**

Composition is a way to create a new class by combining multiple existing classes. The new class, called the composite class, delegates certain tasks to the contained classes, called the component classes.

In C#, composition is implemented using object properties and method calls.

For example, consider the following code:

```csharp
public class Engine
{
    public void Start()
    {
        Console.WriteLine("Engine started");
    }
}

public class Car
{
    private readonly Engine _engine;

    public Car(Engine engine)
    {
        _engine = engine;
    }

    public void Start()
    {
        _engine.Start();
    }
}
```

Here, we define an `Engine` class with a `Start` method and a `Car` class that contains an instance of the `Engine` class. The `Car` class has a `Start` method that delegates the task of starting the engine to the contained `Engine` instance.

## **Comparison**

Inheritance and composition have some similarities and some differences. Here is a comparison of the two:

|  | **Inheritance** | **Composition** |
| --- | --- | --- |
| Code reuse | Inherits properties and methods from a superclass | Delegates tasks to contained component classes |
| Relationship between classes | **Is-a** relationship (a subclass is a type of superclass) | **Has-a** relationship (a composite class has one or more component classes) |
| Flexibility | Subclasses can override or extend the behavior of their superclasses | Composite classes can easily change the behavior of their component classes by replacing them with different implementations |
| Coupling | Tight coupling (subclasses are tightly coupled to their superclasses) | Loose coupling (composite classes are loosely coupled to their component classes) |

## **Conclusion**

Inheritance and composition are two design patterns that allow you to reuse code and create relationships between classes in C#. Inheritance allows you to create a new class that is a modified version of an existing class, while composition allows you to create a new class by combining multiple existing classes.

In general, inheritance is useful when you want to create a subclass that is a specific type of its superclass, and you want to reuse most of the superclass's behavior. Composition is useful when you want to create a new class that delegates certain tasks to contained component classes, and you want to have more flexibility in changing the behavior of the composite class.

It is important to choose the right design pattern for your project, as it can affect the flexibility and maintainability of your code. In some cases, it may be appropriate to use both inheritance and composition together in a single project.