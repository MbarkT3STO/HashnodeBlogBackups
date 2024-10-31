---
title: "Express.js: Routing Basics"
datePublished: Thu Oct 31 2024 19:51:01 GMT+0000 (Coordinated Universal Time)
cuid: cm2xq0p1u000009l4huzl42py
slug: expressjs-routing-basics
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730404131048/d2e927ac-69c8-41dd-a5e0-2c3746515b04.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we will cover the basics of routing in Express.js. Routing is a fundamental part of any web application as it allows you to define how your application responds to different HTTP requests at specific URLs.

## What is Routing?

Routing refers to the mechanism that determines how an application responds to client requests for specific endpoints, which are typically defined by a URL and an HTTP method.

### Setting Up Basic Routes

In your Express application, you can define routes using the app object. Each route can have one or more handler functions that are executed when the route is matched.

1. **Basic Syntax for Defining Routes**  
    The basic syntax for defining a route is as follows:
    
    ```typescript
    app.METHOD(PATH, HANDLER);
    ```
    
    * **METHOD**: The HTTP method (GET, POST, PUT, DELETE, etc.).
        
    * **PATH**: The URL path for the route.
        
    * **HANDLER**: A function that will be executed when the route is matched.
        

### Example of Defining Routes

Here’s how to define some basic routes in your `server.ts` file:

```typescript
import express, { Request, Response } from 'express';

const app = express();
const PORT = 3000;

// GET route
app.get('/', (req: Request, res: Response) => {
    res.send('Welcome to the Express App!');
});

// POST route
app.post('/submit', (req: Request, res: Response) => {
    // Assume req.body contains data from a form submission
    res.send('Form submitted successfully!');
});

// PUT route
app.put('/update', (req: Request, res: Response) => {
    res.send('Resource updated successfully!');
});

// DELETE route
app.delete('/delete', (req: Request, res: Response) => {
    res.send('Resource deleted successfully!');
});

// Start the server
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

## Route Parameters

Express also supports route parameters, which are named segments in the URL that can capture values. They are defined with a colon (`:`) prefix.

### Example of Route Parameters

```typescript
app.get('/users/:id', (req: Request, res: Response) => {
    const userId = req.params.id; // Access the route parameter
    res.send(`User ID is: ${userId}`);
});
```

In this example, if you navigate to `/users/123`, `userId` will be `'123'`.

## Query Parameters

Query parameters are another way to pass data to your routes. They are appended to the URL after a question mark (`?`) and separated by ampersands (`&`).

### Example of Query Parameters

```typescript
app.get('/search', (req: Request, res: Response) => {
    const query = req.query.q; // Access the query parameter
    res.send(`Search query: ${query}`);
});
```

For example, if you navigate to `/search?q=express`, `query` will be `'express'`.

## Chaining Route Handlers

You can also chain multiple route handlers for a specific route:

```typescript
app.route('/products')
    .get((req: Request, res: Response) => {
        res.send('Get all products');
    })
    .post((req: Request, res: Response) => {
        res.send('Add a new product');
    });
```

## Conclusion

In this article, we covered the basics of routing in Express.js, including how to define routes, use route parameters, and handle query parameters. Understanding routing is essential for building dynamic and responsive web applications.