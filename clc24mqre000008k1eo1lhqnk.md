# How to Implement Soft Update and Delete with EF Core?

Entity Framework (EF) Core is a popular object-relational mapper (ORM) for .NET applications that simplifies data access and allows developers to work with data in the form of domain-specific objects. In this article, we will look at how to implement soft delete and update tracking in EF Core using `IsDeleted`, `DeletedOn`, and `DeletedBy` columns for soft delete and `IsUpdated`, `LastUpdateOn`, and `LastUpdateBy` columns for update tracking.

## **Prerequisites**

* .NET Core SDK 3.1 or later
    
* Visual Studio or Visual Studio Code
    
* Familiarity with C# and EF Core
    

## **Setting up the Project**

First, let's create a new .NET Core console application using the following command:

```bash
dotnet new console -o SoftDeleteAndUpdateTracking
```

Next, navigate to the project directory and install the following NuGet packages:

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

The `Microsoft.EntityFrameworkCore` package provides the core EF Core functionality, while the `Microsoft.EntityFrameworkCore.SqlServer` package adds support for using Microsoft SQL Server as the database.ord, you can set the timestamp to the current time instead of actually deleting or updating the record in the database.

## **Defining the Entity and DbContext**

Next, we need to define our entity and DbContext classes. Let's start by creating a `Product` entity with `IsDeleted`, `DeletedOn`, and `DeletedBy` columns for soft delete and `IsUpdated`, `LastUpdateOn`, and `LastUpdateBy` columns for update tracking.

```csharp
using System;

namespace SoftDeleteAndUpdateTracking
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public bool IsDeleted { get; set; }
        public DateTime? DeletedOn { get; set; }
        public string DeletedBy { get; set; }
        public bool IsUpdated { get; set; }
        public DateTime? LastUpdateOn { get; set; }
        public string LastUpdateBy { get; set; }
    }
}
```

Next, we will define our DbContext class. In the `OnModelCreating` method, we will configure the `Product` entity to use the `IsDeleted` column for soft delete and the `IsUpdated`, `LastUpdateOn`, and `LastUpdateBy` columns for update tracking.

```csharp
using Microsoft.EntityFrameworkCore;

namespace SoftDeleteAndUpdateTracking
{
    public class AppDbContext : DbContext
    {
        public DbSet<Product> Products { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(
                "Server=localhost;Database=SoftDeleteAndUpdateTracking;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Product>()
                .HasQueryFilter(p => !p.IsDeleted);

            modelBuilder.Entity<Product>()
                .Property(p => p.IsUpdated)
                .HasDefaultValue(false);

            modelBuilder.Entity<Product>()
                .Property(p => p.LastUpdateOn)
                .HasDefaultValue(null);

            modelBuilder.Entity<Product>()
                .Property(p => p.LastUpdateBy)
                .HasDefaultValue(null);
        }
    }
}
```

With the entity and DbContext defined, we can now use EF Core to create the database and apply the migration.

## **Creating the Database and Applying the Migration**

To create the database and apply the migration, we need to run the following commands:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

The `dotnet ef migrations add` command will create a migration file containing the necessary SQL commands to create the `Product` table in the database. The `dotnet ef database update` command will apply the migration to the database.

## **Implementing Soft Delete**

Now that we have set up the project and created the database, we can implement soft delete in our DbContext. To do this, we will add a `SoftDelete` extension method to the DbContext class.

```csharp
using System;
using System.Linq;
using Microsoft.EntityFrameworkCore;

namespace SoftDeleteAndUpdateTracking
{
    public static class DbContextExtensions
    {
        public static void SoftDelete<T>(this DbContext dbContext, T entity, string deletedBy)
            where T : class
        {
            var entry = dbContext.Entry(entity);
            if (entry.State == EntityState.Detached)
            {
                dbContext.Set<T>().Attach(entity);
                entry = dbContext.Entry(entity);
            }

            entry.Property("IsDeleted").CurrentValue = true;
            entry.Property("DeletedOn").CurrentValue = DateTime.UtcNow;
            entry.Property("DeletedBy").CurrentValue = deletedBy;
            entry.State = EntityState.Modified;
        }
    }
}
```

