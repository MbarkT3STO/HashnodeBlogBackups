---
title: "EF Core: HasConversion - Simplifying Data Type Mapping"
datePublished: Thu Jul 06 2023 14:38:17 GMT+0000 (Coordinated Universal Time)
cuid: cljr962vf000609mm7memfayh
slug: ef-core-hasconversion-simplifying-data-type-mapping
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688653597521/6d9a28bd-2fc9-4e01-9f59-c82dd5a6f129.webp
tags: entity-framework, csharp, net, efcore

---

Entity Framework (EF) Core, provides a powerful feature called `HasConversion` that simplifies this process.

## **Introduction to HasConversion**

The `HasConversion` method in EF Core allows developers to define a conversion between a property's data type in the application and its representation in the database. This feature comes in handy when the data type used in the application doesn't directly match the column type in the database, or when custom conversions are required.

By utilizing `HasConversion`, developers can seamlessly work with different data types without the need for manual data transformation. EF Core takes care of converting the data between the application and the database, making the development process smoother and more efficient.

## **Basic Usage**

Let's explore a basic example to understand how `HasConversion` works. Suppose we have an application that deals with coordinates represented by a custom `Coordinate` class. However, we want to store these coordinates as a string in the database.

First, we define the `Coordinate` class:

```csharp
public class Coordinate
{
    public double Latitude { get; set; }
    public double Longitude { get; set; }
}
```

To specify the conversion, we can use `HasConversion` in our EF Core entity configuration:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<MyEntity>()
        .Property(e => e.Coordinate)
        .HasConversion(
            v => $"{v.Latitude},{v.Longitude}",
            v => new Coordinate
            {
                Latitude = double.Parse(v.Split(',')[0]),
                Longitude = double.Parse(v.Split(',')[1])
            }
        );
}
```

In the above example, the `HasConversion` method takes two lambda expressions. The first expression defines how to convert a `Coordinate` object to a string for storage in the database, while the second expression converts the string back to a `Coordinate` object when retrieved from the database.

## **Customizing Conversion Logic**

Besides the basic scenarios, `HasConversion` provides flexibility for custom conversion logic as well. You can define your own conversion functions using lambda expressions, allowing for complex conversions between application types and database column types.

For example, let's say we have a `Money` class that represents a monetary value with a custom precision. We want to store it as a decimal in the database, while still maintaining the custom precision.

```csharp
public class Money
{
    public decimal Value { get; set; }
    public int Precision { get; set; }
}
```

To configure the conversion for the `Money` class, we can utilize `HasConversion` with custom conversion logic:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<FinancialTransaction>()
        .Property(t => t.Amount)
        .HasConversion(
            v => v.Value / (decimal)Math.Pow(10, v.Precision),
            v => new Money
            {
                Value = v * (decimal)Math.Pow(10, v.Precision),
                Precision = 2
            }
        );
}
```

In the above example, we divide the `Value` property of the `Money` object by 10 raised to the power of the `Precision` property to store it as a decimal in the database. When retrieving the value, we multiply it by the same factor to restore the original precision.