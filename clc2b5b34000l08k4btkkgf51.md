# What is the Difference between ViewBag and ViewData in ASP.NET Core MVC

[ASP.NET](http://ASP.NET) Core is a popular framework for building web applications using the .NET platform. Within [ASP.NET](http://ASP.NET) Core, there are two key mechanisms for transferring data from a controller to a view: ViewBag and ViewData. While these mechanisms serve a similar purpose, they have some notable differences that developers should be aware of.

## **ViewBag**

ViewBag is a dynamic property that is available on the base controller class in [ASP.NET](http://ASP.NET) Core. It allows you to pass data from a controller to a view by assigning values to properties on the ViewBag object. For example:

```csharp
public IActionResult Index()
{
    ViewBag.Message = "Hello, World!";
    return View();
}
```

In the view, you can access the value of the `Message` property using the `@ViewBag` object:

```csharp
@ViewBag.Message
```

One advantage of ViewBag is that it is dynamically typed, which means that you don't need to specify the type of the data being passed. This can make it easier to use in some cases, as you don't need to worry about type casting.

## **ViewData**

Like ViewBag, ViewData is a mechanism for passing data from a controller to a view. However, ViewData is a dictionary object that stores key-value pairs, rather than a dynamic property.

Here is an example of using ViewData to pass data from a controller to a view:

```csharp
public IActionResult Index()
{
    ViewData["Message"] = "Hello, World!";
    return View();
}
```

In the view, you can access the value of the `Message` key using the `ViewData` object:

```csharp
@ViewData["Message"]
```

Unlike ViewBag, ViewData is strongly typed, which means that you need to specify the type of the data being passed. This can make it more error-prone, as you need to be careful about type casting. However, it can also make your code easier to maintain, as the type of the data is explicit and can be checked at compile time.

## **Summary**

In summary, ViewBag and ViewData are both mechanisms for transferring data from a controller to a view in [ASP.NET](http://ASP.NET) Core. ViewBag is a dynamic property that is easy to use, but not strongly typed, while ViewData is a dictionary object that is strongly typed but requires more explicit type casting.

Which one you choose will depend on your specific needs and preferences. Both ViewBag and ViewData can be useful in different situations, so it's important to understand the differences between them and how they work.