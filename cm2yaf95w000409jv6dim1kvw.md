---
title: "Express.js: Using Templating Engines (EJS, Handlebars)"
datePublished: Fri Nov 01 2024 05:22:12 GMT+0000 (Coordinated Universal Time)
cuid: cm2yaf95w000409jv6dim1kvw
slug: expressjs-using-templating-engines-ejs-handlebars
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730438367590/b67a24d5-cd97-4fd2-92c7-c0d1bd41fb44.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we’ll explore how to use templating engines in Express.js, specifically EJS and Handlebars. Templating engines allow us to dynamically generate HTML on the server, helping create more interactive and data-driven web applications.

## What is a Templating Engine?

A **templating engine** enables developers to embed JavaScript logic in HTML files, helping dynamically populate HTML with data. In Express.js, common templating engines include:

* **EJS (Embedded JavaScript)**: Offers an easy syntax and flexibility for rendering HTML with JavaScript.
    
* **Handlebars**: Provides a logic-less templating approach with a cleaner syntax.
    

## Setting Up a Templating Engine in Express

### Step 1: Installing Dependencies

Choose the templating engine to install. Here’s how to install both EJS and Handlebars.

```typescript
npm install ejs
npm install express-handlebars
```

### Step 2: Configuring EJS in Express

1. In your `app.ts`, set EJS as the view engine.
    
2. Create a folder named `views` in the root directory for EJS template files.
    

```typescript
import express from 'express';
const app = express();
app.set('view engine', 'ejs');
```

### Step 3: Creating an EJS Template

Create an `index.ejs` file in the `views` folder.

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home</title>
</head>
<body>
    <h1>Welcome, <%= userName %>!</h1>
</body>
</html>
```

### Step 4: Rendering the Template with Data

Create a route in `app.ts` to render the EJS template with data.

```typescript
app.get('/', (req, res) => {
    res.render('index', { userName: 'Express User' });
});

app.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

### Using Handlebars with Express

To set up Handlebars, follow these steps:

1. Import and configure Handlebars in `app.ts`.
    
    ```typescript
    import express from 'express';
    import { engine } from 'express-handlebars';
    
    const app = express();
    app.engine('handlebars', engine());
    app.set('view engine', 'handlebars');
    ```
    
2. Create a Handlebars template, such as `home.handlebars`, in the `views` folder.
    
    ```typescript
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Home</title>
    </head>
    <body>
        <h1>Hello, {{userName}}!</h1>
    </body>
    </html>
    ```
    
3. Update your route to render this template:
    
    ```typescript
    app.get('/', (req, res) => {
        res.render('home', { userName: 'Express User' });
    });
    ```
    

## Conclusion

With EJS and Handlebars, Express.js applications can dynamically serve HTML based on server-side data. This can enhance interactivity while keeping server responses efficient and well-structured.