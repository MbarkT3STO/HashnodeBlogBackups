---
title: "PostgreSQL: Creating Tables"
datePublished: Thu Jul 20 2023 16:43:46 GMT+0000 (Coordinated Universal Time)
cuid: clkbdtdff000409jt7aomgwos
slug: postgresql-creating-tables
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689871224866/0f6c2a37-434f-4b90-9029-1e024eb4b52e.png
tags: postgresql, sql

---

One of the fundamental operations in PostgreSQL is creating tables to store data efficiently. In this blog post, we'll walk you through the process of creating tables in PostgreSQL, complete with examples to help you get started.

## Creating a Table

To create a table in PostgreSQL, you need to use the `CREATE TABLE` statement. This statement defines the table's structure, including its column names, data types, and optional constraints.

Let's start by creating a simple table to store information about users.

```sql
-- Create a table to store user information
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 18)
);
```

In this example, we create a table named `users` with the following columns:

* `user_id`: An auto-incrementing serial column used as the primary key.
    
* `first_name` and `last_name`: Columns to store the user's first and last names, respectively. The `NOT NULL` constraint ensures these fields cannot be empty.
    
* `email`: A column to store the user's email address. The `UNIQUE` constraint ensures that each email address is unique across the table.
    
* `age`: A column to store the user's age. The `CHECK` constraint ensures that the age is greater than or equal to 18.
    

## Data Types in PostgreSQL

PostgreSQL offers various data types to cater to different types of data. Here are some commonly used data types you might encounter when creating tables:

* `INTEGER` or `INT`: Represents a whole number.
    
* `VARCHAR(n)`: Represents a variable-length character string with a maximum length of `n`.
    
* `CHAR(n)`: Represents a fixed-length character string with a length of `n`.
    
* `DATE`: Represents a date without a time component.
    
* `TIMESTAMP` or `TIMESTAMP WITH TIME ZONE`: Represents a date and time with or without a time zone.
    
* `BOOLEAN`: Represents a true/false value.
    

## Adding Data to the Table

Now that we have our `users` table, let's add some data to it using the `INSERT INTO` statement.

```sql
-- Insert data into the users table
INSERT INTO users (first_name, last_name, email, age)
VALUES ('John', 'Doe', 'john.doe@example.com', 25),
       ('Jane', 'Smith', 'jane.smith@example.com', 30);
```

With this `INSERT INTO` statement, we added two rows of data to the `users` table.

## Altering Tables

Tables are not set in stone, and you may need to modify their structure over time. PostgreSQL provides the `ALTER TABLE` statement to make such changes.

### Adding a New Column

To add a new column to an existing table, you can use the following syntax:

```sql
-- Add a new column to the users table
ALTER TABLE users
ADD COLUMN date_of_birth DATE;
```

### Modifying a Column

If you need to modify the data type or constraints of an existing column, you can do so using the `ALTER COLUMN` clause:

```sql
-- Change the data type of the age column to smallint
ALTER TABLE users
ALTER COLUMN age TYPE SMALLINT;

-- Add a NOT NULL constraint to the date_of_birth column
ALTER TABLE users
ALTER COLUMN date_of_birth SET NOT NULL;
```

### Dropping a Column

To remove a column from a table, you can use the `DROP COLUMN` clause:

```sql
-- Remove the phone_number column from the users table
ALTER TABLE users
DROP COLUMN phone_number;
```

Remember to be cautious when dropping columns, as this operation is irreversible and may result in data loss.