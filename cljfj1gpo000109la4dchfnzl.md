---
title: "ASP.NET Core: How to Implement Event-Sourcing with CQRS?"
datePublished: Wed Jun 28 2023 09:41:24 GMT+0000 (Coordinated Universal Time)
cuid: cljfj1gpo000109la4dchfnzl
slug: aspnet-core-how-to-implement-event-sourcing-with-cqrs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1687903556194/66c6278f-c7e3-46ed-a99e-f89465e77e0b.png
tags: software-development, software-architecture, net, aspnet-core, event-driven-architecture

---

Event-sourcing with Command Query Responsibility Segregation (CQRS) is a powerful pattern that can bring numerous benefits to your [ASP.NET](http://ASP.NET) Core applications. By decoupling the write and read models and using event sourcing, you can achieve better scalability, maintainability, and extensibility. In this article, we will explore how to implement Event-Sourcing with CQRS using EF Core and MediatR in an [ASP.NET](http://ASP.NET) Core application. We'll cover the fundamental concepts, provide practical examples, and use SQL Server as our database.

## **Prerequisites**

Before we dive into the implementation, let's ensure you have the necessary tools and knowledge in place:

* Basic understanding of [ASP.NET](http://ASP.NET) Core and C#
    
* Familiarity with Entity Framework Core (EF Core) and MediatR
    
* Visual Studio 2019 or later, or any other code editor of your choice
    
* SQL Server
    

## **Setting Up the Project**

To get started, let's create a new [ASP.NET](http://ASP.NET) Core Web API project. Open Visual Studio and follow these steps:

1. Click on "Create a new project."
    
2. Select "[ASP.NET](http://ASP.NET) Core Web Application" template.
    
3. Choose a name and location for your project.
    
4. Select ".NET 5.0" or above as the target framework.
    
5. Choose "API" as the project template.
    
6. Click "Create" to generate the project structure.
    

## **Installing the Required Packages**

To implement Event-Sourcing with CQRS, we need to install a few NuGet packages:

```csharp
MediatR.Extensions.Microsoft.DependencyInjection
Microsoft.EntityFrameworkCore.SqlServer
Microsoft.EntityFrameworkCore.Design
```

These packages provide the necessary infrastructure for CQRS and EF Core integration, as well as the SQL Server provider for EF Core.

## **Defining the Domain Events and Commands**

Before we proceed with the implementation, let's define some domain events and commands that will drive our CQRS implementation. In our example, we will build a simple e-commerce application with a product catalog. Create a new folder named "Domain" in your project and add the following classes:

### **Product.cs**

```csharp
namespace YourProject.Domain
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
}
```

### **ProductCreatedEvent.cs**

```csharp
namespace YourProject.Domain
{
    public class ProductCreatedEvent : INotification
    {
        public int ProductId { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
}
```

### **CreateProductCommand.cs**

```csharp
namespace YourProject.Domain
{
    public class CreateProductCommand : IRequest<int>
    {
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
}
```

These classes represent the product entity, the event raised when a product is created, and the command for creating a product.

## **Implementing the Write-Side (Command) Handler**

Now, let's implement the command handler responsible for processing the `CreateProductCommand`. Create a new folder named "Application" in your project and add the following classes:

### **CreateProductCommandHandler.cs**

```csharp
using MediatR;
using YourProject.Domain;

namespace YourProject.Application
{
    public class CreateProductCommandHandler : IRequestHandler<CreateProductCommand, int>
    {
        private readonly YourDbContext _context;
        private readonly IMediator _mediator;

        public CreateProductCommandHandler(YourDbContext context, IMediator mediator)
        {
            _context = context;
            _mediator = mediator;
        }

        public async Task<int> Handle(CreateProductCommand command, CancellationToken cancellationToken)
        {
            var product = new Product
            {
                Name = command.Name,
                Price = command.Price
            };

            _context.Products.Add(product);
            await _context.SaveChangesAsync();

			// Reload the product to get the Id
			await _context.Entry(product).ReloadAsync(cancellationToken);

            var productCreatedEvent = new ProductCreatedEvent
            {
                ProductId = product.Id,
                Name = product.Name,
                Price = product.Price
            };

            await _mediator.Publish(productCreatedEvent);

            return product.Id;
        }
    }
}
```

In this class, we handle the `CreateProductCommand` by creating a new `Product` entity, saving it to the SQL Server database using EF Core, and then publishing the `ProductCreatedEvent` using MediatR.

## **Implementing the Read-Side (Event) Handler**

Next, let's implement the event handler responsible for updating the read-side of our application. Create a new folder named "Infrastructure" in your project and add the following class:

### **ProductReadModel.cs**

```csharp
namespace YourProject.Infrastructure
{
    public class ProductReadModel
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
}
```

### **ProductCreatedEventHandler.cs**

```csharp
using MediatR;
using YourProject.Domain;

namespace YourProject.Infrastructure
{
    public class ProductCreatedEventHandler : INotificationHandler<ProductCreatedEvent>
    {
        private readonly YourReadDbContext _context;

        public ProductCreatedEventHandler(YourReadDbContext context)
        {
            _context = context;
        }

        public async Task Handle(ProductCreatedEvent notification, CancellationToken cancellationToken)
        {
            var product = new ProductReadModel
            {
                Id = notification.ProductId,
                Name = notification.Name,
                Price = notification.Price
            };

            _context.ProductReadModels.Add(product);
            await _context.SaveChangesAsync();
        }
    }
}
```

In this class, we handle the `ProductCreatedEvent` by creating a new `ProductReadModel` and saving it to the read-side database using EF Core.

## **Configuring MediatR and EF Core**

To wire up MediatR and EF Core, we need to configure them in the [ASP.NET](http://ASP.NET) Core dependency injection container. Open the `Startup.cs` file and add the following code inside the `ConfigureServices` method:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...

    services.AddMediatR(typeof(Startup));

    services.AddDbContext<YourDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("YourDbConnection")));

    services.AddDbContext<YourReadDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("YourReadDbConnection")));

    // ...
}
```

Make sure to replace `YourDbContext` and `YourReadDbContext` with the actual EF Core DbContext classes in your application. Also, update the connection strings in the appsettings.json file with your SQL Server database information:

```json
"ConnectionStrings": {
  "YourDbConnection": "Server=(localdb)\\mssqllocaldb;Database=YourDatabase;Trusted_Connection=True;",
  "YourReadDbConnection": "Server=(localdb)\\mssqllocaldb;Database=YourReadDatabase;Trusted_Connection=True;"
}
```

## **The Read-Side and Write-Side in CQRS**

In Command Query Responsibility Segregation (CQRS), the architecture is divided into two distinct sides: the **Read-Side** and the **Write-Side**. Each side serves a different purpose and has separate responsibilities. Let's explore these sides in more detail.

### **Read-Side**

The Read-Side is responsible for handling queries and retrieving data. It focuses on providing optimized read operations for fetching information to display in the user interface. The data in the Read-Side is typically denormalized and structured specifically to meet the needs of the queries.

In our example, the Read-Side is responsible for maintaining a separate database or data store optimized for reading product information. We use EF Core to interact with this database and store the read models.

### **Write-Side**

The Write-Side is responsible for handling commands and performing write operations. It handles all the business logic and updates the state of the system. In CQRS, the Write-Side typically represents the domain model and is responsible for enforcing business rules, validating data, and persisting changes.

In our example, the Write-Side is responsible for processing the `CreateProductCommand` and creating a new `Product` entity. We use EF Core to interact with the SQL Server database and store the write models.

Separating the Read-Side and Write-Side allows us to optimize each side independently. The Write-Side can focus on consistency and ensuring the integrity of the data, while the Read-Side can focus on providing efficient and performant query operations.

By decoupling the Read-Side and Write-Side, we gain several benefits, such as:

* Scalability: We can scale each side independently based on the workload. For example, we can scale the Read-Side horizontally to handle increased read traffic without affecting the Write-Side.
    
* Performance: We can optimize the Read-Side for specific query requirements, allowing faster and more efficient data retrieval.
    
* Flexibility: We can evolve the Read-Side and Write-Side independently, making it easier to introduce new features or modify existing ones without impacting the entire system.
    

Understanding the separation between the Read-Side and Write-Side is crucial in CQRS to design a system that can handle complex domain operations and provide efficient query capabilities.

## **Database Contexts**

In our implementation of Event-Sourcing with CQRS using EF Core and MediatR, we utilize two different database contexts: `YourDbContext` and `YourReadDbContext`. Let's take a closer look at each of them.

### **YourDbContext**

The `YourDbContext` represents the database context for the Write-Side of our application. It is responsible for interacting with the SQL Server database and managing the write models.

Here's an example of how the `YourDbContext` class might look:

```csharp
using Microsoft.EntityFrameworkCore;
using YourProject.Domain;

