---
title: "PostgreSQL: ALTER TABLE"
datePublished: Sat Jul 22 2023 06:18:52 GMT+0000 (Coordinated Universal Time)
cuid: clkdmdgjt00030al481jq8bsk
slug: postgresql-alter-table
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690005889897/da1e45f9-c673-4515-af1a-411c94692a10.png
tags: postgresql, databases, sql

---

PostgreSQL provides a versatile command called `ALTER TABLE` to modify existing tables. In this blog post, we will explore the capabilities of `ALTER TABLE` and walk through various examples to understand its usage.

## What is ALTER TABLE?

`ALTER TABLE` is a SQL command used to modify the structure of an existing table in a PostgreSQL database. With this command, you can add, modify, or remove columns, constraints, and various table options. It allows you to adapt your database schema as your application evolves, making it a crucial tool for any developer or database administrator.

## Altering Table Columns

### Adding a New Column

To add a new column to an existing table, you can use the `ADD COLUMN` clause. Let's say we have a table called `employees`, and we want to add a column to store the employees' phone numbers.

```sql
ALTER TABLE employees
ADD COLUMN phone_number VARCHAR(15);
```

In this example, we are adding a new column named `phone_number` with the data type `VARCHAR(15)` to store phone numbers up to 15 characters long.

### Modifying a Column

If you need to modify the data type or size of an existing column, you can use the `ALTER COLUMN` clause. For instance, let's assume we need to change the data type of the `salary` column from `INTEGER` to `DECIMAL`.

```sql
ALTER TABLE employees
ALTER COLUMN salary TYPE DECIMAL;
```

Now, the `salary` column will store decimal values instead of integers.

### Removing a Column

To remove a column from a table, you can use the `DROP COLUMN` clause. For example, let's remove the `phone_number` column from the `employees` table.

```sql
ALTER TABLE employees
DROP COLUMN phone_number;
```

After executing this command, the `phone_number` column will be removed from the `employees` table.

## Managing Table Constraints

### Adding a Constraint

Constraints ensure data integrity and enforce rules on table columns. To add a constraint to a table, you can use the `ADD CONSTRAINT` clause. Let's add a `NOT NULL` constraint to the `first_name` column of the `employees` table.

```sql
ALTER TABLE employees
ADD CONSTRAINT first_name_not_null CHECK (first_name IS NOT NULL);
```

With this constraint, any new records inserted into the `employees` table must have a non-null value in the `first_name` column.

### Removing a Constraint

To remove a constraint from a table, you can use the `DROP CONSTRAINT` clause. Suppose we want to remove the `first_name_not_null` constraint from the `employees` table.

```sql
ALTER TABLE employees
DROP CONSTRAINT first_name_not_null;
```

After running this command, the `first_name_not_null` constraint will no longer be enforced for the `first_name` column.

## Handling Table Options

### Renaming a Table

To rename an existing table, you can use the `RENAME TO` clause. For example, let's say we want to rename the table `employees` to `staff`.

```sql
ALTER TABLE employees
RENAME TO staff;
```

Now, the table that was previously named `employees` will be known as `staff`.

### Setting a Default Value for a Column

To set a default value for an existing column, you can use the `SET DEFAULT` clause. Let's assume we want to set a default value of 0 for the `bonus` column in the `staff` table.

```sql
ALTER TABLE staff
ALTER COLUMN bonus SET DEFAULT 0;
```

From now on, if no value is provided for the `bonus` column during an `INSERT` operation, the default value of 0 will be assigned to the column.

### Removing a Default Value from a Column

If you want to remove the default value from a column, you can use the `DROP DEFAULT` clause. Let's remove the default value from the `bonus` column in the `staff` table.

```sql
ALTER TABLE staff
ALTER COLUMN bonus DROP DEFAULT;
```

After executing this command, the `bonus` column will no longer have a default value.

## Modifying Table Constraints

### Modifying Constraint Definitions

You can modify the definition of an existing constraint using the `ALTER CONSTRAINT` clause. For example, let's assume we have a table named `orders` with a constraint named `check_order_amount` that checks if the order amount is greater than 0. Now, we want to modify the constraint to ensure that the order amount is greater than or equal to 0.

```sql
ALTER TABLE orders
ALTER CONSTRAINT check_order_amount CHECK (order_amount >= 0);
```

By using the `ALTER CONSTRAINT` clause, we can redefine the check condition of the `check_order_amount` constraint.

## Combining Alterations

It's important to note that you can combine multiple alterations in a single `ALTER TABLE` statement. For instance, let's say we want to add a new column named `department` to the `staff` table and set a default value of 'Unassigned' for the `department` column.

```sql
ALTER TABLE staff
ADD COLUMN department VARCHAR(50) DEFAULT 'Unassigned';
```

With this single statement, we added the `department` column and set its default value to 'Unassigned'.