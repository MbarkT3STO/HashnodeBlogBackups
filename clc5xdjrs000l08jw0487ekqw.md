# Global Query Filters in EF Core

One useful feature of EF Core is the ability to define global query filters. Global query filters allow you to specify filters that are automatically applied to all queries for a given entity type. This can be useful in a variety of scenarios, such as when you want to enforce certain data constraints or implement soft delete functionality in your application.

In this article, we'll take a look at how to use global query filters in EF Core. We'll start by discussing when and why you might want to use global query filters, and then we'll walk through some examples of how to use them in your code.

## **When to Use Global Query Filters**

There are a few key scenarios where global query filters can be useful:

* **Enforcing data constraints**: Global query filters can be used to enforce certain data constraints, such as only allowing queries to return records that meet certain criteria. For example, you might want to only allow queries to return records that are marked as "active" in your database.
    
* **Implementing soft delete functionality**: Global query filters can be used to implement soft delete functionality, which allows you to mark records as deleted rather than physically deleting them from the database. This can be useful if you want to keep a record of deleted records for auditing or other purposes.
    
* **Hiding sensitive data**: Global query filters can be used to hide sensitive data from certain users or user groups. For example, you might want to only allow administrators to see certain types of sensitive data.
    

## **How to Use Global Query Filters**

To use global query filters in EF Core, you'll need to do the following:

1. Add the `[QueryFilter]` attribute to the entity class that you want to apply the filter to.
    
2. Define a method that returns a boolean expression that represents the filter.
    
3. In your `DbContext` class, call the `AddQueryFilter` method and pass in the entity type and the filter method.
    

Here's an example of how you might use global query filters in your code:

```csharp
public class MyDbContext : DbContext
{
    public DbSet<MyEntity> MyEntities { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<MyEntity>().HasQueryFilter(MyEntityFilter);
    }

    private static bool MyEntityFilter(MyEntity entity)
    {
        return entity.IsActive;
    }
}
```

In this example, we've added the `[QueryFilter]` attribute to the `MyEntity` class, and defined a method called `MyEntityFilter` that returns a boolean expression representing the filter. We've then called the `AddQueryFilter` method in the `OnModelCreating` method of our `DbContext` class, passing in the `MyEntity` type and the `MyEntityFilter` method.

Now, every time a query is executed for the `MyEntity` type, the `MyEntityFilter` method will be applied to the query to only return records where the `IsActive` property is `true`.

You can also use global query filters in combination with other filters, such as those defined using LINQ expressions or raw SQL. For example, you might want to apply a global query filter that only returns records where the `IsActive` property is `true`, and then add an additional filter to only return records where the `Name` property contains a specific string. Here's how you might do that:

```csharp
var myEntities = dbContext.MyEntities
    .Where(e => e.Name.Contains("Foo"))
    .ToList();
```

In this case, the global query filter will be applied first, and then the additional `Where` clause will be applied to filter the results further.

It's also worth noting that global query filters can be disabled for a specific query if needed. To do this, you can use the `IgnoreQueryFilters` method on the `DbSet` object. For example:

```csharp
var myEntities = dbContext.MyEntities
    .IgnoreQueryFilters()
    .ToList();
```

This will disable the global query filter for the specific query, allowing you to retrieve all records from the `MyEntities` table.

## Real-World Examples

### Soft-Delete

Imagine you have an `Employee` entity in your application, and you want to allow users to mark employees as "deleted" rather than physically deleting them from the database. This can be useful for keeping a record of deleted employees for auditing or other purposes.

To implement this using global query filters, you might do the following:

1. Add a `IsDeleted` property to the `Employee` entity class, and mark it with the `[QueryFilter]` attribute.
    
2. Define a method that returns a boolean expression representing the filter. This method might look something like this:
    

```csharp
private static bool IsNotDeleted(Employee employee)
{
    return !employee.IsDeleted;
}
```

1. In your `DbContext` class, call the `AddQueryFilter` method and pass in the `Employee` type and the `IsNotDeleted` method.
    
    ```csharp
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Employee>().HasQueryFilter(IsNotDeleted);
    }
    
    private static bool IsNotDeleted(Employee employee)
    {
        return !employee.IsDeleted;
    }
    ```
    

Now, every time a query is executed for the `Employee` entity, the `IsNotDeleted` filter will be applied, ensuring that only non-deleted employees are returned. When users want to delete an employee, you can simply set the `IsDeleted` property to `true` rather than physically deleting the record from the database.

### Enforcing Data Constraints

Imagine you have a `Transaction` entity that represents financial transactions in your application. You want to ensure that only transactions that have been approved by a manager are returned when users query the database.

To implement this using global query filters, you might do the following:

1. Add a `IsApproved` property to the `Transaction` entity class, and mark it with the `[QueryFilter]` attribute.
    
2. Define a method that returns a boolean expression representing the filter. This method might look something like this:
    

```csharp
private static bool IsApproved(Transaction transaction)
{
    return transaction.IsApproved;
}
```

1. In your `DbContext` class, call the `AddQueryFilter` method and pass in the `Transaction` type and the `IsApproved` method.
    
    ```csharp
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Transaction>().HasQueryFilter(IsApproved);
    }
    
    private static bool IsApproved(Transaction transaction)
    {
        return transaction.IsApproved;
    }
    ```
    

Now, every time a query is executed for the `Transaction` entity, the `IsApproved` filter will be applied, ensuring that only approved transactions are returned. This can help to ensure that users are only able to see transactions that have been properly reviewed and approved by a manager.