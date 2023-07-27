---
title: "PostgreSQL: GENERATED ALWAYS AS"
datePublished: Thu Jul 27 2023 11:11:23 GMT+0000 (Coordinated Universal Time)
cuid: clkl20wc9000t0ami5iwq194o
slug: postgresql-generated-always-as
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690456115377/a1d98e98-8179-4381-bd2d-2028f9487bc1.png
tags: postgresql, sql

---

In this blog post, we will explore how to create computed columns in PostgreSQL using the `GENERATED ALWAYS` keyword, and we'll provide some examples to illustrate their potential.

## What are Computed Columns?

Computed columns, as the name suggests, are not physically stored in the database. Instead, they are calculated on-the-fly based on expressions defined during column creation. These expressions can involve one or more existing columns from the same table or constants.

The benefits of using computed columns include:

1. **Performance optimization:** Computed columns allow the database to pre-calculate values and reduce the need for redundant calculations in queries.
    
2. **Data consistency:** As the computed column value is derived from other columns, it ensures that the data remains consistent and avoids data redundancy.
    
3. **Simplified queries:** By storing pre-calculated values, queries become more straightforward and readable.
    

## Using `GENERATED ALWAYS` for Computed Columns

PostgreSQL introduced the `GENERATED` clause in version 12, allowing users to define computed columns with the `ALWAYS` keyword. The `GENERATED ALWAYS` clause ensures that the computed column is always up-to-date and cannot be directly assigned a value during insertion or update. Instead, its value is determined solely by the specified expression.

To create a computed column using `GENERATED ALWAYS`, we use the following syntax:

```sql
ALTER TABLE table_name
ADD column_name data_type GENERATED ALWAYS AS (expression) STORED;
```

* `table_name`: The name of the table in which the computed column will be added.
    
* `column_name`: The name of the computed column to be created.
    
* `data_type`: The data type of the computed column.
    
* `expression`: The expression that defines how the computed column will be calculated.
    
* `STORED`: This keyword is optional and used to specify that the computed column's values are physically stored. If omitted, the column is virtually computed on-the-fly without storage.
    

Let's now dive into some practical examples to better understand how to use `GENERATED ALWAYS` for computed columns.

## Example 1: Calculating Total Price

Consider a hypothetical online store database with a table named `products`. We want to introduce a computed column `total_price`, which calculates the total price of each product based on the unit price and quantity.

```sql
CREATE TABLE products (
  product_id SERIAL PRIMARY KEY,
  product_name VARCHAR(100) NOT NULL,
  unit_price NUMERIC NOT NULL,
  quantity INTEGER NOT NULL,
  total_price NUMERIC GENERATED ALWAYS AS (unit_price * quantity) STORED
);
```

In this example, the `total_price` column is derived by multiplying the `unit_price` with the `quantity`. As it is a `STORED` generated column, PostgreSQL will physically store the calculated `total_price` values.

## Example 2: Displaying Full Name

Let's consider a user information table named `users`. We want to add a computed column `full_name`, which concatenates the `first_name` and `last_name` columns.

```sql
CREATE TABLE users (
  user_id SERIAL PRIMARY KEY,
  first_name VARCHAR(50) NOT NULL,
  last_name VARCHAR(50) NOT NULL,
  full_name VARCHAR(101) GENERATED ALWAYS AS (first_name || ' ' || last_name) STORED
);
```

In this example, the `full_name` column is calculated by concatenating the `first_name` and `last_name` columns with a space in between. The `STORED` keyword ensures that the full names are physically stored in the table.