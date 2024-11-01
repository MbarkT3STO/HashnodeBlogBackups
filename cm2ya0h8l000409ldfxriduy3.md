---
title: "Express.js: Building a RESTful API"
datePublished: Fri Nov 01 2024 05:10:43 GMT+0000 (Coordinated Universal Time)
cuid: cm2ya0h8l000409ldfxriduy3
slug: expressjs-building-a-restful-api
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730437759771/94da5e00-f639-47b8-8811-afe9cc27a539.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we’ll explore how to build a RESTful API using Express.js. RESTful APIs are a standard way to create web services that allow clients to interact with server resources via HTTP requests, using operations like `GET`, `POST`, `PUT`, and `DELETE`.

## What is a RESTful API?

A **RESTful API** (Representational State Transfer) is a type of web service that follows REST principles, focusing on:

* **Stateless interactions**: Each request from the client contains all the information the server needs to fulfill the request.
    
* **Resource-based**: Each endpoint represents a resource, such as a user or a product.
    
* **Standard HTTP methods**: Uses `GET`, `POST`, `PUT`, and `DELETE` for CRUD (Create, Read, Update, Delete) operations.
    

## Setting Up the Project

### Step 1: Initialize the Project and Install Dependencies

Start by initializing a new Node.js project and installing Express.

```typescript
npm init -y
npm install express
```

### Step 2: Set Up Basic Server

Create an `app.ts` file and set up a simple Express server.

```typescript
import express, { Request, Response } from 'express';

const app = express();
const PORT = 3000;

app.use(express.json());

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

## Defining RESTful Routes for a Resource

Let’s create routes for a `Product` resource to handle CRUD operations.

### Step 3: Create Routes for CRUD Operations

Define the routes in `app.ts` for each CRUD operation.

1. **GET /products** - Get all products
    
2. **POST /products** - Create a new product
    
3. **GET /products/**\- Get a specific product by ID
    
4. **PUT /products/**\- Update a product by ID
    
5. **DELETE /products/**\- Delete a product by ID
    

### Implementing Each Route

```typescript
interface Product {
    id: number;
    name: string;
    price: number;
}

let products: Product[] = [
    { id: 1, name: 'Laptop', price: 1000 },
    { id: 2, name: 'Phone', price: 500 }
];

// Get all products
app.get('/products', (req: Request, res: Response) => {
    res.json(products);
});

// Get product by ID
app.get('/products/:id', (req: Request, res: Response) => {
    const id = parseInt(req.params.id);
    const product = products.find(p => p.id === id);
    
    if (product) {
        res.json(product);
    } else {
        res.status(404).json({ error: 'Product not found' });
    }
});

// Create a new product
app.post('/products', (req: Request, res: Response) => {
    const newProduct: Product = {
        id: products.length + 1,
        name: req.body.name,
        price: req.body.price
    };
    products.push(newProduct);
    res.status(201).json(newProduct);
});

// Update a product by ID
app.put('/products/:id', (req: Request, res: Response) => {
    const id = parseInt(req.params.id);
    const product = products.find(p => p.id === id);

    if (product) {
        product.name = req.body.name;
        product.price = req.body.price;
        res.json(product);
    } else {
        res.status(404).json({ error: 'Product not found' });
    }
});

// Delete a product by ID
app.delete('/products/:id', (req: Request, res: Response) => {
    const id = parseInt(req.params.id);
    const productIndex = products.findIndex(p => p.id === id);

    if (productIndex > -1) {
        products.splice(productIndex, 1);
        res.status(204).send();
    } else {
        res.status(404).json({ error: 'Product not found' });
    }
});
```

## Testing the API

### Using Postman or Curl

To test each endpoint, you can use Postman or Curl. Here are the endpoints we covered:

1. **GET** `/products`: Retrieves all products.
    
2. **POST** `/products`: Creates a new product. Example JSON body:
    
    ```typescript
    { "name": "Tablet", "price": 600 }
    ```
    
3. **GET** `/products/:id`: Retrieves a specific product by ID.
    
4. **PUT** `/products/:id`: Updates a specific product by ID. Example JSON body:
    
    ```typescript
    { "name": "Updated Product", "price": 700 }
    ```
    
5. **DELETE** `/products/:id`: Deletes a specific product by ID.
    

## Conclusion

In this article, we created a basic RESTful API with Express.js, handling CRUD operations for a `Product` resource. This setup forms the foundation of REST APIs, allowing clients to interact with server resources.