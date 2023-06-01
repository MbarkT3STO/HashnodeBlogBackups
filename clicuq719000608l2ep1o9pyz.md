---
title: "Clean Code: Pure Functions"
datePublished: Thu Jun 01 2023 08:05:32 GMT+0000 (Coordinated Universal Time)
cuid: clicuq719000608l2ep1o9pyz
slug: clean-code-pure-functions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685606459272/fe6707ff-b959-4b17-b66c-fdf8a468f300.png
tags: csharp, clean-code

---

When it comes to writing clean and maintainable code, one of the fundamental principles is to strive for pure functions. Pure functions are an essential concept in functional programming and can greatly enhance the readability, testability, and overall quality of your code. In this article, we will explore what pure functions are, why they are valuable, and how to write them effectively using C#.

## **Understanding Pure Functions**

In simple terms, a pure function is a function that, given the same input, will always produce the same output and has no side effects. This means that pure functions do not modify any external state, such as global variables or mutable objects, and do not perform I/O operations. Pure functions rely solely on their input parameters to compute a result.

The benefits of using pure functions are numerous:

1. **Readability:** Pure functions have a clear and concise structure since they only depend on their inputs and do not reference any external state. This makes it easier to understand their purpose and behavior.
    
2. **Testability:** Pure functions are inherently easier to test because they do not rely on complex setup or external dependencies. You can simply provide input values and assert the expected output, making unit testing a breeze.
    
3. **Predictability:** Since pure functions do not have side effects, they offer predictable behavior. Given the same input, you can always expect the same output, regardless of when or how many times you invoke the function.
    
4. **Concurrency:** Pure functions are inherently thread-safe, as they do not modify shared state. This property makes it easier to reason about and parallelize your code when dealing with concurrent or distributed systems.
    

## **Writing Pure Functions in C#**

Now, let's dive into some examples of how to write pure functions using C#.

```csharp
// Example 1: Pure function without side effects
public int Add(int a, int b)
{
    return a + b;
}
```

In this example, the `Add` function takes two integers as input and returns their sum. It does not modify any external state and relies solely on its input parameters. Hence, it is a pure function.

```csharp
// Example 2: Pure function with immutable data
public string FormatName(string firstName, string lastName)
{
    return $"{lastName}, {firstName}";
}
```

The `FormatName` function takes a first name and a last name as input and returns a formatted full name. It does not change the values of the input parameters or any external state. The returned value is derived solely from the input, making it a pure function.

```csharp
// Example 3: Pure function with collections
public bool IsEvenNumber(List<int> numbers, int target)
{
    return numbers.Contains(target) && target % 2 == 0;
}
```

In this example, the `IsEvenNumber` function checks if a given list of numbers contains a specific target value and whether the target value is even. It does not modify the input list or any external state, making it a pure function.

## **Tips for Writing Pure Functions in C#**

To write effective pure functions in C#, consider the following tips:

### **1\. Avoid modifying mutable objects**

In functional programming, immutable objects are preferred over mutable ones. When writing pure functions, try to avoid modifying mutable objects directly. Instead, create new instances or use immutable data structures to produce the desired results.

```csharp
// Bad: Modifying mutable object
public void UpdateName(Person person, string newName)
{
    person.Name = newName;
}

// Good: Returning a new instance with updated name
public Person UpdateName(Person person, string newName)
{
    return new Person { Name = newName, Age = person.Age };
}
```

### **2\. Minimize dependencies on external state**

Pure functions should rely solely on their input parameters. Avoid referencing global variables, static properties, or other external state within your functions. This ensures that the behavior of the function remains consistent and predictable.

```csharp
// Bad: Reliance on external state
public int GetNextId()
{
    return _idGenerator.GenerateId(); // Depends on external state
}

// Good: Pure function with no external state
public int GetNextId(int currentId)
{
    return currentId + 1;
}
```

### **3\. Handle null values gracefully**

When dealing with nullable reference types in C#, it's important to handle null values gracefully in pure functions. Consider using nullability annotations and leveraging null coalescing (`??`) or null conditional (`?.`) operators to ensure safe and reliable code execution.

```csharp
// Bad: Unsafe handling of nullable reference type
public string GetFullName(string firstName, string lastName)
{
    return firstName + " " + lastName; // May throw NullReferenceException
}

// Good: Handling nullable reference types
public string GetFullName(string? firstName, string? lastName)
{
    return $"{firstName ?? ""} {lastName ?? ""}";
}
```

### **4\. Avoid I/O operations**

Pure functions should not perform I/O operations, such as reading from files or making network requests. I/O operations introduce side effects and can impact the purity of a function. Instead, consider separating I/O-related logic into separate impure functions or external components.

```csharp
// Bad: I/O operation in a pure function
public bool IsFileExists(string filePath)
{
    return File.Exists(filePath); // I/O operation
}

// Good: Delegating I/O to an impure function or external component
public bool IsFileExists(string filePath)
{
    return FileHelper.CheckFileExists(filePath);
}
```

### **5\. Write unit tests for pure functions**

Since pure functions have well-defined inputs and outputs, they are ideal candidates for unit testing. Take advantage of the predictability and testability of pure functions by writing comprehensive unit tests that cover different scenarios and edge cases.

```csharp
[Test]
public void Add_WithPositiveNumbers_ReturnsSum()
{
    // Arrange
    int a = 5;
    int b = 10;

    // Act
    int result = Add(a, b);

    // Assert
    Assert.AreEqual(15, result);
}
```

By following these tips, you can write cleaner and more maintainable code using pure functions in C#. Embracing the functional programming paradigm and reducing side effects in your codebase will enhance its readability, testability, and overall quality.