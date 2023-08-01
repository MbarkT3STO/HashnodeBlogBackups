---
title: "EF Core and PostgreSQL: Working with Arrays"
datePublished: Tue Aug 01 2023 19:29:02 GMT+0000 (Coordinated Universal Time)
cuid: clksp04m1000d08mjhwa9ahui
slug: ef-core-and-postgresql-working-with-arrays
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690914653540/784d9afc-100a-4496-a70b-99fac1b2c426.png
tags: entity-framework, postgresql, csharp, net, efcore

---

When it comes to working with databases and Entity Framework Core (EF Core), PostgreSQL has gained popularity for its robust features and excellent support for advanced data types. One such powerful feature is its support for arrays, allowing you to store and manipulate collections of values within a single database column. In this blog post, we will explore how to work with PostgreSQL arrays in EF Core, with practical examples to illustrate the concepts.

## 1\. Introduction to PostgreSQL Arrays

PostgreSQL arrays are a native data type that can store multiple elements of the same data type within a single column. Arrays in PostgreSQL are not limited to simple data types like integers or strings; they can store more complex types, such as other arrays or custom types. This flexibility makes them a powerful tool for dealing with structured data.

In EF Core, you can work with PostgreSQL arrays just like any other data type, but there are a few things to keep in mind to make the most of this feature.

## 2\. Setting Up the Project

Before we dive into working with PostgreSQL arrays in EF Core, let's set up a new project to demonstrate the concepts.

```bash
dotnet new console -n EFCorePostgresArrayDemo
cd EFCorePostgresArrayDemo
```

Next, add the required NuGet packages for EF Core and PostgreSQL provider:

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

## 3\. Defining Array Properties in EF Core Entities

Let's create a simple entity representing a blog post with tags as an array.

```csharp
public class BlogPost
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string[] Tags { get; set; }
}
```

In the above example, we have defined a `Tags` property as a string array. EF Core will automatically map this property to a PostgreSQL array when we configure the database context.

## 4\. Querying Data with Array Properties

To demonstrate querying data using array properties, let's assume we have already added some blog posts to the database. We can retrieve all posts that have a specific tag using the `Any` LINQ method.

```csharp
using (var context = new BlogDbContext())
{
    string targetTag = "EF Core";

    var postsWithTag = context.BlogPosts
        .Where(post => post.Tags.Any(tag => tag == targetTag))
        .ToList();

    foreach (var post in postsWithTag)
    {
        Console.WriteLine($"Title: {post.Title}, Tags: {string.Join(", ", post.Tags)}");
    }
}
```

## 5\. Filtering and Searching in Arrays

PostgreSQL provides various array functions that you can use in your EF Core queries. For example, if you want to retrieve posts that have tags starting with "Tutorial," you can use the `StartsWith` method with the `Any` method.

```csharp
using (var context = new BlogDbContext())
{
    string searchTag = "Tutorial";

    var postsWithSearchTag = context.BlogPosts.AsEnumerable()
        .Where(post => post.Tags.Any(tag => tag.StartsWith($"{searchTag}")))
        .ToList();

    foreach (var post in postsWithSearchTag)
    {
        Console.WriteLine($"Title: {post.Title}, Tags: {string.Join(", ", post.Tags)}");
    }
}
```

## 6\. Modifying Arrays

You can modify array properties in EF Core just like any other property. For example, to add a new tag to a blog post, retrieve the post, add the tag to the array, and save the changes.

```csharp
using (var context = new BlogDbContext())
{
    int postId = 1;
    string newTag = "Database";

    var post = context.BlogPosts.Find(postId);
    post.Tags = post.Tags.Append(newTag).ToArray();

    context.SaveChanges();
}
```