---
title: "Clean Code: Indentation and Code Style"
datePublished: Thu Jun 01 2023 08:13:42 GMT+0000 (Coordinated Universal Time)
cuid: clicv0ot4000e09mjb5rjcnmm
slug: clean-code-indentation-and-code-style
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685607102854/b667ca88-343a-4606-855a-d58faabb4f26.png
tags: csharp, clean-code

---

When it comes to writing code, readability and maintainability are of utmost importance. Clean code not only makes it easier for developers to understand and modify the code but also contributes to the overall efficiency of the software development process. In this article, we will focus on the significance of proper indentation and code style in C# programming, along with examples to illustrate best practices.

## **Indentation**

Indentation refers to the consistent spacing used to visually separate blocks of code and make the structure more apparent. It helps improve code readability and assists in quickly identifying logical blocks and their relationships. In C#, indentation is typically achieved by using a consistent number of spaces or tabs for each level of indentation. The most common practice is to use four spaces for each level.

Let's consider an example of a method in C# without proper indentation:

```csharp
public void CalculateTotal()
{
int quantity = 5;
double price = 10.99;
double total = quantity * price;
Console.WriteLine("Total: " + total);
}
```

The code above lacks indentation, making it difficult to discern the structure and logical flow at a glance. Now, let's apply proper indentation to the same code:

```csharp
public void CalculateTotal()
{
    int quantity = 5;
    double price = 10.99;
    double total = quantity * price;
    Console.WriteLine("Total: " + total);
}
```

With the use of consistent indentation, it becomes evident that `quantity`, `price`, and `total` are local variables within the method, and `Console.WriteLine` is a statement inside the method. The code becomes much more readable and easier to understand.

## **Code Style**

Code style encompasses various conventions and guidelines for writing code. Consistency in code style across a project or team is crucial for maintaining a coherent and professional codebase. In C#, several established conventions exist, such as naming conventions, spacing conventions, and capitalization conventions.

### **Naming Conventions**

C# follows specific naming conventions to enhance code readability. Some key guidelines are as follows:

* **PascalCase**: Use PascalCase for class names, method names, and properties. For example: `public class MyClass`, `public void MyMethod()`, `public string MyProperty { get; set; }`.
    
* **camelCase**: Use camelCase for local variables and parameters. For example: `int myVariable`, `void myMethod(int myParameter)`.
    
* **ALL\_CAPS**: Use all capital letters for constants. For example: `const int MAX_COUNT = 100`.
    

### **Spacing Conventions**

Proper spacing within code plays a vital role in its readability. Consistent spacing improves the visual structure and comprehension. Here are some spacing conventions to follow in C#:

* Place a single space between keywords, operators, and operands. For example: `int sum = 2 + 2;`
    
* Use a single space after commas in argument lists and declarations. For example: `void MyMethod(int arg1, int arg2)`.
    
* Place opening braces (`{`) on the same line as the statement, with a single space preceding it. For example:
    

```csharp
if (condition)
{
    // Code block
}
```

### **Capitalization Conventions**

Consistent capitalization helps distinguish different elements within code. Follow these capitalization conventions in C#:

* **PascalCase**: Use PascalCase for namespaces and types. For example: `namespace MyNamespace`, `public class MyClass`.
    
* **camelCase**: Use camelCase for members (fields, properties, methods, events) and local variables. For example: `private int myField`, `public void myMethod()`.