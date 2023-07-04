---
title: "EF Core: Compiled Queries"
datePublished: Tue Jul 04 2023 18:38:50 GMT+0000 (Coordinated Universal Time)
cuid: cljomvqjh000009mk77g3e4qd
slug: ef-core-compiled-queries
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688495270277/0f331386-f8c1-470d-9923-1ad5c272c535.webp
tags: entity-framework, csharp, net, efcore

---

In Entity Framework Core (EF Core), queries play a crucial role in retrieving data from a database. To optimize query performance, EF Core provides a feature called "compiled queries." This feature allows developers to pre-compile LINQ queries into efficient query execution plans, resulting in improved performance and reduced overhead. In this blog post, we will explore EF Core's compiled queries feature and demonstrate its usage with examples.

## **What are Compiled Queries?**

Compiled queries in EF Core refer to LINQ queries that are pre-compiled into executable SQL statements. Unlike normal LINQ queries, which are dynamically compiled at runtime, compiled queries are transformed into parameterized SQL queries during the initial compilation phase. This transformation process allows EF Core to cache the compiled query execution plan, leading to significant performance improvements when executing the same query multiple times.

## **Benefits of Compiled Queries**

Utilizing compiled queries in your EF Core applications can yield several benefits, including:

1. **Improved Performance**: By eliminating the need for dynamic query compilation, compiled queries can significantly reduce the overhead associated with query execution, resulting in faster data retrieval.
    
2. **Reduced Database Round-Trips**: The use of compiled queries enables EF Core to cache the query execution plan, reducing the number of database round-trips required for query execution, which can further enhance performance.
    
3. **Parameterization**: Compiled queries support parameterization, allowing you to easily pass parameters to your queries. This helps prevent SQL injection vulnerabilities and promotes code reusability.
    

## **Using Compiled Queries in EF Core**

To demonstrate the usage of compiled queries in EF Core, let's consider a scenario where we have an `AppDbContext` class representing the application's database context. We'll create a compiled query that retrieves a list of products based on a specific category:

```csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.Linq;
using System.Linq.Expressions;

public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    // Compiled query definition
    private static readonly Func<AppDbContext, string, IQueryable<Product>> GetProductsByCategoryQuery = 
        EF.CompileQuery((AppDbContext context, string category) => 
            context.Products.Where(p => p.Category == category));

    // Usage of compiled query
    public IQueryable<Product> GetProductsByCategory(string category)
    {
        return GetProductsByCategoryQuery(this, category);
    }
}
```

In the code snippet above, we define a compiled query using the `EF.CompileQuery` method. The compiled query takes an `AppDbContext` instance and a `string` parameter representing the category of the products to retrieve. The query selects all products from the `Products` DbSet that match the specified category. Note that the compiled query is defined as a `Func<AppDbContext, string, IQueryable<Product>>`.

To use the compiled query, we create a public method `GetProductsByCategory` in the `AppDbContext` class. This method accepts the category as a parameter and invokes the pre-compiled query, passing the current instance of the database context and the provided category. The compiled query is then executed and returns the filtered list of products.

## **Async Compiled Query**

In addition to regular compiled queries, EF Core also supports async compiled queries. Async compiled queries enable developers to perform asynchronous database operations efficiently by combining the benefits of compiled queries and asynchronous programming. Let's dive into how to use async compiled queries in EF Core.

To demonstrate the usage of async compiled queries, we will modify our previous example of retrieving products by category in the `AppDbContext` class.

```csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.Linq;
using System.Linq.Expressions;
using System.Threading.Tasks;

public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    // Compiled query definition
    private static readonly Func<AppDbContext, string, Task<IQueryable<Product>>> GetProductsByCategoryQuery =
        EF.CompileAsyncQuery((AppDbContext context, string category) =>
            context.Products.Where(p => p.Category == category));

    // Usage of compiled query
    public async Task<IQueryable<Product>> GetProductsByCategoryAsync(string category)
    {
        return await GetProductsByCategoryQuery(this, category);
    }
}
```

In the modified code snippet, we introduced the `EF.CompileAsyncQuery` method to create an async compiled query. This method allows us to compile a LINQ query into an asynchronous execution plan. The compiled query now returns a `Task<IQueryable<Product>>` instead of a synchronous `IQueryable<Product>`.

We also updated the `GetProductsByCategoryAsync` method to be async. This method awaits the execution of the async compiled query and returns the resulting filtered products.

## **Conclusion**

In this article, we explored EF Core's compiled queries feature, which allows pre-compiling LINQ queries for improved performance. We saw how to define and use compiled queries in the `AppDbContext` class, and we also discussed the benefits of async compiled queries for efficient asynchronous database operations. By leveraging compiled queries, developers can optimize query performance and enhance the efficiency of their EF Core applications.