# Understanding the IOptionsSnapshot<T> in ASP.NET Core

In our previous article, we discussed [the usage of the `IOptions<T>` interface in ASP.NET Core](https://mbarkt3sto.hashnode.dev/understanding-the-ioptionst-in-aspnet-core), which allows for easy access to configuration options defined in `appsettings.json` or other sources. However, there is another interface that can also be used for this purpose - the `IOptionsSnapshot<T>`.

## Difference Between `IOptions<T>` and `IOptionsSnapshot<T>`

The main difference between `IOptions<T>` and `IOptionsSnapshot<T>` is that the latter provides a snapshot of the configuration options at the time of injection. This means that any changes made to the configuration after the service has been resolved will not be reflected in the `IOptionsSnapshot<T>` instance.

Let's take a look at an example of how to use `IOptionsSnapshot<T>` in a .NET Core application.

## **Defining a Configuration Class**

First, we need to define a class that will hold our configuration options. Let's call it `MyOptions` and give it a couple of properties:

```csharp
public class MyOptions
{
    public string Option1 { get; set; }
    public int Option2 { get; set; }
}
```

## **Configuring the Options in appsettings.json**

Next, we need to configure our options in the `appsettings.json` file. We can do this by adding a section called `MyOptions` and defining the properties within it:

```json
{
  "MyOptions": {
    "Option1": "value1",
    "Option2": 42
  }
}
```

## **Registering the Options in Startup.cs**

In the `Startup.cs` file, we need to register our options with the service collection. We can do this by using the `services.Configure<T>()` method in the `ConfigureServices()` method:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MyOptions>(Configuration.GetSection("MyOptions"));
}
```

Note that we are using the `Configuration.GetSection("MyOptions")` method to retrieve the `MyOptions` section from the `appsettings.json` file.

## **Injecting the Options in a Service/Controller**

Finally, we can inject the `IOptionsSnapshot<MyOptions>` interface into a service and use it to access the configuration options:

```csharp
public class MyService
{
    private readonly IOptionsSnapshot<MyOptions> _options;

    public MyService(IOptionsSnapshot<MyOptions> options)
    {
        _options = options;
    }

    public void DoSomething()
    {
        Console.WriteLine(_options.Value.Option1); // prints "value1"
        Console.WriteLine(_options.Value.Option2); // prints 42
    }
}
```

As we can see, the `IOptionsSnapshot<T>` interface allows us to easily access the configuration options defined in `appsettings.json` and other sources. It also provides a snapshot of the options at the time of injection, which can be useful in certain scenarios.

In conclusion, `IOptionsSnapshot<T>` is a useful interface in [ASP.NET](http://ASP.NET) Core that provides a snapshot of the configuration options at the time of injection. It can be used in conjunction with `appsettings.json` or other sources to access and utilize these options in our services and controllers. It is important to note that any changes made to the configuration after the service has been resolved will not be reflected in the `IOptionsSnapshot<T>` instance. It's always best to use `IOptionsSnapshot<T>` when you want to make sure that the configuration options that you are using are the same throughout the lifetime of the service, it's a great way to ensure that your service is using the same configuration options that were used during the service's initialization.

## Real-World Examples

### **Mail Service**

A real-world example of using `IOptionsSnapshot<T>` could be in a web application where we have different environments (development, staging, production) and we want to make sure that our services are using the correct configuration options for each environment.

Let's say we have a `MailService` that sends emails to users, and we have different mail server configurations for each environment. We can create a `MailOptions` class that holds these configurations:

```csharp
public class MailOptions
{
    public string Server { get; set; }
    public int Port { get; set; }
    public string UserName { get; set; }
    public string Password { get; set; }
}
```

We can then configure these options in the `appsettings.json` file for each environment:

```json
{
  "MailOptions": {
    "Server": "smtp.example.com",
    "Port": 587,
    "UserName": "noreply@example.com",
    "Password": "secretpassword"
  }
}
```

In the `Startup.cs` file, we can register the options with the service collection and configure it to use the correct options based on the environment:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MailOptions>(Configuration.GetSection("MailOptions"));

    if (Environment.IsDevelopment())
    {
        services.Configure<MailOptions>(options =>
        {
            options.Server = "dev-smtp.example.com";
            options.Port = 25;
            options.UserName = "dev-noreply@example.com";
            options.Password = "dev-secretpassword";
        });
    }
}
```

