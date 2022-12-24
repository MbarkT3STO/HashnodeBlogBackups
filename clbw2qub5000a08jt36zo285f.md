# How to Read Action Method Parameters using IActionFilter in ASP.NET Core MVC

## Introduction to IActionFilter in [ASP.NET](http://ASP.NET) Core MVC

[ASP.NET](http://ASP.NET) Core MVC is a powerful framework for building web applications. One of the features of [ASP.NET](http://ASP.NET) Core MVC is the ability to use filters to modify the behavior of action methods. IActionFilter is an interface that can be implemented to create a filter that runs before or after an action method is executed.

In this article, we will learn how to read action method parameters using IActionFilter in [ASP.NET](http://ASP.NET) Core MVC.

## Implementing IActionFilter

To implement IActionFilter, we need to create a class that implements the interface and overrides the OnActionExecuting and OnActionExecuted methods. The OnActionExecuting method is called before the action method is executed, and the OnActionExecuted method is called after the action method is executed.

**Here is an example of a filter that implements IActionFilter:**

```csharp
public class MyActionFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        // Code to run before the action method is executed
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        // Code to run after the action method is executed
    }
}
```

## Reading Action Method Parameters

To read action method parameters using IActionFilter, we can access the ActionArguments property of the ActionExecutingContext object in the OnActionExecuting method. The ActionArguments property is a dictionary that contains the names and values of the parameters of the action method.

**Here is examples of how to read action method parameters using IActionFilter:**

### Using ActionArguments

```csharp
public class MyActionFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        var parameter1 = context.ActionArguments["parameter1"];
        var parameter2 = context.ActionArguments["parameter2"];
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        // Code to run after the action method is executed
    }
}
```

In this example, we are accessing the values of the "parameter1" and "parameter2" parameters of the action method.

Using IActionFilter in an Action Method

To use an IActionFilter in an action method, we need to apply the filter to the action method using the \[Filter\] attribute.

### Using TryGetValue method

Using the TryGetValue method of the ActionArguments property of the ActionExecutingContext object can be useful when we want to read an optional parameter of an action method. The TryGetValue method attempts to get the value of the specified parameter from the ActionArguments dictionary, and returns a boolean value indicating whether the value was found.

Here is an example of how to use the TryGetValue method to read an optional parameter of an action method:

```csharp
public class MyActionFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        string parameter1;
        if (context.ActionArguments.TryGetValue("parameter1", out parameter1))
        {
            // parameter1 value was found
        }
        else
        {
            // parameter1 value was not found
        }
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        // Code to run after the action method is executed
    }
}
```

In this example, we are using the TryGetValue method to try to get the value of the "parameter1" parameter of the action method. If the value is found, the parameter1 variable is set to the value of the parameter, and the code in the if block is executed. If the value is not found, the parameter1 variable is not set and the code in the else block is executed.

This can be useful when we have an action method with optional parameters that may or may not be provided by the client. By using the TryGetValue method, we can handle both cases and take appropriate action based on whether the parameter value was found or not.

**Here is an example of how to apply an IActionFilter to an action method:**

```csharp
[Filter(typeof(MyActionFilter))]
public IActionResult MyAction(string parameter1, string parameter2)
{
    // Action method code
    return View();
}
```

In this example, we are applying the MyActionFilter filter to the MyAction action method.

## Real examples

### Logger Filter

```csharp
public class LogActionFilter : IActionFilter
{
    private readonly ILogger _logger;

    public LogActionFilter(ILogger<LogActionFilter> logger)
    {
        _logger = logger;
    }

    public void OnActionExecuting(ActionExecutingContext context)
    {
        // Log the action method name and parameters
        _logger.LogInformation("Executing action method {ActionName} with parameters:", context.ActionDescriptor.DisplayName);
        foreach (var parameter in context.ActionArguments)
        {
            _logger.LogInformation("{ParameterName}: {ParameterValue}", parameter.Key, parameter.Value);
        }
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        // Code to run after the action method is executed
    }
}
```

In this example, we have created a LogActionFilter filter that logs the name and parameters of the action method before it is executed. The filter uses the ILogger service to log the information, and the ActionArguments property of the ActionExecutingContext object to access the action method parameters.

To use this filter, we can apply it to an action method using the \[Filter\] attribute:

```csharp
[Filter(typeof(LogActionFilter))]
public IActionResult MyAction(string parameter1, int parameter2)
{
    // Action method code
    return View();
}
```

When the MyAction action method is executed, the LogActionFilter filter will log the name and parameters of the action method before the action method code is executed. This can be useful for logging purposes or for debugging.

### Validation Filter

```csharp
public class ValidateActionFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        string parameter1;
        if (context.ActionArguments.TryGetValue("parameter1", out parameter1))
        {
            if (string.IsNullOrEmpty(parameter1))
            {
                context.Result = new BadRequestObjectResult("Parameter1 cannot be empty.");
            }
        }
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        // Code to run after the action method is executed
    }
}
```

In this example, we have created a ValidateActionFilter filter that validates the "parameter1" parameter of the action method. If the parameter is empty or null, the filter sets the Result property of the ActionExecutingContext object to a BadRequestObjectResult object with an error message. This will cause the action method to return a Bad Request response to the client.

To use this filter, we can apply it to an action method using the \[Filter\] attribute:

```csharp
[Filter(typeof(ValidateActionFilter))]
public IActionResult MyAction(string parameter1, int parameter2)
{
    // Action method code
    return View();
}
```

When the MyAction action method is executed, the ValidateActionFilter filter will validate the "parameter1" parameter before the action method code is executed. If the parameter is empty or null, the filter will return a Bad Request response to the client, and the action method code will not be executed. This can be useful for input validation purposes.

## Conclusion

In this article, we learned how to read action method parameters using IActionFilter in [ASP.NET](http://ASP.NET) Core MVC. We saw how to implement IActionFilter and how to access the action method parameters in the OnActionExecuting method. We also saw how to apply an IActionFilter to an action method using the \[Filter\] attribute.

Using IActionFilter, we can modify the behavior of action methods and access the values of their parameters. This can be useful for a variety of purposes, such as logging, validation, and authorization.