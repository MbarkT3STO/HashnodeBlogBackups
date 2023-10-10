---
title: "DDD: Application Services"
datePublished: Tue Oct 10 2023 16:12:43 GMT+0000 (Coordinated Universal Time)
cuid: clnkitaoe000b09jv5vjq1o7n
slug: ddd-application-services
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1696954146835/1cc8248f-696f-4cf1-925a-b2e6c3c417d1.png
tags: csharp, software-architecture, net, ddd, domain-driven-design

---

Domain-Driven Design (DDD)offers a set of principles and patterns to help developers create complex software systems that closely align with the business domain. In this blog post, we will explore one important concept of Application Services, and provide examples using C#.

## What are Application Services?

Application Services are an integral part of the DDD architectural style. They act as the entry point for handling application-specific operations and orchestrate the interactions between various domain objects and infrastructure layers. Application Services encapsulate business logic and expose a coarse-grained interface to the application clients.

The primary responsibility of an Application Service is to coordinate the execution of multiple domain objects and entities to fulfill a specific use case or business operation. It serves as a boundary between the presentation layer (UI) and the domain layer. Application Services are often used to implement use cases defined in the domain and coordinate the actions of multiple domain objects to achieve the desired outcome.

## Examples of Application Services in C#

Let's consider an example of an e-commerce application that manages orders and inventory. We'll focus on two Application Services: `OrderApplicationService` and `InventoryApplicationService`.

### OrderApplicationService

```csharp
public class OrderApplicationService
{
    private readonly IOrderRepository _orderRepository;
    private readonly IInventoryRepository _inventoryRepository;

    public OrderApplicationService(IOrderRepository orderRepository, IInventoryRepository inventoryRepository)
    {
        _orderRepository = orderRepository;
        _inventoryRepository = inventoryRepository;
    }

    public void PlaceOrder(Guid productId, int quantity)
    {
        var product = _inventoryRepository.GetProduct(productId);
        var availableQuantity = _inventoryRepository.GetAvailableQuantity(productId);

        if (availableQuantity >= quantity)
        {
            var order = new Order(Guid.NewGuid(), product, quantity);
            _orderRepository.Save(order);

            _inventoryRepository.DecreaseQuantity(productId, quantity);

            // Other operations like sending notifications, updating metrics, etc.
        }
        else
        {
            throw new InvalidOperationException("Insufficient quantity available.");
        }
    }
}
```

In the `OrderApplicationService`, the `PlaceOrder` method is responsible for handling the workflow of placing an order. It retrieves the product details and available quantity from the `IInventoryRepository`. If there is sufficient quantity available, it creates a new `Order` object and saves it using the `IOrderRepository`. Additionally, it decreases the quantity in the inventory by the ordered amount.

### InventoryApplicationService

```csharp
public class InventoryApplicationService
{
    private readonly IInventoryRepository _inventoryRepository;

    public InventoryApplicationService(IInventoryRepository inventoryRepository)
    {
        _inventoryRepository = inventoryRepository;
    }

    public void AddProduct(Guid productId, string name, decimal price)
    {
        var product = new Product(productId, name, price);
        _inventoryRepository.SaveProduct(product);
    }

    public void UpdateProduct(Guid productId, string name, decimal price)
    {
        var product = _inventoryRepository.GetProduct(productId);

        if (product != null)
        {
            product.UpdateDetails(name, price);
            _inventoryRepository.SaveProduct(product);
        }
    }
}
```

The `InventoryApplicationService` is responsible for managing the inventory. It provides methods to add a new product and update the details of an existing product. It interacts with the `IInventoryRepository` to persist the changes.

## Can we Replace Application Services with CQRS?

CQRS (Command Query Responsibility Segregation) is another architectural pattern that has gained popularity in the software development community. It advocates for the separation of commands (write operations) and queries (read operations) into separate models, with the goal of improving performance, scalability, and maintainability. Given the overlap in responsibilities between Application Services and CQRS, it's natural to question whether Application Services can be replaced entirely by CQRS. Let's explore this further.

### Understanding CQRS

In a CQRS-based architecture, the command side is responsible for handling write operations and updating the system's state, while the query side is responsible for retrieving data in a denormalized and optimized format. The two sides communicate through explicit messages, often using message queues or event-driven architectures.

CQRS can be a powerful pattern for complex systems with high write and read loads. However, it's important to note that CQRS focuses on handling individual commands or queries, whereas Application Services are designed to encapsulate and orchestrate entire use cases or business operations.

### Complementary Nature

While CQRS and Application Services share some similarities in terms of handling commands and encapsulating business logic, they serve different purposes and can be complementary to each other.

CQRS can be used within the Application Services to handle the individual commands or queries and update the system's state. The Application Services, on the other hand, provide a higher-level abstraction that coordinates the execution of multiple commands or queries to fulfill a specific use case.

### Example: Using CQRS within Application Services

Let's revisit our e-commerce example and see how CQRS can be used in conjunction with Application Services.

```csharp
public class OrderApplicationService
{
    private readonly ICommandBus _commandBus;
    private readonly IQueryBus _queryBus;

    public OrderApplicationService(ICommandBus commandBus, IQueryBus queryBus)
    {
        _commandBus = commandBus;
        _queryBus = queryBus;
    }

    public void PlaceOrder(Guid productId, int quantity)
    {
        var product = _queryBus.Send(new GetProductQuery(productId));
        var availableQuantity = _queryBus.Send(new GetAvailableQuantityQuery(productId));

        if (availableQuantity >= quantity)
        {
            var placeOrderCommand = new PlaceOrderCommand(productId, quantity);
            _commandBus.Send(placeOrderCommand);

            // Other operations like sending notifications, updating metrics, etc.
        }
        else
        {
            throw new InvalidOperationException("Insufficient quantity available.");
        }
    }
}
```

In this modified version of `OrderApplicationService`, CQRS is used to send queries (`GetProductQuery` and `GetAvailableQuantityQuery`) to the query side, which retrieves the necessary data. Then, a command (`PlaceOrderCommand`) is sent to the command side via the command bus (`ICommandBus`) to perform the actual order placement.

By leveraging CQRS within the Application Services, we can achieve the benefits of both patterns. The commands and queries are handled in a separate model, improving scalability and performance, while the Application Services provide a cohesive and high-level abstraction for coordinating complex use cases.