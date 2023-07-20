---
title: "PostgreSQL: Constraints"
datePublished: Thu Jul 20 2023 17:27:52 GMT+0000 (Coordinated Universal Time)
cuid: clkbfe35o000209jq6inj4ve8
slug: postgresql-constraints
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689871544656/871c518a-4211-406c-aa30-4024ba207da5.png
tags: postgresql, sql, plsql

---

When working with relational databases, data integrity is crucial to maintain the accuracy and consistency of the information stored. PostgreSQL provides a robust set of constraints that allow developers to define rules for data validation. In this blog post, we will explore various PostgreSQL constraints and their practical applications with examples.

## What are Constraints?

Constraints in PostgreSQL are rules applied to tables to enforce data integrity. They ensure that data adheres to specific conditions, preventing the insertion of invalid or inconsistent data into the database. PostgreSQL supports several types of constraints, each serving a distinct purpose.

## 1\. NOT NULL Constraint

The `NOT NULL` constraint ensures that a particular column cannot contain any NULL values. It is used to enforce the presence of data in a specific column.

**Example:** Let's create a table called `employees` with a `name` column, which should not accept NULL values.

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    age INTEGER
);
```

In this example, any attempt to insert a row without providing a value for the `name` column will result in an error.

## 2\. UNIQUE Constraint

The `UNIQUE` constraint ensures that the values in a column or a group of columns are unique across the table.

**Example:** Let's create a table called `users` with a `username` column that must be unique for each user.

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR UNIQUE,
    email VARCHAR UNIQUE
);
```

The `UNIQUE` constraint will prevent the insertion of duplicate usernames or emails into the `users` table.

## 3\. PRIMARY KEY Constraint

The `PRIMARY KEY` constraint uniquely identifies each row in a table and ensures that the key values are unique and not NULL. Each table can have only one primary key.

**Example:** Let's create a table called `students` with a `student_id` column as the primary key.

```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    age INTEGER
);
```

The `PRIMARY KEY` constraint automatically enforces uniqueness and non-nullity for the `student_id` column.

## 4\. FOREIGN KEY Constraint

The `FOREIGN KEY` constraint establishes a link between data in two tables, ensuring referential integrity. It helps maintain consistency between related tables.

**Example:** Let's create two tables, `orders` and `customers`, where the `customer_id` in the `orders` table references the `id` column in the `customers` table.

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    email VARCHAR UNIQUE
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    order_date DATE NOT NULL,
    total_amount NUMERIC NOT NULL,
    customer_id INTEGER REFERENCES customers(id)
);
```

With the `FOREIGN KEY` constraint, the `customer_id` column in the `orders` table must refer to an existing `id` in the `customers` table.

## 5\. CHECK Constraint

The `CHECK` constraint allows you to enforce a specific condition on data within a column.

**Example:** Let's create a table called `products` with a `stock_quantity` column, which should not allow negative values.

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR NOT NULL,
    stock_quantity INTEGER CHECK (stock_quantity >= 0),
    price NUMERIC NOT NULL
);
```

With the `CHECK` constraint, any attempt to insert or update a row with a negative `stock_quantity` will be rejected.

## 6\. EXCLUSION Constraint

The `EXCLUSION` constraint is a unique feature of PostgreSQL that allows you to define exclusion conditions to prevent overlapping or conflicting data in a table.

**Example:** Suppose you have a table called `appointments` with columns for `appointment_id`, `start_time`, and `end_time`. To ensure that no two appointments overlap, you can use an `EXCLUSION` constraint.

```sql
CREATE TABLE appointments (
    appointment_id SERIAL PRIMARY KEY,
    start_time TIMESTAMP NOT NULL,
    end_time TIMESTAMP NOT NULL,
    EXCLUDE USING GIST (tsrange(start_time, end_time) WITH &&)
);
```

In this example, the `EXCLUSION` constraint uses the `tsrange` data type to create a range of time that covers the `start_time` and `end_time` columns. The `&&` operator ensures that no two time ranges overlap.

## 7\. DOMAIN Constraint

The `DOMAIN` constraint allows you to define custom data types with constraints, which can then be used across multiple columns or tables.

**Example:** Let's create a domain called `positive_integer` that only accepts positive integer values.

```sql
CREATE DOMAIN positive_integer AS INTEGER CHECK (VALUE > 0);

CREATE TABLE product_reviews (
    review_id SERIAL PRIMARY KEY,
    product_id INTEGER,
    rating positive_integer,
    comment TEXT
);
```

By using the `positive_integer` domain, the `rating` column in the `product_reviews` table will only accept positive integer values.