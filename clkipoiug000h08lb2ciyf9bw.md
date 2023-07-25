---
title: "PostgreSQL: Update Data in a Table"
datePublished: Tue Jul 25 2023 19:50:18 GMT+0000 (Coordinated Universal Time)
cuid: clkipoiug000h08lb2ciyf9bw
slug: postgresql-update-data-in-a-table
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690314525710/e779f9ac-e8c5-4215-bf6c-ec0e8fd7998f.png
tags: postgresql, sql

---

One of the fundamental operations in any database is updating existing records in a table. In this blog post, we'll explore how to update data in a PostgreSQL table with practical examples.

## Understanding the UPDATE Statement

The `UPDATE` statement in PostgreSQL allows you to modify one or more rows in a table based on specified conditions. It follows the general syntax:

```sql
UPDATE table_name
SET column1 = value1,
    column2 = value2,
    ...
WHERE condition;
```

Let's break down the components of this statement:

* `table_name`: The name of the table you want to update data in.
    
* `SET`: Specifies the columns and their new values that you want to update.
    
* `column1`, `column2`, etc.: The columns to be updated.
    
* `value1`, `value2`, etc.: The new values you want to assign to the respective columns.
    
* `WHERE`: An optional clause that filters the rows to be updated based on specific conditions. If omitted, all rows in the table will be updated.
    

## Examples of Updating Data in PostgreSQL

For our examples, let's assume we have a table called `employees` with the following structure:

| emp\_id | first\_name | last\_name | department | salary |
| --- | --- | --- | --- | --- |
| 1 | John | Doe | IT | 50000 |
| 2 | Jane | Smith | HR | 60000 |
| 3 | Bob | Johnson | Finance | 55000 |

### Example 1: Updating a Single Column

Let's say we want to update Jane Smith's department from "HR" to "Marketing". The SQL statement would be:

```sql
UPDATE employees
SET department = 'Marketing'
WHERE first_name = 'Jane' AND last_name = 'Smith';
```

After executing this statement, the `employees` table will look like this:

| emp\_id | first\_name | last\_name | department | salary |
| --- | --- | --- | --- | --- |
| 1 | John | Doe | IT | 50000 |
| 2 | Jane | Smith | Marketing | 60000 |
| 3 | Bob | Johnson | Finance | 55000 |

### Example 2: Updating Multiple Columns

Suppose we need to give John Doe a salary raise and change his department to "Operations". The SQL statement would be:

```sql
UPDATE employees
SET department = 'Operations',
    salary = 55000
WHERE first_name = 'John' AND last_name = 'Doe';
```

The updated `employees` table will be:

| emp\_id | first\_name | last\_name | department | salary |
| --- | --- | --- | --- | --- |
| 1 | John | Doe | Operations | 55000 |
| 2 | Jane | Smith | Marketing | 60000 |
| 3 | Bob | Johnson | Finance | 55000 |

### Example 3: Updating All Rows

In some cases, you might need to update all rows in a table. For instance, let's say we want to reset the salary of all employees to a default value of 50000:

```sql
UPDATE employees
SET salary = 50000;
```

After executing this statement, the `employees` table will be:

| emp\_id | first\_name | last\_name | department | salary |
| --- | --- | --- | --- | --- |
| 1 | John | Doe | Operations | 50000 |
| 2 | Jane | Smith | Marketing | 50000 |
| 3 | Bob | Johnson | Finance | 50000 |

## Update Join in PostgreSQL

While the `UPDATE` statement we covered in the previous section is useful for updating data in a single table, there are scenarios where you might need to update data based on a relationship between multiple tables. This is where the `UPDATE JOIN` comes into play. The `UPDATE JOIN` allows you to combine data from two or more tables and update the target table accordingly. In this section, we'll explore how to use `UPDATE JOIN` in PostgreSQL with a practical example.

### Example Scenario

Let's consider a database schema for an online bookstore. We have two tables: `books` and `authors`.

**Table: books**

| book\_id | title | author\_id | price |
| --- | --- | --- | --- |
| 1 | Pride and Prejudice | 101 | 25 |
| 2 | To Kill a Mockingbird | 102 | 20 |
| 3 | 1984 | 103 | 18 |

**Table: authors**

