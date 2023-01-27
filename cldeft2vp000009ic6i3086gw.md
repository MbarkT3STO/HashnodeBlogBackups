# Scheduling Background Jobs in ASP.NET Core with Quartz.Net

[ASP.NET](http://ASP.NET) Core allows for the scheduling of background jobs using the [Quartz.NET](http://Quartz.NET) library. This library provides a powerful scheduling framework that can be easily integrated into an [ASP.NET](http://ASP.NET) Core application using the [`Quartz.Extensions.Hosting`](http://Quartz.Extensions.Hosting) package.

## What is a Background Job

A background job, also known as a scheduled task or a cron job, is a task that runs in the background without the user's direct interaction. It can be used to perform tasks such as sending emails, generating reports, or cleaning up data at a specific time or interval. These tasks are typically run asynchronously, meaning that they do not block the main thread of execution and do not prevent the user from interacting with the application.

Background jobs are commonly used in web applications to perform tasks that do not need to be done in real-time, such as sending a daily summary email or cleaning up old data in a database. They can also be used to perform tasks that are too resource-intensive to be done in real-time, such as generating a large report or running a complex data analysis.

## **Adding** [**Quartz.NET**](http://Quartz.NET) **to the Container**

To add [Quartz.NET](http://Quartz.NET) to the container, we first need to install the [`Quartz.Extensions.Hosting`](http://Quartz.Extensions.Hosting) package. Once this is done, we can add Quartz to the container by calling the `AddQuartz` method on the `IServiceCollection` object.

```csharp
builder.Services.AddQuartz(q =>
{
    q.UseMicrosoftDependencyInjectionScopedJobFactory();
});
```

The `UseMicrosoftDependencyInjectionScopedJobFactory` method tells Quartz to use the built-in dependency injection framework in [ASP.NET](http://ASP.NET) Core for resolving job instances. This allows for easy integration with other services in the application.

## **Scheduling a Job**

To schedule a job, we first need to define the job class. This class should implement the `IJob` interface from [Quartz.NET](http://Quartz.NET). Here is an example of a job that writes a message to a file:

```csharp
public class UpdateProductsFileJob : IJob
{
    private readonly ILogger<UpdateProductsFileJob> _logger;

    public UpdateProductsFileJob(ILogger<UpdateProductsFileJob> logger)
    {
        _logger = logger;
    }

    public Task Execute(IJobExecutionContext context)
    {
        _logger.LogInformation("Writing message to file...");
        File.WriteAllText("products.txt", "Hello from Quartz.NET!");
        _logger.LogInformation("Message written to file.");

        return Task.CompletedTask;
    }
}
```

To register the job with Quartz, we can use the `AddJob` method. This method takes a configuration object that allows us to set properties such as the job's identity and triggers.

```csharp
var updateProductsFileJobKey = new JobKey(nameof(UpdateProductsFileJob));
q.AddJob<UpdateProductsFileJob>(opt => opt.WithIdentity(updateProductsFileJobKey));
```

## **Scheduling a Trigger**

Once the job is registered, we can schedule a trigger for it. A trigger is used to specify when the job should be executed. There are various types of triggers available in [Quartz.NET](http://Quartz.NET), such as simple triggers and cron triggers.

In this example, we will be using a simple trigger that repeats every 2 minutes.

```csharp
q.AddTrigger(cfg => cfg.ForJob(updateProductsFileJobKey)
.WithIdentity("UpdateProductsFileJob-Trigger")
.WithSimpleSchedule(x => x.WithIntervalInMinutes(2).RepeatForever()));
```

The `WithSimpleSchedule` method takes a configuration object that allows us to set properties such as the repeat interval and repeat count.

## **Adding the Quartz Hosted Service**

Finally, we need to add the Quartz hosted service to the container. This service is responsible for running the scheduled jobs.

```csharp
builder.Services.AddQuartzHostedService(q => q.WaitForJobsToComplete = true);
```

The `AddQuartzHostedService` method takes a configuration object that allows us to set properties such as whether or not to wait for jobs to complete before shutting down the application. In this example, we have set the `WaitForJobsToComplete` property to `true` so that the application will wait for all scheduled jobs to complete before shutting down.

## Full Code of Registration

The full code for registering Quartz, scheduling a job, and scheduling a trigger in this example would be as follows:

```csharp
builder.Services.AddQuartz(q =>
{
    q.UseMicrosoftDependencyInjectionScopedJobFactory();

    var updateProductsFileJobKey = new JobKey(nameof(UpdateProductsFileJob));
    q.AddJob<UpdateProductsFileJob>(opt => opt.WithIdentity(updateProductsFileJobKey));

    q.AddTrigger(cfg => cfg.ForJob(updateProductsFileJobKey)
    .WithIdentity("UpdateProductsFileJob-Trigger")
    .WithSimpleSchedule(x => x.WithIntervalInMinutes(2).RepeatForever()));
});

builder.Services.AddQuartzHostedService(q => q.WaitForJobsToComplete = true);
```

In this example, we first added Quartz to the container and specified that it should use the built-in dependency injection framework in [ASP.NET](http://ASP.NET) Core. Then we defined a job class that implements the IJob interface, registered the job with Quartz, and scheduled a trigger for it with a simple schedule that repeats every 2 minutes. Finally, we added the Quartz hosted service to the container, which is responsible for running the scheduled jobs.  
The `AddQuartzHostedService` method takes a configuration object that allows us to set properties such as whether or not to wait for jobs to complete before shutting down the application. In this example, we have set the `WaitForJobsToComplete` property to `true` so that the application will wait for all scheduled jobs to complete before shutting down.

## **Conclusion**

[Quartz.NET](http://Quartz.NET) provides a powerful scheduling framework that can be easily integrated into an [ASP.NET](http://ASP.NET) Core application. By using the [`Quartz.Extensions.Hosting`](http://Quartz.Extensions.Hosting) package and the built-in dependency injection framework in [ASP.NET](http://ASP.NET) Core, we can easily register jobs and schedule triggers for them.

This example showed how to create a simple job that writes a message to a file and schedule it to run every 2 minutes. However, [Quartz.NET](http://Quartz.NET) offers much more advanced features such as cron triggers, job data maps, and job listeners. These features can be used to create more complex scheduling scenarios in your application.