namespace YourProject.Infrastructure
{
    public class YourDbContext : DbContext
    {
        public YourDbContext(DbContextOptions<YourDbContext> options) : base(options)
        {
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // Configure entity mappings and relationships
            // ...
        }
    }
}
```

In this class, we inherit from the `DbContext` class provided by EF Core. We define a `DbSet<Product>` property to represent the `Product` entity in our database. This property allows us to query and interact with the `Product` table.

The `YourDbContext` class also includes an `OnModelCreating` method where you can configure entity mappings and relationships using the Fluent API. This method is useful when defining custom table names, column mappings, or complex relationships between entities.

### **YourReadDbContext**

The `YourReadDbContext` represents the database context for the Read-Side of our application. It is responsible for interacting with a separate database or data store optimized for reading and managing the read models.

Here's an example of how the `YourReadDbContext` class might look:

```csharp
using Microsoft.EntityFrameworkCore;
using YourProject.Infrastructure;

namespace YourProject.Infrastructure
{
    public class YourReadDbContext : DbContext
    {
        public YourReadDbContext(DbContextOptions<YourReadDbContext> options) : base(options)
        {
        }

        public DbSet<ProductReadModel> ProductReadModels { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // Configure entity mappings and relationships
            // ...
        }
    }
}
```

Similarly to the `YourDbContext`, the `YourReadDbContext` inherits from the `DbContext` class and provides a `DbSet<ProductReadModel>` property to interact with the read model data. This property represents the read model table or data store, in this case, containing the `ProductReadModel` objects.

The `YourReadDbContext` class also includes an `OnModelCreating` method for configuring entity mappings and relationships specific to the read model.

### **Configuring Connection Strings**

To connect your application to the SQL Server databases, you need to configure the connection strings. In the `appsettings.json` file, you can provide the connection string values as follows:

```json
"ConnectionStrings": {
  "YourDbConnection": "Server=<your-server-name>;Database=<your-write-database-name>;Trusted_Connection=True;",
  "YourReadDbConnection": "Server=<your-server-name>;Database=<your-read-database-name>;Trusted_Connection=True;"
}
```

Replace `<your-server-name>` with the name of your SQL Server instance and `<your-write-database-name>` and `<your-read-database-name>` with the names of your write and read databases, respectively.

## **Example Usage in a Controller**

Now that we have implemented Event-Sourcing with CQRS using EF Core and MediatR in our [ASP.NET](http://ASP.NET) Core application, let's see an example of how we can utilize this architecture in a controller. We'll focus on the write operations, specifically creating a new product using the `CreateProductCommand`.

In your controller class, you can inject an instance of `IMediator` to handle the commands and perform the necessary operations. Here's an example:

```csharp
using MediatR;
using Microsoft.AspNetCore.Mvc;
using YourProject.Domain;

