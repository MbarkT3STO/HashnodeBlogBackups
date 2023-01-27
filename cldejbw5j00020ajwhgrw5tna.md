# A Noob Introduction to Hosted Services in ASP.NET Core

One of the key features of [ASP.NET](http://ASP.NET) Core is the ability to create and run background tasks using Hosted Services.

A Hosted Service is a class that implements the `IHostedService` interface, which allows you to run background tasks in your application. These tasks can be used for a variety of purposes, such as sending emails, cleaning up old data, or running scheduled jobs.

## **Creating a Hosted Service**

To create a Hosted Service, you first need to create a new class that implements the `IHostedService` interface. This interface has two methods: `StartAsync` and `StopAsync`. The `StartAsync` method is called when the service is started, and the `StopAsync` method is called when the service is stopped.

Here is an example of a simple Hosted Service that prints a message every 5 seconds:

```csharp
public class TimedHostedService : IHostedService
{
    private readonly ILogger _logger;
    private Timer _timer;

    public TimedHostedService(ILogger<TimedHostedService> logger)
    {
        _logger = logger;
    }

    public Task StartAsync(CancellationToken cancellationToken)
    {
        _logger.LogInformation("Timed Hosted Service running.");

        _timer = new Timer(DoWork, null, TimeSpan.Zero, 
            TimeSpan.FromSeconds(5));

        return Task.CompletedTask;
    }

    private void DoWork(object state)
    {
        _logger.LogInformation("Timed Hosted Service is working.");
    }

    public Task StopAsync(CancellationToken cancellationToken)
    {
        _logger.LogInformation("Timed Hosted Service is stopping.");

        _timer?.Change(Timeout.Infinite, 0);

        return Task.CompletedTask;
    }
}
```

Once you have created your Hosted Service, you need to add it to the `ConfigureServices` method in your `Startup` class:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHostedService<TimedHostedService>();
    //...
}
```

## **Using a Hosted Service**

Once you have created and configured your Hosted Service, it will automatically start running when the application starts and stop when the application stops. You can also use the `IHostedService` interface to start and stop the service manually.

## How to start and stop it manually from a Controller?

First, we need to change the registration method and it would be as follow:

```csharp
builder.Services.AddSingleton<TimedHostedService>();
builder.Services.AddHostedService(s => s.GetRequiredService<TimedHostedService>());
```

Once you have registered your `TimedHostedService` in the `ConfigureServices` method of your `Startup` or `program` class, you can inject it into a controller using dependency injection.

Here's an example of a controller that starts and stops the `TimedHostedService`:

```csharp
public class TimedServiceController : Controller
{
    private readonly TimedHostedService _timedHostedService;

    public TimedServiceController(TimedHostedService timedHostedService)
    {
        _timedHostedService = timedHostedService;
    }

    [HttpPost("start")]
    public IActionResult Start()
    {
        _timedHostedService.StartAsync(CancellationToken.None);
        return Ok();
    }

    [HttpPost("stop")]
    public IActionResult Stop()
    {
        _timedHostedService.StopAsync(CancellationToken.None);
        return Ok();
    }
}
```

In this example, the `Start` method of the controller calls the `StartAsync` method of the `TimedHostedService` to start it, and the `Stop` method calls the `StopAsync` method to stop it.

It's important to note that `StartAsync()` and `StopAsync()` methods of `IHostedService` are asynchronous and returns `Task`. If you are using the above example, you need to await the task before returning the response.

```csharp
[HttpPost("start")]
public async Task<IActionResult> Start()
{
    await _timedHostedService.StartAsync(CancellationToken.None);
    return Ok();
}

[HttpPost("stop")]
public async Task<IActionResult> Stop()
{
    await _timedHostedService.StopAsync(CancellationToken.None);
    return Ok();
}
```

Also, it's important to understand that starting and stopping the service manually would not change the lifetime of the service, it will only invoke the StartAsync and StopAsync methods and whatever logic you have written inside those methods will be executed.

## Should IHostedService be IDisposable?

As for whether it should implement IDisposable, it depends on whether the IHostedService needs to release any resources when it is no longer needed.

If the IHostedService is using any resources that need to be cleaned up when the service is no longer needed, then it should implement IDisposable, and dispose of those resources in the Dispose() method.

However, if the IHostedService does not use any resources that need to be cleaned up, there is no need to implement IDisposable.