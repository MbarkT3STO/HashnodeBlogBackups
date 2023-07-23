---
title: "PostgreSQL: Insert Data into a Table"
datePublished: Sun Jul 23 2023 19:17:07 GMT+0000 (Coordinated Universal Time)
cuid: clkftm5js000k0amma5wy32ku
slug: postgresql-insert-data-into-a-table
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690139074725/d6bf872d-2615-4f6a-b1ca-5ef377b371ff.png
tags: postgresql, databases, sql

---

Before inserting data, it's essential to understand the structure of the table you want to populate. For demonstration purposes, let's consider a simple "users" table with the following schema:

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT
);
```

The `user_id` column is an auto-incrementing primary key, while `first_name`, `last_name`, and `age` columns hold user information.

## Inserting a Single Row

To insert a single row into the "users" table, you can use the `INSERT INTO` statement. Here's an example:

```sql
INSERT INTO users (first_name, last_name, age)
VALUES ('John', 'Doe', 30);
```

In this example, we specify the column names in the parentheses after `INSERT INTO`, followed by the `VALUES` keyword, and then the corresponding data for each column.

## Inserting Multiple Rows

If you want to insert multiple rows in one go, you can use the following syntax:

```sql
INSERT INTO users (first_name, last_name, age)
VALUES
    ('Jane', 'Smith', 25),
    ('Michael', 'Johnson', 40),
    ('Emily', 'Brown', 22);
```

Here, we provide multiple sets of data within the `VALUES` clause, separated by commas.

## Copying Data from CSV

PostgreSQL allows you to bulk-insert data from a CSV (Comma-Separated Values) file. Let's assume you have a file named "user\_data.csv" with the same structure as the "users" table. You can use the following command:

```sql
COPY users (first_name, last_name, age) FROM 'path/to/user_data.csv' DELIMITER ',' CSV HEADER;
```

Replace `'path/to/user_data.csv'` with the actual path to your CSV file. The `DELIMITER ','` specifies that the values are separated by commas, and `CSV HEADER` indicates that the first row contains the column headers.

## Inserting Data from a Query

You can also insert data into a table using the results of a SELECT query. This technique is handy when you want to duplicate data from one table to another or transform existing data. Here's an example:

```sql
INSERT INTO users (first_name, last_name, age)
SELECT first_name, last_name, age FROM temporary_users WHERE age > 25;
```

This query selects records from the "temporary\_users" table where the age is greater than 25 and inserts the results into the "users" table.