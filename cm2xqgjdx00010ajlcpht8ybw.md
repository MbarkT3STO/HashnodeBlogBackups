---
title: "Express.js: Error Handling in Express Applications"
datePublished: Thu Oct 31 2024 20:03:20 GMT+0000 (Coordinated Universal Time)
cuid: cm2xqgjdx00010ajlcpht8ybw
slug: expressjs-error-handling-in-express-applications
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730404895314/074eeb6f-896a-418a-a4db-59e5e32339e6.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we will discuss how to effectively handle errors in Express.js applications. Proper error handling is essential for maintaining a smooth user experience and debugging issues that may arise in your application.

## Why Error Handling is Important

Error handling is crucial because it allows your application to gracefully handle unexpected situations, providing meaningful feedback to users and developers. This helps in identifying issues quickly and ensures the application continues to run smoothly.

## Types of Errors in Express.js

There are generally two types of errors you may encounter in Express.js applications:

1. **Synchronous Errors**: These are errors that occur during the execution of code and can be caught using try-catch blocks.
    
2. **Asynchronous Errors**: These errors arise from asynchronous operations, such as database queries or API calls. They typically require handling with Promises or async/await syntax.
    

## Basic Error Handling

Express provides a default error-handling middleware function that can be used to catch errors. To set it up, define an error-handling middleware function after your route definitions.

### Example of Basic Error Handling

```typescript
import express, { Request, Response, NextFunction } from 'express';

const app = express();
const PORT = 3000;

// Middleware to simulate an error
app.get('/error', (req: Request, res: Response, next: NextFunction) => {
    const err = new Error('Something went wrong!');
    next(err); // Pass the error to the next middleware
});

// Error-handling middleware
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
    console.error(err.message); // Log the error for debugging
    res.status(500).send('Internal Server Error');
});

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

In this example, when you navigate to [`http://localhost:3000/error`](http://localhost:3000/error), the application will log the error message and respond with a 500 status code.

## Handling Errors in Asynchronous Code

When dealing with asynchronous code, it's essential to handle errors properly. You can use try-catch blocks with async/await or handle Promise rejections.

### Example of Handling Asynchronous Errors

```typescript
app.get('/async-error', async (req: Request, res: Response, next: NextFunction) => {
    try {
        // Simulate an asynchronous operation that throws an error
        await Promise.reject(new Error('Async error occurred!'));
    } catch (err) {
        next(err); // Pass the error to the next middleware
    }
});
```

## Custom Error Handling

You can create custom error classes to differentiate between different types of errors, such as validation errors or database errors.

### Example of Custom Error Handling

```typescript
class AppError extends Error {
    statusCode: number;
    isOperational: boolean;

    constructor(message: string, statusCode: number) {
        super(message);
        this.statusCode = statusCode;
        this.isOperational = true;
    }
}

app.get('/custom-error', (req: Request, res: Response, next: NextFunction) => {
    const error = new AppError('Custom error occurred!', 400);
    next(error);
});

// Custom error-handling middleware
app.use((err: AppError, req: Request, res: Response, next: NextFunction) => {
    const status = err.isOperational ? err.statusCode : 500;
    res.status(status).send(err.message);
});
```