namespace YourProject.Controllers
{
    [ApiController]
    [Route("api/products")]
    public class ProductController : ControllerBase
    {
        private readonly IMediator _mediator;

        public ProductController(IMediator mediator)
        {
            _mediator = mediator;
        }

        [HttpPost]
        public async Task<IActionResult> CreateProduct(CreateProductCommand command)
        {
            try
            {
                var productId = await _mediator.Send(command);
                return Ok(productId);
            }
            catch (Exception ex)
            {
                // Handle any exceptions or validation errors
                return BadRequest(ex.Message);
            }
        }
    }
}
```

In this example, we inject an instance of `IMediator` into the `ProductController` constructor. The `IMediator` interface is provided by MediatR and allows us to send commands and queries to their respective handlers.

Inside the `CreateProduct` action method, we receive a `CreateProductCommand` object as a parameter. This command contains the necessary data to create a new product.

We then use the `_mediator.Send` method to send the command to its corresponding command handler. The handler, in turn, processes the command, creates a new product entity, and publishes the `ProductCreatedEvent`.

If the command is successfully processed, we return an `Ok` response with the ID of the created product. If an exception occurs during command processing or validation fails, we return a `BadRequest` response with the corresponding error message.

Note that in a complete implementation, you would likely have additional action methods to handle read operations or other commands based on your application's requirements.

## **Handling "More than one DbContext was found" Error during Migrations**

When working with multiple `DbContext` instances in your [ASP.NET](http://ASP.NET) Core application, you may encounter the "More than one DbContext was found" error when trying to add new migrations. This error occurs because EF Core cannot determine which context to use for creating the migration since there are multiple options available. Fortunately.

### **Solution**

One way to resolve the error is by explicitly specifying the `DbContext` you want to use when executing the migration command. Here's an example of how to do this using the Package Manager Console:

```csharp
Add-Migration MyMigration -Context YourDbContext
```

Replace `YourDbContext` with the actual name of the context you want to use for the migration.

## **Other Purposes for Using Event-Sourcing**

Event-Sourcing is a powerful architectural pattern that offers several benefits beyond just implementing CQRS. Let's explore some of the other purposes for using Event-Sourcing in your [ASP.NET](http://ASP.NET) Core application:

### **1\. Audit Log and Compliance**

One of the significant advantages of Event-Sourcing is that it provides an audit log of all events that occurred within the system. Each event represents a discrete change in the application's state, making it easy to track and analyze the history of changes. This audit log can be valuable for compliance requirements, regulatory audits, or forensic analysis in case of security incidents.

By storing and replaying events, you can accurately recreate the system's state at any given point in time, making it easier to investigate issues or validate data integrity.

### **2\. Temporal Queries and Analytics**

Event-Sourcing also enables temporal queries, allowing you to answer questions about the past state of your application. By replaying events and reconstructing the state of your entities, you can perform analytics and gain insights into trends, patterns, and historical data.

Temporal queries can be useful for generating reports, visualizations, or performing retrospective analysis to identify areas for improvement or make data-driven decisions.

### **3\. Resilience and Disaster Recovery**

Since Event-Sourcing captures every state change as an event, it provides a natural mechanism for resilience and disaster recovery. By replaying events, you can rebuild the system's state in case of failures, errors, or data corruption.

In the event of a catastrophic failure, such as a database failure or complete system outage, Event-Sourcing allows you to restore the system by replaying the stored events and rebuilding the entity state. This ensures that your application can recover from unexpected issues and continue functioning with minimal downtime.

### **4\. Versioning and Evolution of Business Logic**

Event-Sourcing promotes decoupling between the events and the business logic that processes them. This decoupling allows for greater flexibility in evolving the application over time.

As you introduce new features or modify existing ones, you can handle different versions of events and update the event handlers accordingly. This allows for backward compatibility, as old events can still be processed while new events follow updated business logic.

## **Example Audit Log for Product: ProductCreated Event**

To demonstrate an example of an audit log for the ProductCreated event in our Event-Sourcing implementation, we'll store the events in a separate database. We'll define the necessary classes and structures to support this functionality. Let's dive into the details.

### **1\. AuditLogEvent Entity**

We'll start by creating an `AuditLogEvent` entity that represents an individual event in the audit log. This entity will store information about the event, such as the event type, timestamp, user, and any additional data associated with the event.

```csharp
using System;

