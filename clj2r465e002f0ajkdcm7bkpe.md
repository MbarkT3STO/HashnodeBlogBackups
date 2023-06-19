---
title: "C#: The ExpandoObject"
datePublished: Mon Jun 19 2023 11:06:26 GMT+0000 (Coordinated Universal Time)
cuid: clj2r465e002f0ajkdcm7bkpe
slug: c-the-expandoobject
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1687172143339/78bc35e4-a65d-459d-a1f5-69659d54b817.webp
tags: csharp, net

---

In C#, the ExpandoObject is a dynamic object that allows developers to add and remove members dynamically at runtime. It provides a flexible way to work with objects by extending their capabilities beyond their defined structure. This article explores the ExpandoObject in C# and provides examples to showcase its power and versatility.

## What is the ExpandoObject?

The ExpandoObject is a part of the `System.Dynamic` namespace in C#. It is a dynamic object that implements the `IDictionary<string, object>` interface, which means it behaves like a dictionary with string keys and object values. This allows us to dynamically add properties and methods to the ExpandoObject without the need for defining them explicitly.

## Creating an ExpandoObject

Creating an instance of the ExpandoObject is straightforward. You can utilize the `dynamic` keyword to declare and initialize a new ExpandoObject:

```csharp
dynamic expando = new ExpandoObject();
```

## Adding Properties

The beauty of the ExpandoObject lies in its ability to add properties on the fly. You can assign values to properties that do not exist beforehand:

```csharp
expando.Name = "John Doe";
expando.Age = 25;
```

In the above example, we added the `Name` and `Age` properties to the ExpandoObject dynamically.

## Retrieving Property Values

To retrieve the values of the dynamically added properties, you can use the dot notation:

```csharp
string name = expando.Name;
int age = expando.Age;
```

## Adding Methods

In addition to properties, you can also add methods to the ExpandoObject dynamically. This allows you to extend the functionality of the object at runtime. Consider the following example:

```csharp
expando.SayHello = new Action(() =>
{
    Console.WriteLine($"Hello, {expando.Name}!");
});
```

Here, we added a `SayHello` method to the ExpandoObject, which prints a greeting message using the `Name` property.

## Invoking Methods

Once you have added a method to the ExpandoObject, you can invoke it as you would with any regular method:

```csharp
expando.SayHello();
```

Executing the above line of code will call the `SayHello` method and print the greeting message to the console.

## Dynamic Nature

The ExpandoObject's dynamic nature enables you to modify its structure at runtime, which can be advantageous in various scenarios. It is particularly useful when dealing with dynamic data or when you need to add properties and methods dynamically based on certain conditions or user input.

```csharp
if (userInput == "AddProperty")
{
    Console.Write("Enter the property name: ");
    string propertyName = Console.ReadLine();
    Console.Write("Enter the property value: ");
    string propertyValue = Console.ReadLine();

    ((IDictionary<string, object>) expando)[propertyName] = propertyValue;
}
```

In the code snippet above, we prompt the user to enter the name and value of a property they want to add to the ExpandoObject. The entered property is then added dynamically, allowing for a customizable object structure.

## Real-World Examples

### Dynamic data mapping

One real-world example where the ExpandoObject can be useful is in scenarios involving dynamic data mapping or object projection. Let's consider a scenario where you have an application that receives data from various sources, each with its own data structure and format.

In such cases, using the ExpandoObject allows you to handle the data dynamically without the need to define rigid class structures for each data source. You can parse the incoming data, create an ExpandoObject, and dynamically add properties based on the received data fields. This provides a flexible way to work with diverse data structures without the need for extensive upfront modeling.

For instance, imagine you're building a data integration system that receives JSON payloads from different APIs. Each API may have a different schema and set of fields. By using the ExpandoObject, you can dynamically map the incoming JSON data to the ExpandoObject properties. This allows you to process and manipulate the data, perform transformations, or store it in a different format, all without the need for predefined classes matching every possible schema.

Here's a simplified example to illustrate the idea:

