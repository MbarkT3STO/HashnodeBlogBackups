# How to Auto Update Created, Updated and Deleted Timestamps in Entity Framework Core?

In many applications, it is useful to track when entities are created, updated, and deleted. Entity Framework Core (EF Core) provides several ways to achieve this, including using database triggers, database default values, and EF Core's built-in features. In this article, we will focus on using EF Core's built-in features to automatically update timestamps when entities are created, updated, and deleted.

## **Setting up the Entity and DbContext**

To begin, let's set up a simple entity class with three timestamp properties: `CreatedAt`, `UpdatedAt`, and `DeletedAt`. We will also add a `IsDeleted` property to track whether the entity has been deleted or not.

```csharp
public class MyEntity
{
    public int Id { get; set; }
    public string Name { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime UpdatedAt { get; set; }
    public DateTime? DeletedAt { get; set; }
    public bool IsDeleted { get; set; }
}
```

Next, we will define our `DbContext` class and configure the `MyEntity` entity to use the timestamp properties we defined above.

```csharp
public class MyDbContext : DbContext
{
    public DbSet<MyEntity> MyEntities { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<MyEntity>()
            .Property(e => e.CreatedAt)
            .HasDefaultValueSql("GETDATE()");

        modelBuilder.Entity<MyEntity>()
            .Property(e => e.UpdatedAt)
            .HasDefaultValueSql("GETDATE()");
    }
}
```

In the `OnModelCreating` method, we are using the `HasDefaultValueSql` method to set the `CreatedAt` and `UpdatedAt` properties to the current date and time using the `GETDATE()` SQL function. This will ensure that these properties are automatically set to the current date and time whenever a new `MyEntity` is inserted into the database.

## **Handling Updates and Deletes**

Now that we have set up the timestamps for inserts, let's turn our attention to updates and deletes. To handle updates, we can use EF Core's `SaveChanges` method to update the `UpdatedAt` property whenever an entity is saved.

```csharp
Copy codepublic override int SaveChanges()
{
    var modifiedEntities = ChangeTracker.Entries()
        .Where(e => e.State == EntityState.Modified);

    foreach (var entity in modifiedEntities)
    {
        entity.Property("UpdatedAt").CurrentValue = DateTime.Now;
    }

    return base.SaveChanges();
}
```

We can use a similar approach to handle deletes. However, instead of deleting the entity from the database, we will set the `DeletedAt` property to the current date and time and set the `IsDeleted` property to `true`. This allows us to "soft delete" the entity, meaning it is still present in the database but is no longer considered active.

```csharp
public override int SaveChanges()
{
    var modifiedEntities = ChangeTracker.Entries()
    .Where(e => e.State == EntityState.Modified);

foreach (var entity in modifiedEntities)
{
    entity.Property("UpdatedAt").CurrentValue = DateTime.Now;
}

var deletedEntities = ChangeTracker.Entries()
    .Where(e => e.State == EntityState.Deleted);

foreach (var entity in deletedEntities)
{
    entity.State = EntityState.Modified;
    entity.Property("DeletedAt").CurrentValue = DateTime.Now;
    entity.Property("IsDeleted").CurrentValue = true;
}

return base.SaveChanges();
```

## **Querying Deleted Entities**

Now that we have implemented the ability to "soft delete" entities, we may want to include deleted entities in our queries by default. To do this, we can override the `OnModelCreating` method in our `DbContext` class and use the `QueryFilter` method to apply a filter to all queries for the `MyEntity` type.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<MyEntity>()
        .Property(e => e.CreatedAt)
        .HasDefaultValueSql("GETDATE()");

    modelBuilder.Entity<MyEntity>()
        .Property(e => e.UpdatedAt)
        .HasDefaultValueSql("GETDATE()");

    modelBuilder.Entity<MyEntity>()
        .HasQueryFilter(e => !e.IsDeleted);
}
```

With this filter in place, all queries for `MyEntity` will exclude deleted entities by default. If you want to include deleted entities in a specific query, you can use the `IgnoreQueryFilters` method to override the filter.

```csharp
var allEntities = context.MyEntities
    .IgnoreQueryFilters()
    .ToList();
```

In this article, we have seen how to use EF Core's built-in features to automatically update timestamps when entities are created, updated, and deleted. We have also learned how to "soft delete" entities and include them in queries using the `QueryFilter` and `IgnoreQueryFilters` methods. By following these techniques, you can easily track the creation, update, and deletion of entities in your EF Core applications.