---
title: "Express.js: Building Your First Express Server"
datePublished: Thu Oct 31 2024 19:39:35 GMT+0000 (Coordinated Universal Time)
cuid: cm2xplzr800000al46xr22r8a
slug: expressjs-building-your-first-express-server
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730403106223/65cccfa1-7bf2-44f5-93b4-56bf8c4263fc.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we’ll cover how to build your first server with Express.js using TypeScript. This includes setting up a basic server, creating routes, and testing the server locally.

## Prerequisites

Make sure you have:

* A project setup similar to the one we covered in [Express.js: Setting Up an Express.js Project in VS Code](https://mbarkt3sto.hashnode.dev/expressjs-setting-up-the-project-in-vs-code).
    
* TypeScript and Express installed in the project.
    

## Step 1: Set Up the Basic Server

1. **Create** `server.ts`  
    In your `src` folder, open or create `server.ts` and add the following code to create a basic Express server.
    
    ```typescript
    import express, { Request, Response } from 'express';
    
    const app = express();
    const PORT = process.env.PORT || 3000;
    
    // Root route
    app.get('/', (req: Request, res: Response) => {
        res.send('Hello, Express!');
    });
    
    // Start the server
    app.listen(PORT, () => {
        console.log(`Server is running on http://localhost:${PORT}`);
    });
    ```
    
    This code creates an Express application and sets it to listen on a specified port. The `app.get()` method defines a route that sends a message when you visit the root URL.
    

## Step 2: Add Middleware for JSON Parsing

Express provides built-in middleware to handle JSON data. Let’s add this middleware to the server so it can process JSON in request bodies:

```typescript
app.use(express.json());
```

Place this line right after initializing the `app` variable. It enables JSON parsing for incoming requests.

## Step 3: Define Additional Routes

Add a few more routes to practice handling various HTTP methods like `GET` and `POST`.

1. **Add a Welcome Route**  
    Define a `/welcome` route in `server.ts`:
    
    ```typescript
    app.get('/welcome', (req: Request, res: Response) => {
        res.send('Welcome to our Express server!');
    });
    ```
    
2. **Add a POST Route**  
    Now, let’s create a POST route for submitting data:
    
    ```typescript
    app.post('/submit', (req: Request, res: Response) => {
        const data = req.body;
        res.json({
            message: 'Data received successfully!',
            receivedData: data
        });
    });
    ```
    
    This route accepts JSON data in the request body and sends it back in the response.
    

## Step 4: Compile and Run the Server

To run the server, first compile TypeScript to JavaScript:

```bash
npx tsc
```

Then start the server:

```typescript
node dist/server.js
```

Open a browser and go to `http://localhost:3000` to see the root route message. Visit `http://localhost:3000/welcome` to test the welcome route.

## Step 5: Test Routes Using Postman

1. **Open Postman** and create a new request with:
    
    * **URL**: `http://localhost:3000/submit`
        
    * **Method**: POST
        
    * **Body**: Select **JSON** format and enter:
        
        ```json
        {
          "name": "Express User",
          "age": 25
        }
        ```
        
2. **Send the request**. You should receive a JSON response with the message and received data.
    

---

To improve the development experience, let’s set up **Nodemon** to automatically restart the server whenever changes are made, saving time and streamlining your workflow.

## Step 6: Install Nodemon

1. **Install Nodemon as a Dev Dependency**  
    In your project directory, run the following command:
    
    ```typescript
    npm install --save-dev nodemon
    ```
    
    This installs Nodemon, which will monitor changes in your TypeScript files and automatically restart the server.
    
2. **Update the** `package.json` Scripts  
    Modify the `scripts` section in your `package.json` to use Nodemon for development. Add the following script:
    
    ```json
    "scripts": {
        "start": "node dist/server.js",
        "dev": "nodemon --watch 'src/**/*.ts' --exec 'npx ts-node' src/server.ts"
    }
    ```
    
    * `start`: This script runs the compiled JavaScript server in production mode.
        
    * `dev`: This script uses Nodemon to watch for changes in `.ts` files within the `src` folder. When changes are detected, Nodemon uses `ts-node` to execute `src/server.ts` directly in TypeScript.
        
3. **Run the Dev Script**  
    Start the server in development mode by running:
    
    ```typescript
    npm run dev
    ```
    
    Now, Nodemon will monitor the `src` folder for changes and restart the server automatically when you save a file.
    

## Step 7: Test the Nodemon Workflow

To confirm that Nodemon is working:

1. Open `src/server.ts` and make a small change, like updating the response text in the root route:
    
    ```typescript
    app.get('/', (req: Request, res: Response) => {
        res.send('Hello, Express with Nodemon!');
    });
    ```
    
2. Save the file. Nodemon should automatically detect the change and restart the server. Check the browser or Postman by going to [`http://localhost:3000`](http://localhost:3000) to see the updated message.
    

## Common Nodemon Commands

* **Stop the Server**: Use `Ctrl + C` in the terminal to stop Nodemon.
    
* **Restart Manually**: If needed, type `rs` in the terminal to manually restart the server.