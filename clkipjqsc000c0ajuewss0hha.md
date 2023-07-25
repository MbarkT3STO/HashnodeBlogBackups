---
title: "PostgreSQL: Upsert Operation"
datePublished: Tue Jul 25 2023 19:46:35 GMT+0000 (Coordinated Universal Time)
cuid: clkipjqsc000c0ajuewss0hha
slug: postgresql-upsert-operation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690312536822/583ac3d0-d788-4dcc-95bc-2c7e0275de60.png
tags: postgresql, databases, sql

---

An upsert is a combination of "insert" and "update" where you want to insert a new row into a table if it doesn't already exist, or update the existing row if it does. This process can be tricky to handle efficiently, but PostgreSQL provides a powerful feature that makes upserts a breeze. In this blog post, we'll explore the upsert operation in PostgreSQL with examples to help you master this useful technique.

## Understanding the Anatomy of Upsert

In PostgreSQL, the upsert operation is accomplished using the `INSERT INTO ON CONFLICT` statement. The key to making it work is the `ON CONFLICT` clause, which allows you to specify what action to take when there is a conflict between the new row and an existing row in the table.

Here's the basic syntax for the upsert operation:

```sql
INSERT INTO table_name (column1, column2, ..., columnN)
VALUES (value1, value2, ..., valueN)
ON CONFLICT (conflict_column)
DO UPDATE SET column1 = value1, column2 = value2, ..., columnN = valueN;
```

Let's break down the components:

* `table_name`: The name of the target table where you want to perform the upsert.
    
* `(column1, column2, ..., columnN)`: The list of columns you want to insert data into.
    
* `VALUES (value1, value2, ..., valueN)`: The values you want to insert into the specified columns.
    
* `ON CONFLICT (conflict_column)`: The column that may cause a conflict (e.g., a unique constraint or primary key).
    
* `DO UPDATE SET ...`: The columns you want to update in case of a conflict.
    

## Example 1: Simple Upsert

Let's consider a hypothetical table called "employees" with the following structure:

| id (Primary Key) | name | department |
| --- | --- | --- |
| 1 | John | Engineering |
| 2 | Jane | Marketing |

Suppose we want to upsert a new employee record or update an existing one based on the "id" column. We can use the following query:

```sql
INSERT INTO employees (id, name, department)
VALUES (3, 'Alice', 'Finance')
ON CONFLICT (id)
DO UPDATE SET name = 'Alice', department = 'Finance';
```

In this example, if there is no record with `id = 3`, a new row will be inserted. However, if a row with `id = 3` already exists, the "name" and "department" fields will be updated with the new values.

## Example 2: Upsert with Constraint Violation

Consider a table called "students" with the following structure:

| roll\_no (Unique Constraint) | name | age |
| --- | --- | --- |
| 101 | John | 21 |
| 102 | Jane | 22 |

Now, let's perform an upsert on the "students" table:

```sql
INSERT INTO students (roll_no, name, age)
VALUES (103, 'Alice', 20)
ON CONFLICT (roll_no)
DO UPDATE SET name = EXCLUDED.name, age = EXCLUDED.age;
```

In this example, if a student with `roll_no = 103` is not present in the table, a new row will be inserted. However, if a row with `roll_no = 103` already exists, the "name" and "age" fields will be updated with the new values using the special `EXCLUDED` table.

### **Understanding the EXCLUDED Pseudo-table**

We mentioned the use of the `EXCLUDED` pseudo-table within the `DO UPDATE SET` clause. The `EXCLUDED` table is a special table that represents the values of the conflicting row that caused the upsert operation to be triggered. It allows you to reference the incoming row's values during the update phase of the upsert.

The `EXCLUDED` pseudo-table is especially useful when dealing with unique constraints or exclusion constraints, as it provides an easy way to access the values that would have been inserted if there were no conflicts.

Let's dive deeper into how `EXCLUDED` works:

### **EXCLUDED Columns**

When you use `EXCLUDED` in the `DO UPDATE SET` clause, you can refer to the columns that caused the conflict in the first place. For instance, in the "students" table example from the previous section, the `roll_no` column had a unique constraint. So, when an upsert operation attempted to insert a new row with a `roll_no` that already existed, the `EXCLUDED.roll_no` value would represent the conflicting value.

