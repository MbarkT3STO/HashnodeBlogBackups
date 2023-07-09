---
title: "DDD: Domain Entities in C#"
datePublished: Sun Jul 09 2023 18:52:17 GMT+0000 (Coordinated Universal Time)
cuid: cljvskahm000309mk80hnd0gs
slug: ddd-domain-entities-in-c
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688928060087/41a5437f-e1c9-4ecf-8a03-14ae08f02ced.png
tags: csharp, software-architecture, net, ddd, domain-driven-design

---

## Introduction

In Domain-Driven Design (DDD), the concept of Domain Entities plays a crucial role in representing and encapsulating the business logic of an application. Domain Entities are objects that have an identity and are defined by their attributes and behaviors. In this article, we will explore the fundamentals of Domain Entities in C# and how they contribute to building robust and maintainable software.

## Anatomy of a Domain Entity

A Domain Entity, in its simplest form, is a class that represents a concept within the domain of the application. It encapsulates the state and behavior relevant to that concept. The key characteristics of a Domain Entity are:

* **Identity**: Every Domain Entity must have a unique identifier that distinguishes it from other entities. The identity enables the system to track and reference entities accurately.
    
* **Attributes**: Domain Entities have properties that represent their current state. These attributes capture the information necessary for the entity to fulfill its purpose within the domain.
    
* **Behavior**: Domain Entities encapsulate the behavior or operations that can be performed on them. These operations often involve manipulating the entity's state or interacting with other entities within the domain.
    

## Implementing a Domain Entity in C#

Let's take a concrete example of a `Customer` entity in a hypothetical e-commerce domain. The `Customer` entity may have properties such as `Id`, `Name`, `Email`, and `Address`. Additionally, it may have behaviors like updating the address or placing an order.

To implement the `Customer` entity in C#, we can create a class as follows:

```csharp
public class Customer
{
    public Guid Id { get; private set; }
    public string Name { get; private set; }
    public string Email { get; private set; }
    public string Address { get; private set; }

    public Customer(Guid id, string name, string email, string address)
    {
        Id = id;
        Name = name;
        Email = email;
        Address = address;
    }

    public void UpdateAddress(string newAddress)
    {
        // Validate new address and perform necessary logic
        Address = newAddress;
    }

    public void PlaceOrder(Order order)
    {
        // Logic for placing an order
    }
}
```

In this example, the `Customer` class has properties for `Id`, `Name`, `Email`, and `Address`, which represent the attributes of the entity. The class constructor is responsible for initializing these properties. Additionally, we have defined two behaviors: `UpdateAddress` and `PlaceOrder`, which operate on the `Customer` entity.

## Guidelines for Designing Domain Entities

When designing Domain Entities, it is essential to follow certain guidelines to ensure their effectiveness and maintainability:

### Encapsulation and Invariants

Domain Entities should encapsulate their state and behavior, exposing only the necessary properties and methods. By encapsulating the internals, entities maintain control over their state and enforce invariants (rules that must hold true) to maintain the integrity of the domain model.

### Identity Management

Every Domain Entity must have a unique identity. In C#, it is common to use a unique identifier, such as a `Guid` or a custom identifier. The identity should be immutable once set to ensure consistency.

### Separation of Concerns

Domain Entities should focus solely on representing the core concepts within the domain and should not be concerned with infrastructure or persistence-related details. Separating concerns allows for better maintainability, testability, and reusability of the domain model.

### Aggregates and Aggregate Roots

In DDD, Domain Entities are often organized into aggregates, which are clusters of entities that are treated as a single unit during data modifications. Aggregates have an aggregate root, which acts as the entry

point for accessing and modifying the entities within the aggregate. Careful consideration should be given to aggregate boundaries to ensure consistency and transactional integrity.

## Additional Considerations

While the core concepts of Domain Entities have been covered, there are a few additional considerations to keep in mind when working with Domain Entities in C#:

### Immutability

