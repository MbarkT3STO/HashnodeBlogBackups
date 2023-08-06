---
title: "DDD: Domain Services & Application Services"
datePublished: Sun Aug 06 2023 18:28:34 GMT+0000 (Coordinated Universal Time)
cuid: clkzs1mlx00050alf8mge6ov9
slug: ddd-domain-services-application-services
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1691345706472/a9afe869-1e4d-42a2-b5d4-7c89176555f0.png
tags: software-architecture, net, software-engineering, ddd, domain-driven-design

---

Two essential concepts in DDD are Domain Services and Application Services, which play distinct roles in shaping the architecture of a DDD application. In this article, we'll delve into these concepts and provide practical examples using C#.

## Domain Services

Domain Services represent the core business logic that doesn't naturally fit within the confines of an individual entity or value object. These services encapsulate operations or processes that involve multiple domain entities and value objects, making them essential for maintaining the integrity of the domain.

Let's consider an e-commerce platform where we need to calculate the total price of an order with various products and apply discounts. Here's a simplified example of a Domain Service to achieve this:

```csharp
public class OrderPricingService
{
    public decimal CalculateTotalPrice(Order order)
    {
        decimal totalPrice = 0;

        foreach (var lineItem in order.LineItems)
        {
            totalPrice += lineItem.Product.Price * lineItem.Quantity;
        }

        // Apply discounts and other logic here

        return totalPrice;
    }
}
```

In this example, `OrderPricingService` is a Domain Service responsible for calculating the total price of an order. It collaborates with multiple entities (`Order`, `Product`, and `LineItem`) to achieve its purpose.

## Application Services

Application Services act as intermediaries between the domain layer and the external world, such as user interfaces or APIs. They orchestrate the execution of use cases, handle input validation, and coordinate interactions between domain objects.

Continuing with the e-commerce example, an Application Service might handle the process of placing an order:

```csharp
public class OrderApplicationService
{
    private readonly OrderRepository _orderRepository;
    private readonly OrderPricingService _orderPricingService;

    public OrderApplicationService(OrderRepository orderRepository, OrderPricingService orderPricingService)
    {
        _orderRepository = orderRepository;
        _orderPricingService = orderPricingService;
    }

    public void PlaceOrder(List<CartItem> cartItems)
    {
        // Create domain entities and value objects

        var order = new Order(cartItems);
        
        // Calculate total price using the Domain Service
        order.TotalPrice = _orderPricingService.CalculateTotalPrice(order);

        // Save order using the repository
        _orderRepository.Save(order);
    }
}
```

In this code, `OrderApplicationService` handles the process of placing an order. It collaborates with both the Domain Service (`OrderPricingService`) and the repository (`OrderRepository`) to fulfill the use case.

## Best Practices

While understanding the concepts of Domain Services and Application Services is crucial, following best practices can help you effectively implement them in your C# projects within the context of Domain-Driven Design.

### Domain Services Best Practices

1. **Single Responsibility:** Domain Services should have a single, clear responsibility related to the domain logic they encapsulate. Avoid mixing unrelated operations within a single Domain Service.
    
2. **Collaboration:** Domain Services often collaborate with domain entities and value objects. Keep the interactions and dependencies clear to ensure a cohesive design.
    
3. **Statelessness:** Domain Services are typically stateless and state should be maintained within the domain entities. This promotes easier testing and reusability.
    
4. **Naming Conventions:** Choose meaningful names for Domain Services that reflect the specific domain operation they perform. Clarity in naming enhances the readability of your code.
    

### Application Services Best Practices

1. **Orchestration:** Application Services orchestrate interactions between various domain objects, including Domain Services. They should focus on the flow of use cases and business processes.
    
2. **Input Validation:** Validate input parameters before invoking Domain Services. Proper validation ensures that invalid or malicious inputs don't compromise the integrity of the domain.
    
3. **Thin Controllers, Fat Services:** Keep Application Services thin by avoiding complex business logic within them. Delegate most of the domain-specific operations to Domain Services.
    
4. **Transaction Management:** Application Services often define transaction boundaries to ensure consistency when multiple domain operations need to be executed together.
    

### Dependency Injection

Both Domain Services and Application Services can benefit from dependency injection. This promotes decoupling, testability, and modularity. By injecting dependencies, you make it easier to swap out implementations or mock services during testing.

```csharp
public class OrderApplicationService
{
    private readonly IOrderRepository _orderRepository;
    private readonly IOrderPricingService _orderPricingService;

    public OrderApplicationService(IOrderRepository orderRepository, IOrderPricingService orderPricingService)
    {
        _orderRepository = orderRepository;
        _orderPricingService = orderPricingService;
    }

    // ...
}
```

## Simple Implementation Demo

Let's dive a bit deeper into how you can implement Domain Services and Application Services in a C# project using a simple scenario.

### Setting Up the Project

For this example, let's consider a library management system. We'll create a domain service to handle the process of borrowing books and an application service to coordinate the borrowing process.

1. **Create Domain Entities:**
    

```csharp
public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    // Other properties
}

public class LibraryMember
{
    public int Id { get; set; }
    public string Name { get; set; }
    // Other properties
}

public class Borrowing
{
    public int Id { get; set; }
    public Book BorrowedBook { get; set; }
    public LibraryMember Borrower { get; set; }
    public DateTime BorrowDate { get; set; }
    // Other properties
}
```

1. **Create Domain Services:**
    