We can then inject the `IOptionsSnapshot<MailOptions>` interface into the `MailService` and use it to access the correct mail server configurations:

```csharp
public class MailService
{
    private readonly IOptionsSnapshot<MailOptions> _options;

    public MailService(IOptionsSnapshot<MailOptions> options)
    {
        _options = options;
    }

    public void SendEmail(string to, string subject, string body)
    {
        // Connect to the mail server using the options
        var server = _options.Value.Server;
        var port = _options.Value.Port;
        var userName = _options.Value.UserName;
        var password = _options.Value.Password;

        // Send the email
    }
}
```

With this example, we can ensure that our `MailService` is using the correct mail server configurations for each environment, without having to worry about any changes made to the configuration after the service has been resolved.

### **Client Options**

Another real-world example of using `IOptionsSnapshot<T>` could be in a web application that serves multiple clients. Each client may have different configurations, such as different database connection strings, different security settings, etc.

We can create a `ClientOptions` class that holds the configurations for each client:

```csharp
public class ClientOptions
{
    public string Name { get; set; }
    public string ConnectionString { get; set; }
    public bool IsSecure { get; set; }
}
```

We can configure these options in the `appsettings.json` file for each client:

```json
{
  "ClientOptions": {
    "Client1": {
      "Name": "Client 1",
      "ConnectionString": "Server=client1-db.example.com;Database=client1;User Id=client1;Password=secretpassword1;",
      "IsSecure": true
    },
    "Client2": {
      "Name": "Client 2",
      "ConnectionString": "Server=client2-db.example.com;Database=client2;User Id=client2;Password=secretpassword2;",
      "IsSecure": false
    }
  }
}
```

In the `Startup.cs` file, we can register the options with the service collection and configure it to use the correct options based on the client:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<ClientOptions>(Configuration.GetSection("ClientOptions"));

    services.Configure<ClientOptions>(options =>
    {
        var clientName = GetClientName();
        options.Value = options.Get(clientName);
    });
}
```

The `GetClientName()` method mentioned in the above example is a hypothetical method that is used to determine which client the application is currently serving. This method could be implemented in various ways depending on the specific requirements of the application.

For example, it could be based on a query parameter passed in the URL, a cookie value, or a header value in the HTTP request. It could also be based on the domain name of the request, or it could be hard-coded in the application.

```csharp
private string GetClientName()
{
    // Example 1: Get client name from query parameter
    var clientName = HttpContext.Request.Query["client"].FirstOrDefault();

    // Example 2: Get client name from cookie value
    var clientName = HttpContext.Request.Cookies["client"];

    // Example 3: Get client name from header value
    var clientName = HttpContext.Request.Headers["client"];

    // Example 4: Get client name from domain name
    var clientName = HttpContext.Request.Host.Host;

    // Example 5: Hard-coded client name
    var clientName = "Client1";

    return clientName;
}
```

We can then inject the `IOptionsSnapshot<ClientOptions>` interface into a service and use it to access the correct client configurations:

```csharp
public class ClientService
{
    private readonly IOptionsSnapshot<ClientOptions> _options;

    public ClientService(IOptionsSnapshot<ClientOptions> options)
    {
        _options = options;
    }

    public void DoSomething()
    {
        var clientName = _options.Value.Name;
        var connectionString = _options.Value.ConnectionString;
        var isSecure = _options.Value.IsSecure;

        // Use the client configurations
    }
}
```

In this example, we can ensure that our `ClientService` is using the correct configurations for each client, without having to worry about any changes made to the configuration after the service has been resolved.

`IOptionsSnapshot<T>` is a great way to ensure that your service is using the same configuration options that were used during the service's initialization.