To enhance the stability and predictability of Domain Entities, consider making them immutable whenever possible. Immutability ensures that once an entity is created, its state cannot be changed. Instead, any modifications to the entity result in the creation of a new instance. Immutable entities promote thread safety, simplify reasoning about state, and enable better integration with functional programming techniques.

### Value Objects

In addition to Domain Entities, DDD introduces the concept of Value Objects. Value Objects are objects that don't have a distinct identity but are defined by their attributes. These objects represent concepts that are more focused on their value rather than their identity. Examples of Value Objects in an e-commerce domain could be `Money`, `Address`, or `Email`. Value Objects are typically implemented as immutable objects, emphasizing their immutability and equality based on attribute values.

### Persistence and Mapping

While Domain Entities should not be concerned with persistence, they often need to be stored and retrieved from a database or other data storage mechanisms. To bridge the gap between the domain model and persistence, an Object-Relational Mapping (ORM) or Data Access Layer can be used. This layer is responsible for mapping the properties and relationships of Domain Entities to the underlying data storage. Various ORM frameworks, such as Entity Framework or NHibernate, provide tools and conventions to simplify this mapping process.

### Testing Domain Entities

Unit testing Domain Entities is crucial to ensure their correctness and behavior within the domain. Test cases should cover the different attributes, behaviors, and edge cases of the entities. Since Domain Entities often encapsulate complex business rules, writing extensive test suites can help uncover any discrepancies or issues with the implementation. Tools like NUnit, xUnit, or MSTest can be used for writing and executing unit tests for Domain Entities.

## Equality and Identity of Domain Entities

One important aspect to consider when working with Domain Entities is equality and identity. Domain Entities often need to be compared for equality, and it's crucial to define what constitutes equality for an entity within the domain.

In C#, equality can be determined by overriding the `Equals` method and implementing the `IEquatable<T>` interface. Let's enhance our `Customer` example to include equality checks based on the `Id` property:

```csharp
public class Customer : IEquatable<Customer>
{
    public Guid Id { get; private set; }
    // ...

    public override bool Equals(object obj)
    {
        return Equals(obj as Customer);
    }

    public bool Equals(Customer other)
    {
        if (other is null)
            return false;

        return Id == other.Id;
    }

    public override int GetHashCode()
    {
        return Id.GetHashCode();
    }

    public static bool operator ==(Customer left, Customer right)
    {
        if (left is null)
            return right is null;

        return left.Equals(right);
    }

    public static bool operator !=(Customer left, Customer right)
    {
        return !(left == right);
    }
}
```

In this updated implementation, we override the `Equals` method to compare the `Id` properties of two `Customer` objects. We also override the `GetHashCode` method to ensure consistency with equality comparisons. Additionally, we define the equality operators `==` and `!=` to allow for more expressive equality checks.

With these equality implementations in place, we can now compare `Customer` objects using standard equality operators:

```csharp
Customer customer1 = new Customer(Guid.NewGuid(), "John Doe", "john@example.com", "123 Main St");
Customer customer2 = new Customer(Guid.NewGuid(), "Jane Smith", "jane@example.com", "456 Elm St");
Customer customer3 = new Customer(customer1.Id, "John Doe", "john@example.com", "789 Oak St");

bool areEqual = customer1 == customer3;  // true
bool areNotEqual = customer1 != customer2;  // true
```

By providing a well-defined notion of equality, we enable the use of Domain Entities in collections, such as dictionaries or sets, and ensure correct behavior when comparing and manipulating entities within the domain.

## Entity Identity and Persistence

Domain Entities often need to be persisted and retrieved from a data store. In such cases, it's essential to establish a consistent and reliable identity management strategy.

The `Id` property in our `Customer` entity serves as the unique identifier, allowing entities to be differentiated even if their attribute values are the same. When storing entities in a database, the `Id` property typically corresponds to the primary key of the corresponding table.

When retrieving entities from a data store, it's important to ensure that the identity is preserved. If two entities have the same `Id`, they should represent the same logical entity within the domain, regardless of any differences in their attribute values. ORM frameworks often handle this identity management automatically, mapping database records to domain entities based on the entity's identity property.