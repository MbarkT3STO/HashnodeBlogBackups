# What is CORS in ASP.NET Core?

Cross-Origin Resource Sharing (CORS) is a security feature implemented by web browsers that blocks web pages from making requests to a different domain than the one that served the web page. In other words, it prevents a malicious website from making unauthorized requests to a different domain, potentially stealing sensitive data.

[ASP.NET](http://ASP.NET) Core allows developers to easily configure CORS for their web applications. This can be done by using the `Microsoft.AspNetCore.Cors` package, which provides a middleware for handling CORS requests.

# **Enabling CORS in** [**ASP.NET**](http://ASP.NET) **Core**

To enable CORS in an [ASP.NET](http://ASP.NET) Core web application, you can use the `UseCors` method in the `Startup` class. The following example shows how to enable CORS for all origins, headers, and methods:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors();
}

public void Configure(IApplicationBuilder app)
{
    app.UseCors(options => options.AllowAnyOrigin().AllowAnyHeader().AllowAnyMethod());

    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

# **Configuring CORS in** [**ASP.NET**](http://ASP.NET) **Core**

You can also configure CORS to only allow specific origins, headers, and methods. The following example shows how to allow requests from [`http://example.com`](http://example.com) and [`http://www.example.com`](http://www.example.com), with the `Content-Type` header, and the `GET` and `POST` methods:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors();
}

public void Configure(IApplicationBuilder app)
{
    app.UseCors(options => options
        .WithOrigins("http://example.com", "http://www.example.com")
        .WithHeaders(HeaderNames.ContentType)
        .WithMethods("GET", "POST")
    );
    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

# **Conclusion**

CORS is a security feature that prevents malicious websites from making unauthorized requests to a different domain. [ASP.NET](http://ASP.NET) Core provides an easy way to configure CORS for web applications by using the `Microsoft.AspNetCore.Cors` package. Developers can enable CORS for all origins, headers, and methods or configure CORS to only allow specific origins, headers, and methods.