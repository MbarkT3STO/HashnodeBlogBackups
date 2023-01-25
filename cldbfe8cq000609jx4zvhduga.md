# Setting up and Using MassTransit in ASP.Net Core

MassTransit is a powerful message-based communication library for building distributed applications using the .NET framework. It provides a simple and flexible way to send and receive messages between different parts of your application, or even between different applications. In this article, we will show you how to set up MassTransit in an [ASP.NET](http://ASP.NET) Core application and use it to send and receive messages using RabbitMQ as the message broker.

## Before Setting up MassTransit

It's important to note that to use MassTransit, it is required to install and set up RabbitMQ. RabbitMQ is a message broker that is used to store and forward messages in a MassTransit application. Without RabbitMQ, MassTransit will not be able to send or receive messages.

We talked before about [How to Setting Up RabbitMQ on Windows?](https://mbarkt3sto.hashnode.dev/how-to-setting-up-rabbitmq-on-windows)

Once you have RabbitMQ set up and running, you can proceed with setting up MassTransit in your [ASP.NET](http://ASP.NET) Core application.

## **Install MassTransit**

To start using MassTransit in your [ASP.NET](http://ASP.NET) Core application, you first need to install the package using NuGet. Open the Package Manager Console in Visual Studio and run the following commands:

```http
Install-Package MassTransit
Install-Package MassTransit.RabbitMQ
```

## **Setting up the application as a publisher**

### Configure MassTransit

To set up your application as a publisher, you need to configure MassTransit to use RabbitMQ as the message broker and provide the connection details such as the hostname, port, username, and password.

First, you need to add the following code to the `Startup.cs` file to configure MassTransit:

```csharp
services.AddMassTransit(x =>
{
    x.AddBus(provider => Bus.Factory.CreateUsingRabbitMq(config =>
    {
        config.Host("rabbitmq://localhost", h =>
        {
            h.Username("guest");
            h.Password("guest");
        });
    }));
});
```

**Explication:**

This code is adding MassTransit to [ASP.NET](http://ASP.NET) Core application's service collection using the `services.AddMassTransit(x => { ... })` method. This method takes a lambda function as a parameter, which is used to configure MassTransit.

The line `x.AddBus(provider => Bus.Factory.CreateUsingRabbitMq(config => { ... }))` is adding a bus instance to the service collection. The `Bus.Factory.CreateUsingRabbitMq` method is used to create a new bus instance that uses RabbitMQ as the message broker. The `config` parameter passed to this method is a configuration callback that allows you to configure various aspects of the bus, such as the host settings.

The [`config.Host`](http://config.Host)`("rabbitmq://`[`localhost`](http://localhost)`", h => { ... })` line is used to configure the host that the bus will connect to. The first parameter is the connection string and the second parameter is a callback that allows you to configure the host settings, in this case, the username and password. This line of code is specifying the host to be "rabbitmq://[localhost](http://localhost)" and the username and password to be "guest".

This configuration allows the application to use MassTransit to send and receive messages using RabbitMQ as a message broker. The bus is configured to use RabbitMQ and the username and password are provided, allowing it to connect to the RabbitMQ server.

**Or:**

```csharp
services.AddMassTransit(x => x.UsingRabbitMq());
```

This code is a simplified configuration of MassTransit to use RabbitMQ as the message transport.

You can also create a separate class to configure MassTransit, and call this class in Startup.cs, it's up to your preference.

### Create a Message/Data Class

Then, you need to create a message class that represents the message/data you want to send. For example:

```csharp
public class MyMessage
{
    public string Text { get; set; }
}
```

You can then use the `IBus` interface to send a message from your controller or other parts of your application.

```csharp
[ApiController]
[Route("[controller]")]
public class MyController : ControllerBase
{
    private readonly IBus _bus;
    public MyController(IBus bus)
    {
        _bus = bus;
    }
    // Other Controller's logic goes here
}
```

Here's an example of how to do this in a controller action:

```csharp
[ApiController]
[Route("[controller]")]
public class MyController : ControllerBase
{
    private readonly IBus _bus;
    public MyController(IBus bus)
    {
        _bus = bus;
    }

    [HttpPost]
    public async Task<IActionResult> SendMessage(string text)
    {
        var message = new MyMessage { Text = text };

        var queueToPublishTo = new Uri("rabbitmq://localhost/Queue1");
        var endPoint = await _bus.GetSendEndpoint(queueToPublishTo);
        await endPoint.Send(message );

        return Ok();
    }
}
```

**Explication:**

The `MyController` class has a constructor that takes an `IBus` instance as a parameter. This allows you to use the `IBus` interface to send messages from the controller's actions. In this case, the `SendMessage` action takes a string parameter called `text` and creates a new instance of the `MyMessage` class with the `Text` property set to the value of the `text` parameter.

The `SendMessage` action uses the `IBus` interface to send the message to a specific endpoint. The `var queueToPublishTo = new Uri("rabbitmq://`[`localhost/Queue1`](http://localhost/Queue1)`")` line is used to specify the endpoint URL and the queue name. Then, it uses `_bus.GetSendEndpoint(queueToPublishTo)` to get the endpoint object and then sends the message using `await endPoint.Send(message)`.

This code allows to send messages to a specific endpoint and queue, the endpoint URL and queue name are hardcoded in the application.

**Or :**

```csharp
[ApiController]
[Route("[controller]")]
public class MyController : ControllerBase
{
    private readonly IPublishEndpoint _publishEndpoint;

    public MyController(IPublishEndpoint publishEndpoint)
    {
        _publishEndpoint = publishEndpoint;
    }

    [HttpPost]
    public async Task<IActionResult> SendMessage(string text)
    {
        var message = new MyMessage { Text = text };
        await _publishEndpoint.Publish(message);
        return Ok();
    }
}
```

**Explication:**

In this example, the `IPublishEndpoint` is injected in the controller's constructor. The `IPublishEndpoint` is used to publish the message using the `Publish` method, passing in an instance of the message.

The `IBus` instance is no longer required and the code for getting a send endpoint for a specific queue and sending the message is not required since `IPublishEndpoint` is used to only publish the message.

The `IPublishEndpoint` is a subset of `IBus` and it only allows message to be published but not received.

### Avoid Hard Coding The EndPoint

hardcoding the endpoint URL and queue name in the application is not a good practice, as it can make it difficult to change the destination endpoint or queue without modifying the code.

One way to avoid hardcoding the endpoint URL and queue name is to use configuration settings, such as those in the `appsettings.json` file. You can store the endpoint URL and queue name in the configuration file and then use the `IConfiguration` service to retrieve the values at runtime. This way, you can change the endpoint URL and queue name without modifying the code.

*We talked before in previous articles about :*  
[**Understanding the IOptions&lt;T&gt; in**](https://mbarkt3sto.hashnode.dev/understanding-the-ioptionst-in-aspnet-core) [**ASP.NET**](http://ASP.NET) [**Core**](https://mbarkt3sto.hashnode.dev/understanding-the-ioptionst-in-aspnet-core)  
[**Understanding the IOptionsSnapshot&lt;T&gt; in**](https://mbarkt3sto.hashnode.dev/understanding-the-ioptionssnapshott-in-aspnet-core) [**ASP.NET**](http://ASP.NET) [**Core**](https://mbarkt3sto.hashnode.dev/understanding-the-ioptionssnapshott-in-aspnet-core)  
[**Understanding IOptionsMonitor&lt;T&gt; in**](https://mbarkt3sto.hashnode.dev/understanding-ioptionsmonitort-in-aspnet-core) [**ASP.NET**](http://ASP.NET) [**Core**](https://mbarkt3sto.hashnode.dev/understanding-ioptionsmonitort-in-aspnet-core)

For example, in your `appsettings.json` file you can add the following configuration:

```csharp
"Endpoints": {
    "Queue1": "rabbitmq://localhost/Queue1"
  // Other endpoints ...
}
```

Then, in your `Startup.cs` class, you can add the following code to configure your endpoint:

```csharp
services.Configure<EndpointConfig>(Configuration.GetSection("Endpoints"));
```

The `EndpointConfig` class is a plain C# class that holds the endpoint URL and queue name as properties. The properties should match the keys in the configuration file. Here's an example of how the `EndpointConfig` class might look like:

```csharp
public class EndpointConfig
{
    public string Queue1 { get; set; }
}
```

This class has only one property named `Queue1` of type `string` that matches the key in the configuration file. The class could have other properties as well, depending on the endpoints that are used in the application.

Finally, in your controller, you can use the `IOptions<EndpointConfig>` service to get the endpoint configuration, and use it to send messages to the right endpoint.

```csharp
[ApiController]
[Route("[controller]")]
public class MyController : ControllerBase
{
    private readonly IBus _bus;
    private readonly IOptions<EndpointConfig> _endpointConfig;
    public MyController(IBus bus, IOptions<EndpointConfig> endpointConfig)
    {
        _bus = bus;
        _endpointConfig = endpointConfig;
    }

    [HttpPost]
    public async Task<IActionResult> SendMessage(string text)
    {
        var message = new MyMessage { Text = text };

        var queueToPublishTo = new Uri(_endpointConfig.Value.Queue1);
        var endPoint = await _bus.GetSendEndpoint(queueToPublishTo );
        await endPoint.Send(message);

        return Ok();
    }
}
```

By using this method, you can change the endpoint URL and queue name in the `appsettings.json` file without modifying the code, and it makes it easy to change the destination endpoint or queue without modifying the code.

## **Setting up the application as a subscriber**

### Configure MassTransit

To set up your application as a subscriber, you need to configure MassTransit to use RabbitMQ as the message broker and provide the connection details such as the hostname, port, username, and password.

```csharp
services.AddMassTransit(x =>
{
    x.AddConsumer<MyMessageConsumer>();

    x.AddBus(provider => Bus.Factory.CreateUsingRabbitMq(cfg =>
    {
        cfg.Host("rabbitmq://localhost", h =>
        {
            h.Username("guest");
            h.Password("guest");
        });

        cfg.ReceiveEndpoint("Queue1", ep =>
        {
            ep.PrefetchCount = 16;
            ep.UseMessageRetry(r => r.Interval(2, 100));
            ep.ConfigureConsumer<MyMessageConsumer>(provider);
        });
    }));
});
```

**Explication:**

This code is adding MassTransit to [ASP.NET](http://ASP.NET) Core application's service collection using the `services.AddMassTransit(x => { ... })` method. This method takes a lambda function as a parameter, which is used to configure MassTransit.

The first line, `x.AddConsumer<MyMessageConsumer>();` is adding the `MyMessageConsumer` class as a message handler. The `MyMessageConsumer` class is a class that implements the `IConsumer<T>` interface, which is the basic interface for handling messages in MassTransit. It allows to consume messages of a specific type.

The second line, `x.AddBus(provider => Bus.Factory.CreateUsingRabbitMq(cfg => { ... }))` is adding a bus instance to the service collection. The `Bus.Factory.CreateUsingRabbitMq` method is used to create a new bus instance that uses RabbitMQ as the message broker. The `cfg` parameter passed to this method is a configuration callback that allows you to configure various aspects of the bus, such as the host and receive endpoints.

The [`cfg.Host`](http://cfg.Host)`("rabbitmq://`[`localhost`](http://localhost)`", h => { ... })` line is used to configure the host that the bus will connect to. The first parameter is the connection string and the second parameter is a callback that allows you to configure the host settings, in this case the username and password.

The `cfg.ReceiveEndpoint("Queue1", ep => { ... })` line is used to configure a receive endpoint on the bus. The first parameter is the name of the endpoint and the second parameter is a callback that allows you to configure various aspects of the endpoint, such as the prefetch count, retry settings, and consumer.

The `ep.PrefetchCount = 16;` is used to set the prefetch count, which is the number of messages that can be prefetched for consumers.

The `ep.UseMessageRetry(r => r.Interval(2, 100))` is used to enable message retries for this endpoint, with a retry interval of 2 seconds and a maximum of 100 retries.

The `ep.ConfigureConsumer<MyMessageConsumer>(provider);` is used to configure the consumer that will handle messages received by this endpoint. The `MyMessageConsumer` class is the same class that was added as a message handler earlier in the code.

This configuration allows the application to receive messages from a queue named "Queue1" and process them using the `MyMessageConsumer` class. The bus is configured to use RabbitMQ and the username and password are provided, allowing it to connect to the RabbitMQ server. The endpoint is configured with a prefetch count of 16, message retries with an interval of 2 seconds and 100 maximum retries.

**Or:**

```csharp
services.AddMassTransit(x =>
{
    x.AddBus(provider => Bus.Factory.CreateUsingRabbitMq(cfg =>
    {
        cfg.ReceiveEndpoint("Queue1", ep => ep.Consumer<MyMessageConsumer>());
    }));
});
```

**Explication:**

This code is configuring an instance of MassTransit's `IBus` to use RabbitMQ as the message transport and adding it to the service collection in [ASP.NET](http://ASP.NET) Core application.

`services.AddMassTransit(x => { ... })` is a method call that adds MassTransit services to the [ASP.NET](http://ASP.NET) Core service collection. The method takes a single parameter which is a lambda that is used to configure MassTransit options.

`x.AddBus(provider => Bus.Factory.CreateUsingRabbitMq(cfg => { ... }))` is a method call that adds an instance of the `IBus` to the service collection. The method takes a single parameter, which is a lambda that creates an instance of the `IBus`. `Bus.Factory.CreateUsingRabbitMq(cfg => { ... })` is a factory method that creates an instance of the `IBus` and configures it to use RabbitMQ as the message transport.

`cfg.ReceiveEndpoint("Queue1", ep => ep.Consumer<MyMessageConsumer>())` is a method call that creates a receive endpoint for the RabbitMQ queue named "Queue1". The receive endpoint is a way for the bus to receive messages from a queue. The second parameter is a lambda that configures the endpoint. `ep.Consumer<MyMessageConsumer>()` is a method call that adds a consumer to the receive endpoint. The consumer specified is an instance of the class `MyMessageConsumer` that is expected to handle the messages received on this endpoint.

The consumer class `MyMessageConsumer` should be an implementation of `MassTransit.IConsumer<T>` where `T` is the message type that consumer is expected to handle *(it is our next step).*

### Create a Message Handler

You also need to create a message handler that will process the messages received by the subscriber. For example:

```csharp
public class MyMessageConsumer : IConsumer<MyMessage>
{
    public async Task Consume(ConsumeContext<MyMessage> context)
    {
                File.AppendAllText("mbark.txt", $"Recieved data:{context.Message.Text}");
    }
}
```

**Explication:**

This code defines a class named `MyMessageConsumer` that implements the `IConsumer<MyMessage>` interface. The `IConsumer<T>` interface is the basic interface for handling messages in MassTransit. It requires the implementation of a single method named `Consume` that takes a single parameter of type `ConsumeContext<T>` where T is the message type.

The `MyMessageConsumer` class has a single method named `Consume` that takes a single parameter of type `ConsumeContext<MyMessage>` which is the context for the message being consumed. The method is asynchronous and it is decorated with the `async` keyword. The method's implementation writes the `Text` property of the `MyMessage` object to a file named "mbark.txt" using the `File.AppendAllText()` method.

When a message of type `MyMessage` is received by the endpoint, the `Consume` method of the `MyMessageConsumer` will be invoked, and the message will be passed as the parameter to the `Consume` method.

This allows the application to process the messages received by the endpoint in a specific way, in this case, it writes the message's text to a file named "mbark.txt" using the `File.AppendAllText()` method. This consumer is able to handle messages of type `MyMessage` and it is commonly used in scenarios where the application needs to consume messages and log them in a file.

## Difference between Send and Publish

In MassTransit, the main difference between `Send` and `Publish` is the way messages are delivered and handled.

* `Send` is used to send a message to a specific endpoint. When you `Send` a message, you need to specify the endpoint's address, and the message is delivered to that specific endpoint. This method is used when you need to send a message to a specific recipient or when you know that the message should only be handled by one specific consumer.
    
* `Publish` is used to send a message to multiple subscribers. When you `Publish` a message, you don't specify an endpoint's address. Instead, the message is delivered to all subscribers that have subscribed to that message type. This method is used when you need to send a message to multiple recipients or when you don't know how many consumers will handle the message.
    

In summary, `Send` is a point-to-point operation, where the message is sent to a specific endpoint, whereas `Publish` is a one-to-many operation, where the message is sent to multiple subscribers.

It's important to note that, when using `Send`, the message is sent to a specific endpoint, and that endpoint is responsible for delivering the message to the appropriate consumer. When using `Publish`, the message is sent to the message broker, and the broker is responsible for delivering the message to all subscribers.

In general, it's recommended to use `Send` when you know that the message should only be handled by one specific consumer and `Publish` when you need to send a message to multiple recipients or when you don't know how many consumers will handle the message.