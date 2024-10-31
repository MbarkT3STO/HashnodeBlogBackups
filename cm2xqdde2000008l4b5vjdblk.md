---
title: "Express.js: Using JSON and URL-encoded Data"
datePublished: Thu Oct 31 2024 20:00:52 GMT+0000 (Coordinated Universal Time)
cuid: cm2xqdde2000008l4b5vjdblk
slug: expressjs-using-json-and-url-encoded-data
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730404734110/fd00c8f1-892d-4587-bbbf-36d43c6c472c.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we will explore how to handle JSON and URL-encoded data in Express.js applications. Understanding how to parse and work with different data formats is essential for building robust APIs and web applications.

## What is JSON?

JSON (JavaScript Object Notation) is a lightweight data interchange format that is easy to read and write for humans and machines. It is commonly used to transmit data between a server and a web application.

### Example of JSON Data

Here’s an example of JSON data:

```typescript
{
    "name": "John Doe",
    "age": 30,
    "email": "john@example.com"
}
```

## Parsing JSON Data in Express

To handle JSON data in Express, you can use the built-in middleware `express.json()`, which parses incoming requests with JSON payloads.

### Example of Parsing JSON Data

```typescript
import express, { Request, Response } from 'express';

const app = express();
const PORT = 3000;

// Use express.json() middleware to parse JSON data
app.use(express.json());

app.post('/user', (req: Request, res: Response) => {
    const user = req.body; // Access the parsed JSON data
    res.send(`User created: ${JSON.stringify(user)}`);
});

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

In this example, when you send a POST request to `/user` with a JSON body, Express will parse the JSON and make it available in `req.body`.

## What is URL-encoded Data?

URL-encoded data is a way to encode data for transmission via URLs. It is commonly used when submitting form data. The data is formatted as key-value pairs, similar to query parameters.

### Example of URL-encoded Data

A typical URL-encoded string looks like this:

```typescript
name=John+Doe&age=30&email=john%40example.com
```

## Parsing URL-encoded Data in Express

To handle URL-encoded data, you can use the built-in middleware `express.urlencoded()`, which parses incoming requests with URL-encoded payloads.

### Example of Parsing URL-encoded Data

```typescript
// Use express.urlencoded() middleware to parse URL-encoded data
app.use(express.urlencoded({ extended: true }));

app.post('/user', (req: Request, res: Response) => {
    const user = req.body; // Access the parsed URL-encoded data
    res.send(`User created: ${JSON.stringify(user)}`);
});
```

In this example, when you send a POST request to `/user` with URL-encoded form data, Express will parse it and make it available in `req.body`.

## Using Both JSON and URL-encoded Data

You can use both JSON and URL-encoded data parsing middleware in your application. This allows your endpoints to handle different types of data.

### Example of Using Both Middleware

```typescript
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.post('/data', (req: Request, res: Response) => {
    const jsonData = req.body; // Can be either JSON or URL-encoded
    res.send(`Received data: ${JSON.stringify(jsonData)}`);
});
```