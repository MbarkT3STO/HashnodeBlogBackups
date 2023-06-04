---
title: "Builder Design Pattern"
datePublished: Sun Jun 04 2023 09:21:24 GMT+0000 (Coordinated Universal Time)
cuid: clih7raxl000509kz187sd3r6
slug: builder-design-pattern
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685870191078/6a4c0f31-31b5-467b-9fca-2a87f8750174.png
tags: design-patterns, csharp, software-design

---

The Builder design pattern is a creational design pattern that allows for the construction of complex objects step by step. It separates the construction of an object from its representation, allowing the same construction process to create different representations.

## **Introduction to the Builder Design Pattern**

In software development, there are scenarios where objects need to be created with numerous attributes and options. Initializing an object with a long list of parameters can be cumbersome and error-prone. The Builder design pattern provides a solution by encapsulating the object creation process and simplifying the construction of complex objects.

The key idea behind the Builder pattern is to have a separate builder class that handles the construction logic. The builder class is responsible for creating and configuring the object step by step, ensuring that the object is created in a consistent and valid state.

## **Advantages of the Builder Design Pattern**

The Builder design pattern offers several advantages:

1. **Separation of concerns**: The pattern separates the construction logic from the object's representation, allowing for more flexibility and maintainability in the codebase.
    
2. **Complex object creation**: It simplifies the process of creating complex objects by breaking it down into smaller, manageable steps.
    
3. **Code readability**: The builder class provides a clear and readable API for constructing objects, making the code more self-explanatory.
    
4. **Consistent object creation**: The pattern ensures that the object is constructed in a consistent and valid state by enforcing the required steps in the construction process.
    
5. **Variety of object configurations**: The builder pattern allows for the creation of different configurations of an object by using different builders, without modifying the object's class.
    

## **Implementation of the Builder Design Pattern**

Let's consider an example where we need to create a `House` object with various attributes such as the number of rooms, bathrooms, and whether it has a garage or a swimming pool. We'll use the Builder design pattern to simplify the creation process.

First, we define the `House` class:

```csharp
public class House
{
    public int Rooms { get; set; }
    public int Bathrooms { get; set; }
    public bool HasGarage { get; set; }
    public bool HasSwimmingPool { get; set; }
}
```

Next, we create the `HouseBuilder` class responsible for constructing the `House` object:

```csharp
public class HouseBuilder
{
    private House house;

    public HouseBuilder()
    {
        house = new House();
    }

    public HouseBuilder SetRooms(int rooms)
    {
        house.Rooms = rooms;
        return this;
    }

    public HouseBuilder SetBathrooms(int bathrooms)
    {
        house.Bathrooms = bathrooms;
        return this;
    }

    public HouseBuilder SetHasGarage(bool hasGarage)
    {
        house.HasGarage = hasGarage;
        return this;
    }

    public HouseBuilder SetHasSwimmingPool(bool hasSwimmingPool)
    {
        house.HasSwimmingPool = hasSwimmingPool;
        return this;
    }

    public House Build()
    {
        return house;
    }
}
```

Finally, we can use the `HouseBuilder` to construct the `House` object step by step:

```csharp
HouseBuilder builder = new HouseBuilder();
House house = builder.SetRooms(3)
                     .SetBathrooms(2)
                     .SetHasGarage(true)
                     .SetHasSwimmingPool(false)
                     .Build();
```

The code above demonstrates how the Builder pattern simplifies the construction of the `House` object. Instead of passing multiple parameters to a constructor, we can use the builder's fluent interface to set the desired attributes individually.

## **Using Extension Methods with the Builder Design Pattern**

In addition to the basic implementation of the Builder design pattern, we can enhance the usability and readability of the code by leveraging C#'s extension methods. Extension methods allow us to add new methods to existing types without modifying their source code. Let's see how we can use extension methods to further improve the Builder pattern implementation.

First, let's define an extension class for the `HouseBuilder`:

```csharp
public static class HouseBuilderExtensions
{
    public static HouseBuilder WithRooms(this HouseBuilder builder, int rooms)
    {
        return builder.SetRooms(rooms);
    }

    public static HouseBuilder WithBathrooms(this HouseBuilder builder, int bathrooms)
    {
        return builder.SetBathrooms(bathrooms);
    }

    public static HouseBuilder WithGarage(this HouseBuilder builder)
    {
        return builder.SetHasGarage(true);
    }

    public static HouseBuilder WithSwimmingPool(this HouseBuilder builder)
    {
        return builder.SetHasSwimmingPool(true);
    }
}
```

The extension class adds additional methods to the `HouseBuilder` class, providing a more natural and expressive way to set the attributes of a `House`.

Now, let's see how we can use these extension methods to construct a `House` object:

```csharp
HouseBuilder builder = new HouseBuilder();

House house = builder.WithRooms(3)
                     .WithBathrooms(2)
                     .WithGarage()
                     .Build();
```

By using extension methods, we can chain the method calls in a fluent manner, making the code more concise and readable.

## **Conclusion**

The use of extension methods with the Builder design pattern enhances the flexibility and readability of the code. By extending the builder class with additional methods, we can provide a more natural and expressive API for constructing complex objects. Extension methods, combined with the Builder pattern, make the construction process more intuitive and user-friendly, leading to cleaner and more maintainable code.