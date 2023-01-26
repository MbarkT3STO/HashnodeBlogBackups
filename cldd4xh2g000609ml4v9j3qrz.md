# Enabling Lazy Loading in EF Core

One of the features of EF Core is the ability to enable lazy loading, which can improve the performance of your application by deferring the loading of related data until it is actually needed.

## **What is Lazy Loading?**

Lazy loading is a technique that defers the loading of related data until it is actually needed. For example, when loading an entity from the database, lazy loading allows you to only load the properties that you need, rather than loading all of the properties of the entity and its related entities. This can improve the performance of your application by reducing the amount of data that needs to be loaded from the database.

## **How to Enable Lazy Loading in EF Core?**

To enable lazy loading in EF Core, you will need to configure your context to use lazy loading. This can be done by setting the `UseLazyLoadingProxies` property to `true` on your `DbContext` options.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder.UseLazyLoadingProxies();
}
```

## **Example**

Consider a simple example where we have a `Blog` class that has a collection of `Post` entities.

```csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public Blog Blog { get; set; }
}
```

Without lazy loading, when we load a `Blog` entity, the `Posts` collection will be loaded from the database at the same time. With lazy loading enabled, the `Posts` collection will not be loaded until it is actually accessed.

```csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .FirstOrDefault(b => b.BlogId == 1);

    Console.WriteLine(blog.Url); // This will be loaded immediately
    Console.WriteLine(blog.Posts.Count()); // This will be loaded only when accessed
}
```

## Important

It's important to note that lazy loading has some drawbacks as well. Since the related data is not loaded until it is accessed, it can lead to additional roundtrips to the database, which can negatively impact performance. Additionally, if you are not careful when accessing related data, you may accidentally trigger a load when you don't intend to.

Additionally, when working with detached entities, lazy loading won't work as expected, so it's best practice to turn off the lazy loading feature when working with detached entities.

Another way to load the related data without lazy loading is to use the `Include` method provided by EF Core. The `Include` method allows you to explicitly specify which related data should be loaded along with the primary data. This can help you avoid the potential performance issues caused by lazy loading, while still allowing you to control the amount of data that is loaded from the database.

```csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Include(b => b.Posts) // this will load the related data
        .FirstOrDefault(b => b.BlogId == 1);

    Console.WriteLine(blog.Url);
    Console.WriteLine(blog.Posts.Count());
}
```

## **Conclusion**

Enabling lazy loading in EF Core can help improve the performance of your application by deferring the loading of related data until it is actually needed. By setting the `UseLazyLoadingProxies` property to `true` on your `DbContext` options, you can enable lazy loading in your EF Core application.