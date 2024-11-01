---
title: "Express.js: Serving Static Files and Assets"
datePublished: Fri Nov 01 2024 05:25:22 GMT+0000 (Coordinated Universal Time)
cuid: cm2yajbst00010al334y0hext
slug: expressjs-serving-static-files-and-assets
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730438583318/b4d7ff2c-a490-4d3a-a222-c8a15082a5e0.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we'll explore how to serve static files such as images, CSS, JavaScript, and other assets in an Express.js application. Serving static assets is essential for any web application, as it helps enhance the user experience by providing styles, scripts, and media content.

## What are Static Files?

**Static files** are files that do not change on the server side and are directly served to the client as they are. Common examples include:

* **CSS files**: Stylesheets for designing the layout and appearance.
    
* **JavaScript files**: Scripts for client-side interactions.
    
* **Images, fonts, and other assets**.
    

## Setting Up a Static Folder in Express.js

Express provides a simple way to serve static files using the built-in `express.static` middleware.

### Step 1: Create a Public Folder

Create a folder named `public` in your project directory, where all static files (CSS, JS, images) will be stored.

```plaintext
your-project/
│
├── public/
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── script.js
│   └── images/
│       └── logo.png
└── app.ts
```

### Step 2: Configure Express to Serve Static Files

In your `app.ts` file, use the `express.static` middleware to serve files from the `public` folder.

```typescript
import express from 'express';
const app = express();

app.use(express.static('public')); // Serve static files from the "public" folder

app.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

Now, all files within the `public` folder can be accessed directly by their paths. For instance:

* CSS file: [`http://localhost:3000/css/style.css`](http://localhost:3000/css/style.css)
    
* JavaScript file: [`http://localhost:3000/js/script.js`](http://localhost:3000/js/script.js)
    
* Image file: [`http://localhost:3000/images/logo.png`](http://localhost:3000/images/logo.png)
    

### Step 3: Using Static Files in HTML

You can link these files in your HTML templates. For example, if you’re using EJS:

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Express App</title>
    <link rel="stylesheet" href="/css/style.css">
</head>
<body>
    <h1>Welcome to My Express App!</h1>
    <img src="/images/logo.png" alt="Logo">
    <script src="/js/script.js"></script>
</body>
</html>
```

## Customizing the Static Files Directory

You can also serve static files from a different directory by specifying a custom path. For instance, if you want to serve files from a folder named `assets`, you can do:

```typescript
app.use('/static', express.static('assets'));
```

In this case, your files will be accessible at [`http://localhost:3000/static/`](http://localhost:3000/static/)`...`.

## Example: Serving Static CSS and JavaScript

Suppose you have a `style.css` file with the following content:

```css
/* public/css/style.css */
body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
}
```

And a `script.js` file:

```typescript
// public/js/script.js
console.log('Hello from JavaScript!');
```

When you access the page, the styles from `style.css` will be applied, and you’ll see the JavaScript message in the browser console.

## Conclusion

Using `express.static` to serve static files is an efficient way to manage client-side assets in your Express.js application. This setup improves load times by directly serving assets and allows you to organize resources effectively within your project.