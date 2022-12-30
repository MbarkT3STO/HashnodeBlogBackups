# How to Create and use a Custom Middleware in ASP.Net Core

One of the key features of [ASP.NET](http://ASP.NET) Core is its use of middleware, which is a set of components that handle requests and responses in the application pipeline. Middleware is responsible for tasks such as logging, authentication, and authorization, and it allows developers to customize the request-response pipeline to meet their specific needs.

# **Creating a Custom Middleware in** [**ASP.NET**](http://ASP.NET) **Core**

Creating a custom middleware in [ASP.NET](http://ASP.NET) Core is straightforward and involves just a few steps. Here's how to do it:

1. Create a new [ASP.NET](http://ASP.NET) Core web application using Visual Studio or the dotnet command-line tool.
    
2. In the `Startup` class, locate the `Configure` method. This method is where you will add your custom middleware to the pipeline.
    
3. Define a new middleware class that will handle requests and responses. This class should implement the `IMiddleware` interface, which has a single `InvokeAsync` method.
    

Here's an example of a simple middleware class that logs the incoming request and outgoing response:

```csharp
public class LoggingMiddleware : IMiddleware
{
    private readonly ILogger _logger;

    public LoggingMiddleware(ILogger<LoggingMiddleware> logger)
    {
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        _logger.LogInformation($"Received request for {context.Request.Path}");
        await next(context);
        _logger.LogInformation($"Finished handling request for {context.Request.Path}");
    }
}
```

1. In the `Configure` method, add the middleware to the pipeline using the `UseMiddleware` extension method. For example:
    

```csharp
app.UseMiddleware<LoggingMiddleware>();
```

This will add the `LoggingMiddleware` to the pipeline, and it will be executed for every incoming request.

# **Using Dependency Injection with Custom Middleware**

[ASP.NET](http://ASP.NET) Core includes built-in support for dependency injection, which allows you to easily inject services into your middleware class. For example, you can inject the `ILogger` service into the `LoggingMiddleware` class, as shown in the example above. This allows you to easily use the logging service in your middleware without having to manually resolve it.

To use dependency injection with your custom middleware, you need to add it to the service container in the `ConfigureServices` method of the `Startup` class. For example:

```csharp
services.AddTransient<LoggingMiddleware>();
```

# **Ordering Middleware in the Pipeline**

The order in which middleware is added to the pipeline is important, as it determines the order in which the middleware will be executed. In general, you should add middleware that performs tasks such as logging and error handling at the beginning of the pipeline, and middleware that performs tasks such as authentication and authorization towards the end of the pipeline.

For example, consider the following pipeline configuration:

```csharp
app.UseMiddleware<ErrorHandlingMiddleware>();
app.UseMiddleware<LoggingMiddleware>();
app.UseMiddleware<AuthenticationMiddleware>();
app.UseMiddleware<AuthorizationMiddleware>();
```

In this configuration, the `ErrorHandlingMiddleware` will be executed first, followed by the `LoggingMiddleware`. If the request passes through the error handling and logging middleware without any issues, it will then be passed to the `AuthenticationMiddleware` for authentication, and finally to the `AuthorizationMiddleware` for authorization.

It is important to note that the order of middleware can also be controlled using branch routing. This allows you to specify different middleware pipelines for different routes, allowing for more granular control over the request-response pipeline.

## Real-World Examples

### Authentication Middleware

A common use case for custom middleware is to implement authentication and authorization in an [ASP.NET](http://ASP.NET) Core application. Here's an example of how you might create custom middleware to handle authentication and authorization using JSON Web Tokens (JWTs):

1. Create a new [ASP.NET](http://ASP.NET) Core web application using Visual Studio or the dotnet command-line tool.
    
2. In the `Startup` class, locate the `ConfigureServices` method and add the necessary services for JWT authentication. For example:
    

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidIssuer = Configuration["Jwt:Issuer"],
                ValidAudience = Configuration["Jwt:Audience"],
                IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(Configuration["Jwt:Key"]))
            };
        });
```

1. In the `Startup` class, locate the `Configure` method and add the authentication middleware to the pipeline. For example:
    

```csharp
app.UseAuthentication();
```

This will add the JWT authentication middleware to the pipeline, and it will be executed for every incoming request.

1. Create a custom authorization middleware class that will handle authorization for specific routes or controllers. This class should implement the `IMiddleware` interface, and it should use the `Authorize` attribute to check the authorization status of the current user. Here's an example:
    

```csharp
public class CustomAuthorizationMiddleware : IMiddleware
{
    private readonly IAuthorizationService _authorizationService;

    public CustomAuthorizationMiddleware(IAuthorizationService authorizationService)
    {
        _authorizationService = authorizationService;
    }

    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        if (!context.User.Identity.IsAuthenticated)
        {
            context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
            return;
        }

        if (!await _authorizationService.AuthorizeAsync(context.User, null, "customPolicy"))
        {
            context.Response.StatusCode = (int)HttpStatusCode.Forbidden;
            return;
        }

        await next(context);
    }
}
```

1. In the `Configure` method, add the custom authorization middleware to the pipeline using the `UseMiddleware` extension method. For example:
    

```csharp
app.UseMiddleware<CustomAuthorizationMiddleware>();
```

This will add the custom authorization middleware to the pipeline, and it will be executed for every incoming request after the authentication middleware.

With this setup, incoming requests will first be authenticated using JWTs, and then authorized using the custom authorization middleware. This allows you to easily control access to specific routes or controllers in your application based on the authorization status of the current user.

### Error Handling Middleware

Another common use case for custom middleware is to implement error handling in an [ASP.NET](http://ASP.NET) Core application. Here's an example of how you might create custom middleware to handle errors and return custom error responses to the client:

1. Create a new [ASP.NET](http://ASP.NET) Core web application using Visual Studio or the dotnet command-line tool.
    
2. Create a custom error handling middleware class that will handle errors and return custom error responses to the client. This class should implement the `IMiddleware` interface, and it should use the `IExceptionHandlerFeature` interface to access information about the exception that was thrown. Here's an example:
    

```csharp
public class ErrorHandlingMiddleware : IMiddleware
{
    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        try
        {
            await next(context);
        }
        catch (Exception ex)
        {
            if (context.Response.HasStarted)
            {
                throw;
            }

            context.Response.Clear();
            context.Response.StatusCode = (int)HttpStatusCode.InternalServerError;
            context.Response.ContentType = "application/json";
            var error = new
            {
                Message = "An error occurred while processing your request.",
                Exception = ex.Message
            };
            await context.Response.WriteAsync(JsonConvert.SerializeObject(error));
        }
    }
}
```

1. In the `Configure` method, add the error handling middleware to the pipeline using the `UseMiddleware` extension method. For example:
    

```csharp
app.UseMiddleware<ErrorHandlingMiddleware>();
```

This will add the error handling middleware to the pipeline, and it will be executed for every incoming request.

With this setup, any exceptions that are thrown during the processing of the request will be caught by the error handling middleware, and a custom error response will be returned to the client. This allows you to handle errors in a consistent and controlled manner, and it helps to ensure that the client receives a useful error message rather than a default error page.

# **Conclusion**

Custom middleware is a powerful feature of [ASP.NET](http://ASP.NET) Core that allows you to customize the request-response pipeline to meet your specific needs. By creating your own middleware and adding it to the pipeline, you can easily perform tasks such as logging, authentication, and authorization in your application. Using dependency injection with custom middleware also allows you to easily inject services into your middleware class, making it easier to use these services within your middleware. By carefully ordering your middleware in the pipeline, you can ensure that the middleware is executed in the correct order to meet the needs of your application.