---
title: "Prisma ORM: Setting Up with an Express.js Project and PostgreSQL"
datePublished: Fri Nov 01 2024 16:44:20 GMT+0000 (Coordinated Universal Time)
cuid: cm2yyshmc00000ak0188v8qob
slug: prisma-orm-setting-up-with-an-expressjs-project-and-postgresql
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730439479551/8c1988d3-0996-42cf-84e0-a281d0965d1b.png
tags: javascript, databases, prisma, prismaorm

---

## Overview

In this article, we will go through the steps required to set up Prisma ORM in an Express.js project specifically for PostgreSQL. This setup will lay the foundation for interacting with your PostgreSQL database through Prisma in future articles.

### Prerequisites

Before we proceed, make sure you have the following installed:

* **Node.js** (version 14 or later)
    
* **PostgreSQL** (a running instance)
    
* **npm** or **yarn** (as a package manager)
    

### Step 1: Create a New Express.js Project

Open your terminal and create a new directory for your project:

```typescript
mkdir prisma-express-postgres
cd prisma-express-postgres
```

Initialize a new Node.js project:

```typescript
npm init -y
```

### Step 2: Install Required Dependencies

Install Express.js and Prisma as dependencies:

```typescript
npm install express
npm install prisma --save-dev
```

You also need to install the PostgreSQL client for Node.js:

```typescript
npm install @prisma/client
```

### Step 3: Initialize Prisma

Next, initialize Prisma in your project:

```typescript
npx prisma init
```

This command creates a `prisma` directory containing a `schema.prisma` file, along with a `.env` file for environment variables.

### Step 4: Configure PostgreSQL Connection

Open the `.env` file and set up your PostgreSQL connection string. Replace the placeholders with your actual database credentials:

```typescript
ATABASE_URL="postgresql://USER:PASSWORD@localhost:5432/DATABASE_NAME?schema=public"
```

Make sure to use the correct `USER`, `PASSWORD`, and `DATABASE_NAME`.

### Step 5: Define Your Database Schema

Open the `schema.prisma` file in the `prisma` directory and define your data model. For this example, let’s create a simple `User` model:

```typescript
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String @unique
}
```

### Step 6: Run Migrations

After defining your data model, you need to create the database tables based on your schema. Run the following command to create a migration:

```typescript
npx prisma migrate dev --name init
```

This command creates a new migration file and applies it to your database.

### Step 7: Generate the Prisma Client

Now, generate the Prisma Client, which provides a type-safe query API:

```typescript
npx prisma generate
```

### Step 8: Create a Basic Express Server

Next, create a new file named `server.js` in your project root. Add the following code to set up a basic Express server:

```typescript
const express = require('express');
const { PrismaClient } = require('@prisma/client');

const app = express();
const prisma = new PrismaClient();

app.use(express.json());

app.get('/', (req, res) => {
  res.send('Hello, Prisma with Express and PostgreSQL!');
});

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

### Step 9: Start the Server

Finally, run the server with the following command:

```typescript
node server.js
```

Visit [`http://localhost:3000`](http://localhost:3000) in your browser, and you should see the message "Hello, Prisma with Express and PostgreSQL!"

## Conclusion

In this article, we walked through the setup of Prisma ORM in an Express.js project configured for PostgreSQL. We established a connection to the database, defined a data model, and created a basic Express server. In the next article, we will explore how to define models with the Prisma schema in more detail.