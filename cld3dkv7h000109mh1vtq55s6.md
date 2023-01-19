# Understanding IOptionsMonitor<T> in ASP.NET Core

[ASP.NET](http://ASP.NET) Core provides a powerful configuration system that allows developers to easily manage application settings. One of the key components of this system is the `IOptionsMonitor<T>` interface, which provides a way to access and monitor configuration options of a specific type `T`. In this article, we will explore the basics of `IOptionsMonitor<T>`, including how to use it in an [ASP.NET](http://ASP.NET) Core application, and how it differs from the related `IOptionsSnapshot<T>` interface.

## **What is IOptionsMonitor&lt;T&gt;?**

`IOptionsMonitor<T>` is an interface that allows you to access and monitor configuration options of a specific type `T`. It is typically used to access application-specific settings that are defined in a configuration file, such as `appsettings.json`. `IOptionsMonitor<T>` provides a way to retrieve the current value of the options, as well as register for notifications when the options change.

## **Using IOptionsMonitor&lt;T&gt; in** [**ASP.NET**](http://ASP.NET) **Core**

To use `IOptionsMonitor<T>` in an [ASP.NET](http://ASP.NET) Core application, you first need to define a POCO (Plain Old CLR Object) class that represents the options you want to access. This class should have properties that match the keys in your configuration file. For example, if you have the following `appsettings.json` file:

```json
{
  "MyOptions": {
    "Option1": "value1",
    "Option2": true
  }
}
```

You can define a POCO class like this:

```csharp
public class MyOptions
{
    public string Option1 { get; set; }
    public bool Option2 { get; set; }
}
```

You then need to register the options in the `ConfigureServices` method of your `Startup` class, using the `services.Configure<T>()` method. This method binds the options defined in the configuration file to the POCO class you defined.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MyOptions>(Configuration.GetSection("MyOptions"));
    //...
}
```

Once the options are registered, you can use `IOptionsMonitor<T>` to access the current value of the options. To do this, you need to inject an instance of `IOptionsMonitor<T>` into your controller or service.

```csharp
public class MyController : Controller
{
    private readonly IOptionsMonitor<MyOptions> _options;

    public MyController(IOptionsMonitor<MyOptions> options)
    {
        _options = options;
    }

    public IActionResult Index()
    {
        var option1 = _options.CurrentValue.Option1;
        var option2 = _options.CurrentValue.Option2;
        //...
    }
}
```

# **Changing the Original Value in appsettings.json**

In addition to changing the values of options during runtime, you can also change the original values in the appsettings.json file.

To do this, you need to update the value in the appsettings.json file and then trigger a reload of the configuration.

Here is an example of how you can change the value of an option in the appsettings.json file and then reload the configuration.

```csharp
public class MyController : Controller
{
    private readonly IConfiguration _config;

    public MyController(IConfiguration config)
    {
        _config = config;
    }

    public IActionResult UpdateOption()
    {
        // Update the value in the appsettings.json file
        _config["MyOptions:Option1"] = "newValue";
        
        // Reload the configuration
        _config.Reload();
        return Ok();
    }
}
```

It's important to note that not all configuration providers support dynamic reloading of the configuration. For example, the built-in `JsonConfigurationProvider` does not support dynamic reloading. In such cases, you would need to use a third-party library such as `Microsoft.Extensions.Configuration.AzureAppConfiguration` which provides support for dynamic reloading of configuration from Azure App Configuration.

It is also important to note that when you change the values in the appsettings.json file, the changes will only take effect when the application is restarted. If you want to make changes in the appsettings.json during runtime, you need to use a different configuration provider such as Azure App Configuration or Consul.

## **Differences between IOptionsMonitor&lt;T&gt; and IOptionsSnapshot&lt;T&gt;**

`IOptionsMonitor<T>` and `IOptionsSnapshot<T>` are similar interfaces, but there are some important differences between them.

`IOptionsSnapshot<T>` allows you to access the options at a specific point in time and does not provide a way to register for notifications when the options change. This is suitable for scenarios where the options are read once and not expected to change during the lifetime of the application.

On the other hand, `IOptionsMonitor<T>` provides a way to access the current value of the options, as well as register for notifications when the options change. This is useful for scenarios where the options may change during the lifetime of the application, such as when using a configuration provider that supports dynamic reloading of the configuration.

It's important to note that in the previous article, we talked about [the usage of IOptions&lt;T&gt;](https://mbarkt3sto.hashnode.dev/understanding-the-ioptionst-in-aspnet-core) and in the other article we covered [the IOptionsSnapshot&lt;T&gt;](https://mbarkt3sto.hashnode.dev/understanding-the-ioptionssnapshott-in-aspnet-core)

In conclusion, `IOptionsMonitor<T>` is a powerful and flexible interface that allows you to easily manage application settings in [ASP.NET](http://ASP.NET) Core. By understanding how to use it, you can take full advantage of the configuration system provided by [ASP.NET](http://ASP.NET) Core and make your application more configurable and adaptable to changing requirements.