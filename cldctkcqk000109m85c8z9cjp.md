# Overriding SaveChanges in EF Core

Entity Framework (EF) Core is a popular Object-Relational Mapping (ORM) framework for .NET applications. It allows developers to interact with databases using a familiar object-oriented syntax, rather than writing raw SQL queries. One of the key features of EF Core is its ability to automatically track changes made to entities and persist those changes to the database when the `SaveChanges` method is called.

However, in some cases, developers may want to override the default behavior of `SaveChanges` to perform additional actions or validation before or after the changes are saved. In this article, we will explore how to override `SaveChanges` in EF Core and why it may be useful to do so.

## **Why Override SaveChanges?**

There are several reasons why a developer may want to override `SaveChanges` in EF Core:

* **Auditing**: Recording information about who made changes to which entities and when.
    
* **Validation**: Checking that the data being saved is valid before it is committed to the database.
    
* **Performance**: Batching multiple changes together to improve performance.
    
* **Security**: Checking that the user has the necessary permissions to make the changes.
    

## **How to Override SaveChanges**

There are two ways to override `SaveChanges` in EF Core:

1. **Override the** `SaveChanges` method in the DbContext: The easiest way to override `SaveChanges` is to simply override the method in the DbContext class.
    

```csharp
public class MyDbContext : DbContext
{
    protected override int SaveChanges()
    {
        // Your custom logic here
        return base.SaveChanges();
    }
}
```

1. **Use the** `IChangeTracker.Tracked` event: EF Core also provides an event that is raised before changes are saved to the database, called `IChangeTracker.Tracked`. This event can be used to perform custom logic before changes are saved.
    

```csharp
public class MyDbContext : DbContext
{
    public MyDbContext(DbContextOptions<MyDbContext> options) : base(options)
    {
        ChangeTracker.Tracked += OnTracked;
    }

    private void OnTracked(object sender, EntityTrackedEventArgs args)
    {
        // Your custom logic here
    }
}
```

It is important to note that the `SaveChanges` method must be called after the custom logic is executed in order for the changes to be persisted to the database.

## **Example**

Here is an example of using the `SaveChanges` method to perform audit logging:

```csharp
public class MyDbContext : DbContext
{

    protected override int SaveChanges()
    {
        var auditEntries = OnBeforeSaveChanges();
        var result = base.SaveChanges();
        OnAfterSaveChanges(auditEntries);
        return result;
    }

    private List<AuditEntry> OnBeforeSaveChanges()
    {
        ChangeTracker.DetectChanges();
        var auditEntries = new List<AuditEntry>();
        foreach (var entry in ChangeTracker.Entries())
        {
            if (entry.State == EntityState.Added ||
                entry.State == EntityState.Deleted ||
                entry.State == EntityState.Modified)
            {
                auditEntries.Add(new AuditEntry(entry));
            }
        }

        return auditEntries;
    }        

    private void OnAfterSaveChanges(List<AuditEntry> auditEntries)
    {
        if (auditEntries == null || auditEntries.Count == 0)
            return;
    
        foreach (var auditEntry in auditEntries)
        {
            // Save audit entry to the database
            // ...
        }
    }

}
```

```csharp
public class AuditEntry
{
    public AuditEntry(EntityEntry entry)
    {
        Entry = entry;
        // Set other properties, such as UserId, Timestamp, etc.
        // ...
    }

    public EntityEntry Entry { get; }
    public string UserId { get; set; }
    public DateTime Timestamp { get; set; }
    // Other properties
    // ...
}
```

In this example, the `SaveChanges` method is overridden to call two custom methods `OnBeforeSaveChanges` and `OnAfterSaveChanges` before and after the changes are saved to the database. The `OnBeforeSaveChanges` method creates a list of `AuditEntry` objects for each entity that has been added, modified, or deleted. The `OnAfterSaveChanges` method then saves those audit entries to the database.

It's important to note that this is just an example, and the exact implementation will depend on the requirements of your application.

In conclusion, overriding `SaveChanges` in EF Core can be useful for a variety of reasons such as auditing, validation, performance, and security. It can be easily done by overriding the `SaveChanges` method in the DbContext class or by using the `IChangeTracker.Tracked` event. With these powerful features, developers can have more control over the persistence of data, and implement custom logic to handle specific scenarios.

## Real-World Examples

### Change Inventory Levels of Products

A real-world example of when it may be useful to override `SaveChanges` in EF Core could be for an e-commerce application.

Let's say we want to keep track of the inventory levels for our products in the database. Whenever a user places an order, we need to decrement the quantity of the product in the inventory. We could do this by creating a method in our repository class that retrieves the product, decrements the quantity and then saves the changes.

However, this approach could cause a race condition if multiple orders are placed for the same product at the same time. To avoid this, we could override the `SaveChanges` method in our DbContext to handle the inventory update atomically with the rest of the changes.

```csharp
public class ECommerceDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
    public DbSet<Order> Orders { get; set; }

    protected override int SaveChanges()
    {
        var changedOrders = ChangeTracker.Entries<Order>()
            .Where(o => o.State == EntityState.Added || o.State == EntityState.Modified);
        
        foreach (var order in changedOrders)
        {
            var product = Products.Find(order.ProductId);
            product.Quantity -= order.Quantity;
        }

        return base.SaveChanges();
    }
}
```

In this example, before the changes are saved to the database, the `SaveChanges` method retrieves all of the `Order` entities that have been added or modified, and updates the corresponding product's quantity. By doing this in the `SaveChanges` method, we can ensure that the inventory update is performed atomically with the rest of the changes, avoiding the race condition.

It's important to note that this is just an example, and the exact implementation will depend on the requirements of your application.

### Enforce a Specific Workflow

Another real-world example of when it may be useful to override `SaveChanges` in EF Core could be for a web application that needs to enforce a specific workflow for certain entities.

Let's say we have a `Task` entity in our application, and tasks can only be marked as "completed" if they have been reviewed by a manager. We could enforce this workflow by overriding the `SaveChanges` method in our DbContext.

```csharp
public class WorkflowDbContext : DbContext
{
    public DbSet<Task> Tasks { get; set; }

    protected override int SaveChanges()
    {
        var changedTasks = ChangeTracker.Entries<Task>()
            .Where(t => t.State == EntityState.Modified);
        
        foreach (var task in changedTasks)
        {
            if (task.IsCompleted && !task.IsManagerReviewed)
            {
                throw new InvalidOperationException("Task cannot be completed without review by a manager.");
            }
        }

        return base.SaveChanges();
    }
}
```

In this example, before the changes are saved to the database, the `SaveChanges` method retrieves all of the `Task` entities that have been modified and checks if the task is marked as completed, but hasn't been reviewed by a manager. If so, it throws an exception, preventing the changes from being saved and enforcing the workflow rule.

By doing this in the `SaveChanges` method, we can ensure that the workflow rule is enforced for all changes made to the `Task` entity, regardless of how they are made (e.g. through the application UI, API, or a background process).

As I said before it's important to know that this is just an example, and the exact implementation will depend on the requirements of your application.