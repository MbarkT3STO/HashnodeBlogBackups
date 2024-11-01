---
title: "Express.js: Working with Sessions and Cookies"
datePublished: Fri Nov 01 2024 04:57:28 GMT+0000 (Coordinated Universal Time)
cuid: cm2y9jgaa000a09jqexo27zjl
slug: expressjs-working-with-sessions-and-cookies
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730437029936/58a59f47-9d00-4286-b677-b5edee048734.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we’ll explore how to manage sessions and cookies in an Express.js application. Sessions and cookies are essential for preserving user data across multiple HTTP requests and for maintaining user authentication.

## Understanding Sessions and Cookies

* **Cookies**: Small pieces of data stored on the client’s browser. They can be used to store information such as user preferences and session IDs.
    
* **Sessions**: Server-side storage of user data. Sessions use a unique identifier (session ID) stored as a cookie on the client to track data stored on the server.
    

## Setting Up Express Session Management

To work with sessions in Express, we’ll use the `express-session` package.

### Step 1: Install `express-session`

First, install the `express-session` package in your project:

```typescript
npm install express-session
```

### Step 2: Configure Session Middleware

Add the session middleware to your Express application to set up sessions.

```typescript
import express from 'express';
import session from 'express-session';

const app = express();
const PORT = 3000;

// Session middleware configuration
app.use(session({
    secret: 'your_secret_key', // Replace with a strong secret in production
    resave: false,
    saveUninitialized: true,
    cookie: { secure: false } // Set to true if using HTTPS
}));
```

### Step 3: Storing and Accessing Session Data

To store data in a session, assign it to the `req.session` object. Let’s create a route that stores a user ID in the session upon login.

```typescript
app.post('/login', (req, res) => {
    // Simulate user login
    req.session.userId = 1; // Typically, this would be fetched from a database
    res.send('User logged in, session data stored');
});
```

To retrieve session data, access `req.session` in other routes:

```typescript
app.get('/profile', (req, res) => {
    if (req.session.userId) {
        res.send(`User profile for user ID: ${req.session.userId}`);
    } else {
        res.send('User not logged in');
    }
});
```

### Step 4: Destroying Sessions

To log a user out, destroy the session data:

```typescript
app.post('/logout', (req, res) => {
    req.session.destroy((err) => {
        if (err) {
            return res.status(500).send('Error logging out');
        }
        res.send('User logged out, session destroyed');
    });
});
```

## Working with Cookies

Express has a built-in way to set cookies. Let’s set a cookie with a user’s preferred theme.

### Setting Cookies

Use `res.cookie()` to set a cookie:

```typescript
app.get('/set-theme', (req, res) => {
    res.cookie('theme', 'dark', { maxAge: 900000, httpOnly: true });
    res.send('Theme cookie set');
});
```

### Accessing Cookies

Use `req.cookies` to access cookies. For this, we’ll need the `cookie-parser` middleware.

1. Install `cookie-parser`:
    
    ```typescript
    npm install cookie-parser
    ```
    
2. Configure it in your app:
    
    ```typescript
    import cookieParser from 'cookie-parser';
    
    app.use(cookieParser());
    
    app.get('/get-theme', (req, res) => {
        const theme = req.cookies.theme || 'default';
        res.send(`Current theme: ${theme}`);
    });
    ```
    

### Clearing Cookies

To delete a cookie, use `res.clearCookie()`:

```typescript
app.get('/clear-theme', (req, res) => {
    res.clearCookie('theme');
    res.send('Theme cookie cleared');
});
```