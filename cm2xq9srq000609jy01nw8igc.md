---
title: "Express.js: Working with Query Parameters and URL Parameters"
datePublished: Thu Oct 31 2024 19:58:05 GMT+0000 (Coordinated Universal Time)
cuid: cm2xq9srq000609jy01nw8igc
slug: expressjs-working-with-query-parameters-and-url-parameters
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730404510896/b430c17a-a02d-40a5-87a0-d3775abed486.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we will explore how to work with query parameters and URL parameters in Express.js. Understanding how to handle these parameters is crucial for creating dynamic applications that respond to user input.

## What Are Query Parameters?

Query parameters are a way to pass additional data to the server via the URL. They are appended to the URL after a question mark (`?`) and are separated by ampersands (`&`).

### Example of Query Parameters

A typical URL with query parameters looks like this:

```typescript
http://localhost:3000/search?query=express&type=framework
```

In this example, `query` and `type` are the query parameters.

### Accessing Query Parameters in Express

To access query parameters in an Express route, you can use `req.query`.

#### Example:

```typescript
import express, { Request, Response } from 'express';

const app = express();
const PORT = 3000;

app.get('/search', (req: Request, res: Response) => {
    const { query, type } = req.query;
    res.send(`Search query: ${query}, Type: ${type}`);
});

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

If you navigate to [`http://localhost:3000/search?query=express&type=framework`](http://localhost:3000/search?query=express&type=framework), the response will be:

```typescript
Search query: express, Type: framework
```

## What Are URL Parameters?

URL parameters, also known as route parameters, are defined in the route path. They are part of the URL and can be used to capture specific values.

### Example of URL Parameters

A URL with URL parameters looks like this:

```typescript
http://localhost:3000/users/123
```

Here, `123` is a URL parameter representing a user ID.

### Accessing URL Parameters in Express

To access URL parameters, you can use `req.params`.

#### Example:

```typescript
app.get('/users/:id', (req: Request, res: Response) => {
    const userId = req.params.id;
    res.send(`User ID: ${userId}`);
});
```

If you navigate to [`http://localhost:3000/users/123`](http://localhost:3000/users/123), the response will be:

```typescript
User ID: 123
```

## Combining Query and URL Parameters

You can combine both query and URL parameters in a single route to create more dynamic endpoints.

### Example:

```typescript
app.get('/users/:id/orders', (req: Request, res: Response) => {
    const userId = req.params.id;
    const status = req.query.status;
    res.send(`User ID: ${userId}, Order Status: ${status}`);
});
```

If you navigate to [`http://localhost:3000/users/123/orders?status=pending`](http://localhost:3000/users/123/orders?status=pending), the response will be:

```typescript
User ID: 123, Order Status: pending
```

## Validation and Handling Missing Parameters

When working with parameters, it's essential to validate their presence and type. You can use middleware or inline validation for this purpose.

### Example of Validation:

```typescript
app.get('/users/:id', (req: Request, res: Response) => {
    const userId = req.params.id;

    if (!userId) {
        return res.status(400).send('User ID is required');
    }

    // Further processing
    res.send(`User ID: ${userId}`);
});
```