The `SoftDelete` method accepts an entity and a `deletedBy` parameter as inputs and sets the `IsDeleted`, `DeletedOn`, and `DeletedBy` properties of the entity to `true`, the current UTC time, and the `deletedBy` parameter, respectively. It then sets the state of the entity to `Modified` so that the changes are persisted to the database when the DbContext is saved.

## **Implementing Update Tracking**

To implement update tracking, we will add an `UpdateTracking` extension method to the DbContext class. This method will be similar to the `SoftDelete` method, but it will set the `IsUpdated`, `LastUpdateOn`, and `LastUpdateBy` properties instead.

```csharp
using System;
using System.Linq;
using Microsoft.EntityFrameworkCore;

namespace SoftDeleteAndUpdateTracking
{
    public static class DbContextExtensions
    {
        public static void UpdateTracking<T>(this DbContext dbContext, T entity, string updatedBy)
            where T : class
        {
            var entry = dbContext.Entry(entity);
            if (entry.State == EntityState.Detached)
            {
                dbContext.Set<T>().Attach(entity);
                entry = dbContext.Entry(entity);
            }

            entry.Property("IsUpdated").CurrentValue = true;
            entry.Property("LastUpdateOn").CurrentValue = DateTime.UtcNow;
            entry.Property("LastUpdateBy").CurrentValue = updatedBy;
            entry.State = EntityState.Modified;
        }
    }
}
```

## **Using the Soft Delete and Update Tracking Extension Methods**

Now that we have implemented the soft delete and update tracking extension methods, we can use them in our application.

To demonstrate how to use these methods, let's add a `Main` method to our console application that creates a new product, soft deletes it, and then updates it.

```csharp
using System;
using Microsoft.EntityFrameworkCore;

namespace SoftDeleteAndUpdateTracking
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var dbContext = new AppDbContext())
            {
                // Create a new product
                var product = new Product
                {
                    Name = "Product 1",
                    Price = 10.99m
                };
                dbContext.Products.Add(product);
                dbContext.SaveChanges();

                // Soft delete the product
                dbContext.SoftDelete(product, "user1");
                dbContext.SaveChanges();

                // Update the product
                product.Name = "Product 1 (Updated)";
                product.Price = 12.99m;
                dbContext.UpdateTracking(product, "user1");
                dbContext.SaveChanges();
            }
        }
    }
}
```

In the above example, we create a new product, soft delete it, and then update it. The `SoftDelete` and `UpdateTracking` extension methods are used to set the `IsDeleted`, `DeletedOn`, `DeletedBy`, `IsUpdated`, `LastUpdateOn`, and `LastUpdateBy` properties of the product entity accordingly.

## Note

It's important to note that the implementation of soft delete and update tracking presented in this article is just one way to implement these features in EF Core. There are many other ways to achieve the same result, such as using a `Deleted` base class that implements the `IsDeleted`, `DeletedOn`, and `DeletedBy` columns, or using triggers in the database to automatically set the values of these columns.

Regardless of the approach you choose, it's important to consider the trade-offs and choose the approach that best fits the needs of your application.

## Using Interface and Extension Methods

To make it easier to implement soft update and delete using interfaces in EF Core, you can define extension methods similar to the ones described in previous sections.

Here's an example of how you can implement soft update and delete using interfaces and extension methods in EF Core:

