# CQRS in ASP.NET Core 6 with EF Core and MediatR

## **Introduction**

Command Query Responsibility Segregation (CQRS) is a design pattern that separates the responsibilities of read and write operations in a software system. This pattern can be particularly useful in large, complex systems that need to handle a high volume of read and write requests.

In this article, we'll explore how to implement CQRS in an [ASP.NET](http://ASP.NET) 6 web application using Entity Framework Core (EF Core) and MediatR.

## **Understanding CQRS**

![A Simple Step-by-Step Guide To CQRS in MVC Web API (with MediatR) - YouTube](https://i.ytimg.com/vi/bgoGtkmUAQg/maxresdefault.jpg align="center")

In a traditional software system, the same database or data store is used for both read and write operations. This can lead to bottlenecks and performance issues as the volume of requests increases.

CQRS seeks to solve this problem by separating the read and write operations into two separate models. The read model is optimized for querying data, while the write model is optimized for storing and updating data.

This separation of responsibilities allows the system to scale more efficiently, as the read model can be designed to handle a high volume of queries without affecting the write model, and vice versa.

## **Implementing CQRS in** [**ASP.NET**](http://ASP.NET) **6**

To implement CQRS in an [ASP.NET](http://ASP.NET) 6 web application, we'll use two libraries: EF Core and MediatR.

EF Core is a lightweight, cross-platform object-relational mapper (ORM) that simplifies the process of working with databases in .NET. It provides a high-level API for executing CRUD (create, read, update, delete) operations against a database.

MediatR is a library that simplifies the process of implementing the CQRS pattern in .NET. It provides a simple, abstracted API for sending and handling commands and queries.

## **Setting up EF Core**

To use EF Core in an [ASP.NET](http://ASP.NET) 6 web application, you'll need to install the `Microsoft.EntityFrameworkCore` NuGet package.

Once the package is installed, you'll need to create a `DbContext` class that represents your database. This class will contain the entity classes that map to the tables in your database, as well as any DbSet properties for querying and saving data.

For example, here's a simple `DbContext` class for a database with a `Customers` table:

```csharp
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
        : base(options)
    { }

    public DbSet<Customer> Customers { get; set; }
}
```

Next, you'll need to configure your database connection string in the `appsettings.json` file:

```csharp
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=CQRSExample;Trusted_Connection=True;"
  }
}
```

Finally, you'll need to register your `DbContext` class with the [ASP.NET](http://ASP.NET) 6 dependency injection (DI) container. This can be done in the `ConfigureServices` method of the `Startup` class:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<AppDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
}
```

## **Setting up MediatR**

To install the MediatR library in your [ASP.NET](http://ASP.NET) 6 project, you'll need to install the `MediatR` NuGet package.

Once the package is installed, you'll need to register MediatR with the DI container in the `ConfigureServices` method of the `Startup` class:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMediatR(typeof(Startup));
}
```

# **Creating Commands and Queries**

In MediatR, a command is a request to perform an action, such as creating or updating a record in the database. A query is a request for information, such as fetching a list of records from the database.

To create a command or query, you'll need to create a request class and a corresponding handler class.

For example, here's a simple command for creating a new customer record:

```csharp
public class CreateCustomerCommand : IRequest<int>
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}

public class CreateCustomerCommandHandler : IRequestHandler<CreateCustomerCommand, int>
{
    private readonly AppDbContext _context;

    public CreateCustomerCommandHandler(AppDbContext context)
    {
        _context = context;
    }

    public async Task<int> Handle(CreateCustomerCommand request, CancellationToken cancellationToken)
    {
        var customer = new Customer
        {
            FirstName = request.FirstName,
            LastName = request.LastName,
            Email = request.Email
        };

        _context.Customers.Add(customer);
        await _context.SaveChangesAsync();

        return customer.Id;
    }
}
```

Here, the `CreateCustomerCommand` class defines the request data, and the `CreateCustomerCommandHandler` class contains the logic for handling the request. The handler class uses EF Core to add the new customer record to the database and save the changes.

Similarly, here's a simple query for fetching a list of customers:

```csharp
public class GetCustomersQuery : IRequest<List<Customer>>
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

public class GetCustomersQueryHandler : IRequestHandler<GetCustomersQuery, List<Customer>>
{
    private readonly AppDbContext _context;

    public GetCustomersQueryHandler(AppDbContext context)
    {
        _context = context;
    }

    public async Task<List<Customer>> Handle(GetCustomersQuery request, CancellationToken cancellationToken)
    {
        var customers = await _context.Customers
            .Where(c => c.FirstName.Contains(request.FirstName) && c.LastName.Contains(request.LastName))
            .ToListAsync();

        return customers;
    }
}
```

Here, the `GetCustomersQuery` class defines the request data, and the `GetCustomersQueryHandler` class contains the logic for handling the request. The handler class uses EF Core to fetch the list of customers from the database based on the provided first and last names.

# **Sending Commands and Queries**

To send a command or query using MediatR, you'll need to inject an instance of the `IMediator` interface into your controller or service class. You can then use the `Send` method of the `IMediator` instance to send the request and get the response.

For example, here's how you might send the `CreateCustomerCommand` in a controller action:

```csharp
public class CustomersController : Controller
{
    private readonly IMediator _mediator;

    public CustomersController(IMediator mediator)
    {
        _mediator = mediator;
    }

    [HttpPost]
    public async Task<IActionResult> Create(CreateCustomerCommand command)
    {
        var customerId = await _mediator.Send(command);

        return RedirectToAction("Details", new { id = customerId });
    }
}
```

Here, the `Create` action uses the `Send` method of the `IMediator` instance to send the `CreateCustomerCommand` request and receive the response (the customer's ID).

Similarly, here's how you might send the `GetCustomersQuery` in a service class:

```csharp
public class CustomersService
{
    private readonly IMediator _mediator;

    public CustomersService(IMediator mediator)
    {
        _mediator = mediator;
    }

    public async Task<List<Customer>> GetCustomers(string firstName, string lastName)
    {
        var query = new GetCustomersQuery
        {
            FirstName = firstName,
            LastName = lastName
        };

        return await _mediator.Send(query);
    }
}
```

Here, the `GetCustomers` method uses the `Send` method of the `IMediator` instance to send the `GetCustomersQuery` request and receive the response (a list of customers).

# **Conclusion**

CQRS is a powerful design pattern for separating the responsibilities of read and write operations in a software system. By using EF Core and MediatR, it's easy to implement CQRS in an [ASP.NET](http://ASP.NET) 6 web application and benefit from the improved scalability and performance that it offers.