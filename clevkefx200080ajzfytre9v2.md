---
title: "Casing Methods in C#: A Guide to When to Use Each One"
datePublished: Sun Mar 05 2023 15:45:16 GMT+0000 (Coordinated Universal Time)
cuid: clevkefx200080ajzfytre9v2
slug: casing-methods-in-c-a-guide-to-when-to-use-each-one
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1678030949967/193a8e8d-8e1d-4310-9cd9-43d50fb64444.webp
tags: csharp, programming-blogs, net, clean-code

---

In C#, casing methods are used to define the naming convention of variables, methods, classes, and other elements of code. There are several different casing methods to choose from, each with its own advantages and disadvantages. In this guide, we'll cover the most common casing methods and when to use each one.

## **Pascal Case**

Pascal case is a casing method where the first letter of each word in a name is capitalized, and there are no spaces between words. For example, `CustomerName` is an example of a variable in Pascal case. This casing method is often used for naming classes, properties, and methods in C#.

When to use Pascal case:

* For naming classes, properties, and methods in C#.
    
* When you want to create a name that is easy to read and understand.
    
* When you want to conform to the .NET Framework Naming Guidelines.
    

Example:

```csharp
public class CustomerManager
{
    public string CustomerName { get; set; }

    public void UpdateCustomer()
    {
        // Method code here
    }
}
```

## **Camel Case**

Camel case is a casing method where the first letter of the first word is lowercase, and the first letter of each subsequent word is capitalized. There are no spaces between words. For example, `customerName` is an example of a variable in camel case. This casing method is often used for naming local variables and method parameters.

When to use Camel case:

* For naming local variables and method parameters.
    
* When you want to create a name that is easy to read and understand.
    

Example:

```csharp
public void UpdateCustomer(string customerName, int customerId)
{
    // Method code here
}
```

## **Snake Case**

Snake case is a casing method where each word is separated by an underscore. For example, `customer_name` is an example of a variable in snake case. This casing method is not commonly used in C#, but is more common in other programming languages.

When to use Snake case:

* When working with other programming languages that use snake case.
    
* When you want to create a name that is easy to read and understand.
    

Example:

```csharp
public class Customer_Manager
{
    public string customer_name { get; set; }

    public void update_customer()
    {
        // Method code here
    }
}
```

## **Kebab Case**

Kebab case is a casing method where each word is separated by a hyphen. For example, `customer-name` is an example of a variable in kebab case. **<mark>This casing method is not commonly used/supported in C#</mark>**, but is more common in other programming languages.

When to use Kebab case:

* When working with other programming languages that use kebab case.
    
* When you want to create a name that is easy to read and understand.
    
* Routes.
    

Example:

```csharp
public class CustomerManager
{
    public string customer-name { get; set; }

    public void update-customer()
    {
        // Method code here
    }
}
```

## **Conclusion**

Choosing the right casing method in C# can help make your code more readable and easier to understand. When choosing a casing method, consider the context in which it will be used and whether it conforms to any naming conventions or guidelines. By following these best practices, you can create code that is both easy to read and easy to maintain.