---
title: "DDD: Entity Property Accessors & Methods"
datePublished: Mon Jul 10 2023 10:05:56 GMT+0000 (Coordinated Universal Time)
cuid: cljwp78tf000i09js60cc4z2j
slug: ddd-entity-property-accessors-methods
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688982905888/8d6ea0ef-d7c4-434b-b6e6-491ecb9a7520.webp
tags: csharp, software-architecture, net, ddd, domain-driven-design

---

In Domain-Driven Design (DDD), entities play a crucial role in modeling the core concepts of a domain. Entities encapsulate behavior and data, representing the real-world objects that the application interacts with. When designing entities, it's important to carefully consider how to expose and manipulate their properties. In this blog post, we will explore the best practices for defining entity property accessors and methods in C#.

## **Property Accessors**

Properties in entities provide a way to access and modify their internal state. However, in DDD, it is generally recommended to make the setters of properties private or protected to enforce encapsulation and maintain domain integrity. Instead, we can use public methods to modify the entity's state. Let's take a look at an example:

```csharp
public class Order : Entity
{
    public decimal TotalAmount { get; private set; }
    
    public void AddProduct(Product product, int quantity)
    {
        // Calculate the amount based on product price and quantity
        decimal amount = product.Price * quantity;
        
        // Update the total amount
        TotalAmount += amount;
        
        // Perform other necessary operations
        // ...
    }
}
```

In the above code snippet, the `TotalAmount` property has a private setter, ensuring that it can only be modified from within the `Order` class. The `AddProduct` method is responsible for updating the `TotalAmount` based on the product price and quantity. By encapsulating the logic within the entity, we maintain control over the state and ensure consistency.

## **Domain Methods**

In addition to property accessors, entities often expose domain-specific methods that encapsulate behavior related to the entity's responsibilities. These methods allow us to model complex operations and enforce business rules within the domain. Let's consider an example:

```csharp
public class BankAccount : Entity
{
    public decimal Balance { get; private set; }
    
    public void Deposit(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("The deposit amount must be positive.");
        }
        
        // Update the balance
        Balance += amount;
        
        // Perform other necessary operations
        // ...
    }
    
    public void Withdraw(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("The withdrawal amount must be positive.");
        }
        
        if (amount > Balance)
        {
            throw new InvalidOperationException("Insufficient balance.");
        }
        
        // Update the balance
        Balance -= amount;
        
        // Perform other necessary operations
        // ...
    }
}
```

In this example, the `Deposit` and `Withdraw` methods encapsulate the behavior of depositing and withdrawing money from a bank account. These methods enforce business rules such as validating the deposit or withdrawal amount and ensuring sufficient balance. By providing these domain-specific methods, we encapsulate the logic within the entity itself and prevent invalid state transitions.