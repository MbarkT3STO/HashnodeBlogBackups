---
title: "Express.js: Implementing Authentication and Authorization"
datePublished: Thu Oct 31 2024 20:18:58 GMT+0000 (Coordinated Universal Time)
cuid: cm2xr0n9f000109mhdm8j9ruf
slug: expressjs-implementing-authentication-and-authorization
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730405748509/08a340c2-3fbb-4f05-8fa3-229177726253.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we will explore how to implement authentication and authorization in an Express.js application. These are critical components of web applications that ensure that only authorized users can access specific resources.

## Understanding Authentication and Authorization

* **Authentication**: The process of verifying the identity of a user. It confirms whether the user is who they claim to be.
    
* **Authorization**: The process of determining whether an authenticated user has permission to access specific resources or perform certain actions.
    

## Common Authentication Strategies

1. **Session-based Authentication**: User credentials are stored in a session on the server. Upon successful login, the server creates a session and sends a cookie to the client.
    
2. **Token-based Authentication**: After user authentication, the server generates a token (e.g., JWT - JSON Web Token) that is sent to the client. The client sends this token with each request to access protected resources.
    

### Example of Token-based Authentication with JWT

In this example, we will implement token-based authentication using JWT.

### Step 1: Install Required Packages

First, you need to install the required packages:

```typescript
npm install express jsonwebtoken bcryptjs dotenv
```

### Step 2: Create User Model

For demonstration, we will create a simple in-memory user model.

```typescript
// user.ts
export interface User {
    id: number;
    username: string;
    password: string; // In a real application, passwords should be hashed
}

let users: User[] = [
    { id: 1, username: 'user1', password: 'password1' },
    { id: 2, username: 'user2', password: 'password2' },
];

export const findUser = (username: string) => {
    return users.find((user) => user.username === username);
};
```

### Step 3: Set Up the Express Application

Now, set up the Express application.

```typescript
import express, { Request, Response } from 'express';
import jwt from 'jsonwebtoken';
import bcrypt from 'bcryptjs';
import dotenv from 'dotenv';
import { findUser, User } from './user';

dotenv.config();

const app = express();
const PORT = 3000;

// Middleware to parse JSON
app.use(express.json());
```

### Step 4: Implement Login Route

Next, implement the login route to authenticate users.

```typescript
app.post('/login', async (req: Request, res: Response) => {
    const { username, password } = req.body;
    const user: User | undefined = findUser(username);

    if (user && (await bcrypt.compare(password, user.password))) {
        const token = jwt.sign({ id: user.id, username: user.username }, process.env.JWT_SECRET || 'your-secret-key', { expiresIn: '1h' });
        res.json({ token });
    } else {
        res.status(401).send('Invalid credentials');
    }
});
```

### Step 5: Middleware to Protect Routes

Create middleware to protect routes and check for the JWT token.

```typescript
const authenticateToken = (req: Request, res: Response, next: Function) => {
    const token = req.headers['authorization']?.split(' ')[1];

    if (!token) {
        return res.sendStatus(401); // Unauthorized
    }

    jwt.verify(token, process.env.JWT_SECRET || 'your-secret-key', (err) => {
        if (err) {
            return res.sendStatus(403); // Forbidden
        }
        next();
    });
};
```

### Step 6: Protect Routes

Now, you can use the `authenticateToken` middleware to protect routes.

```typescript
app.get('/protected', authenticateToken, (req: Request, res: Response) => {
    res.send('This is a protected route');
});
```

### Step 7: Start the Server

Finally, start the server.

```typescript
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

---

In addition to access tokens, it is common to implement refresh tokens in your authentication strategy. Refresh tokens allow users to obtain new access tokens without having to log in again, enhancing the user experience while maintaining security.

## Understanding Refresh Tokens

* **Access Tokens**: Short-lived tokens used to access protected resources. They typically have a limited lifespan (e.g., 15 minutes to 1 hour).
    
* **Refresh Tokens**: Long-lived tokens used to obtain new access tokens. Refresh tokens are stored securely on the client and are used to request new access tokens when the original ones expire.
    

## Why Use Refresh Tokens?

1. **Improved Security**: Access tokens can expire quickly, reducing the window of opportunity for misuse. If an access token is compromised, it will only be valid for a short time.
    
2. **Enhanced User Experience**: Users do not need to log in frequently, as refresh tokens can be used to obtain new access tokens automatically.
    

## Step 1: Update the User Model

In our user model, we will modify it to include refresh tokens. For simplicity, we will keep the refresh token in memory.

```typescript
let users: User[] = [
    { id: 1, username: 'user1', password: 'password1' },
    { id: 2, username: 'user2', password: 'password2' },
];

interface UserWithToken extends User {
    refreshToken?: string;
}

export const findUser = (username: string) => {
    return users.find((user) => user.username === username);
};
```

## Step 2: Generate Refresh Tokens

Modify the login route to generate a refresh token along with the access token.

```typescript
app.post('/login', async (req: Request, res: Response) => {
    const { username, password } = req.body;
    const user: UserWithToken | undefined = findUser(username);

    if (user && (await bcrypt.compare(password, user.password))) {
        const accessToken = jwt.sign({ id: user.id, username: user.username }, process.env.JWT_SECRET || 'your-secret-key', { expiresIn: '15m' });
        const refreshToken = jwt.sign({ id: user.id, username: user.username }, process.env.JWT_SECRET || 'your-secret-key', { expiresIn: '7d' });

        // Store refresh token in user model
        user.refreshToken = refreshToken;

        res.json({ accessToken, refreshToken });
    } else {
        res.status(401).send('Invalid credentials');
    }
});
```

## Step 3: Create a Route to Refresh Tokens

Add a route to handle the refresh token logic. This route will verify the refresh token and provide a new access token.

```typescript
app.post('/refresh', (req: Request, res: Response) => {
    const { refreshToken } = req.body;

    if (!refreshToken) {
        return res.sendStatus(401); // Unauthorized
    }

    const user: UserWithToken | undefined = users.find((user) => user.refreshToken === refreshToken);
    if (!user) {
        return res.sendStatus(403); // Forbidden
    }

    jwt.verify(refreshToken, process.env.JWT_SECRET || 'your-secret-key', (err) => {
        if (err) {
            return res.sendStatus(403); // Forbidden
        }

        const newAccessToken = jwt.sign({ id: user.id, username: user.username }, process.env.JWT_SECRET || 'your-secret-key', { expiresIn: '15m' });
        res.json({ accessToken: newAccessToken });
    });
});
```

## Step 4: Invalidate Refresh Tokens

To enhance security, you may want to implement a mechanism to invalidate refresh tokens, such as logging out or rotating tokens after use. For simplicity, we will add a logout route.

```typescript
app.post('/logout', (req: Request, res: Response) => {
    const { refreshToken } = req.body;

    // Invalidate the refresh token
    const user: UserWithToken | undefined = users.find((user) => user.refreshToken === refreshToken);
    if (user) {
        user.refreshToken = undefined; // Clear refresh token
    }

    res.sendStatus(204); // No Content
});
```

## Conclusion

In this continuation of our article on authentication and authorization, we implemented refresh tokens to improve security and user experience. We covered how to generate refresh tokens upon login, how to refresh access tokens using the refresh token, and how to invalidate tokens upon logout.

By implementing both access and refresh tokens, you create a robust authentication system that helps secure your Express.js applications.