---
title: "EF Core and PostgreSQL: Working with JSON Data"
datePublished: Sun Jul 30 2023 17:23:43 GMT+0000 (Coordinated Universal Time)
cuid: clkppn9xt000c09l89oyg8p7t
slug: ef-core-and-postgresql-working-with-json-data
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690737068011/be62fa00-cbfe-4650-8e25-9655b87c9d11.png
tags: entity-framework, postgresql, net, efcore

---

PostgreSQL is a powerful relational database that supports JSON data types, allowing developers to store and retrieve JSON data directly in the database. In this blog post, we will explore how to leverage Entity Framework Core (EF Core) to work with JSON data in PostgreSQL.

## Prerequisites

Before we dive into the implementation, ensure you have the following prerequisites in place:

1. A .NET Core project with EF Core installed (`Microsoft.EntityFrameworkCore`)
    
2. PostgreSQL database set up and accessible from your application
    
3. Entity Framework Core PostgreSQL provider (`Npgsql.EntityFrameworkCore.PostgreSQL`) added to your project
    
4. And the following EF Core packages :
    
    `Microsoft.EntityFrameworkCore.Design`
    
    `Microsoft.EntityFrameworkCore.Tools`
    

## Step 1: Define the Model

First, let's create a model that includes a property to store JSON data. For demonstration purposes, let's assume we are building an application to store information about books, and we want to store additional metadata in JSON format.

```csharp
public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Author { get; set; }
    public string Genre { get; set; }
    public string JsonMetadata { get; set; }
}
```

## Step 2: Configure EF Core Context

Next, configure the EF Core context to work with PostgreSQL and map the JSON property to the appropriate PostgreSQL data type.

```csharp
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public DbSet<Book> Books { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseNpgsql("your_postgresql_connection_string");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Book>(entity =>
        {
            entity.Property(b => b.JsonMetadata)
                  .HasColumnType("jsonb"); // Use jsonb data type for PostgreSQL
        });
    }
}
```

Replace `"your_postgresql_connection_string"` with your actual PostgreSQL connection string.

## Step 3: Store JSON Data

With the model and context configured, you can now store JSON data in the PostgreSQL database.

```csharp
using (var dbContext = new AppDbContext())
{
    var book = new Book
    {
        Title = "Sample Book",
        Author = "John Doe",
        Genre = "Fiction",
        JsonMetadata = "{\"ISBN\": \"1234567890\", \"Pages\": 256, \"Language\": \"English\"}"
    };

    dbContext.Books.Add(book);
    dbContext.SaveChanges();
}
```

In this example, we store JSON metadata for a book, including properties like ISBN, number of pages, and language, within the `JsonMetadata` property.

## Step 4: Retrieve JSON Data

Retrieving JSON data is just as simple. You can query the database and access the JSON properties like regular properties.

```csharp
using (var dbContext = new AppDbContext())
{
    var book = dbContext.Books.FirstOrDefault(b => b.Title == "Sample Book");

    if (book != null)
    {
        // Access JSON properties
        string isbn = book.JsonMetadata["ISBN"];
        int pages = book.JsonMetadata["Pages"];
        string language = book.JsonMetadata["Language"];

        // Do something with the retrieved JSON data
    }
}
```

## JSON as Complex Type

So far, we have been storing and retrieving JSON data in PostgreSQL as a simple string using the `JsonMetadata` property of the `Book` model. However, EF Core allows us to work with JSON data more elegantly by treating it as a complex type. This approach enables stronger typing, better code readability, and easier maintenance. Let's explore how to use JSON as a complex type in EF Core.

### Step 1: Define a Complex Type for JSON

To work with JSON as a complex type, we need to define a corresponding .NET class that represents the structure of the JSON data. In our case, we will create a `BookMetadata` class to hold the additional metadata.

```csharp
public class BookMetadata
{
    public string ISBN { get; set; }
    public int Pages { get; set; }
    public string Language { get; set; }
}
```

### Step 2: Update the Book Model

Now that we have defined the complex type `BookMetadata`, we can update the `Book` model to use it as the type for the `JsonMetadata` property.

```csharp
public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Author { get; set; }
    public string Genre { get; set; }
    public BookMetadata JsonMetadata { get; set; }
}
```

### Step 3: Configure Complex Type in EF Core

To configure EF Core to treat `BookMetadata` as a complex type, we need to override the `OnModelCreating` method in the `AppDbContext` class.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Book>(entity =>
    {
        entity.Property(b => b.JsonMetadata)
              .HasConversion(
                  metadata => JsonConvert.SerializeObject(metadata),
                  json => JsonConvert.DeserializeObject<BookMetadata>(json)
              )
              .HasColumnType("jsonb"); // Use jsonb data type for PostgreSQL
    });
}
```

**Or:**

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Book>(entity =>
    {
        entity.Property(b => b.JsonMetadata)
              .HasConversion(
                  metadata => JsonSerializer.Serialize(metadata, new JsonSerializerOptions { PropertyNamingPolicy = null }),
                  json => JsonSerializer.Deserialize<SocialMedia>(json, new JsonSerializerOptions { PropertyNamingPolicy = null })
              )
              .HasColumnType("jsonb"); // Use jsonb data type for PostgreSQL
    });
}
```

In this configuration, we use the `HasConversion` method to specify how to convert the complex type `BookMetadata` to and from JSON when storing and retrieving data from the database.

### Step 4: Store JSON Data as Complex Type

Now, let's update our data insertion code to use the `BookMetadata` complex type.

```csharp
using (var dbContext = new AppDbContext())
{
    var book = new Book
    {
        Title = "Sample Book",
        Author = "John Doe",
        Genre = "Fiction",
        JsonMetadata = new BookMetadata
        {
            ISBN = "1234567890",
            Pages = 256,
            Language = "English"
        }
    };

    dbContext.Books.Add(book);
    dbContext.SaveChanges();
}
```

### Step 5: Retrieve JSON Data as Complex Type

When retrieving data, EF Core automatically converts the JSON data to the `BookMetadata` complex type.

```csharp
using (var dbContext = new AppDbContext())
{
    var book = dbContext.Books.FirstOrDefault(b => b.Title == "Sample Book");

    if (book != null)
    {
        // Access complex type properties directly
        string isbn = book.JsonMetadata.ISBN;
        int pages = book.JsonMetadata.Pages;
        string language = book.JsonMetadata.Language;

        // Do something with the retrieved complex type data
    }
}
```

## Conclusion

Using JSON as a complex type in EF Core allows us to work with JSON data more intuitively and efficiently. By representing JSON data as .NET classes, we gain type safety and can easily access properties, making our code more robust and maintainable. PostgreSQL's support for JSON data types, combined with EF Core's versatility, provides a powerful combination for developing modern applications with dynamic data requirements.