```csharp
public interface ISoftDelete
{
    bool IsDeleted { get; set; }
    DateTime? DeletedOn { get; set; }
    string DeletedBy { get; set; }
}

public interface ISoftUpdate
{
    bool IsUpdated { get; set; }
    DateTime? LastUpdateOn { get; set; }
    string LastUpdateBy { get; set; }
}

public class Product : ISoftDelete, ISoftUpdate
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public bool IsDeleted { get; set; }
    public DateTime? DeletedOn { get; set; }
    public string DeletedBy { get; set; }
    public bool IsUpdated { get; set; }
    public DateTime? LastUpdateOn { get; set; }
    public string LastUpdateBy { get; set; }
}

public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>()
            .HasQueryFilter(p => !((ISoftDelete)p).IsDeleted);
    }
}

public static class DbContextExtensions
{
    public static void SoftDelete<T>(this DbContext dbContext, T entity, string deletedBy)
        where T : class, ISoftDelete
    {
        var entry = dbContext.Entry(entity);
        if (entry.State == EntityState.Detached)
        {
            dbContext.Set<T>().Attach(entity);
            entry = dbContext.Entry(entity);
        }

        entry.Property("IsDeleted").CurrentValue = true;
        entry.Property("DeletedOn").CurrentValue = DateTime.UtcNow;
        entry.Property("DeletedBy").CurrentValue = deletedBy;
        entry.State = EntityState.Modified;
    }

    public static void UpdateTracking<T>(this DbContext dbContext, T entity, string updatedBy)
        where T : class, ISoftUpdate
    {
        var entry = dbContext.Entry(entity);
        if (entry.State == EntityState.Detached)
        {
            dbContext.Set<T>().Attach(entity);
            entry = dbContext.Entry(entity);
        }

        entry.Property("IsUpdated").CurrentValue = true;
        entry.Property("LastUpdateOn").CurrentValue = DateTime.UtcNow;
        entry.Property("LastUpdateBy").CurrentValue = updatedBy;
        entry.State = EntityState.Modified;
    }
}
```

In the above example, we have defined the `ISoftDelete` and `ISoftUpdate` interfaces, and the `Product` entity implements both interfaces. We have also added a global query filter to the `Product` entity in the `OnModelCreating` method of the DbContext to exclude deleted entities from queries.

We have also defined the `SoftDelete` and `UpdateTracking` extension methods, which accept an entity and a user identifier as inputs and set the `IsDeleted`, `DeletedOn`, and `DeletedBy` properties for soft delete, or the `IsUpdated`, `LastUpdateOn`, and `LastUpdateBy` properties for update tracking. These methods are generic and can be used with any entity that implements the appropriate interface.

To use the extension methods, you can simply call them on your DbContext and pass in the entity and user identifier as arguments:

```csharp
using (var dbContext = new AppDbContext())
{
    // Soft delete the first product
    var product = dbContext.Products.First();
    dbContext.SoftDelete(product, "user1");
    dbContext.SaveChanges();

    // Update the second product
    product = dbContext.Products.Skip(1).First();
    product.Name = "Product 2 (Updated)";
    product.Price = 14.99m;
    dbContext.UpdateTracking(product, "user1");
    dbContext.SaveChanges();
}
```

This approach has the advantage of being simple and flexible, as it allows you to mark any entity as "deletable" or "updatable" by implementing the appropriate interface, and provides a convenient way to set the values of the soft delete and update tracking columns using extension methods. However, it requires you to add the `IsDeleted`, `DeletedOn`, `DeletedBy`, `IsUpdated`, `LastUpdateOn`, and `LastUpdateBy` properties to each entity that you want to be deletable or updatable, which may not be practical in all cases.

## **Finally**

In this article, we learned how to implement soft delete and update tracking in EF Core using `IsDeleted`, `DeletedOn`, and `DeletedBy` columns for soft delete and `IsUpdated`, `LastUpdateOn`, and `LastUpdateBy` columns for update tracking. We also saw how to use the `SoftDelete` and `UpdateTracking` extension methods to set the values of these columns in our DbContext. With these techniques, you can easily track the deletion and update history of your entities in your EF Core application.