# The most common SQL commands that everyone should know

## **Introduction**

SQL (Structured Query Language) is a programming language used to manage and manipulate data stored in relational database management systems (RDBMS) such as MySQL, Oracle, and Microsoft SQL Server. In this article, we will cover the most common SQL commands that every developer should know when working with SQL Server. These commands include:

*   `SELECT`
    
*   `INSERT`
    
*   `UPDATE`
    
*   `DELETE`
    
*   `CREATE TABLE`
    
*   `ALTER TABLE`
    
*   `DROP TABLE`
    

## **SELECT**

The `SELECT` statement is used to retrieve data from a database. It is the most commonly used SQL command and is often used in combination with a `WHERE` clause to filter the results.

```sql
SELECT * FROM customers WHERE city = 'New York';
```

This `SELECT` statement retrieves all columns (\*) from the `customers` table where the city is 'New York'.

## **INSERT**

The `INSERT` statement is used to add new records to a table.

```sql
INSERT INTO customers (customer_id, name, city) VALUES (1, 'John Smith', 'New York');
```

This `INSERT` statement adds a new record to the `customers` table with the values (1, 'John Smith', 'New York').

## **UPDATE**

The `UPDATE` statement is used to modify existing records in a table.

```sql
UPDATE customers SET city = 'Los Angeles' WHERE customer_id = 1;
```

This `UPDATE` statement changes the city for the customer with the `customer_id` of 1 to 'Los Angeles'.

## **DELETE**

The `DELETE` statement is used to delete records from a table.

```sql
DELETE FROM customers WHERE customer_id = 1;
```

This `DELETE` statement removes the record with the `customer_id` of 1 from the `customers` table.

## **CREATE TABLE**

The `CREATE TABLE` statement is used to create a new table in the database.

```sql
CREATE TABLE customers (
  customer_id INT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  city VARCHAR(255)
);
```

This `CREATE TABLE` statement creates a new table called `customers` with three columns: `customer_id`, `name`, and `city`. The `customer_id` column is an integer and is defined as the primary key, which means it must be unique and cannot be null. The `name` column is a string of maximum length 255 and is defined as not null, which means it cannot be left empty. The `city` column is a string of maximum length 255 and is not defined as not null, so it can be left empty.

## **ALTER TABLE**

The `ALTER TABLE` statement is used to modify the structure of an existing table.

```sql
ALTER TABLE customers ADD email VARCHAR(255);
```

This `ALTER TABLE` statement adds a new column called `email` to the `customers` table. The `email` column is a string of maximum length 255.

## **DROP TABLE**

The `DROP TABLE` statement is used to delete a table from the database.

```sql
DROP TABLE customers;
```

In addition to the basic commands covered above, there are several other SQL commands that are commonly used in SQL Server. These include:

*   `TRUNCATE TABLE`
    
*   `DISTINCT`
    
*   `GROUP BY`
    
*   `HAVING`
    
*   `ORDER BY`
    
*   `LIMIT`
    
*   `INNER JOIN`
    
*   `LEFT JOIN`
    
*   `RIGHT JOIN`
    
*   `FULL OUTER JOIN`
    

## **TRUNCATE TABLE**

The `TRUNCATE TABLE` statement is used to delete all data from a table, but unlike the `DELETE` statement, it is more efficient as it does not generate a transaction log.

```sql
TRUNCATE TABLE customers;
```

This `TRUNCATE TABLE` statement removes all data from the `customers` table.

## **DISTINCT**

The `DISTINCT` keyword is used in the `SELECT` statement to return only unique rows.

```sql
SELECT DISTINCT city FROM customers;
```

This `SELECT` statement returns a list of all unique cities in the `customers` table.

## **GROUP BY**

The `GROUP BY` clause is used in the `SELECT` statement to group the results by one or more columns.

```sql
SELECT COUNT(*), city FROM customers GROUP BY city;
```

This `SELECT` statement returns the number of customers in each city by grouping the results by the `city` column.

## **HAVING**

The `HAVING` clause is used in the `SELECT` statement to filter the grouped results based on a specific condition.

```sql
SELECT COUNT(*), city FROM customers GROUP BY city HAVING COUNT(*) > 10;
```

This `SELECT` statement returns the number of customers in each city where there are more than 10 customers.

## **ORDER BY**

The `ORDER BY` clause is used in the `SELECT` statement to sort the results in ascending or descending order.

```sql
SELECT * FROM customers ORDER BY name ASC;
```

This `SELECT` statement returns all rows from the `customers` table and sorts the results by the `name` column in ascending order.

## **LIMIT**

The `LIMIT` clause is used in the `SELECT` statement to limit the number of results returned.

```sql
SELECT * FROM customers LIMIT 10;
```

This `SELECT` statement returns the first 10 rows from the `customers` table.

## **INNER JOIN**

The `INNER JOIN` clause is used to combine rows from two or more tables based on a common column.

```sql
SELECT c.name, o.order_id FROM customers c INNER JOIN orders o ON c.customer_id = o.customer_id;
```

This `SELECT` statement returns the `name` and `order_id` of all customers who have placed an order by combining the `customers` and `orders` tables based on the common `customer_id` column.