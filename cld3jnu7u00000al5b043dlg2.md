# Configuration of EF Core Entities using FluentAPI

EF Core uses a set of configuration classes to define the structure of the database and the relationships between the entities. These configuration classes are used to map the .NET classes to the database tables and columns, and to specify the rules for working with the data.

In this article, we will discuss how to configure EF Core entities using the Fluent API, which provides a more expressive and flexible way to configure the entities than the traditional data annotations.

## **Defining the Entity**

The first step in configuring an EF Core entity is to define the class that represents the entity. The class should have properties that correspond to the columns in the table, and it should be decorated with the `DbSet<T>` attribute, which tells EF Core that the class represents an entity.

Here is an example of a simple `Student` entity class:

```csharp
public class Student
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}
```

## **Configuring the Table and Columns**

The next step is to configure the database table and columns that the entity maps to. This can be done using the Fluent API in the `OnModelCreating` method of the `DbContext` class.

Here is an example of how to configure the `Student` entity to map to a `Students` table with columns `Id`, `FirstName`, `LastName`, and `Email`:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Student>()
        .ToTable("Students")
        .Property(s => s.Id).IsRequired().ValueGeneratedOnAdd();
    modelBuilder.Entity<Student>()
        .Property(s => s.FirstName).HasMaxLength(50).IsRequired();
    modelBuilder.Entity<Student>()
        .Property(s => s.LastName).HasMaxLength(50).IsRequired();
    modelBuilder.Entity<Student>()
        .Property(s => s.Email).HasMaxLength(100).IsRequired();
}
```

In the above example, we are using the Fluent API to configure the `Student` entity to map to a `Students` table. We are also configuring the `Id` property as the primary key and a required field, and the other properties as required fields with maximum length of 50 and 100.

## **Configuring Relationships**

EF Core also allows you to configure the relationships between entities. There are three types of relationships that can be configured: one-to-one, one-to-many, and many-to-many.

Here is an example of how to configure a one-to-many relationship between the `Student` and `Course` entities:

```csharp
public class Student
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    public ICollection<Course> Courses { get; set; }
}

public class Course
{
public int Id { get; set; }
public string Name { get; set; }
public int StudentId { get; set; }
public Student Student { get; set; }
}
```

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
modelBuilder.Entity<Student>()
.ToTable("Students")
.Property(s => s.Id).IsRequired().ValueGeneratedOnAdd();
modelBuilder.Entity<Student>()
.Property(s => s.FirstName).HasMaxLength(50).IsRequired();
modelBuilder.Entity<Student>()
.Property(s => s.LastName).HasMaxLength(50).IsRequired();
modelBuilder.Entity<Student>()
.Property(s => s.Email).HasMaxLength(100).IsRequired();

modelBuilder.Entity<Course>()
    .ToTable("Courses")
    .Property(c => c.Id).IsRequired().ValueGeneratedOnAdd();
modelBuilder.Entity<Course>()
    .Property(c => c.Name).HasMaxLength(100).IsRequired();
modelBuilder.Entity<Course>()
    .HasOne(c => c.Student)
    .WithMany(s => s.Courses)
    .HasForeignKey(c => c.StudentId);
}
```

In this example, we have defined a one-to-many relationship between the `Student` and `Course` entities, where each student can have multiple courses, and each course is associated with one student. We have used the `HasOne` and `WithMany` methods of the Fluent API to define the relationship, and we have also specified the foreign key property `StudentId` on the `Course` entity.

## **Configuration using Separate Classes**

Another way to configure EF Core entities is by using separate configuration classes. This approach allows for better organization and separation of concerns, especially when working with a large number of entities and complex configurations.

To use this approach, you need to create a separate configuration class for each entity. The configuration class should inherit from the `IEntityTypeConfiguration<TEntity>` interface, where `TEntity` is the entity it's configuring.

Here is an example of a configuration class for the `Student` entity:

```csharp
public class StudentConfiguration : IEntityTypeConfiguration<Student>
{
    public void Configure(EntityTypeBuilder<Student> builder)
    {
        builder.ToTable("Students");
        builder.HasKey(s => s.Id);
        builder.Property(s => s.Id).ValueGeneratedOnAdd();
        builder.Property(s => s.FirstName).HasMaxLength(50).IsRequired();
        builder.Property(s => s.LastName).HasMaxLength(50).IsRequired();
        builder.Property(s => s.Email).HasMaxLength(100).IsRequired();
    }
}
```

As you can see, the `Configure` method of the configuration class is used to define the configuration for the entity. It's very similar to what we've seen before when using the Fluent API in the `OnModelCreating` method of the `DbContext` class.

