---
title: "Express.js: Understanding Middleware in Express"
datePublished: Thu Oct 31 2024 19:43:05 GMT+0000 (Coordinated Universal Time)
cuid: cm2xpqhq7000209jx7mk6fzc3
slug: expressjs-understanding-middleware-in-express
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730403638307/ac7bcbb3-aa74-4b9a-a583-56dc6df48ec4.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we will explore middleware in Express.js. Middleware functions are the backbone of Express applications, enabling powerful features like request handling, response manipulation, and logging. We will cover how to create custom middleware and use built-in middleware functions.

## What is Middleware?

Middleware in Express is a function that has access to the request (`req`) and response (`res`) objects and can modify them or end the request-response cycle. Middleware can also call the next middleware function in the stack using the `next()` function.

### Types of Middleware

1. **Application-level Middleware**: Functions that are bound to an instance of the app object.
    
2. **Router-level Middleware**: Functions that are bound to an instance of `express.Router()`.
    
3. **Built-in Middleware**: Functions provided by Express, such as `express.json()` and `express.urlencoded()`.
    
4. **Third-party Middleware**: Middleware provided by the community (e.g., `morgan`, `cors`).
    
5. **Error-handling Middleware**: Functions specifically designed to handle errors.
    

## Step 1: Creating Custom Middleware

Let’s create a simple custom middleware function that logs the request method and URL.

1. **Create a New Middleware File**  
    In the `src/middleware` directory (create it if it doesn't exist), create a file named `logger.ts`.
    
    ```typescript
    // src/middleware/logger.ts
    import { Request, Response, NextFunction } from 'express';
    
    const logger = (req: Request, res: Response, next: NextFunction) => {
        console.log(`${req.method} ${req.url}`);
        next(); // Call the next middleware function
    };
    
    export default logger;
    ```
    
2. **Use the Middleware in** `server.ts`  
    Open `src/server.ts` and import the middleware:
    
    ```typescript
    import logger from './middleware/logger';
    
    // Use the logger middleware
    app.use(logger);
    ```
    

## Step 2: Using Built-in Middleware

Express comes with built-in middleware to handle various tasks. Let’s use the `express.json()` middleware to parse JSON request bodies.

1. **Add JSON Parsing Middleware**  
    Make sure to include the following line in your `server.ts` file (after initializing the app):
    
    ```typescript
    app.use(express.json());
    ```
    
    This middleware will automatically parse incoming JSON requests.
    

## Step 3: Implementing Error-handling Middleware

Error-handling middleware is a special type of middleware that takes four arguments: `err`, `req`, `res`, and `next`. Let’s create a simple error-handling middleware function.

1. **Create Error-handling Middleware**  
    Add the following error-handling middleware to the end of your `server.ts` file:
    
    ```typescript
    app.use((err: any, req: Request, res: Response, next: NextFunction) => {
        console.error(err.stack);
        res.status(500).send('Something broke!');
    });
    ```
    
    This middleware logs the error stack and sends a generic error response.
    

## Step 4: Testing Middleware

1. **Test Logging Middleware**  
    Start your server and visit various routes. You should see the method and URL of each request logged in the terminal.
    
2. **Test JSON Parsing Middleware**  
    Use Postman to send a POST request to [`http://localhost:3000/submit`](http://localhost:3000/submit) with a JSON body. The logger middleware should log the request, and the server should respond with the received data.
    
3. **Test Error-handling Middleware**  
    You can test the error-handling middleware by throwing an error intentionally. For example, add this route before the error-handling middleware:
    
    ```typescript
    app.get('/error', (req: Request, res: Response) => {
        throw new Error('This is a test error!');
    });
    ```
    
    Visiting [`http://localhost:3000/error`](http://localhost:3000/error) should trigger the error-handling middleware and display the error response.
    

## Conclusion

Middleware is a powerful feature in Express.js that allows you to handle requests and responses effectively. By creating custom middleware, utilizing built-in middleware, and implementing error-handling middleware, you can build more robust and manageable applications.