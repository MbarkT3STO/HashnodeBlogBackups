---
title: "DDD: Value Objects"
datePublished: Sun Jul 09 2023 19:29:44 GMT+0000 (Coordinated Universal Time)
cuid: cljvtwg8v00010al6dnf2d310
slug: ddd-value-objects
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688930410355/91e49d15-7bb4-452a-9b60-0b98e4de8aed.png
tags: csharp, software-architecture, net, ddd, domain-driven-design

---

## **What are Value Objects?**

In Domain-Driven Design, Value Objects represent immutable entities that encapsulate a set of attributes or properties. They derive their value solely from their attributes and are considered equal if their attributes match, rather than being identified by an identifier or reference. Value Objects are primarily used to model concepts or characteristics that don't have a distinct identity but play a crucial role in the domain.

Unlike **Entities**, which are distinguished by their unique identifiers, Value Objects are distinguished by their state. This means that if two Value Objects have the same attribute values, they are considered equal, regardless of whether they are the same instance or not.

## **Characteristics of Value Objects**

Value Objects possess a few essential characteristics:

### **Immutability**

Value Objects are immutable, meaning their state cannot be modified after they are created. This immutability ensures that Value Objects maintain their integrity and guarantees that their attributes remain consistent throughout their lifetime.

### **Equality by Value**

Value Objects are compared based on the equality of their attribute values. If two Value Objects have identical attribute values, they are considered equal, irrespective of their references. This allows for simplified comparisons and enables Value Objects to be used effectively in collections and dictionaries.

### **Side Effect-Free Operations**

Value Objects should not perform any side-effecting operations or have behavior that modifies the system or its state. They are purely focused on representing values and performing operations that derive new Value Objects based on their existing state.

## **Implementing Value Objects in C#**

Let's dive into some practical examples to understand how Value Objects are implemented in C#:

```csharp
public class Money : IEquatable<Money>
{
    public decimal Amount { get; }
    public string Currency { get; }

    public Money(decimal amount, string currency)
    {
        Amount = amount;
        Currency = currency;
    }

    public override bool Equals(object obj)
    {
        return Equals(obj as Money);
    }

    public bool Equals(Money other)
    {
        if (other is null)
            return false;

        return Amount == other.Amount && Currency == other.Currency;
    }

    public override int GetHashCode()
    {
        return HashCode.Combine(Amount, Currency);
    }
}
```

In this example, we have a `Money` class representing a monetary value. It has two properties: `Amount`, which holds the numerical value, and `Currency`, which stores the currency code. The class overrides the `Equals` and `GetHashCode` methods to ensure equality by value, comparing the `Amount` and `Currency` attributes.

## **Benefits of Value Objects**

Value Objects offer several advantages when applied correctly:

### **Enhanced Expressiveness**

By explicitly modeling Value Objects, we provide a rich vocabulary that closely aligns with the domain. This improves the readability and maintainability of the codebase, making it easier to understand the system's behavior.

### **Encapsulation of Behavior**

Value Objects encapsulate both data and behavior related to a specific concept within the domain. This encapsulation helps in keeping the codebase organized and promotes the Single Responsibility Principle (SRP).

### **Immutability and Safety**

The immutability of Value Objects guarantees that they cannot be accidentally modified, reducing the risk of introducing bugs or unexpected behavior. It ensures the predictability and safety of the system.