| author\_id | author\_name | nationality |
| --- | --- | --- |
| 101 | Jane Austen | British |
| 102 | Harper Lee | American |
| 103 | George Orwell | British |

### Example 1: Updating Book Prices by Author Nationality

Suppose we want to apply a discount to the book prices based on the authors' nationalities. We will reduce the prices by 10% for British authors and 5% for American authors.

To achieve this, we'll use the `UPDATE JOIN` along with a `CASE` statement to conditionally update the prices in the `books` table.

```sql
UPDATE books AS b
SET price = CASE
            WHEN a.nationality = 'British' THEN round(b.price * 0.9, 2)
            WHEN a.nationality = 'American' THEN round(b.price * 0.95, 2)
            ELSE b.price
            END
FROM authors AS a
WHERE b.author_id = a.author_id;
```

Let's break down this `UPDATE JOIN` statement:

* `books AS b`: We define an alias "b" for the `books` table, which we'll use to reference it later in the statement.
    
* `SET price = CASE ... END`: This is a `CASE` statement that allows us to specify conditions to update the `price` column based on the `nationality` column in the `authors` table.
    
* `round(b.price * 0.9, 2)`: The `price` will be multiplied by 0.9 (for British authors) or 0.95 (for American authors) and rounded to 2 decimal places to apply the discount.
    
* `FROM authors AS a`: We bring in the `authors` table and define an alias "a" for it to reference it later in the statement.
    
* `WHERE` [`b.author`](http://b.author)`_id =` [`a.author`](http://a.author)`_id`: This is the join condition that connects the `books` and `authors` tables based on the `author_id` column.
    

After executing the `UPDATE JOIN` statement, the updated `books` table will be:

| book\_id | title | author\_id | price |
| --- | --- | --- | --- |
| 1 | Pride and Prejudice | 101 | 22.50 |
| 2 | To Kill a Mockingbird | 102 | 19.00 |
| 3 | 1984 | 103 | 18.00 |

As you can see, the prices have been updated based on the authors' nationalities.

### Example 2: Updating Loyalty Points based on Total Order Quantities

Sure, let's explore another example of using the `UPDATE JOIN` in PostgreSQL. Consider a scenario where we have a database with two tables: `orders` and `customers`.

**Table: orders**

| order\_id | product\_name | quantity | customer\_id |
| --- | --- | --- | --- |
| 1 | Laptop | 2 | 101 |
| 2 | Smartphone | 1 | 102 |
| 3 | Headphones | 3 | 103 |

**Table: customers**

| customer\_id | customer\_name | loyalty\_points |
| --- | --- | --- |
| 101 | John Smith | 100 |
| 102 | Jane Doe | 50 |
| 103 | Bob Johnson | 200 |

Now, let's say we want to update the `loyalty_points` of customers based on their total order quantities. For every 5 products ordered, a customer should receive 10 additional loyalty points.

To achieve this, we'll use the `UPDATE JOIN` along with aggregation and a `CASE` statement to calculate and update the `loyalty_points` in the `customers` table.

```sql
UPDATE customers AS c
SET loyalty_points = c.loyalty_points + (total_order_quantity / 5) * 10
FROM (
    SELECT customer_id, SUM(quantity) AS total_order_quantity
    FROM orders
    GROUP BY customer_id
) AS o
WHERE c.customer_id = o.customer_id;
```

In this `UPDATE JOIN` statement:

* We first define an alias "c" for the `customers` table and use it to reference the table in the `SET` clause.
    
* We calculate the total order quantity for each customer using a subquery (aliased as "o") that performs aggregation with `SUM(quantity)` and `GROUP BY customer_id` on the `orders` table.
    
* Then, we use the `CASE` statement to calculate the additional `loyalty_points` based on the total order quantity.
    
* Finally, we use the join condition `WHERE c.customer_id = o.customer_id` to connect the `customers` and subquery result (orders) based on the `customer_id` column.
    

After executing the `UPDATE JOIN` statement, the updated `customers` table will be:

| customer\_id | customer\_name | loyalty\_points |
| --- | --- | --- |
| 101 | John Smith | 120 |
| 102 | Jane Doe | 60 |
| 103 | Bob Johnson | 230 |

As you can see, the `loyalty_points` have been updated based on the total order quantities for each customer.