namespace YourProject.AuditLog
{
    public class AuditLogEvent
    {
        public int Id { get; set; }
        public string EventType { get; set; }
        public DateTime Timestamp { get; set; }
        public string User { get; set; }
        public string EventData { get; set; }
    }
}
```

In this example, the `AuditLogEvent` class contains properties like `Id` (unique identifier for the event), `EventType` (describes the type of the event, e.g., "ProductCreated"), `Timestamp` (when the event occurred), `User` (the user who initiated the event), and `EventData` (additional data related to the event).

### **2\. AuditLogDbContext**

Next, we'll define a dedicated `AuditLogDbContext` that will handle the storage of audit log events in a separate database.

```csharp
using Microsoft.EntityFrameworkCore;

namespace YourProject.AuditLog
{
    public class AuditLogDbContext : DbContext
    {
        public DbSet<AuditLogEvent> AuditLogEvents { get; set; }

        public AuditLogDbContext(DbContextOptions<AuditLogDbContext> options) : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // Configure entity mappings, constraints, etc.
        }
    }
}
```

In this class, we define a `DbSet<AuditLogEvent>` property called `AuditLogEvents` to represent the table/collection for storing the audit log events.

### **3\. Storing ProductCreated Events**

When handling the ProductCreated event in the domain logic, we'll also publish the event to the audit log by storing it in the separate `AuditLogDbContext`.

```csharp
using MediatR;
using YourProject.AuditLog;
using YourProject.Domain.Events;
using System.Text.Json;

