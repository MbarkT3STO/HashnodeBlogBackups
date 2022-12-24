# What is 'default' keyword in C#?

In C#, the `default` keyword is used to retrieve the default value of a type. This is particularly useful when working with reference types or nullable value types.

## **Default values for reference types**

For reference types, the default value is `null`. For example:

```csharp
string str = default; // str is set to null
```

## **Default values for value types**

For value types, the default value is the default value of the type's underlying data type. For example:

```csharp
int i = default; // i is set to 0
double d = default; // d is set to 0.0
bool b = default; // b is set to false
```

## **Default values for nullable value types**

For nullable value types, the default value is `null`. For example:

```csharp
int? i = default; // i is set to null
double? d = default; // d is set to null
bool? b = default; // b is set to null
```

## **Using** `default` with generic types

The `default` keyword can also be used with generic types. For example:

```csharp
T DefaultValue<T>()
{
    return default;
}

int i = DefaultValue<int>(); // i is set to 0
string str = DefaultValue<string>(); // str is set to null
```

## **Using** `default` in conditional statements

The `default` keyword can also be used in conditional statements, such as a `switch` statement. For example:

```csharp
int i = 3;

switch (i)
{
    case 1:
        Console.WriteLine("Case 1");
        break;
    case 2:
        Console.WriteLine("Case 2");
        break;
    default:
        Console.WriteLine("Default case");
        break;
}
```

In this example, the `default` case will be executed because the value of `i` does not match any of the other cases.

## **Conclusion**

The `default` keyword is a useful tool in C# for retrieving the default value of a type. It can be used with reference types, value types, nullable value types, and generic types. It is also commonly used in conditional statements to specify a default case.