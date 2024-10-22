---
title: "Svelte: Dealing with ASP.NET Core API"
datePublished: Tue Oct 22 2024 10:31:18 GMT+0000 (Coordinated Universal Time)
cuid: cm2kb28bx000s09mh8iid2d2f
slug: svelte-dealing-with-aspnet-core-api
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729592696346/1421b68f-cd63-48c3-98de-39d187a03cb2.png
tags: csharp, javascript, backend, frontend-development, aspnet-core, svelte

---

When building full-stack applications, it's common to integrate a frontend framework like Svelte with a backend API. [ASP.NET](http://ASP.NET) Core is a powerful backend framework that can be used to create APIs for interacting with databases, handling user authentication, and much more. In this article, we will cover how to connect a Svelte application to an [ASP.NET](http://ASP.NET) Core API, including making HTTP requests, handling responses, and managing error handling.

## Setting Up the [ASP.NET](http://ASP.NET) Core API

Before we can integrate Svelte with the [ASP.NET](http://ASP.NET) Core API, we need a functioning API. Here’s a basic example of how to set up an API using [ASP.NET](http://ASP.NET) Core.

### Step 1: Create the [ASP.NET](http://ASP.NET) Core API

1. Open a terminal and run the following command to create a new [ASP.NET](http://ASP.NET) Core Web API project:
    
    ```bash
    dotnet new webapi -n SvelteApi
    cd SvelteApi
    ```
    
2. In the `Controllers` folder, create a simple API to manage tasks (or any entity).
    

### Example: TaskController

```csharp
[ApiController]
[Route("api/[controller]")]
public class TaskController : ControllerBase
{
    private static List<string> tasks = new List<string>
    {
        "Task 1",
        "Task 2",
        "Task 3"
    };

    [HttpGet]
    public IActionResult GetTasks()
    {
        return Ok(tasks);
    }

    [HttpPost]
    public IActionResult AddTask([FromBody] string task)
    {
        tasks.Add(task);
        return Ok(tasks);
    }

    [HttpDelete("{index}")]
    public IActionResult DeleteTask(int index)
    {
        if (index >= 0 && index < tasks.Count)
        {
            tasks.RemoveAt(index);
            return Ok(tasks);
        }
        return NotFound();
    }
}
```

3. Run the API by using:
    
    ```bash
    dotnet run
    ```
    

By default, the API will be available at [`http://localhost:5000/api/task`](http://localhost:5000/api/task).

## Connecting Svelte to [ASP.NET](http://ASP.NET) Core API

Now that we have the [ASP.NET](http://ASP.NET) Core API running, let’s move to the Svelte part. We will make HTTP requests to the API to fetch, add, and delete tasks.

### Step 1: Install the `fetch` Dependency

We’ll use the native `fetch` API to send HTTP requests from the Svelte frontend.

### Step 2: Create a Svelte App

If you don’t already have a Svelte app, create one using:

```bash
npx degit sveltejs/template my-svelte-app
cd my-svelte-app
npm install
```

### Step 3: Create a Service to Interact with the API

In your Svelte project, create a `src/services/api.js` file to centralize the API calls.

```typescript
const API_URL = 'http://localhost:5000/api/task';

export const getTasks = async () => {
  const response = await fetch(API_URL);
  if (!response.ok) {
    throw new Error('Failed to fetch tasks');
  }
  return await response.json();
};

export const addTask = async (task) => {
  const response = await fetch(API_URL, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(task)
  });
  if (!response.ok) {
    throw new Error('Failed to add task');
  }
  return await response.json();
};

export const deleteTask = async (index) => {
  const response = await fetch(`${API_URL}/${index}`, {
    method: 'DELETE'
  });
  if (!response.ok) {
    throw new Error('Failed to delete task');
  }
  return await response.json();
};
```

### Step 4: Display and Manage Tasks in Svelte

Now, let’s use the service we created to display tasks and allow adding or deleting them.

#### Example: Task Management in Svelte

```svelte
<script>
  import { onMount } from 'svelte';
  import { getTasks, addTask, deleteTask } from './services/api.js';

  let tasks = [];
  let newTask = '';

  // Fetch tasks on component mount
  onMount(async () => {
    try {
      tasks = await getTasks();
    } catch (error) {
      console.error(error);
    }
  });

  // Function to add a task
  async function handleAddTask() {
    if (newTask.trim()) {
      tasks = await addTask(newTask);
      newTask = ''; // Reset input field
    }
  }

  // Function to delete a task
  async function handleDeleteTask(index) {
    tasks = await deleteTask(index);
  }
</script>

<h1>Task List</h1>

<input type="text" bind:value={newTask} placeholder="Add new task" />
<button on:click={handleAddTask}>Add Task</button>

<ul>
  {#each tasks as task, index}
    <li>
      {task} 
      <button on:click={() => handleDeleteTask(index)}>Delete</button>
    </li>
  {/each}
</ul>
```

### Explanation:

* **API Calls**: The component uses `onMount` to fetch tasks when the component loads. The `getTasks`, `addTask`, and `deleteTask` functions handle API communication.
    
* **Adding and Deleting Tasks**: Users can add a task through an input field and delete existing tasks, with the UI updating accordingly.
    

### Handling Errors

It's essential to handle errors when dealing with external APIs. In the above example, errors are logged to the console, but in a real-world scenario, you could display error messages to users.

## CORS Configuration

If you encounter CORS (Cross-Origin Resource Sharing) issues when connecting the Svelte frontend to the [ASP.NET](http://ASP.NET) Core API, you can enable CORS in the API as follows:

1. In `Startup.cs`, add the CORS policy:
    

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowAll",
            builder =>
            {
                builder.AllowAnyOrigin()
                       .AllowAnyMethod()
                       .AllowAnyHeader();
            });
    });

    services.AddControllers();
}
```

2. In the `Configure` method, use the CORS policy:
    

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseCors("AllowAll");
    app.UseRouting();
    app.UseEndpoints(endpoints => { endpoints.MapControllers(); });
}
```

## Conclusion

Integrating Svelte with an [ASP.NET](http://ASP.NET) Core API allows you to build powerful full-stack applications with modern frontend and backend technologies. By utilizing the `fetch` API, Svelte can interact with the [ASP.NET](http://ASP.NET) Core backend for data fetching and manipulation. In the next topic, we’ll explore handling authentication and authorization in Svelte using [ASP.NET](http://ASP.NET) Core API.