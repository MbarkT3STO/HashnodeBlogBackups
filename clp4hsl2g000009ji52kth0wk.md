---
title: "The Result Design Pattern"
datePublished: Sat Nov 18 2023 20:19:16 GMT+0000 (Coordinated Universal Time)
cuid: clp4hsl2g000009ji52kth0wk
slug: the-result-design-pattern
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1700338125551/c4746b66-ed56-4030-9f6f-890a6b3a7c2d.webp
tags: design-patterns, csharp, net

---

In the world of programming, developers often encounter scenarios where the traditional approach to handling errors might fall short. This is where the Result pattern comes into play, offering a robust mechanism for managing success and failure outcomes.

## Understanding the Result Pattern

The Result pattern is a design pattern used to represent the outcome of an operation, be it successful or unsuccessful. Instead of relying on exceptions for error handling, the Result pattern encourages a more explicit and predictable approach.

In C#, the Result pattern is commonly implemented using a generic type that encapsulates both the result value and any associated error information. This empowers developers to handle outcomes in a more controlled manner.

## Implementing the Result Pattern

Let's walk through the steps to implement the Result pattern in C#.

### Step 1: Define the Result Class

```csharp
public class Result<T>
{
    public T Value { get; }
    public string Error { get; }
    public bool IsSuccess => Error == null;

    private Result(T value, string error)
    {
        Value = value;
        Error = error;
    }

    public static Result<T> Success(T value) => new Result<T>(value, null);
    public static Result<T> Failure(string error) => new Result<T>(default, error);
}
```

### Step 2: Usage Examples

Now, let's explore how to use the Result pattern with practical examples.

#### Example 1: Successful Operation

```csharp
public Result<int> Divide(int numerator, int denominator)
{
    if (denominator == 0)
        return Result<int>.Failure("Cannot divide by zero.");

    return Result<int>.Success(numerator / denominator);
}
```

#### Example 2: Unsuccessful Operation

```csharp
public Result<string> ValidateInput(string input)
{
    if (string.IsNullOrWhiteSpace(input))
        return Result<string>.Failure("Input cannot be empty or whitespace.");

    return Result<string>.Success("Input is valid.");
}
```

## Advanced Usage: Result Pattern with Error Details

Expanding on the basic Result pattern, you can enhance error handling by incorporating additional details about the encountered issues. Let's explore an advanced version of the Result class.

### Step 3: Enhancing the Result Class

```csharp
public class Result<T>
{
    public T Value { get; }
    public bool IsSuccess => ErrorMessages.Count == 0;
    public IReadOnlyList<string> ErrorMessages { get; }

    private Result(T value, List<string> errorMessages)
    {
        Value = value;
        ErrorMessages = errorMessages ?? new List<string>();
    }

    public static Result<T> Success(T value) => new Result<T>(value, new List<string>());
    public static Result<T> Failure(string errorMessage) => new Result<T>(default, new List<string> { errorMessage });

    public static Result<T> Failure(IEnumerable<string> errorMessages) =>
        new Result<T>(default, new List<string>(errorMessages));
}
```

### Step 4: Updated Usage Examples

Let's modify the previous examples to include more detailed error information.

#### Example 1: Successful Operation

```csharp
public Result<int> Divide(int numerator, int denominator)
{
    if (denominator == 0)
        return Result<int>.Failure("Cannot divide by zero.");

    return Result<int>.Success(numerator / denominator);
}
```

#### Example 2: Unsuccessful Operation with Error Details

```csharp
public Result<string> ValidateInput(string input)
{
    var errorMessages = new List<string>();

    if (string.IsNullOrWhiteSpace(input))
        errorMessages.Add("Input cannot be empty or whitespace.");g

    // Additional validation logic and error messages can be added here.

    return Result<string>.Failure(errorMessages);
}
```