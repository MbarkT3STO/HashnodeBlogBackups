---
title: "DDD: Specifications"
datePublished: Mon Jul 10 2023 14:38:26 GMT+0000 (Coordinated Universal Time)
cuid: cljwyxoom000x0ajvan8u6u74
slug: ddd-specifications
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688999463525/c2701e84-fe31-4c85-974c-d88abb50f180.webp
tags: csharp, software-architecture, net, ddd, domain-driven-design

---

In Domain-Driven Design (DDD), specifications play a crucial role in expressing complex business rules and conditions. Specifications encapsulate these rules into reusable components, making it easier to reason about domain logic. In this blog post, we will explore how to create specifications. Additionally, we will learn how to convert a specification into an expression for more dynamic querying.

## What is a Specification?

In DDD, a specification represents a condition or rule that objects in a domain must satisfy. It acts as a declarative language for expressing complex criteria. Specifications can be used for various purposes, such as filtering collections, validating entities, or defining query predicates.

## Creating a Specification Interface

One way to define specifications in C# is by using an interface. Let's create an example specification interface called `ISpecification<T>`:

```csharp
public interface ISpecification<T>
{
    bool IsSatisfiedBy(T item);
}
```

The `ISpecification<T>` interface contains a single method `IsSatisfiedBy`, which takes an object of type `T` and returns a boolean indicating whether the specification is satisfied by the given item.

## Implementing a Specification

To implement a specification, you can create a concrete class that implements the `ISpecification<T>` interface. Let's say we have a domain of `Product` entities and we want to create a specification to filter products that are in stock and have a price less than $100:

```csharp
public class InStockAndLessThan100DollarsSpecification : ISpecification<Product>
{
    public bool IsSatisfiedBy(Product product)
    {
        return product.IsInStock && product.Price < 100;
    }
}
```

In the above example, the `InStockAndLessThan100DollarsSpecification` class implements the `ISpecification<Product>` interface. It defines the `IsSatisfiedBy` method to check if a product is in stock (`product.IsInStock`) and has a price less than $100 (`product.Price < 100`).

## Combining Specifications with Logical Operators

Specifications can be combined using logical operators like AND, OR, and NOT to express more complex conditions. Let's create another specification to filter products that are both in stock and have a price less than $100, or their name contains the word "sale":

```csharp
public class ComplexProductSpecification : ISpecification<Product>
{
    private readonly ISpecification<Product> _inStockAndLessThan100Spec;
    private readonly ISpecification<Product> _nameContainsSaleSpec;

    public ComplexProductSpecification()
    {
        _inStockAndLessThan100Spec = new InStockAndLessThan100DollarsSpecification();
        _nameContainsSaleSpec = new NameContainsSaleSpecification();
    }

    public bool IsSatisfiedBy(Product product)
    {
        return _inStockAndLessThan100Spec.IsSatisfiedBy(product) || _nameContainsSaleSpec.IsSatisfiedBy(product);
    }
}
```

In this example, the `ComplexProductSpecification` class combines the `InStockAndLessThan100DollarsSpecification` and `NameContainsSaleSpecification` using the OR operator (`||`). It checks if a product satisfies either of the specifications.

## Converting a Specification to an Expression

Sometimes it's useful to convert a specification into an expression for dynamic querying or integration with an ORM (Object-Relational Mapping) framework. Let's modify the `InStockAndLessThan100DollarsSpecification` to include an expression property:

```csharp
public class InStockAndLessThan100DollarsSpecification : ISpecification<Product>
{
    public bool IsSatisfiedBy(Product product)
    {
        return product.IsInStock && product.Price < 100;
    }

    public Expression<Func<Product, bool>> ToExpression()
    {
        return product => product.IsInStock && product.Price < 100;
    }
}
```

In the updated `InStockAndLessThan100DollarsSpecification`, we added a new method called `ToExpression` that returns an `Expression<Func<Product, bool>>`. This expression can be used for querying a collection of `Product` entities dynamically.