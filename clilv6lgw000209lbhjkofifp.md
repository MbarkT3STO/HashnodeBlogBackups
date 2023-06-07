---
title: "How to Implement Event-Sourcing with CQRS using EF Core and MediatR ?"
datePublished: Wed Jun 07 2023 15:28:13 GMT+0000 (Coordinated Universal Time)
cuid: clilv6lgw000209lbhjkofifp
slug: how-to-implement-event-sourcing-with-cqrs-using-ef-core-and-mediatr
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686151586243/4e07f29e-7a42-4ea3-9635-bdc45d6dd18f.png
tags: microservices, software-development, software-architecture

---

## **Introduction**

Event-sourcing is a powerful pattern that allows you to capture all changes to an application's state as a sequence of events. This approach provides a historical view of the application's state and enables you to reconstruct past states at any point in time. When combined with the Command Query Responsibility Segregation (CQRS) pattern, event-sourcing becomes even more potent. In this article, we will explore how to implement event-sourcing with CQRS using Entity Framework Core (EF Core), and MediatR.

## **What is Event-Sourcing and CQRS?**

Event-sourcing is a pattern that represents the state of an application as a series of events. Instead of storing the current state of an object, you store a log of events that have occurred, which can be replayed to reconstruct the state at any given point in time. Each event represents a discrete change in the application's state and is immutable.

CQRS, on the other hand, is a pattern that separates the read and write operations in an application. It distinguishes between commands (requests that modify state) and queries (requests that retrieve state). By segregating these concerns, you can optimize the read and write models independently, leading to better performance and scalability.

## **Implementing Event-Sourcing with CQRS**

To implement event-sourcing with CQRS follow these steps:

### **Step 1: Set Up the Project**

Create a new C# project in your preferred development environment. Install the necessary NuGet packages:

* EF Core: `Microsoft.EntityFrameworkCore`
    