namespace YourProject.Domain.EventHandlers
{
    public class ProductCreatedEventHandler : INotificationHandler<ProductCreatedEvent>
    {
        private readonly AuditLogDbContext _auditLogDbContext;

        public ProductCreatedEventHandler(AuditLogDbContext auditLogDbContext)
        {
            _auditLogDbContext = auditLogDbContext;
        }

        public async Task Handle(ProductCreatedEvent notification, CancellationToken cancellationToken)
        {
            // Process the event logic
            // ...

            // Store the event in the audit log
            var auditLogEvent = new AuditLogEvent
            {
                EventType = "ProductCreated",
                Timestamp = DateTime.UtcNow,
                User = "MBARK",
                EventData = JsonSerializer.Serialize(notification)
            };

            _auditLogDbContext.AuditLogEvents.Add(auditLogEvent);
            await _auditLogDbContext.SaveChangesAsync();
        }
    }
}
```

In this example, we inject the `AuditLogDbContext` into the `ProductCreatedEventHandler`. Inside the `Handle` method, we perform the event-specific logic and create an `AuditLogEvent` object with the relevant information. We then add this event to the `AuditLogEvents` DbSet and save the changes to persist it in the audit log database.

### **4\. Configuring the AuditLogDbContext**

To ensure the `AuditLogDbContext` is configured correctly, we need to register it in the application's startup code.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...

    services.AddDbContext<AuditLogDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("AuditLogDbConnection")));

    // ...
}
```

Here, we register the `AuditLogDbContext` using the appropriate database provider and connection string. Make sure to update the connection string ("AuditLogDbConnection") to point to the desired database where the audit log events will be stored.

## **Replaying Events from the Audit Log**

