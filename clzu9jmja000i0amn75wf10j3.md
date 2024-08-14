---
title: "ASP.NET Core: Implementing Caching with SQL Server"
datePublished: Wed Aug 14 2024 19:47:25 GMT+0000 (Coordinated Universal Time)
cuid: clzu9jmja000i0amn75wf10j3
slug: aspnet-core-implementing-caching-with-sql-server
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723664803863/b3576c13-cd93-4aef-8999-d2f5c9541b6f.webp
tags: sql-server, csharp, net, aspnet-core

---

Caching is a powerful technique for improving the performance and scalability of your [ASP.NET](http://ASP.NET) Core applications. By temporarily storing frequently accessed data in memory, caching reduces the need to query the database repeatedly. In this blog post, we’ll explore how to implement caching in an [ASP.NET](http://ASP.NET) Core application using SQL Server as the data source, with examples using Entity Framework Core (EF Core).

### What is Caching?

Caching is the process of storing copies of data in a temporary storage location, called a cache, so that future requests for that data can be served more quickly. In the context of a web application, caching can reduce database load and improve response times, leading to better user experience and reduced server costs.

### Why Use Caching in [ASP.NET](http://ASP.NET) Core?

Caching is particularly useful in scenarios where data does not change frequently and the cost of fetching it from the database is high. By caching the results of expensive queries or frequently accessed data, you can significantly reduce the load on your SQL Server and improve the overall performance of your application.

### Step 1: Setting Up the [ASP.NET](http://ASP.NET) Core Project

First, Create a new [ASP.NET](http://ASP.NET) Core Web API project.

Next, add the necessary packages for Entity Framework Core and SQL Server.

```powershell
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.InMemory
dotnet add package Microsoft.Extensions.Caching.Memory
dotnet add package Microsoft.Extensions.Caching.SqlServer
```

### Step 2: Configuring EF Core to Use SQL Server

We’ll start by creating a simple model and a corresponding `DbContext` to interact with SQL Server.

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("YourConnectionStringHere");
    }
}
```

Update `Startup.cs` to register the `AppDbContext` with the dependency injection container.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<AppDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddMemoryCache(); // Register in-memory caching
    services.AddControllers();
}
```

### Step 3: Implementing In-Memory Caching

In-memory caching stores data in the memory of the web server. It’s a fast and simple way to cache data but is only effective for single-instance applications.

Here’s how to implement in-memory caching for a `GetProducts` method in a controller.

```csharp
[ApiController]
[Route("[controller]")]
public class ProductsController : ControllerBase
{
    private readonly AppDbContext _context;
    private readonly IMemoryCache _cache;
    private const string CacheKey = "products_cache";

    public ProductsController(AppDbContext context, IMemoryCache cache)
    {
        _context = context;
        _cache = cache;
    }

    [HttpGet]
    public async Task<IEnumerable<Product>> GetProducts()
    {
        if (!_cache.TryGetValue(CacheKey, out IEnumerable<Product> products))
        {
            products = await _context.Products.ToListAsync();

            var cacheOptions = new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5),
                SlidingExpiration = TimeSpan.FromMinutes(2)
            };

            _cache.Set(CacheKey, products, cacheOptions);
        }

        return products;
    }
}
```

### Step 4: Using Distributed Caching with SQL Server

In-memory caching is limited to the memory of the server on which the application is running. For a distributed application or multiple server instances, you’ll need to use distributed caching. [ASP.NET](http://ASP.NET) Core supports distributed caching with SQL Server.

First, configure distributed caching in `Startup.cs`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<AppDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDistributedSqlServerCache(options =>
    {
        options.ConnectionString = Configuration.GetConnectionString("Connection of the database that will holde cache");
        options.SchemaName = "dbo";
        options.TableName = "CacheTableName";
    });

    services.AddControllers();
}
```

**NOTE:**

Create you cache table by executing the following SQL script:

```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[CacheTableName](
	[Id] [nvarchar](449) NOT NULL,
	[Value] [varbinary](max) NOT NULL,
	[ExpiresAtTime] [datetimeoffset](7) NOT NULL,
	[SlidingExpirationInSeconds] [bigint] NULL,
	[AbsoluteExpiration] [datetimeoffset](7) NULL,
PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, 
	IGNORE_DUP_KEY = OFF, 
	ALLOW_ROW_LOCKS = ON, 
	ALLOW_PAGE_LOCKS = ON, 
	OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

Then, update the `ProductsController` to use distributed caching.

```csharp
public class ProductsController : ControllerBase
{
    private readonly AppDbContext _context;
    private readonly IDistributedCache _cache;
    private const string CacheKey = "products_cache";

    public ProductsController(AppDbContext context, IDistributedCache cache)
    {
        _context = context;
        _cache = cache;
    }

    [HttpGet]
    public async Task<IEnumerable<Product>> GetProducts()
    {
        var cachedProducts = await _cache.GetStringAsync(CacheKey);
        if (cachedProducts != null)
        {
            return JsonSerializer.Deserialize<IEnumerable<Product>>(cachedProducts);
        }

        var products = await _context.Products.ToListAsync();
        var serializedProducts = JsonSerializer.Serialize(products);

        await _cache.SetStringAsync(CacheKey, serializedProducts, new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5),
            SlidingExpiration = TimeSpan.FromMinutes(2)
        });

        return products;
    }
}
```

### Best Practices for Implementing Caching in [ASP.NET](http://ASP.NET) Core

Caching is a powerful tool, but it should be used thoughtfully to avoid potential pitfalls. Here are some best practices to consider when implementing caching in your [ASP.NET](http://ASP.NET) Core application:

#### 1\. **Cache Appropriately**

Not all data should be cached. Cache data that is expensive to retrieve and does not change frequently. For example, consider caching reference data, configuration settings, or results of complex calculations.

#### 2\. **Use Expiration Policies**

Set appropriate expiration policies to prevent stale data from being served. Absolute expiration sets a fixed duration after which the cache entry is invalidated, while sliding expiration resets the expiration time whenever the cached data is accessed.

#### 3\. **Monitor Cache Usage**

Keep an eye on cache usage and performance metrics. Monitor cache hit rates, memory usage, and database load to ensure that caching is providing the expected benefits.

#### 4\. **Handle Cache Invalidation**

Implement a strategy for cache invalidation when underlying data changes. This could involve clearing the cache when data is updated or using cache tags to invalidate related cache entries.

#### 5\. **Consider Cache Size**

For in-memory caching, consider the size of the data being cached and the available memory on your server. Large cache sizes can lead to memory pressure and potentially degrade application performance.

#### 6\. **Leverage Distributed Caching for Scalability**

If your application runs on multiple servers or instances, use distributed caching to ensure consistency across servers. SQL Server distributed caching is a reliable option, but you could also explore other distributed caching solutions like Redis or NCache.