* MediatR: [`MediatR.Extensions.Microsoft`](http://MediatR.Extensions.Microsoft)`.DependencyInjection`
    

### **Step 2: Define the Events**

Start by defining the events that will represent the changes in your application's state. Each event should be a simple POCO (Plain Old CLR Object) class with properties that describe the change. For example:

```csharp
public class OrderCreatedEvent
{
    public Guid OrderId { get; set; }
    public string CustomerName { get; set; }
    // Additional event properties
}

public class OrderShippedEvent
{
    public Guid OrderId { get; set; }
    public DateTime ShippedDate { get; set; }
    // Additional event properties
}

// Define other events as needed
```

### **Step 3: Create the Event Store**

The event store is responsible for persisting the events and providing methods to retrieve and store events. It can be implemented using a database, file system, or any other persistent storage. For simplicity, we'll use EF Core and a database as the event store.

Create a new class, `EventStoreContext`, that extends `DbContext` from EF Core. Define a `DbSet` for each event type:

```csharp
public class EventStoreContext : DbContext
{
    public DbSet<OrderCreatedEvent> OrderCreatedEvents { get; set; }
    public DbSet<OrderShippedEvent> OrderShippedEvents { get; set; }

    // Other event DbSets

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // Configure the database connection here
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Define any required configurations or constraints here
    }
}
```

### Explain the `order.Apply(orderCreatedEvent)` method

The `order.Apply(orderCreatedEvent)` method is a hypothetical method that is used in the example to demonstrate how events can be applied to mutate the state of an entity in the write model.

In event-sourcing, events represent changes to an application's state. Instead of directly modifying the properties of an object, events are used to capture the intent or action that leads to a state change. By applying events to an entity, we can reconstruct the state of the entity by replaying the events in the correct order.

The `Apply` method in the `Order` class is a custom method that takes an event as a parameter and applies the event to mutate the state of the order entity. The implementation of this method will depend on the specific requirements and logic of your application.

For example, in the context of the `Order` class, the `Apply` method for the `OrderCreatedEvent` might update properties of the order entity such as `CustomerId` or `OrderDate`. The method could look like this:

```csharp
private void Apply(OrderCreatedEvent @event)
{
    OrderId = @event.OrderId;
    CustomerName = @event.CustomerName;
    // Additional logic or property updates based on the event
}
```

The `Apply` method allows the entity to handle the event and update its state accordingly. By applying the events in the correct order, the entity can be reconstructed to its current state or any past state by replaying the events from the event store.

**Note** that the implementation of event application logic can vary depending on the complexity of the domain model and the specific requirements of your application.

### **Step 4: Implement the Write Model**

The write model is responsible for handling commands and applying the corresponding events to mutate the application's state. Create a new class, `Order`, which represents an order entity:

```csharp
public class Order
{
    // Define properties and fields for the order entity
    // Example: public Guid OrderId { get; private set; }
    
    // Define methods to apply events and mutate the state
    // Example: private void Apply(OrderCreatedEvent @event) { ... }
}
```

Implement the command handlers using MediatR. Each command handler should retrieve the corresponding entity from the write model, apply the required events, and save the changes to the event store. For example:

```csharp
public class CreateOrderCommandHandler : IRequestHandler<CreateOrderCommand>
{
    private readonly EventStoreContext _eventStoreContext;

    public CreateOrderCommandHandler(EventStoreContext eventStoreContext)
    {
        _eventStoreContext = eventStoreContext;
    }

    public async Task<Unit> Handle(CreateOrderCommand request, CancellationToken cancellationToken)
    {
        // Retrieve the order entity from the event store
        var order = await _eventStoreContext.Orders.FirstOrDefaultAsync(o => o.OrderId == request.OrderId, cancellationToken);

        // Apply the OrderCreatedEvent to mutate the order entity
        var orderCreatedEvent = new OrderCreatedEvent
        {
            OrderId = request.OrderId,
            CustomerName = request.CustomerName
        };
        order.Apply(orderCreatedEvent);

        // Store the OrderCreatedEvent in the event store
        await _eventStoreContext.OrderCreatedEvents.AddAsync(orderCreatedEvent, cancellationToken);

        // Save the changes to the event store
        await _eventStoreContext.SaveChangesAsync(cancellationToken);

        return Unit.Value;
    }
}

// Implement other command handlers similarly
```

### **Step 5: Implement the Read Model**

The read model is responsible for handling queries and retrieving the application's state. Create a new class, `OrderViewModel`, which represents the read model for the order entity:

```csharp
public class OrderViewModel
{
    // Define properties for the order view model
    // Example: public Guid OrderId { get; set; }
}
```

Implement the query handlers using MediatR. Each query handler should retrieve the required data from the read model and return the result. For example:

```csharp
public class GetOrderQueryHandler : IRequestHandler<GetOrderQuery, OrderViewModel>
{
    private readonly EventStoreContext _eventStoreContext;

    public GetOrderQueryHandler(EventStoreContext eventStoreContext)
    {
        _eventStoreContext = eventStoreContext;
    }

    public async Task<OrderViewModel> Handle(GetOrderQuery request, CancellationToken cancellationToken)
    {
        // Retrieve the order view model from the event store or a dedicated read model database
        var order = await _eventStoreContext.Orders.FirstOrDefaultAsync(o => o.OrderId == request.OrderId, cancellationToken);

        // Map the order entity to the order view model
        var orderViewModel = new OrderViewModel
        {
            OrderId = order.OrderId
        };

        return orderViewModel;
    }
}

// Implement other query handlers similarly
```

### **Step 6: Configure Dependency Injection**

Configure the dependency injection container (e.g., using the built-in `IServiceCollection` in [ASP.NET](http://ASP.NET) Core) to register the necessary services and handlers. For example:

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDbContext<EventStoreContext>();

        services.AddMediatR(typeof(Startup).Assembly);

        // Register other services and handlers as needed
    }
}
```

## **Conclusion**

Event-sourcing with CQRS is a powerful pattern for building scalable and maintainable applications. By implementing event-sourcing with CQRS using C#, EF Core, and MediatR, you can effectively capture and manage changes to your application's state. The combination of event-sourcing and CQRS allows for flexibility, scalability, and a historical view of your application's data.