To use the configuration class, you need to register it in the `OnModelCreating` method of the `DbContext` class:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfiguration(new StudentConfiguration());
}
```

You can also create configuration classes for relationships:

```csharp
public class CourseConfiguration : IEntityTypeConfiguration<Course>
{
    public void Configure(EntityTypeBuilder<Course> builder)
    {
        builder.ToTable("Courses");
        builder.HasKey(c => c.Id);
        builder.Property(c => c.Id).ValueGeneratedOnAdd();
        builder.Property(c => c.Name).HasMaxLength(100).IsRequired();
        builder.HasOne(c => c.Student)
            .WithMany(s => s.Courses)
            .HasForeignKey(c => c.StudentId);
    }
}
```

and register it in the same way:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfiguration(new StudentConfiguration());
    modelBuilder.ApplyConfiguration(new CourseConfiguration());
}
```

Using separate configuration classes allows for better organization and separation of concerns. It also allows you to reuse configuration classes across multiple DbContexts.

There is another way to register the configuration classes in the `OnModelCreating` method of the `DbContext` class, using the `Add` method of the `ModelBuilder` class.

Instead of using the `ApplyConfiguration` method, you can use the `Add` method to register the configuration classes.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Add(new StudentConfiguration());
    modelBuilder.Add(new CourseConfiguration());
}
```

This method is a bit more verbose than the `ApplyConfiguration` method, but it provides the same functionality.

Another way is to use a assembly scanning and register all the configuration classes in one go.

```csharp
emodelBuilder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());
```

This way, you don't need to register each configuration class individually. It scan the current assembly for all the classes that implement `IEntityTypeConfiguration<T>` and register them automatically.

So, you can choose the registration method that best fits your needs and code organization.

## Seed the Database with Initial Data

EF Core provides a way to seed the database with initial data, also known as "seed data". Seed data is useful for pre-populating the database with a set of data that is needed for the application to work correctly. For example, you might want to seed the database with a list of predefined users, roles, or lookup values.

There are two ways to seed data with EF Core: using the `HasData` method of the Fluent API, or by creating a custom `DbContext` seed method.

### **Using the HasData Method**

The `HasData` method of the Fluent API allows you to seed data directly in the configuration class. You can use it in the `Configure` method of the configuration class, right after you define the entity configuration. Here is an example of how to seed the `Students` table with some initial data:

```csharp
public class StudentConfiguration : IEntityTypeConfiguration<Student>
{
    public void Configure(EntityTypeBuilder<Student> builder)
    {
        builder.ToTable("Students");
        builder.HasKey(s => s.Id);
        builder.Property(s => s.Id).ValueGeneratedOnAdd();
        builder.Property(s => s.FirstName).HasMaxLength(50).IsRequired();
        builder.Property(s => s.LastName).HasMaxLength(50).IsRequired();
        builder.Property(s => s.Email).HasMaxLength(100).IsRequired();
        builder.HasData(
            new Student { Id = 1, FirstName = "John", LastName = "Doe", Email = "johndoe@email.com" },
            new Student { Id = 2, FirstName = "Jane", LastName = "Doe", Email = "janedoe@email.com" }
        );
    }
}
```

### **Using Custom DbContext Seed Method**

Another way to seed data is by creating a custom seed method in the `DbContext` class. This method can be called in the `OnModelCreating` method to seed the data after the model is created.

Here is an example of how to create a custom seed method in the `DbContext` class:

```csharp
public class ApplicationDbContext : DbContext
{
    public DbSet<Student> Students { get; set; }
    //other DbSets

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());
    }
    public void SeedData(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Student>().HasData(
            new Student { Id = 1, FirstName = "John", LastName = "Doe", Email = "johndoe@email.com" },
            new Student { Id = 2, FirstName = "Jane", LastName = "Doe", Email = "janedoe@email.com" }
        );
    }
}
```

## Cascading Update and Delete

When working with entities that have relationships, it's important to consider what happens to the related entities when an entity is updated or deleted. EF Core allows you to configure cascading actions to specify how the related entities should be handled when the parent entity is updated or deleted.

There are three cascading actions that can be configured:

* **Cascade**: When a parent entity is updated or deleted, the related entities are updated or deleted as well.
    
* **Restrict**: When a parent entity is updated or deleted, an exception is thrown if there are any related entities.
    
* **Set Null**: When a parent entity is deleted, the foreign key on the related entities is set to null. This option is only available for nullable foreign keys.
    

Here is an example of how to configure cascading actions using the Fluent API:

```csharp
modelBuilder.Entity<Course>()
    .HasOne(c => c.Student)
    .WithMany(s => s.Courses)
    .HasForeignKey(c => c.StudentId)
    .OnDelete(DeleteBehavior.Cascade);
```

In this example, we have defined a one-to-many relationship between the `Student` and `Course` entities and set the cascading action to `DeleteBehavior.Cascade`. This means that when a student is deleted, all the courses associated with that student will also be deleted.

It's important to note that the default cascading behavior is `Restrict`, meaning that an exception will be thrown if there are any related entities when the parent entity is updated or deleted.

Another thing to consider when working with cascading actions is that they can have a significant impact on the data integrity of your database. For example, if you have a cascading delete action and you delete a parent entity without realizing it, all the related entities will also be deleted. This can lead to data loss and unexpected behavior in the application. It's important to be mindful of the cascading actions you are using and to test them thoroughly to ensure they work as expected.