# Introduction to Variance and Contravariance in C#

In C#, variance and contravariance are concepts that allow for flexibility in the use of generic types. These concepts allow developers to use generic types in a way that is more intuitive and easier to read. In this article, we will take a look at what variance and contravariance are, how they are used in C#, and some examples of their use.

## **What is Variance?**

In C#, variance refers to the ability of a type parameter in a generic class or interface to be used in a more flexible way. There are two types of variance: covariance and contravariance.

Covariance allows a generic type to be used in a more general way. For example, if a class `MyClass<T>` has a type parameter `T`, covariance allows us to use `MyClass<T>` with a type that is more derived (inherits from) the type `T`.

Contravariance, on the other hand, allows a generic type to be used in a more specific way. It allows us to use `MyClass<T>` with a type that is a base (ancestor) of the type `T`.

## **How to Use Variance in C#**

To use variance in C#, we need to use the `out` and `in` keywords. The `out` keyword is used for covariance, and the `in` keyword is used for contravariance.

Here is an example of how to use the `out` keyword to create a covariant generic interface:

```csharp
public interface ICovariant<out T>
{
    T GetValue();
}
```

And here is an example of how to use the `in` keyword to create a contravariant generic interface:

```csharp
public interface IContravariant<in T>
{
    void SetValue(T value);
}
```

## **Examples of Variance in C#**

Here are some examples of how variance can be used in C#.

### **Example 1: Covariant Generic Interface**

In this example, we have a generic interface `ICovariant<T>` that has a covariant type parameter `T`. This allows us to use the interface with a type that is more derived than the type specified in the type parameter.

```csharp
public interface ICovariant<out T>
{
    T GetValue();
}

public class MyClass<T> : ICovariant<T>
{
    private T value;

    public MyClass(T value)
    {
        this.value = value;
    }

    public T GetValue()
    {
        return value;
    }
}

public class DerivedClass : MyClass<string>
{
    public DerivedClass(string value) : base(value) { }
}

ICovariant<string> covariant = new DerivedClass("hello");
```

In this example, we are able to use the `DerivedClass` with the `ICovariant<T>` interface, even though `DerivedClass` is derived from `MyClass<T>`, which in turn is derived from `ICovariant<T>`. This is because the type parameter `T` in `ICovariant<T>` is covariant.

### **Example 2: Contravariant Generic Interface**

In this example, we have a generic interface `IContravariant<T>` that has a contravariant type parameter `T`. This allows us to use the interface with a type that is a base of the type specified in the type parameter.

```csharp
public interface IContravariant<in T>
{
    void SetValue(T value);
}

public class MyClass<T> : IContravariant<T>
{
    private T value;

    public void SetValue(T value)
    {
        this.value = value;
    }
}

public class BaseClass
{
}

public class DerivedClass : BaseClass
{
}

IContravariant<BaseClass> contravariant = new MyClass<DerivedClass>();
contravariant.SetValue(new DerivedClass());
```

In this example, we are able to use the `MyClass<T>` with the `IContravariant<T>` interface, even though `MyClass<T>` implements `IContravariant<T>`, and `T` is of type `DerivedClass`. This is because the type parameter `T` in `IContravariant<T>` is contravariant, which allows us to use it with a type that is a base of `DerivedClass`.

## Real-World Examples

### **Example 1: Covariant IEnumerable&lt;T&gt;**

The `IEnumerable<T>` interface in C# is covariant, which means that we can use it with a type that is more derived than the type specified in the type parameter. This allows us to write code that is more intuitive and easier to read.

For example, consider the following code:

```csharp
List<string> stringList = new List<string> { "hello", "world" };
IEnumerable<string> strings = stringList;
```

In this code, we are able to use the `List<string>` class with the `IEnumerable<string>` interface, even though `List<T>` is derived from `IEnumerable<T>`. This is because the type parameter `T` in `IEnumerable<T>` is covariant, which allows us to use it with a type that is more derived than `string`.

### **Example 2: Contravariant Action&lt;T&gt;**

The `Action<T>` delegate in C# is contravariant, which means that we can use it with a type that is a base of the type specified in the type parameter. This allows us to write code that is more intuitive and easier to read.

For example, consider the following code:

```csharp
Action<object> action = o => Console.WriteLine(o);
Action<string> stringAction = action;
stringAction("hello");
```

In this code, we are able to use the `Action<object>` delegate with the `Action<string>` delegate, even though `string` is a base of `object`. This is because the type parameter `T` in `Action<T>` is contravariant, which allows us to use it with a type that is a base of `string`.

### **Example: Covariant and Contravariant LINQ Methods**

The LINQ (Language Integrated Query) methods in C#, such as `Where`, `Select`, and `OrderBy`, use variance and contravariance to allow for greater flexibility in their use.

For example, consider the following code that uses the `Where` method:

```csharp
IEnumerable<object> objects = new List<object> { 1, "hello", 2, "world" };
IEnumerable<string> strings = objects.Where(o => o is string);
```

In this code, we are able to use the `Where` method with the `objects` collection, even though it is of type `IEnumerable<object>` and the `Where` method expects an `IEnumerable<string>`. This is because the `IEnumerable<T>` interface is covariant, which allows us to use it with a type that is more derived than `object`.

Similarly, we can use the `Select` and `OrderBy` methods in a similar way:

```csharp
IEnumerable<string> strings = objects.Select(o => (string)o);
IEnumerable<string> strings = objects.OrderBy(o => (string)o);
```

In these examples, the `Select` and `OrderBy` methods use contravariant type parameters, which allows us to use them with a type that is a base of `string`.

Overall, variance and contravariance in C# allow for greater flexibility and simplicity in the use of LINQ methods, making it easier to write and maintain LINQ queries.

## **Conclusion**

Variance and contravariance in C# allow for greater flexibility in the use of generic types. By using the `out` and `in` keywords, we can create covariant and contravariant generic interfaces and classes, which can be used in a more intuitive and easier to read way.