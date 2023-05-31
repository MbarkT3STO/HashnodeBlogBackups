---
title: "Functional Programming Paradigm"
datePublished: Wed May 31 2023 14:43:16 GMT+0000 (Coordinated Universal Time)
cuid: clibthtn0000109jodci32xhm
slug: functional-programming-paradigm
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685543768998/50718caf-22a1-42a9-9b61-ca21bd554841.webp
tags: csharp, paradigm

---

Functional programming is a programming paradigm that focuses on writing code by composing pure functions to solve problems. It emphasizes immutability, higher-order functions, and declarative programming. While C# is primarily an object-oriented language, it also supports functional programming features that allow developers to leverage the benefits of this paradigm. In this article, we will explore the key concepts of functional programming in C# and provide examples to demonstrate their usage.

## **Immutability**

One of the fundamental principles of functional programming is immutability, which means that once a value is assigned, it cannot be changed. In C#, we can achieve immutability by using the `readonly` keyword for fields and properties or by using immutable data structures provided by libraries like `System.Collections.Immutable`.

```csharp
public class ImmutablePerson
{
    public readonly string Name;
    public readonly int Age;

    public ImmutablePerson(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

In the above example, the `ImmutablePerson` class is immutable as its properties are declared as `readonly`. Once the properties are set in the constructor, their values cannot be modified, ensuring immutability.

## **Higher-Order Functions**

Functional programming promotes the use of higher-order functions, which are functions that can take other functions as arguments or return functions as results. In C#, delegates and lambda expressions enable the creation and usage of higher-order functions.

```csharp
public static int Add(int a, int b)
{
    return a + b;
}

public static int Multiply(int a, int b)
{
    return a * b;
}

public static int Calculate(int a, int b, Func<int, int, int> operation)
{
    return operation(a, b);
}

// Usage
int result = Calculate(2, 3, Add);       // result = 5
int result2 = Calculate(2, 3, Multiply); // result2 = 6
```

In the above example, the `Calculate` function accepts two integers and a function `operation` that performs a specific arithmetic operation. By passing different functions (`Add` and `Multiply`), we can achieve different results, making the `Calculate` function higher-order.

## **Pure Functions**

Pure functions are essential in functional programming as they produce the same output for the same input and have no side effects. C# allows the creation of pure functions by avoiding the modification of external state and relying on parameters for computation.

```csharp
public static int Square(int number)
{
    return number * number;
}
```

The `Square` function is a pure function as it only depends on the input parameter `number` and returns the square of that number. It does not modify any external state or have side effects, making it predictable and reusable.

## **Declarative Programming**

Functional programming encourages declarative programming, where the focus is on describing what the code should accomplish rather than how to accomplish it. C# provides LINQ (Language Integrated Query) for writing declarative code.

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

var squares = from number in numbers
              select number * number;
```

In the above example, we use LINQ to declare that we want to square each number in the `numbers` list. The result is a new sequence (`squares`) that contains the squared values without explicitly defining how the computation is performed.

## **Pattern Matching**

C# 7.0 introduced pattern matching, which is a powerful feature of functional programming. It allows developers to match patterns in data structures and perform actions based on those patterns.

```csharp
public static string GetShapeDescription(object shape)
{
    switch (shape)
    {
        case Circle c:
            return $"Circle with radius {c.Radius}";
        case Rectangle r:
            return $"Rectangle with width {r.Width} and height {r.Height}";
        default:
            return "Unknown shape";
    }
}

// Usage
var circle = new Circle(5);
var description = GetShapeDescription(circle); // "Circle with radius 5"
```

In the above example, the `GetShapeDescription` function uses pattern matching to determine the type of the `shape` object and perform the corresponding action. By matching different patterns, we can handle various scenarios and provide appropriate behavior.

## **Recursion**

Recursion is another key concept in functional programming. It involves solving problems by breaking them down into smaller subproblems and solving each subproblem recursively. C# supports recursion, allowing developers to implement algorithms that exhibit self-referential behavior.

```csharp
public static int Factorial(int n)
{
    if (n == 0)
        return 1;
    else
        return n * Factorial(n - 1);
}

// Usage
int result = Factorial(5); // result = 120
```

In the above example, the `Factorial` function calculates the factorial of a given number `n`. It uses recursion to break down the problem into smaller subproblems by calling itself with a smaller input (`n - 1`) until reaching the base case (`n == 0`), where the recursion stops.

## **Immutable Data Structures**

In functional programming, immutability extends beyond individual values to entire data structures. Immutable data structures ensure that once created, their state cannot be modified. C# provides libraries like `System.Collections.Immutable` that offer immutable data structures, such as lists, dictionaries, and queues.

```csharp
using System.Collections.Immutable;

public static void UpdateList(ImmutableList<int> numbers)
{
    numbers = numbers.Add(4);
}

// Usage
var numbers = ImmutableList<int>.Empty;
UpdateList(numbers);
Console.WriteLine(numbers.Count); // Output: 0
```

In the above example, the `UpdateList` function takes an `ImmutableList<int>` as a parameter. However, attempting to modify the list inside the function has no effect because the `Add` method returns a new list with the added element, leaving the original list unchanged.

## **Composition and Pipelines**

Functional programming promotes the composition of functions to build complex operations. C# provides various techniques for function composition, such as lambda expressions and LINQ's fluent syntax. These enable developers to create pipelines where the output of one function becomes the input for the next.

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };

var squaredAndFiltered = numbers
    .Where(n => n % 2 == 0)
    .Select(n => n * n);

foreach (var number in squaredAndFiltered)
{
    Console.WriteLine(number);
}
```

In the above example, we use the LINQ methods `Where` and `Select` to create a pipeline of operations on the `numbers` list. The `Where` method filters the even numbers, and the `Select` method squares each number. The result is a sequence of squared and filtered numbers that are printed using a `foreach` loop.

## **Conclusion**

Functional programming in C# introduces a different approach to writing code by emphasizing immutability, higher-order functions, pure functions, declarative programming, and pattern matching. While C# is primarily an object-oriented language, incorporating functional programming principles can enhance code quality, readability, and maintainability. By leveraging the functional programming features available in C#, developers can explore new avenues for solving problems efficiently and elegantly.