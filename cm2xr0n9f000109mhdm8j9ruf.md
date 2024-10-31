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

## Conclusion

In this article, we implemented authentication and authorization in an Express.js application using JWT. We created a simple login route, protected routes with middleware, and handled token verification. This is a foundational aspect of building secure web applications.