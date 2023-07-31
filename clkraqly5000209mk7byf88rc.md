---
title: "EF Core and PostgreSQL: Check Constraints"
datePublished: Mon Jul 31 2023 20:01:57 GMT+0000 (Coordinated Universal Time)
cuid: clkraqly5000209mk7byf88rc
slug: ef-core-and-postgresql-check-constraints
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690833276972/d5b52d6a-4d20-4531-908a-13cdb4f74cbe.png
tags: entity-framework, postgresql, sql, net, efcore

---

Check constraints are an essential aspect of maintaining data integrity in a PostgreSQL database. They allow you to define rules that data in a specific column must adhere to, ensuring that only valid and acceptable data can be inserted or updated. In this blog post, we will explore how to add check constraints to a PostgreSQL database using Entity Framework (EF) Core.

To understand what Check constraint is take a look here: [PostgreSQL: Constraints](https://mbarkt3sto.hashnode.dev/postgresql-constraints#heading-5-check-constraint)

## Prerequisites

Before we proceed, ensure that you have the following installed:

1. PostgreSQL database server
    
2. .NET Core SDK (version compatible with EF Core)
    
3. Entity Framework Core package
    

## Setting up the Project

To get started, create a new .NET Core project or use an existing one that already has EF Core configured. If you haven't set up EF Core yet, you can do so by running the following command in your project directory:

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

Next, install the PostgreSQL provider for EF Core:

```bash
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

Now, let's create a simple model for demonstration purposes. For this blog post, we'll use a hypothetical `Product` class with a `Price` property that we want to constrain to be greater than zero.

```csharp
// Product.cs
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

## Adding Check Constraints

To add a check constraint using EF Core, we'll leverage the `HasCheckConstraint` method during the model configuration.

```csharp
// YourDbContext.cs
using Microsoft.EntityFrameworkCore;

public class YourDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // Configure your PostgreSQL connection string here
        optionsBuilder.UseNpgsql("Your_PostgreSQL_Connection_String");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>(entity =>
        {
            entity.Property(p => p.Price)
                .HasCheckConstraint("CK_Product_Price", "Price > 0");
        });
    }
}
```

In the above code snippet, we use the `HasCheckConstraint` method to add a check constraint named `CK_Product_Price` to the `Price` property of the `Product` entity. The constraint ensures that the `Price` is always greater than zero.

## Applying Migrations

After adding the check constraint, create a new migration:

```bash
dotnet ef migrations add AddCheckConstraintToProduct
```

This command generates a migration file with the necessary code to apply the check constraint to the database.

Finally, apply the migration to update the database:

```bash
dotnet ef database update
```

## Testing the Check Constraint

Now that the check constraint is in place, let's test it with some sample data:

```csharp
using (var context = new YourDbContext())
{
    var product1 = new Product { Name = "Product A", Price = 10.99m };
    var product2 = new Product { Name = "Product B", Price = -5.00m }; // Invalid data

    context.Products.Add(product1);
    context.Products.Add(product2);

    try
    {
        context.SaveChanges();
    }
    catch (DbUpdateException ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
    }
}
```

When you run the above code, you'll notice that an exception is thrown for `product2`, as the check constraint prevents negative prices.

## Dealing with Camel Case Issues/Errors

If you encounter errors related to camel case issues when adding check constraints using EF Core with PostgreSQL, fear not! The problem likely stems from differences in naming conventions between C# and PostgreSQL. By default, EF Core uses camel case for property names (e.g., `Price` becomes `price`), while PostgreSQL typically prefers snake case (e.g., `Price` becomes `price`). Fortunately, we can easily address this discrepancy.

### Option 1: Change Property Name Convention

One way to handle this is by changing the property name convention to snake case globally for all entities in the EF Core configuration. We can do this in the `OnModelCreating` method of our `YourDbContext` class.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .ForNpgsqlUseLowerCaseNamingConvention()
        .Entity<Product>(entity =>
        {
            entity.Property(p => p.Price)
                .HasCheckConstraint("ck_product_price", "price > 0");
        });
}
```

With the addition of `.ForNpgsqlUseLowerCaseNamingConvention()` in the `OnModelCreating` method, EF Core will now use snake case for all entity properties, which aligns with PostgreSQL's naming convention.

### Option 2: Use Data Annotations

An alternative approach is to use data annotations to explicitly define the column name for the property with the check constraint.

```csharp
using System.ComponentModel.DataAnnotations.Schema;

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }

    [Column("price")]
    public decimal Price { get; set; }
}
```

By adding the `[Column("price")]` attribute to the `Price` property, we specify that this property corresponds to the `price` column in the database. This ensures the correct column name is used when applying the check constraint.

### Option 3: Manual Check Constraint SQL

If you prefer to retain camel case naming conventions and avoid changing property names, you can manually execute SQL commands to add the check constraint. This way, you have complete control over the SQL statement.

```csharp
public class YourDbContext : DbContext
{
    // ...

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // ...

        // Ensure the constraint name is in lowercase to adhere to PostgreSQL conventions
        string checkConstraintSql = "ALTER TABLE \"Products\" ADD CONSTRAINT \"ck_product_price\" CHECK (\"Price\" > 0);";

        modelBuilder.Entity<Product>.HasCheckConstraint("ck_product_price", checkConstraintSql);
    }
}
```

By using `modelBuilder.HasCheckConstraint`, you can execute custom SQL commands for adding check constraints without affecting the property names.

### Option 4: Using `HasCheckConstraint` with Quoted Property Name

Another way to deal with camel case issues when adding check constraints in PostgreSQL using EF Core is to use the `HasCheckConstraint` method with the property name enclosed in double quotes (`""`). This will ensure that the exact property name is used, and no name conversion will take place.

```csharp
// YourDbContext.cs
using Microsoft.EntityFrameworkCore;

public class YourDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // Configure your PostgreSQL connection string here
        optionsBuilder.UseNpgsql("Your_PostgreSQL_Connection_String");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>(entity =>
        {
            entity.Property(p => p.Price)
                .HasCheckConstraint("CK_Product_Price", "\"Price\" > 0");
        });
    }
}
```

In the code above, we pass the property name `"Price"` enclosed in double quotes as the second argument to `HasCheckConstraint`. This approach ensures that EF Core uses the exact property name without any naming convention modifications.

With this option, you can retain your preferred C# property naming conventions and directly specify the property names as they are in your model when adding check constraints.