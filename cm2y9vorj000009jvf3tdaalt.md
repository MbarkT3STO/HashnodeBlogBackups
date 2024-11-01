---
title: "Express.js: Connecting to a Database"
datePublished: Fri Nov 01 2024 05:06:59 GMT+0000 (Coordinated Universal Time)
cuid: cm2y9vorj000009jvf3tdaalt
slug: expressjs-connecting-to-a-database
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730437267098/b026cf67-9184-4dbb-ae5d-66426b62f68f.webp
tags: express, javascript, nodejs, expressjs-cilb5apda0066e053g7td7q24

---

In this article, we’ll explore how to connect an Express.js application to a database. Databases allow us to store and retrieve data dynamically, enabling our applications to handle user data, perform CRUD operations, and manage persistent information. We’ll use PostgreSQL as an example and connect it to Express.js using Sequelize, an ORM (Object-Relational Mapper) for TypeScript.

## Choosing a Database

Express.js is compatible with most relational and NoSQL databases, including:

* **Relational**: PostgreSQL, MySQL, SQLite, SQL Server
    
* **NoSQL**: MongoDB, CouchDB, DynamoDB
    

For this guide, we’ll use **PostgreSQL** with Sequelize to manage our database in a TypeScript-based Express project.

## Setting Up Sequelize and PostgreSQL

### Step 1: Install PostgreSQL and Sequelize Packages

In your project, install `sequelize`, `pg` (PostgreSQL driver), and `sequelize-typescript` for better TypeScript support.

```typescript
npm install sequelize sequelize-typescript pg pg-hstore
```

### Step 2: Configure Sequelize

Create a new `sequelize.ts` file to configure Sequelize and establish a connection to the PostgreSQL database.

```typescript
import { Sequelize } from 'sequelize-typescript';

const sequelize = new Sequelize({
    database: 'your_database_name',
    username: 'your_username',
    password: 'your_password',
    host: 'localhost',
    dialect: 'postgres',
    models: [__dirname + '/models'] // Adjust path to your models folder
});

export default sequelize;
```

Replace `your_database_name`, `your_username`, and `your_password` with your actual PostgreSQL database credentials.

### Step 3: Create a Database Model

Models represent tables in a database. Let’s create a simple `User` model as an example. In your project, create a `models` folder, and inside, create a `User.ts` file.

```typescript
import { Model, Table, Column, DataType } from 'sequelize-typescript';

@Table({
    timestamps: true,
    tableName: 'users'
})
export class User extends Model {
    @Column({
        type: DataType.STRING,
        allowNull: false
    })
    username!: string;

    @Column({
        type: DataType.STRING,
        allowNull: false
    })
    password!: string;
}
```

### Step 4: Sync Database and Test Connection

In your main server file (e.g., `app.ts`), import the `sequelize` instance and synchronize it to create tables based on your models.

```typescript
import express from 'express';
import sequelize from './sequelize';
import { User } from './models/User';

const app = express();
const PORT = 3000;

app.use(express.json());

sequelize.sync()
    .then(() => {
        console.log('Database & tables created!');
        app.listen(PORT, () => {
            console.log(`Server is running on http://localhost:${PORT}`);
        });
    })
    .catch((error) => console.error('Error connecting to the database:', error));
```

Running the application will create the `users` table in your PostgreSQL database.

## Creating Routes to Interact with the Database

Let’s set up some basic routes to perform CRUD operations on the `User` model.

### 1\. Creating a New User

```typescript
app.post('/users', async (req, res) => {
    try {
        const { username, password } = req.body;
        const user = await User.create({ username, password });
        res.status(201).json(user);
    } catch (error) {
        res.status(500).json({ error: 'Failed to create user' });
    }
});
```

### 2\. Retrieving All Users

```typescript
app.get('/users', async (req, res) => {
    try {
        const users = await User.findAll();
        res.json(users);
    } catch (error) {
        res.status(500).json({ error: 'Failed to retrieve users' });
    }
});
```

### 3\. Updating a User

```typescript
app.put('/users/:id', async (req, res) => {
    try {
        const { id } = req.params;
        const { username, password } = req.body;
        const user = await User.findByPk(id);

        if (user) {
            user.username = username;
            user.password = password;
            await user.save();
            res.json(user);
        } else {
            res.status(404).json({ error: 'User not found' });
        }
    } catch (error) {
        res.status(500).json({ error: 'Failed to update user' });
    }
});
```

### 4\. Deleting a User

```typescript
app.delete('/users/:id', async (req, res) => {
    try {
        const { id } = req.params;
        const user = await User.findByPk(id);

        if (user) {
            await user.destroy();
            res.status(204).send();
        } else {
            res.status(404).json({ error: 'User not found' });
        }
    } catch (error) {
        res.status(500).json({ error: 'Failed to delete user' });
    }
});
```

## Conclusion

In this article, we connected an Express.js application to a PostgreSQL database using Sequelize, defined a `User` model, and set up CRUD operations to interact with the database. This setup allows us to store and manage user data dynamically.