```csharp
dynamic expando = new ExpandoObject();
string jsonData = GetDataFromAPI();

// Deserialize JSON and map it to the ExpandoObject
var data = JsonConvert.DeserializeObject<Dictionary<string, object>>(jsonData);
foreach (var kvp in data)
{
    ((IDictionary<string, object>) expando)[kvp.Key] = kvp.Value;
}

// Access the dynamically added properties
string name = expando.Name;
int age = expando.Age;
```

In the above scenario, the ExpandoObject is used to map the JSON data dynamically, allowing you to access the properties like `Name` and `Age` without needing a predefined class or knowing the structure of the incoming data in advance. This flexibility is particularly valuable when dealing with external data sources where the schema can change over time or vary across different sources.

### User-defined or customizable objects

Another real-world example where the ExpandoObject can be beneficial is in scenarios involving user-defined or customizable objects.

Consider a scenario where you are developing a content management system (CMS) that allows users to create custom data structures or define their own object schemas. In this case, the ExpandoObject can be used to dynamically create and manipulate objects based on user specifications.

Here's an example:

```csharp
dynamic userObject = new ExpandoObject();
Console.WriteLine("Enter property names and values. Enter 'done' to finish.");

while (true)
{
    Console.Write("Property name: ");
    string propertyName = Console.ReadLine();

    if (propertyName == "done")
        break;

    Console.Write("Property value: ");
    string propertyValue = Console.ReadLine();

    ((IDictionary<string, object>) userObject)[propertyName] = propertyValue;
}

Console.WriteLine("User object properties:");
foreach (var property in userObject)
{
    Console.WriteLine($"{property.Key}: {property.Value}");
}
```

In this scenario, the ExpandoObject allows users to define their own object structure by adding properties and values dynamically at runtime. The application prompts the user to enter property names and values until they indicate they are done. The properties and values are then stored in the ExpandoObject and can be accessed and manipulated as needed.

This approach allows users to create and manage their own data structures within the CMS without requiring predefined classes or fixed schemas. The ExpandoObject enables the application to accommodate a wide range of user-defined object structures, making it suitable for flexible and customizable systems.

Whether you are building a CMS, a form builder, or any application that requires user-defined object structures, the ExpandoObject provides a powerful tool to handle dynamic properties and allow users to define their own objects without strict predefined schemas.

## **Limitations and Considerations**

While the ExpandoObject offers great flexibility, it's important to be aware of its limitations and consider appropriate use cases:

### **Performance Overhead**

Dynamic objects like the ExpandoObject involve additional runtime checks, which can introduce some performance overhead compared to statically typed objects. If you're working with large datasets or performance-critical scenarios, it's advisable to evaluate the impact and consider alternative approaches.

### **Lack of Compile-Time Safety**

Since the ExpandoObject allows for dynamic member additions, it bypasses the compile-time checks provided by the C# compiler. Typos or incorrect property names won't be caught until runtime, which can lead to potential bugs. It's essential to ensure proper testing and validation when working with dynamically added members.

### **Intellisense and IDE Support**

One downside of dynamic objects is the lack of Intellisense support in most integrated development environments (IDEs). Without compile-time information, the IDE may not provide accurate suggestions or help with member discovery. This can slow down development and make it harder to navigate the codebase.

### **Serialization and Compatibility**

When it comes to serializing ExpandoObjects, certain serialization frameworks may not handle them as expected. It's crucial to check the compatibility of your chosen serialization mechanism with dynamic objects and consider alternative serialization strategies if needed.

### **Use Cases**

While the ExpandoObject offers flexibility, it's important to carefully consider whether it's the most appropriate tool for a given situation. If you have a known structure or fixed requirements, using static typing and predefined classes/interfaces may provide better code maintainability and readability.

## Conclusion

The ExpandoObject in C# is a powerful tool that enables developers to work with dynamic objects. It allows for adding and removing properties and methods dynamically at runtime, providing flexibility and adaptability. Whether you are working with dynamic data or need to extend an object's functionality on the fly, the ExpandoObject is a valuable feature in your C# toolkit.