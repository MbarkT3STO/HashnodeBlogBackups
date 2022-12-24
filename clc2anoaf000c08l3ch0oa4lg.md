# What is the Null Coalescing Operator (??) in C#?

The null coalescing operator (`??`) is a shorthand way to check for a null value in C#. It is used to assign a default value to a variable if the variable is `null`.

Here is the syntax for using the null coalescing operator:

```csharp
variable = expression1 ?? expression2;
```

The `expression1` is evaluated first. If it is not `null`, its value is returned. If `expression1` is `null`, `expression2` is evaluated and its value is returned.

Here is an example of using the null coalescing operator:

```csharp
string str = null;
string result = str ?? "default value";
Console.WriteLine(result);  // Outputs "default value"
```

In the example above, the variable `str` is `null`, so the value of `"default value"` is assigned to `result`.

The null coalescing operator can be used in a chain to check for multiple null values:

```csharp
string str1 = null;
string str2 = null;
string result = str1 ?? str2 ?? "default value";
console.WriteLine(result);  // Outputs "default value"
```

In the example above, both `str1` and `str2` are `null`, so the value of `"default value"` is assigned to `result`.

The null coalescing operator can also be used with classes and objects:

```csharp
Person person = null;
string result = person?.Name ?? "default value";
console.WriteLine(result);  // Outputs "default value"
```

In the example above, the `person` object is `null`, so the value of `"default value"` is assigned to `result`.

The null coalescing operator is a useful tool for handling null values in C#, allowing you to provide a default value if a variable is `null`. It can save you time and reduce the amount of code you need to write by combining the null check and assignment into a single statement.