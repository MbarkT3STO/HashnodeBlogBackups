---
title: "Creating a simple CRUD ASP.NET Core API with EF Core and PostgreSQL"
datePublished: Thu Jul 27 2023 16:30:23 GMT+0000 (Coordinated Universal Time)
cuid: clkldf50000020amfdod876zu
slug: creating-a-simple-crud-aspnet-core-api-with-ef-core-and-postgresql
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690474698971/f9ff4231-84d7-41f4-b669-987741dc1e55.webp
tags: postgresql, csharp, net, aspnet-core, efcore

---

## Introduction

In this tutorial, we will walk through the process of building a CRUD (Create, Read, Update, Delete) [ASP.NET](http://ASP.NET) Core API application using Entity Framework (EF) Core and PostgreSQL as the database. [ASP.NET](http://ASP.NET) Core is a powerful and flexible framework for building web applications, and EF Core is a lightweight, cross-platform Object-Relational Mapping (ORM) tool that simplifies working with databases.

## Prerequisites

Before we begin, make sure you have the following installed on your development machine:

1. [.NET Core SDK](https://dotnet.microsoft.com/download) (version 3.1, 6 or later)
    
2. [Visual Studio](https://visualstudio.microsoft.com/) or [Visual Studio Code](https://code.visualstudio.com/) with the C# extension installed
    

## Step 1: Create a new [ASP.NET](http://ASP.NET) Core API project

Open your terminal or command prompt and execute the following command to create a new [ASP.NET](http://ASP.NET) Core API project:

```bash
dotnet new webapi -n YourProjectName
cd YourProjectName
```

This will create a new [ASP.NET](http://ASP.NET) Core API project with the name "YourProjectName."

## Step 2: Install Entity Framework Core and Npgsql

To work with PostgreSQL using EF Core, we need to install the required packages. In the terminal or command prompt, navigate to your project folder and run the following commands:

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

These commands will install the necessary EF Core and PostgreSQL packages for your project.

## Step 3: Define the model and context

In this example, let's create a simple model representing a "TodoItem." Open the `Models` folder in your project and create a new file named `TodoItem.cs`. Add the following code to define the model:

```csharp
namespace YourProjectName.Models
{
    public class TodoItem
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public bool IsComplete { get; set; }
    }
}
```

Next, let's create a new file named `AppDbContext.cs` in the `Data` folder (you may need to create the `Data` folder). Add the following code to define the database context:

```csharp
using Microsoft.EntityFrameworkCore;

namespace YourProjectName.Data
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
        {
        }

        public DbSet<TodoItem> TodoItems { get; set; }
    }
}
```

## Step 4: Configure the database connection

In the `appsettings.json` file, add the connection string to your PostgreSQL database:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Database=YourDatabaseName;Username=YourUsername;Password=YourPassword"
  },
  ...
}
```

Replace `YourDatabaseName`, `YourUsername`, and `YourPassword` with your PostgreSQL database credentials.

## Step 5: Register the database context

In the `Startup.cs` file, add the following code inside the `ConfigureServices` method to register the database context:

```csharp
using Microsoft.EntityFrameworkCore;
using YourProjectName.Data;

...

public void ConfigureServices(IServiceCollection services)
{
    // Add database context with PostgreSQL
    services.AddDbContext<AppDbContext>(options =>
        options.UseNpgsql(Configuration.GetConnectionString("DefaultConnection")));

    ...
}
```

## Step 6: Implement CRUD operations

In the `Controllers` folder, you'll find the `WeatherForecastController.cs` file. For simplicity, we'll modify this controller to implement CRUD operations for our `TodoItem`.

Replace the content of the `WeatherForecastController.cs` file with the following:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using YourProjectName.Data;
using YourProjectName.Models;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace YourProjectName.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class TodoItemsController : ControllerBase
    {
        private readonly AppDbContext _context;

        public TodoItemsController(AppDbContext context)
        {
            _context = context;
        }

        // GET: api/TodoItems
        [HttpGet]
        public async Task<ActionResult<IEnumerable<TodoItem>>> GetTodoItems()
        {
            return await _context.TodoItems.ToListAsync();
        }

        // GET: api/TodoItems/5
        [HttpGet("{id}")]
        public async Task<ActionResult<TodoItem>> GetTodoItem(int id)
        {
            var todoItem = await _context.TodoItems.FindAsync(id);

            if (todoItem == null)
            {
                return NotFound();
            }

            return todoItem;
        }

        // POST: api/TodoItems
        [HttpPost]
        public async Task<ActionResult<TodoItem>> PostTodoItem(TodoItem todoItem)
        {
            _context.TodoItems.Add(todoItem);
            await _context.SaveChangesAsync();

            return CreatedAtAction(nameof(GetTodoItem), new { id = todoItem.Id });
        }

        // PUT: api/TodoItems/5
        [HttpPut("{id}")]
        public async Task<IActionResult> PutTodoItem(int id, TodoItem todoItem)
        {
            if (id != todoItem.Id)
            {
                return BadRequest();
            }

            _context.Entry(todoItem).State = EntityState.Modified;
             await _context.SaveChangesAsync();
           
            return NoContent();
        }

        // DELETE: api/TodoItems/5
        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteTodoItem(int id)
        {
            var todoItem = await _context.TodoItems.FindAsync(id);
            if (todoItem == null)
            {
                return NotFound();
            }

            _context.TodoItems.Remove(todoItem);
            await _context.SaveChangesAsync();

            return NoContent();
        }

        private bool TodoItemExists(int id)
        {
            return _context.TodoItems.Any(e => e.Id == id);
        }
    }
}
```

## Step 7: Test the API

Now that we have implemented the CRUD operations, it's time to test the API.

1. Launch your PostgreSQL server.
    
2. Run your [ASP.NET](http://ASP.NET) Core API application by executing the following command in the terminal or command prompt:
    

```bash
dotnet run
```

1. Use tools like [Postman](https://www.postman.com/) or [Swagger](https://swagger.io/) to test the API endpoints (e.g., GET, POST, PUT, DELETE) for the `TodoItem`.
    

## Conclusion

Congratulations! You have successfully created a CRUD [ASP.NET](http://ASP.NET) Core API application with EF Core and PostgreSQL as the database. You learned how to define a model, configure the database connection, and implement CRUD operations using EF Core. Feel free to explore further and add more features to your API.

If you want to see the complete source code for a demo project like this one, you can find it on GitHub. Take a look at the project here: [ASP.NET](http://ASP.NET) [Core CRUD Demo with EF Core and PostgreSQL](https://github.com/MbarkT3STO/ASP-With-EF-Core-And-PostgreSQL-CRUD-DEMO).