After storing events in the audit log, one of the significant benefits of Event-Sourcing is the ability to replay those events to rebuild the state of the system at any given point in time. In our [ASP.NET](http://ASP.NET) Core application, we can implement event replay functionality to achieve this. Let's explore how to replay events from the audit log.

### **1\. Retrieving Events from the Audit Log**

To replay events, we need to retrieve them from the audit log database. We can create a dedicated service or class that encapsulates the logic for retrieving events.

```csharp
using System.Collections.Generic;
using YourProject.AuditLog;

namespace YourProject.EventReplay
{
    public class EventReplayService
    {
        private readonly AuditLogDbContext _auditLogDbContext;

        public EventReplayService(AuditLogDbContext auditLogDbContext)
        {
            _auditLogDbContext = auditLogDbContext;
        }

        public IEnumerable<AuditLogEvent> GetEvents()
        {
            return _auditLogDbContext.AuditLogEvents.ToList();
        }
    }
}
```

In this example, we create an `EventReplayService` that depends on the `AuditLogDbContext`. The `GetEvents` method retrieves all the events from the `AuditLogEvents` DbSet and returns them as an enumerable collection.

### **2\. Replaying Events**

Once we have retrieved the events, we can replay them to rebuild the state of the system. Typically, this involves executing the necessary event handlers or processors that update the system's state based on the events.

```csharp
using MediatR;
using YourProject.Domain.EventHandlers;
using YourProject.Domain.Events;

namespace YourProject.EventReplay
{
    public class EventReplayService
    {
        private readonly AuditLogDbContext _auditLogDbContext;
        private readonly IMediator _mediator;

        public EventReplayService(AuditLogDbContext auditLogDbContext, IMediator mediator)
        {
            _auditLogDbContext = auditLogDbContext;
            _mediator = mediator;
        }

        public async Task ReplayEvents()
        {
            var events = await _auditLogDbContext.AuditLogEvents.ToListAsync();

            foreach (var auditLogEvent in events)
            {
                // Determine the event type and execute the corresponding event handler
                switch (auditLogEvent.EventType)
                {
                    case "ProductCreated":
                        var productCreatedEvent = new ProductCreatedEvent
                        {
                            // Populate the event properties based on the event data
                            // ...
                        };
                        await _mediator.Publish(productCreatedEvent);
                        break;

                    // Handle other event types...

                    default:
                        // Unknown event type
                        break;
                }
            }
        }
    }
}
```

In this example, we modify the `EventReplayService` to use the `IMediator` interface from MediatR. Inside the `ReplayEvents` method, we iterate over the events retrieved from the audit log and determine the event type. Based on the event type, we create an instance of the corresponding event class (e.g., `ProductCreatedEvent`) and populate its properties with the relevant data from the audit log event. We then publish the event using the MediatR's `Publish` method, allowing the event handlers to process the event and update the system's state accordingly.

### 3\. Registering the EventReplayService

To register the `EventReplayService` as a service in your [ASP.NET](http://ASP.NET) Core application, you need to add it to the dependency injection container. Here's an example of how you can register the `EventReplayService` in your startup class:

```csharp
using YourProject.EventReplay;

public class Startup
{
    // Other configuration code...

    public void ConfigureServices(IServiceCollection services)
    {
        // Register the EventReplayService
        services.AddScoped<EventReplayService>();

        // Other service registrations...
        // Other configurations...
    }

    // Other methods...
}
```

In this example, the `EventReplayService` is registered using the `AddScoped` method, which creates a new instance of the service per scope. Adjust the lifetime of the service (`AddScoped`, `AddSingleton`, or `AddTransient`) based on your application's requirements.

### **4\. Triggering Event Replay**

To trigger the event replay process, you can create an endpoint or a background task that calls the `ReplayEvents` method.

```csharp
using Microsoft.AspNetCore.Mvc;
using YourProject.EventReplay;

namespace YourProject.Controllers
{
    [ApiController]
    [Route("api/eventreplay")]
    public class EventReplayController : ControllerBase
    {
        private readonly EventReplayService _eventReplayService;

        public EventReplayController(EventReplayService eventReplayService)
        {
            _eventReplayService = eventReplayService;
        }

        [HttpPost]
        public IActionResult ReplayEvents()
        {
            _eventReplayService.ReplayEvents();

            return Ok("Event replay initiated");
        }
    }
}
```

In this example, we create an `EventReplayController` with a single endpoint that triggers the event replay process by calling the `ReplayEvents` method of the `EventReplayService`. You can customize the endpoint route and authentication/authorization requirements as per your application's needs.

## **Wait, wait, What the Difference Between Publishing Events from Commands and from the ReplayEvents Method?**

In an Event-Sourcing system, events are typically published as a result of executing commands or handling domain-specific actions. However, there is a difference in the context and purpose of publishing events from commands and from the ReplayEvents method. Let's explore these differences.

### **Publishing Events from Commands**

When events are published from commands, it usually occurs during the normal flow of the application when a user or an external system triggers a command. Commands represent actions or intentions that drive the behavior of the system. When a command is executed, it may result in changes to the system's state, and events are published to capture those changes.

The process of publishing events from commands involves the following steps:

1. Command Execution: The command is executed, typically within a command handler, which performs the necessary business logic and updates the state of the system.
    
2. Event Generation: As part of the command handling process, one or more events are generated to represent the changes that occurred as a result of executing the command.
    
3. Event Publishing: The generated events are published using a suitable mechanism, such as an event bus or a mediator, to notify interested components within the system. This allows event handlers to react and update their own state or trigger further actions.
    

Publishing events from commands serves as a means of communicating the state changes and propagating them throughout the system, ensuring that other components are aware of and can respond to those changes.

### **Publishing Events from the ReplayEvents Method**

On the other hand, publishing events from the ReplayEvents method is part of the event replay process, which is distinct from the normal flow of the application. Event replay involves reconstructing the state of the system by processing previously stored events. It allows us to rebuild the state of the system at a specific point in time by replaying the events in the order they occurred.

The process of publishing events from the ReplayEvents method involves the following steps:

1. Retrieving Events: The events are retrieved from the audit log or event store, typically through a dedicated service or component responsible for handling event replay.
    
2. Event Replay: Each event is processed, often by executing the corresponding event handler(s) to update the system's state based on the event's data. This may involve executing domain-specific logic or updating aggregates/entities.
    
3. Event Publishing: As events are replayed, they are published using the same event publishing mechanism as in the command flow. The published events can trigger additional event handlers, which may update their own state or perform any necessary actions.
    

The purpose of publishing events from the ReplayEvents method is to replicate the past state changes and update the system's state accordingly. It allows us to rebuild the state of the system as if the events were happening in real-time.

## **Avoiding Duplicate Event Storage during Replay**

To avoid storing events again in the database during event replay, you can introduce an `IsReplay` property in your events. This property indicates whether the event is being replayed or not. By checking the value of this property in your event handlers, you can determine whether to store the event in the database or not. Let's modify the code for the `ProductCreatedEventHandler` to include the `IsReplay` property.

```csharp
using MediatR;
using YourProject.AuditLog;
using YourProject.Domain.Events;
using System.Text.Json;

namespace YourProject.Domain.EventHandlers
{
    public class ProductCreatedEventHandler : INotificationHandler<ProductCreatedEvent>
    {
        // The rest of code stay the same

        public async Task Handle(ProductCreatedEvent notification, CancellationToken cancellationToken)
        {
            // Process the event logic
            // ...

            if (!notification.IsReplay)
            {
                // Store the event in the audit log
                var auditLogEvent = new AuditLogEvent
                {
                    EventType = "ProductCreated",
                    Timestamp = DateTime.UtcNow,
                    User = "MBARK",
                    EventData = JsonSerializer.Serialize(notification)
                };

                _auditLogDbContext.AuditLogEvents.Add(auditLogEvent);
                await _auditLogDbContext.SaveChangesAsync();
            }
        }
    }
}
```

In this modified code, we introduce an `IsReplay` property in the `ProductCreatedEvent` class (assuming it inherits from a base event class):

```csharp
public class ProductCreatedEvent : INotification
{
    // Other event properties...

    public bool IsReplay { get; set; }
}
```

When replaying events, ensure that the `IsReplay` property is set to `true` before publishing the events. This way, when the event handlers process the events during replay, they will skip the database storage logic based on the value of `IsReplay`.

By introducing the `IsReplay` property and checking its value in the event handlers, you can control whether to store the event in the database or perform any other actions specific to replay scenarios. This allows you to differentiate between events published during the normal flow of the application and events being replayed to rebuild the system's state.

Remember to set the `IsReplay` property accordingly when publishing events during replay to ensure that the event handlers can identify and handle them correctly.

### What about ReplayEvents method?

When implementing the `ReplayEvents` method in the service, you can pass the `IsReplay` property to the events being replayed to indicate that they are part of the replay process. Here's an updated version of the `ReplayEvents` method that includes the `IsReplay` property:

```csharp
using MediatR;
using YourProject.Domain.Events;

namespace YourProject.EventReplay
{
    public class EventReplayService
    {
        private readonly AuditLogDbContext _auditLogDbContext;
        private readonly IMediator _mediator;

        public EventReplayService(AuditLogDbContext auditLogDbContext, IMediator mediator)
        {
            _auditLogDbContext = auditLogDbContext;
            _mediator = mediator;
        }

        public async Task ReplayEvents()
        {
            var events = await _auditLogDbContext.AuditLogEvents.ToListAsync();

            foreach (var auditLogEvent in events)
            {
                // Determine the event type and execute the corresponding event handler
                switch (auditLogEvent.EventType)
                {
                    case "ProductCreated":
                        var productCreatedEvent = new ProductCreatedEvent
                        {
                            // Populate the event properties based on the event data
                            // ...
                            IsReplay = true // Set IsReplay property to true during replay
                        };
                        await _mediator.Publish(productCreatedEvent);
                        break;

                    // Handle other event types...

                    default:
                        // Unknown event type
                        break;
                }
            }
        }
    }
}
```

In this updated code, when creating an instance of each event during the replay process, you set the `IsReplay` property to `true`. This indicates that the event is being replayed and should be processed differently in the event handlers, as we saw earlier in the `ProductCreatedEventHandler`.

By including the `IsReplay` property in the events and setting it to `true` during replay, you can handle them appropriately in the event handlers. This allows you to differentiate between events triggered during the normal application flow and events replayed for rebuilding the system's state.

## **Other Concepts**

In addition to implementing event sourcing with CQRS using EF Core and MediatR, there are several other important concepts and considerations that can enhance your event-driven architecture. Let's explore some of them briefly:

#### 1\. Event Versioning and Compatibility

As your application evolves, event schemas may change. It's crucial to handle event versioning and ensure backward compatibility. Consider techniques such as event versioning, event migration, or supporting multiple event versions to handle changes in event structures.

#### 2\. Eventual Consistency

Event sourcing typically relies on eventual consistency, where changes propagated through events may not be immediately reflected across all components. Understand the implications of eventual consistency and design your system accordingly. Consider techniques like sagas or compensating actions to maintain data integrity.

#### 3\. Event Serialization and Messaging

When events are published and consumed by different services, serialization and messaging protocols play a vital role. Choose appropriate serialization formats (e.g., JSON, Protobuf) and messaging frameworks (e.g., RabbitMQ, Apache Kafka) that align with your application's requirements in terms of performance, reliability, and scalability.

#### 4\. Error Handling and Resilience

Implement robust error handling mechanisms to handle failures during event processing. Consider techniques such as retry policies, dead-letter queues, or circuit breakers to ensure the system's resilience and fault tolerance.

#### 5\. Snapshotting

Snapshotting allows you to optimize the event replay process by storing periodic snapshots of aggregate/entity states. Instead of replaying all events from the beginning, you can restore the state from the nearest snapshot and replay only the subsequent events. This can significantly improve performance for systems with a large event history.

#### 6\. Eventual Eventual Consistency

In some scenarios, you may need to ensure eventual consistency not only within a single service but also across multiple services in a distributed system. Techniques like distributed transactions, message brokers, or event-driven data synchronization can be used to achieve eventual eventual consistency.

#### 7\. Event-driven Integration with External Systems

Event-driven architectures can be extended to integrate with external systems through event-driven integration patterns. Consider using concepts like event adapters, event-driven APIs, or event-driven data replication to connect and synchronize data with external systems.

## Demo Project

If you're eager to see a practical implementation of event-sourcing with CQRS using EF Core and MediatR, you're in luck! I have provided a demo project on GitHub that showcases the concepts discussed in this article. You can find the demo project at [**GitHub - EventSourcingDemo**](https://github.com/MbarkT3STO/EventSourcingDemo). Feel free to explore the code, experiment with it, and gain hands-on experience with event-driven architectures.

## Conclusion

In this article, we explored [ASP.NET](http://ASP.NET) Core, event-sourcing, CQRS, EF Core, and MediatR to build event-driven architectures. We learned how to implement event-sourcing with CQRS using these technologies and discussed concepts like event replay, versioning, serialization, messaging, resilience, and integration. By embracing event-driven architecture, we can create scalable, flexible, and maintainable systems that adapt to evolving business needs.