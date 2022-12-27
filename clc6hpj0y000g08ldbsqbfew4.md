# Action Filters in ASP.NET Core

[ASP.NET](http://ASP.NET) Core action filters are attributes that can be applied to a controller action or a controller. They allow you to modify the way an action is executed and can be used to perform tasks such as logging, caching, and authorization.

## **Types of Action Filters**

There are four types of action filters in [ASP.NET](http://ASP.NET) Core:

* **Authorization filters**: These filters are executed before the action method is invoked and can be used to implement authorization policies.
    
* **Resource filters**: These filters are also executed before the action method is invoked and can be used to manage resources such as database connections.
    
* **Action filters**: These filters are executed before and after the action method is invoked and can be used to modify the action's execution or the result that is returned.
    
* **Exception filters**: These filters are executed when an exception is thrown during the execution of the action method and can be used to handle or log the exception.
    

## **Creating a Custom Action Filter**

To create a custom action filter, you need to create a class that derives from the `ActionFilterAttribute` class and overrides one or more of the following methods:

* `OnActionExecuting(ActionExecutingContext context)`: This method is executed before the action method is invoked and can be used to modify the action's execution or the result that is returned.
    
* `OnActionExecuted(ActionExecutedContext context)`: This method is executed after the action method is invoked and can be used to modify the result that is returned.
    
* `OnResultExecuting(ResultExecutingContext context)`: This method is executed before the result of the action is executed and can be used to modify the result that is returned.
    
* `OnResultExecuted(ResultExecutedContext context)`: This method is executed after the result of the action is executed and can be used to modify the result that is returned.
    

Here is an example of a custom action filter that logs the execution time of an action:

```csharp
public class LogExecutionTimeFilter : ActionFilterAttribute
{
    private Stopwatch _stopwatch;

    public override void OnActionExecuting(ActionExecutingContext context)
    {
        _stopwatch = Stopwatch.StartNew();
    }

    public override void OnActionExecuted(ActionExecutedContext context)
    {
        _stopwatch.Stop();
        Console.WriteLine($"Action '{context.ActionDescriptor.DisplayName}' executed in {_stopwatch.ElapsedMilliseconds}ms.");
    }
}
```

To use the custom action filter, you can apply it to a controller action or a controller using the `[LogExecutionTimeFilter]` attribute:

```csharp
[LogExecutionTimeFilter]
public IActionResult Index()
{
    // Action code goes here
    return View();
}
```

## **Ordering Action Filters**

By default, action filters are executed in the following order:

1. Authorization filters
    
2. Resource filters
    
3. Action filters
    
4. Result filters
    

You can modify the order in which action filters are executed by setting the `Order` property of the `ActionFilterAttribute` class. Filters with a lower `Order` value are executed before filters with a higher `Order` value.

```csharp
[LogExecutionTimeFilter(Order = 1)]
[AddHeaderFilter(Order = 2)]
public IActionResult Index()
{
    // Action code goes here
    return View();
}
```

## **Cancelling Action Execution**

You can cancel the execution of an action by setting the `Result` property of the `ActionExecutingContext` or `ActionExecutedContext` object in the `OnActionExecuting` or `OnActionExecuted` method of an action filter.

Here is an example of an action filter that cancels the execution of an action if the `Cancel` query string parameter is set to `true`:

```csharp
public class CancelActionFilter : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (context.HttpContext.Request.Query.ContainsKey("cancel"))
        {
            context.Result = new ContentResult
            {
                Content = "Action cancelled by CancelActionFilter."
            };
        }
    }
}
```

## Examples

Here are some examples of real-world scenarios where action filters might be used:

* **Authorization**: You can use an authorization filter to enforce policies that determine whether a user is allowed to access a particular action. For example, you might have an action that allows users to view sensitive data, and you want to ensure that only authenticated users with the appropriate permissions are allowed to access it.
    
* **Caching**: You can use an action filter to cache the results of an action, so that subsequent requests for the same data can be served more quickly. This can be particularly useful for actions that retrieve data from a database or other external source, as it reduces the number of expensive database queries that need to be made.
    
* **Logging**: You can use an action filter to log information about the execution of an action, such as the time it took to execute, the parameters passed to it, and the result that was returned. This can be useful for debugging and performance analysis.
    
* **Error handling**: You can use an exception filter to handle errors that occur during the execution of an action. For example, you might want to log the error, display a friendly error message to the user, or redirect the user to a different page.
    
* **Input validation**: You can use an action filter to validate the input passed to an action, and return an error if the input is invalid. This can help prevent malicious or invalid data from being passed to your application, and ensure that it is only processing valid data.
    
* **Response manipulation**: You can use an action filter to modify the response that is returned by an action. For example, you might want to add headers to the response, or modify the content of the response before it is sent to the client.
    

### Logging

Here is an example of an action filter that logs the execution time of an action:

```csharp
public class LogExecutionTimeFilter : ActionFilterAttribute
{
    private Stopwatch _stopwatch;

    public override void OnActionExecuting(ActionExecutingContext context)
    {
        _stopwatch = Stopwatch.StartNew();
    }

    public override void OnActionExecuted(ActionExecutedContext context)
    {
        _stopwatch.Stop();
        Console.WriteLine($"Action '{context.ActionDescriptor.DisplayName}' executed in {_stopwatch.ElapsedMilliseconds}ms.");
    }
}
```

To use the custom action filter, you can apply it to a controller action or a controller using the `[LogExecutionTimeFilter]` attribute:

```csharp
[LogExecutionTimeFilter]
public IActionResult Index()
{
    // Action code goes here
    return View();
}
```

Every time the `Index` action is executed, the execution time will be logged to the console. You can modify the `OnActionExecuted` method to log the execution time to a file or a database, or to send it to a logging service like Elasticsearch or Loggly.

### Caching

Here is an example of an action filter that caches the results of an action for a specified number of seconds:

```csharp
public class CacheActionFilter : ActionFilterAttribute
{
    private readonly int _duration;

    public CacheActionFilter(int duration)
    {
        _duration = duration;
    }

    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (context.HttpContext.Request.Method == "GET")
        {
            var cacheKey = GenerateCacheKey(context.HttpContext.Request);
            var cacheEntry = MemoryCache.Default.Get(cacheKey);
            if (cacheEntry != null)
            {
                context.Result = new ContentResult
                {
                    Content = cacheEntry.ToString()
                };
            }
        }
    }

    public override void OnActionExecuted(ActionExecutedContext context)
    {
        if (context.HttpContext.Request.Method == "GET" && context.Result is ContentResult)
        {
            var cacheKey = GenerateCacheKey(context.HttpContext.Request);
            var cacheEntry = (context.Result as ContentResult).Content;
            MemoryCache.Default.Set(cacheKey, cacheEntry, DateTime.Now.AddSeconds(_duration));
        }
    }

    private string GenerateCacheKey(HttpRequest request)
    {
        var keyBuilder = new StringBuilder();
        keyBuilder.Append(request.Path);
        foreach (var query in request.Query)
        {
            keyBuilder.AppendFormat("{0}={1}&", query.Key, query.Value);
        }
        return keyBuilder.ToString();
    }
}
```

To use the custom action filter, you can apply it to a controller action or a controller using the `[CacheActionFilter(duration)]` attribute, where `duration` is the number of seconds for which the result should be cached:

```csharp
[CacheActionFilter(3600)]
public IActionResult Index()
{
    // Action code goes here
    return View();
}
```

This action filter will cache the result of the `Index` action for one hour (3600 seconds). Subsequent requests for the same action within that time period will be served from the cache, rather than executing the action again.

**Note:** In this example, I used the `MemoryCache` class to store the cache entries in memory. You can also use a distributed cache like Redis or SQL Server to store the cache entries if you need to share the cache across multiple servers or processes.

### **Input Validation**

Here is an example of an action filter that validates the input passed to an action using data annotations:

```csharp
public class ValidateModelStateFilter : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.ModelState.IsValid)
        {
            context.Result = new BadRequestObjectResult(context.ModelState);
        }
    }
}
```

To use the custom action filter, you can apply it to a controller action or a controller using the `[ValidateModelStateFilter]` attribute:

```csharp
[ValidateModelStateFilter]
public IActionResult Create(CreateModel model)
{
    // Action code goes here
    return View();
}
```

In this example, the `Create` action expects a `CreateModel` object as input. You can use data annotations on the `CreateModel` class to specify validation rules for the input, such as required fields, minimum and maximum values, and regular expressions.

For example:

```csharp
public class CreateModel
{
    [Required]
    public string Name { get; set; }

    [Range(1, 100)]
    public int Age { get; set; }

    [EmailAddress]
    public string Email { get; set; }
}
```

If the input does not meet the validation rules, the action filter will return a `400 Bad Request` response to the client with the list of validation errors.

Note: In this example, I used the `BadRequestObjectResult` class to return the validation errors. You can also use other classes like `BadRequestResult` or `BadRequestActionResult` to return a `400 Bad Request` response.

## **Conclusion**

Action filters are a powerful feature of [ASP.NET](http://ASP.NET) Core that allow you to modify the execution of actions and the results that are returned. They can be used for tasks such as logging, caching, authorization, and resource management. With the ability to create custom action filters and control the order in which they are executed, you have a lot of flexibility to customize the behavior of your application.