Here's the relevant part of the query from the previous example:

```sql
ON CONFLICT (roll_no)
DO UPDATE SET name = EXCLUDED.name, age = EXCLUDED.age;
```

In this case, [`EXCLUDED.name`](http://EXCLUDED.name) and `EXCLUDED.age` refer to the values that would have been inserted if the `roll_no` conflict didn't occur. By referencing `EXCLUDED` in the `SET` clause, you can easily update the conflicting row with the new values.

## Other Methods to Achieve Upsert

In addition to using the `INSERT INTO ON CONFLICT` statement with the `EXCLUDED` pseudo-table, PostgreSQL provides alternative methods to achieve the upsert operation. Let's explore two more approaches using the `MERGE` statement and the `CTE` (Common Table Expressions).

### Method 1: Upsert with MERGE Statement

The `MERGE` statement, also known as an "upsert" statement, is a SQL standard that has been adopted by some database systems, including PostgreSQL. It allows you to perform insert, update, or delete operations based on a specified condition, making it a powerful tool for handling upsert scenarios.

Here's the basic syntax for using the `MERGE` statement in PostgreSQL:

```sql
MERGE INTO target_table AS target
USING source_table AS source
ON (target.conflict_column = source.conflict_column)
WHEN MATCHED THEN
  UPDATE SET target.column1 = source.value1, target.column2 = source.value2, ..., target.columnN = source.valueN
WHEN NOT MATCHED THEN
  INSERT (column1, column2, ..., columnN)
  VALUES (source.value1, source.value2, ..., source.valueN);
```

Let's illustrate this with an example:

Assume we have a table called "books" with the following structure:

| isbn (Primary Key) | title | author |
| --- | --- | --- |
| 9781234567890 | Book A | Author X |
| 9789876543210 | Book B | Author Y |

We want to upsert new book records based on the `isbn` column. We can use the `MERGE` statement as follows:

```sql
MERGE INTO books AS target
USING (VALUES ('9780123456789', 'Book C', 'Author Z')) AS source (isbn, title, author)
ON (target.isbn = source.isbn)
WHEN MATCHED THEN
  UPDATE SET title = source.title, author = source.author
WHEN NOT MATCHED THEN
  INSERT (isbn, title, author)
  VALUES (source.isbn, source.title, source.author);
```

In this example, if the book with `isbn = '9780123456789'` exists in the "books" table, its title and author will be updated with the new values. Otherwise, a new row with the specified `isbn`, `title`, and `author` will be inserted.

### Method 2: Upsert with Common Table Expressions (CTE)

Common Table Expressions (CTEs) provide a way to define temporary result sets that can be used within a subsequent SQL statement. By leveraging CTEs, you can perform upsert operations in PostgreSQL efficiently.

Here's how to achieve an upsert using CTEs:

```sql
WITH source_data (conflict_column, column1, column2, ..., columnN) AS (
  VALUES ('conflict_value', 'value1', 'value2', ..., 'valueN')
)
INSERT INTO target_table (conflict_column, column1, column2, ..., columnN)
SELECT conflict_column, column1, column2, ..., columnN
FROM source_data
ON CONFLICT (conflict_column)
DO UPDATE SET column1 = EXCLUDED.column1, column2 = EXCLUDED.column2, ..., columnN = EXCLUDED.columnN;
```

Let's demonstrate this with an example:

Suppose we have a table called "inventory" with the following structure:

| product\_code (Primary Key) | quantity |
| --- | --- |
| 1001 | 50 |
| 1002 | 30 |

We want to upsert inventory data based on the `product_code` column. Here's the CTE-based upsert query:

```sql
WITH source_data (product_code, quantity) AS (
  VALUES (1003, 20)
)
INSERT INTO inventory (product_code, quantity)
SELECT product_code, quantity
FROM source_data
ON CONFLICT (product_code)
DO UPDATE SET quantity = EXCLUDED.quantity;
```

In this example, if a record with `product_code = 1003` doesn't exist in the "inventory" table, a new row will be inserted with the specified product code and quantity. If the product code already exists, the quantity will be updated with the new value.