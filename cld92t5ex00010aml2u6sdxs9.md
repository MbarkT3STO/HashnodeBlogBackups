# Auto-Generating GUID Properties in EF Core

## **Introduction**

GUID, or globally unique identifier, is a 128-bit value that is used to identify resources in a unique and consistent way. In Entity Framework Core (EF Core), a GUID can be used as the primary key for a table, and it can be automatically generated when a new row is inserted into the table. This can be achieved by using the `HasDefaultValueSql` method on the `Property` configuration.

## **Setting up a GUID property in EF Core**

To set up a GUID property in EF Core, you need to first create a model class that represents the table in your database. For example, let's say we have a `Product` table in our database, and we want the `Id` property to be a GUID. Our model class would look like this:

```csharp
public class Product
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

Next, we need to configure the `Id` property in our `DbContext` class. Here's an example of how to do that:

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>()
            .Property(b => b.Id)
            .HasDefaultValueSql("NEWID()");
    }
}
```

In the above example, we are using the `Property` method to specify the `Id` property of the `Product` entity, and then we are calling the `HasDefaultValueSql` method with the value of "NEWID()". This tells EF Core to use the `NEWID()` function in SQL Server to generate a new GUID when a new row is inserted into the table.

## **Using the GUID property in code**

Once you have set up the GUID property in EF Core, you can use it in your code just like any other property. Here's an example of how to insert a new product into the `Product` table:

```csharp
using (var context = new MyDbContext())
{
    var product = new Product
    {
        Name = "Product 1",
        Price = 19.99m
    };
    context.Products.Add(product);
    context.SaveChanges();
}
```

In this example, the `Id` property of the `product` object will be automatically set to a new GUID when the `SaveChanges` method is called.

## **Additional Considerations**

It's worth noting that the use of the `HasDefaultValueSql` method will only work if your database provider supports the `NEWID()` function. For example, if you are using a MySQL database, you will need to use the `UUID()` function instead.

## **Conclusion**

Using the `HasDefaultValueSql` method on the `Property` configuration in EF Core, it is possible to automatically generate a GUID when a new row is inserted into a table. This can be a useful feature for creating unique and consistent identifiers for resources in your application.