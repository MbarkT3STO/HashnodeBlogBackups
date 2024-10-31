---
title: "Express.js: Handling HTTP Requests and Responses"
datePublished: Thu Oct 31 2024 19:46:20 GMT+0000 (Coordinated Universal Time)
cuid: cm2xpuopd000109jsf7zf1i46
slug: expressjs-handling-http-requests-and-responses
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730403850530/8c053200-3f32-45a9-82df-bd18195752d6.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we will dive into handling HTTP requests and responses in Express.js. Understanding how to work with different HTTP methods, manage request data, and send responses is crucial for building robust web applications.

## Overview of HTTP Methods

HTTP defines several methods (or verbs) that indicate the desired action to be performed on a resource. The most common methods include:

* **GET**: Retrieve data from the server.
    
* **POST**: Submit data to the server, often resulting in a change on the server.
    
* **PUT**: Update existing data on the server.
    
* **DELETE**: Remove data from the server.
    

## Step 1: Setting Up Routes for Different HTTP Methods

Let’s start by adding routes for each of these HTTP methods in our `server.ts` file.

### Adding a GET Route

```typescript
app.get('/items', (req: Request, res: Response) => {
    res.json([{ id: 1, name: 'Item 1' }, { id: 2, name: 'Item 2' }]);
});
```

### Adding a POST Route

```typescript
app.post('/items', (req: Request, res: Response) => {
    const newItem = req.body; // Get the new item from the request body
    // Here you would typically save the item to a database
    res.status(201).json({
        message: 'Item created successfully',
        item: newItem,
    });
});
```

### Adding a PUT Route

```typescript
app.put('/items/:id', (req: Request, res: Response) => {
    const itemId = req.params.id; // Get the item ID from the URL
    const updatedItem = req.body; // Get the updated item data
    // Here you would typically update the item in the database
    res.json({
        message: 'Item updated successfully',
        itemId: itemId,
        updatedData: updatedItem,
    });
});
```

### Adding a DELETE Route

```typescript
app.delete('/items/:id', (req: Request, res: Response) => {
    const itemId = req.params.id; // Get the item ID from the URL
    // Here you would typically delete the item from the database
    res.json({
        message: 'Item deleted successfully',
        itemId: itemId,
    });
});
```

## Step 2: Testing Your Routes

You can test your newly created routes using Postman or any API testing tool.

1. **GET Request**  
    Send a GET request to [`http://localhost:3000/items`](http://localhost:3000/items). You should receive a JSON response with the list of items.
    
2. **POST Request**  
    Send a POST request to [`http://localhost:3000/items`](http://localhost:3000/items) with a JSON body, such as:
    
    ```typescript
    {
        "name": "New Item"
    }
    ```
    
    You should receive a success message with the created item data.
    
3. **PUT Request**  
    Send a PUT request to [`http://localhost:3000/items/1`](http://localhost:3000/items/1) with updated data:
    
    ```typescript
    {
        "name": "Updated Item"
    }
    ```
    
    You should receive a success message indicating that the item has been updated.
    
4. **DELETE Request**  
    Send a DELETE request to [`http://localhost:3000/items/1`](http://localhost:3000/items/1). You should receive a success message confirming the item deletion.
    

## Step 3: Understanding Request and Response Objects

The `req` (request) and `res` (response) objects contain useful properties and methods for handling requests and responses.

### Request Object (`req`)

* `req.params`: Contains route parameters (e.g., from `/items/:id`).
    
* `req.query`: Contains query string parameters (e.g., from `/items?sort=asc`).
    
* `req.body`: Contains data sent in the request body (for POST and PUT requests).
    

### Response Object (`res`)

* `res.json(data)`: Sends a JSON response.
    
* `res.status(code)`: Sets the HTTP status code for the response.
    
* `res.send(data)`: Sends a response with the specified data.
    

## Conclusion

In this article, we covered the essential aspects of handling HTTP requests and responses in Express.js. By defining routes for different HTTP methods and understanding how to work with request and response objects, you can build dynamic and interactive web applications.In this article, we will dive into handling HTTP requests and responses in Express.js. Understanding how to work with different HTTP methods, manage request data, and send responses is crucial for building robust web applications.