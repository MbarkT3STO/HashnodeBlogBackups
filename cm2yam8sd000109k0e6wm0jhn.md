---
title: "Express.js: CORS and Cross-Origin Resource Sharing"
datePublished: Fri Nov 01 2024 05:27:38 GMT+0000 (Coordinated Universal Time)
cuid: cm2yam8sd000109k0e6wm0jhn
slug: expressjs-cors-and-cross-origin-resource-sharing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730438790593/cce78a8a-f5f9-449e-82b9-1b01595dfe90.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we’ll cover **Cross-Origin Resource Sharing (CORS)** in Express.js applications. CORS is a security feature that restricts how resources on a web page can be requested from another domain. Understanding and configuring CORS is essential when building applications that need to interact with APIs from different domains.

## What is CORS?

**CORS** is a security mechanism implemented by web browsers to prevent unwanted or malicious cross-origin HTTP requests. By default, browsers block requests from one origin (domain, protocol, or port) to another unless the server explicitly allows it.

For example, if your frontend is hosted on [`http://localhost:3000`](http://localhost:3000) and it attempts to fetch data from an API at [`http://api.example.com`](http://api.example.com), the request will be blocked unless [`http://api.example.com`](http://api.example.com) has set the appropriate CORS headers.

## Enabling CORS in Express.js

To enable CORS in Express.js, you can use the `cors` middleware package, which simplifies adding the required headers to allow cross-origin requests.

### Step 1: Install the CORS Middleware

First, install the `cors` package.

```typescript
npm install cors
```

### Step 2: Configuring CORS in Your Application

In your `app.ts` file, import and use the `cors` middleware.

```typescript
import express from 'express';
import cors from 'cors';

const app = express();
app.use(cors()); // Enables CORS for all routes

app.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

By default, this configuration will allow requests from any origin. However, you can customize the settings to specify allowed origins, methods, and headers.

### Step 3: Configuring Specific CORS Options

To allow CORS only for certain origins, define the origins in the `cors` options. Here’s an example of how to allow only requests from [`http://localhost:3000`](http://localhost:3000):

```typescript
const corsOptions = {
    origin: 'http://localhost:3000',
    optionsSuccessStatus: 200
};

app.use(cors(corsOptions));
```

You can also allow multiple origins by using an array:

```typescript
const corsOptions = {
    origin: ['http://localhost:3000', 'http://example.com'],
    optionsSuccessStatus: 200
};

app.use(cors(corsOptions));
```

### Allowing Specific HTTP Methods and Headers

If you want to restrict the allowed HTTP methods or headers, configure them in the `cors` options as follows:

```typescript
const corsOptions = {
    origin: 'http://localhost:3000',
    methods: ['GET', 'POST', 'PUT'], // Allowed methods
    allowedHeaders: ['Content-Type', 'Authorization'], // Allowed headers
    optionsSuccessStatus: 200
};

app.use(cors(corsOptions));
```

## Handling CORS for Specific Routes

In some cases, you may want to apply CORS only to certain routes rather than the entire application. You can apply the `cors` middleware selectively:

```typescript
app.get('/public-data', cors(), (req, res) => {
    res.json({ message: 'This data is publicly accessible' });
});

app.get('/restricted-data', (req, res) => {
    res.json({ message: 'This data is restricted' });
});
```

In the example above, the `/public-data` route allows cross-origin requests, while `/restricted-data` does not.

## Testing CORS

To test CORS, you can use tools like **Postman** or **curl** to see if the server responds with the appropriate headers. Additionally, you can check for CORS errors in the browser's developer console.

## Common CORS Issues

1. **Missing CORS Headers**: Ensure the server includes the necessary `Access-Control-Allow-Origin` header for cross-origin requests.
    
2. **Preflight Requests**: For complex requests, browsers send an OPTIONS request before the actual request. Make sure your server can handle this.
    
3. **Incorrect Allowed Origins**: Double-check the allowed origins to ensure they match your frontend’s URL.
    

## Conclusion

Configuring CORS in Express.js allows secure cross-origin requests while preventing unwanted access. The `cors` middleware provides flexibility in controlling which origins, methods, and headers are permitted, making it easy to build secure APIs that interact with different clients.