```csharp
public interface IBorrowingService
{
    void BorrowBook(Book book, LibraryMember borrower);
}

public class BorrowingService : IBorrowingService
{
    public void BorrowBook(Book book, LibraryMember borrower)
    {
        // Logic to create a new Borrowing instance and save it
        // This could involve checking availability, setting due dates, etc.
    }
}
```

1. **Create Application Service:**
    

```csharp
public class LibraryApplicationService
{
    private readonly IBorrowingService _borrowingService;

    public LibraryApplicationService(IBorrowingService borrowingService)
    {
        _borrowingService = borrowingService;
    }

    public void BorrowBook(int bookId, int memberId)
    {
        // Fetch book and member entities from repositories or data store

        // Invoke the BorrowingService to perform the borrowing process
        _borrowingService.BorrowBook(book, member);
    }
}
```

### Benefits of the Setup

In this setup, the `BorrowingService` represents a Domain Service responsible for handling the complexities of the borrowing process. It ensures that domain logic related to borrowing is encapsulated in a dedicated service.

The `LibraryApplicationService` acts as an Application Service that coordinates the interaction between different parts of the system. It's responsible for handling input validation, fetching necessary data, and invoking the appropriate Domain Service to perform the required operation.

By structuring the code in this manner, you achieve separation of concerns and maintain a clear distinction between domain-specific logic and application orchestration.

## Placing Services in the Right Layers

In Domain-Driven Design (DDD), the placement of Domain Services and Application Services within the appropriate layers is crucial for maintaining a clear separation of concerns and achieving a well-structured architecture. Let's discuss where these services should reside and why.

### Domain Services

Domain Services are inherently part of the domain logic, as they encapsulate complex business operations that don't naturally fit within a single entity or value object. Therefore, Domain Services should reside within the **domain layer**.

The domain layer is responsible for representing the core business logic and is the heart of your application. Placing Domain Services here keeps the domain logic cohesive and easily accessible to other domain objects.

In terms of project structure, your domain services might be organized like this:

```csharp
MyApp.Domain
│   Book.cs
│   LibraryMember.cs
│   Borrowing.cs
└───Services
│       IBorrowingService.cs
│       BorrowingService.cs
```

### Application Services

Application Services, on the other hand, should reside within the **application layer**. The application layer is responsible for handling use cases, interacting with external systems (like UI or APIs), and orchestrating domain operations.

Placing Application Services in the application layer maintains a clear boundary between the domain logic and the interactions with the outside world. This separation ensures that the domain layer remains focused on the business logic, while the application layer manages the use cases and flow of the application.

In your project structure, Application Services could be organized as follows:

```csharp
MyApp.Application
│   LibraryApplicationService.cs
└───Services
│       IBorrowingService.cs
```

### The final Project Structure

The following is an example of how the project structure could be organized for your library management system, incorporating the Domain Services and Application Services:

```csharp
MyLibraryApp
│
├── MyApp.Domain                (Domain Layer)
│   ├── Entities
│   │   Book.cs
│   │   LibraryMember.cs
│   │   Borrowing.cs
│   ├── Services
│   │   IBorrowingService.cs
│   │   BorrowingService.cs
│
├── MyApp.Application           (Application Layer)
│   ├── Services
│   │   IBorrowingService.cs
│   ├── LibraryApplicationService.cs
│
├── MyApp.Infrastructure        (Infrastructure Layer)
│   ├── Repositories            (Data Access)
│   │   BookRepository.cs
│   │   LibraryMemberRepository.cs
│   │   BorrowingRepository.cs
│   ├── ...                     (Other infrastructure components)
│
├── MyApp.UI                    (User Interface Layer - Example)
│   ├── Controllers
│   │   BorrowController.cs
│   ├── ...
│
├── MyApp.API                   (API Layer - Example)
│   ├── Controllers
│   │   BorrowApiController.cs
│   ├── ...
│
├── MyApp.Tests                 (Tests)
│   ├── Domain
│   │   ...
│   ├── Application
│   │   ...
│   ├── Infrastructure
│   │   ...
│
└── MyApp.Shared                (Shared Components)
    ├── Models
    │   BookModel.cs
    │   LibraryMemberModel.cs
    │   ...
    ├── ...
```

In this structure:

* The **Domain Layer** (`MyApp.Domain`) contains the domain entities (`Book`, `LibraryMember`, `Borrowing`) and the domain-specific services (`IBorrowingService`, `BorrowingService`).
    
* The **Application Layer** (`MyApp.Application`) includes the application services (`LibraryApplicationService`) and interfaces that define interactions with the domain layer.
    
* The **Infrastructure Layer** (`MyApp.Infrastructure`) contains components like repositories (`BookRepository`, `LibraryMemberRepository`, `BorrowingRepository`) responsible for data access. Other infrastructure-related components could be included here as well.
    
* The **User Interface Layer** (`MyApp.UI`) and **API Layer** (`MyApp.API`) are examples of how your application might interact with users or external systems. These layers would have their own respective controllers, views (for UI), or API endpoints (for APIs).
    
* The **Tests** (`MyApp.Tests`) directory is where you would write your unit tests, categorized by layers or components being tested.
    
* The **Shared Components** (`MyApp.Shared`) directory might contain models that are shared between layers, ensuring consistency and minimizing duplication.
    

### Dependency Injection and Interfaces

To maintain flexibility and adhere to the Dependency Inversion Principle, you should define interfaces for your Domain Services and Application Services. These interfaces should be placed within the same layer as the services they represent.

For instance, the `IBorrowingService` interface should be defined in the same layer as the `BorrowingService` implementation. The same goes for the Application Services.