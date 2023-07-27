---
title: "PostgreSQL: ALWAYS AS IDENTITY"
datePublished: Thu Jul 27 2023 11:03:26 GMT+0000 (Coordinated Universal Time)
cuid: clkl1qo7i000d0amiavutc691
slug: postgresql-always-as-identity
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690455691634/e37f38fe-9b41-443d-a922-c67a5de33e30.png
tags: postgresql, sql

---

One of the notable features of PostgreSQL is its ability to create identity columns. An identity column is a column that automatically generates unique values for each new row inserted into the table. In this blog post, we'll explore how to create an identity column in a PostgreSQL table using the `ALWAYS AS IDENTITY` syntax, along with practical examples to demonstrate the process.

## Understanding Identity Columns in PostgreSQL

An identity column, also known as a auto-incrementing or serial column in other databases, is a column that automatically generates a unique value for each new row inserted into the table. It simplifies the process of assigning sequential or unique identifiers to records without requiring any external intervention. PostgreSQL introduced the `GENERATED` feature in version 12, allowing users to create identity columns with the `ALWAYS AS IDENTITY` syntax.

## Syntax for Creating an Identity Column

The syntax for creating an identity column using `ALWAYS AS IDENTITY` is as follows:

```sql
CREATE TABLE table_name (
    column_name data_type GENERATED ALWAYS AS IDENTITY
);
```

* `table_name`: The name of the table where the identity column will be added.
    
* `column_name`: The name of the column that will serve as the identity column.
    
* `data_type`: The data type of the identity column.
    

It's essential to note that the `ALWAYS AS IDENTITY` option ensures that the identity property is always enforced, meaning you cannot explicitly insert values into the identity column.

## Examples of Creating Identity Columns

### Example 1: Creating a Simple Identity Column

Let's create a simple table named `employees` with an identity column `employee_id` of data type `BIGINT`.

```sql
CREATE TABLE employees (
    employee_id BIGINT GENERATED ALWAYS AS IDENTITY
);
```

Now, whenever a new row is inserted into the `employees` table without specifying a value for the `employee_id`, PostgreSQL will automatically generate a unique identifier for that column.

### Example 2: Creating an Identity Column with a Seed and Increment

You can also specify a seed value and an increment value for the identity column. Let's create a new table called `orders` with an identity column `order_id` of data type `SERIAL` starting from 100 and incrementing by 5 for each new row.

```sql
CREATE TABLE orders (
    order_id SERIAL GENERATED ALWAYS AS IDENTITY (START WITH 100 INCREMENT BY 5)
);
```

In this example, the first row inserted will have `order_id` as 100, the next row will have 105, then 110, and so on.

### Example 3: Adding an Identity Column to an Existing Table

Suppose you have an existing table named `customers`, and you want to add an identity column named `customer_id` to it. You can use the `ALTER TABLE` statement to achieve this:

```sql
ALTER TABLE customers
ADD COLUMN customer_id BIGINT GENERATED ALWAYS AS IDENTITY;
```

The `customers` table will now have a new identity column `customer_id`, and PostgreSQL will automatically populate it with unique values for existing and new rows.