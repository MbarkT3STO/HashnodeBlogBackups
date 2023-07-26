---
title: "PostgreSQL: A Comprehensive Guide to SELECT Statement"
datePublished: Wed Jul 26 2023 19:48:44 GMT+0000 (Coordinated Universal Time)
cuid: clkk52d99000208ms0fxbbwnj
slug: postgresql-a-comprehensive-guide-to-select-statement
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690394762586/3e27078d-ffdd-4c48-a2d2-695b90f5dcab.png
tags: postgresql, databases, sql

---

## Introduction

When working with databases, one of the most essential and frequently used operations is retrieving data. In PostgreSQL, the `SELECT` statement allows you to retrieve data from one or more tables based on specified conditions. This powerful statement forms the backbone of data querying in PostgreSQL. In this blog post, we will explore the `SELECT` statement in depth, covering its syntax, usage, and various examples.

## Syntax

The basic syntax of the `SELECT` statement is as follows:

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

* `column1, column2, ...`: The columns you want to retrieve data from. Use `*` to select all columns.
    
* `table_name`: The name of the table from which data will be retrieved.
    
* `condition`: (Optional) The condition(s) that specify which rows to select based on specific criteria.
    

## Examples

Let's dive into some practical examples to understand how the `SELECT` statement works.

### 1\. Retrieving all columns from a table

To retrieve all columns from a table, use the `*` wildcard:

```sql
SELECT *
FROM employees;
```

This query will fetch all records from the `employees` table, including all columns.

### 2\. Selecting specific columns

To select specific columns, list their names after the `SELECT` keyword:

```sql
SELECT first_name, last_name, email
FROM employees;
```

This query will retrieve only the `first_name`, `last_name`, and `email` columns from the `employees` table.

### 3\. Adding aliases

You can use aliases to provide temporary names to columns:

```sql
SELECT first_name AS "First Name", last_name AS "Last Name", email AS "Email"
FROM employees;
```

The query will display the columns with aliases, making the output more descriptive.

### 4\. Filtering data with WHERE

The `WHERE` clause allows you to filter data based on specific conditions:

```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE department = 'HR';
```

This query will retrieve the `first_name`, `last_name`, and `hire_date` columns from employees who belong to the 'HR' department.

### 5\. Combining conditions

You can combine multiple conditions using logical operators such as `AND` and `OR`:

```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE department = 'HR' AND salary > 50000;
```

This query will fetch employees from the 'HR' department with a salary greater than $50,000.

### 6\. Sorting results

You can sort the query results using the `ORDER BY` clause:

```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE department = 'HR'
ORDER BY hire_date DESC;
```

This query will retrieve 'HR' department employees sorted in descending order based on their hire dates.

### 7\. Limiting the number of results

Use the `LIMIT` clause to restrict the number of rows returned:

```sql
SELECT first_name, last_name
FROM employees
LIMIT 10;
```

This query will retrieve only the first 10 rows from the `employees` table.

## Grouping Data with GROUP BY

In addition to retrieving individual records, the `SELECT` statement in PostgreSQL also allows us to perform aggregate functions on data. The `GROUP BY` clause is used to group rows based on one or more columns, and then aggregate functions can be applied to these groups to obtain summary information. Let's explore how this works with examples.

### 8\. Using GROUP BY with Aggregate Functions

Consider a scenario where we have a `sales` table with columns `product`, `category`, and `price`. We can use the `GROUP BY` clause to find the total sales for each product category:

```sql
SELECT category, SUM(price) AS total_sales
FROM sales
GROUP BY category;
```

This query will group the records by the `category` column and calculate the total sales for each category using the `SUM` aggregate function.

### 9\. Filtering Groups with HAVING

The `HAVING` clause works similarly to the `WHERE` clause, but it is used specifically with the `GROUP BY` clause to filter groups based on aggregate results:

```sql
SELECT category, AVG(price) AS average_price
FROM sales
GROUP BY category
HAVING AVG(price) > 100;
```

This query will calculate the average price for each product category and then filter out those categories with an average price greater than $100.

### 10\. Grouping by Multiple Columns

You can also use multiple columns in the `GROUP BY` clause to create more granular groups:

```sql
SELECT category, product, COUNT(*) AS total_sales
FROM sales
GROUP BY category, product;
```

This query will group the data by both `category` and `product`, and then count the number of sales for each unique combination.

### 11\. Using GROUP BY with ORDER BY

Combining `GROUP BY` with `ORDER BY` allows you to sort the groups based on aggregate values:

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC;
```

This query will group employees by `department`, calculate the average salary for each department, and then sort the results in descending order based on the average salary.

### 12\. Grouping by Date or Time Intervals

In some cases, you might want to group data by date or time intervals. PostgreSQL allows you to use functions like `DATE_TRUNC` for this purpose:

```sql
SELECT DATE_TRUNC('month', order_date) AS month, SUM(total_amount) AS monthly_sales
FROM orders
GROUP BY DATE_TRUNC('month', order_date);
```

This query will group the orders by the month in which they were placed and calculate the total sales for each month.

## Distinct Records with DISTINCT

When querying data from a database, you might encounter scenarios where you want to eliminate duplicate records from the result set. The `DISTINCT` keyword in the `SELECT` statement allows you to retrieve only unique records from the specified columns. Let's explore how to use `DISTINCT` with some examples.

### 13\. Retrieving Distinct Values

Consider a `colors` table with a column named `color_name`. If you want to retrieve all unique color names from this table, you can use the `DISTINCT` keyword as follows:

```sql
SELECT DISTINCT color_name
FROM colors;
```

This query will fetch only the distinct color names from the `colors` table, removing any duplicate entries.

### 14\. Combining DISTINCT with Multiple Columns

You can use `DISTINCT` with multiple columns to retrieve unique combinations of values:

```sql
SELECT DISTINCT first_name, last_name
FROM employees;
```

This query will return only the unique combinations of `first_name` and `last_name` from the `employees` table.

### 15\. Using DISTINCT with ORDER BY

You can combine `DISTINCT` with `ORDER BY` to sort the distinct values:

```sql
SELECT DISTINCT department
FROM employees
ORDER BY department;
```

This query will retrieve the distinct department names from the `employees` table and sort them in alphabetical order.

### 16\. Limiting the Number of Distinct Results

If you want to retrieve a specific number of distinct records, you can use `LIMIT` in combination with `DISTINCT`:

```sql
SELECT DISTINCT product_name
FROM sales
LIMIT 5;
```

This query will fetch the first 5 distinct product names from the `sales` table.

### 17\. Using DISTINCT with Aggregate Functions

`DISTINCT` can be used in conjunction with aggregate functions to perform calculations on unique values:

```sql
SELECT department, COUNT(DISTINCT job_title) AS unique_job_titles
FROM employees
GROUP BY department;
```

This query will count the number of unique job titles within each department and present the results grouped by department.

## Combining Data with JOIN

In real-world scenarios, data is often distributed across multiple tables in a relational database. The `JOIN` operation allows you to combine data from two or more tables based on related columns. This enables you to retrieve and analyze data across multiple entities. Let's dive into various types of joins with examples.

### 18\. Inner Join

The `INNER JOIN` retrieves only the rows that have matching values in both tables.

Consider two tables: `employees` and `departments`. To retrieve a list of employees along with their corresponding department names, you can use an inner join as follows:

```sql
SELECT employees.first_name, employees.last_name, departments.department_name
FROM employees
INNER JOIN departments ON employees.department_id = departments.department_id;
```

This query will combine data from both tables based on the `department_id`, displaying the `first_name` and `last_name` of employees along with their respective `department_name`.

### 19\. Left Join (or Left Outer Join)

The `LEFT JOIN` retrieves all rows from the left table and the matching rows from the right table. If there's no match in the right table, NULL values are returned for the columns of the right table.

Continuing from the previous example, suppose you want to retrieve all employees and their corresponding department names. However, you also want to include employees who have not been assigned to any department. You can use a left join as follows:

```sql
SELECT employees.first_name, employees.last_name, departments.department_name
FROM employees
LEFT JOIN departments ON employees.department_id = departments.department_id;
```

This query will return all employees, along with their department names if available. Employees without a department assignment will have NULL in the `department_name` column.

### 20\. Right Join (or Right Outer Join)

The `RIGHT JOIN` is the opposite of the `LEFT JOIN`. It retrieves all rows from the right table and the matching rows from the left table. If there's no match in the left table, NULL values are returned for the columns of the left table.

For instance, if you want to see a list of departments and their employees, including departments without any employees, you can use a right join:

```sql
SELECT departments.department_name, employees.first_name, employees.last_name
FROM employees
RIGHT JOIN departments ON employees.department_id = departments.department_id;
```

This query will return all departments along with their employees if available. Departments without any employees will have NULL values in the `first_name` and `last_name` columns.

### 21\. Full Outer Join

The `FULL OUTER JOIN` retrieves all rows from both tables and combines them. If there's no match in one of the tables, NULL values are returned for the columns of the table without a match.

For example, if you want to see a comprehensive list of all employees and all departments, including employees without a department assignment and departments without any employees, you can use a full outer join:

```sql
SELECT employees.first_name, employees.last_name, departments.department_name
FROM employees
FULL OUTER JOIN departments ON employees.department_id = departments.department_id;
```

This query will return all employees and all departments, merging the data from both tables. Any employee without a department and any department without an employee will have NULL values in the corresponding columns.

## Utilizing Subqueries

In SQL, a subquery is a query nested inside another query, allowing you to retrieve data from one or more tables based on the results of an inner query. Subqueries are a powerful tool for writing complex and efficient queries. Let's delve into subqueries with examples using sample tables.

### Sample Tables

Consider two sample tables: `orders` and `customers`.

#### `orders` Table

| order\_id | customer\_id | total\_amount |
| --- | --- | --- |
| 1 | 101 | 150.00 |
| 2 | 102 | 200.00 |
| 3 | 101 | 75.00 |
| 4 | 103 | 300.00 |
| 5 | 102 | 50.00 |

#### `customers` Table

| customer\_id | customer\_name |
| --- | --- |
| 101 | John |
| 102 | Jane |
| 103 | Smith |
| 104 | Emily |

### 22\. Basic Subquery

A simple subquery can be used to retrieve data based on a condition from another table. For example, to find all orders made by customer 'Jane', you can use a subquery like this:

```sql
SELECT order_id, total_amount
FROM orders
WHERE customer_id = (SELECT customer_id FROM customers WHERE customer_name = 'Jane');
```

Output:

| order\_id | total\_amount |
| --- | --- |
| 2 | 200.00 |
| 5 | 50.00 |

### 23\. Subquery with Aggregate Function

You can also use subqueries with aggregate functions to perform calculations based on related data. To find the total amount spent by each customer, you can use a subquery like this:

```sql
SELECT customer_id, customer_name, 
  (SELECT SUM(total_amount) FROM orders WHERE orders.customer_id = customers.customer_id) AS total_spent
FROM customers;
```

Output:

| customer\_id | customer\_name | total\_spent |
| --- | --- | --- |
| 101 | John | 225.00 |
| 102 | Jane | 250.00 |
| 103 | Smith | 300.00 |
| 104 | Emily | NULL |

### 24\. Subquery with IN Operator

The `IN` operator allows you to check if a value exists within a set of values returned by a subquery. For instance, to find orders made by any of the top-spending customers, you can use a subquery with the `IN` operator:

```sql
SELECT order_id, total_amount
FROM orders
WHERE customer_id IN (SELECT customer_id FROM customers ORDER BY total_spent DESC LIMIT 2);
```

Output:

| order\_id | total\_amount |
| --- | --- |
| 2 | 200.00 |
| 4 | 300.00 |
| 5 | 50.00 |

### 25\. Subquery with EXISTS Operator

The `EXISTS` operator allows you to check if a subquery returns any rows. For example, to find customers who have placed at least one order, you can use the `EXISTS` operator:

```sql
SELECT customer_id, customer_name
FROM customers
WHERE EXISTS (SELECT 1 FROM orders WHERE orders.customer_id = customers.customer_id);
```

Output:

| customer\_id | customer\_name |
| --- | --- |
| 101 | John |
| 102 | Jane |
| 103 | Smith |

## Uniting Data with the UNION Operator

In SQL, the `UNION` operator allows you to combine the results of two or more `SELECT` queries into a single result set. It is used to unite data from different tables or queries that have the same column structure. Let's explore the `UNION` operator with examples using sample tables.

### Sample Tables

Consider two sample tables: `employees` and `contractors`.

#### `employees` Table

| emp\_id | first\_name | last\_name | department |
| --- | --- | --- | --- |
| 1 | John | Smith | HR |
| 2 | Jane | Doe | Marketing |
| 3 | Michael | Johnson | Finance |

#### `contractors` Table

| contractor\_id | first\_name | last\_name | department |
| --- | --- | --- | --- |
| 101 | Emily | Brown | IT |
| 102 | James | Lee | HR |
| 103 | Lily | Wang | Marketing |

### 26\. Basic UNION

The basic usage of the `UNION` operator is to combine the results of two similar queries. For instance, if you want to retrieve a list of all employees and contractors, you can use a `UNION` like this:

```sql
SELECT emp_id, first_name, last_name, department
FROM employees
UNION
SELECT contractor_id, first_name, last_name, department
FROM contractors;
```

Output:

| emp\_id | first\_name | last\_name | department |
| --- | --- | --- | --- |
| 1 | John | Smith | HR |
| 2 | Jane | Doe | Marketing |
| 3 | Michael | Johnson | Finance |
| 101 | Emily | Brown | IT |
| 102 | James | Lee | HR |
| 103 | Lily | Wang | Marketing |

### 27\. UNION with Order and Limit

You can also use `ORDER BY` and `LIMIT` with `UNION` to sort the final result and restrict the number of rows returned. For example, to get a list of all employees and contractors sorted alphabetically by their last names and limited to the top 4, you can use the following query:

```sql
SELECT emp_id, first_name, last_name, department
FROM employees
UNION
SELECT contractor_id, first_name, last_name, department
FROM contractors
ORDER BY last_name
LIMIT 4;
```

Output:

| emp\_id | first\_name | last\_name | department |
| --- | --- | --- | --- |
| 2 | Jane | Doe | Marketing |
| 1 | John | Smith | HR |
| 101 | Emily | Brown | IT |
| 103 | Lily | Wang | Marketing |

### 28\. UNION with Different Columns

To use the `UNION` operator, the number and data types of columns in all queries must match. If the columns differ, you can use `NULL` values to fill in the missing columns. For instance, if the `employees` table has an additional column called `position`, and the `contractors` table lacks this column, you can still use `UNION` with `NULL` values:

```sql
SELECT emp_id, first_name, last_name, department, position
FROM employees
UNION
SELECT contractor_id, first_name, last_name, department, NULL
FROM contractors;
```

Output:

| emp\_id | first\_name | last\_name | department | position |
| --- | --- | --- | --- | --- |
| 1 | John | Smith | HR | NULL |
| 2 | Jane | Doe | Marketing | NULL |
| 3 | Michael | Johnson | Finance | NULL |
| 101 | Emily | Brown | IT | NULL |
| 102 | James | Lee | HR | NULL |
| 103 | Lily | Wang | Marketing | NULL |

## Handling NULL Values with the IS NULL Operator

In SQL, the `IS NULL` operator allows you to check for the presence of NULL values in columns. NULL represents the absence of a value, and it is essential to handle NULL values properly in your queries to avoid unexpected results. Let's explore how to use the `IS NULL` operator with examples using sample tables.

### Sample Table

Consider a sample table `employees` with the following data:

| emp\_id | first\_name | last\_name | department | salary |
| --- | --- | --- | --- | --- |
| 1 | John | Smith | HR | 50000 |
| 2 | Jane | Doe | Marketing | 60000 |
| 3 | Michael | Johnson | NULL | 55000 |
| 4 | Emily | Brown | Finance | NULL |
| 5 | James | Lee | HR | 48000 |

### 29\. Basic IS NULL

The basic usage of the `IS NULL` operator is to check for NULL values in a specific column. For example, to find employees with missing department information, you can use the following query:

```sql
SELECT emp_id, first_name, last_name
FROM employees
WHERE department IS NULL;
```

Output:

| emp\_id | first\_name | last\_name |
| --- | --- | --- |
| 3 | Michael | Johnson |

### 30\. Combining IS NULL and IS NOT NULL

You can combine `IS NULL` and `IS NOT NULL` to retrieve records with and without NULL values. For instance, to find employees with and without salary information, you can use the following query:

```sql
SELECT emp_id, first_name, last_name
FROM employees
WHERE salary IS NULL OR salary IS NOT NULL;
```

This query will return all records from the `employees` table because it includes both employees with known salaries and employees without salary information.

### 31\. Handling NULL Values with COALESCE

The `COALESCE` function allows you to replace NULL values with a specified default value. For example, to display the department names of employees and use 'N/A' for those without a department, you can use the following query:

```sql
SELECT emp_id, first_name, last_name, COALESCE(department, 'N/A') AS department
FROM employees;
```

Output:

| emp\_id | first\_name | last\_name | department |
| --- | --- | --- | --- |
| 1 | John | Smith | HR |
| 2 | Jane | Doe | Marketing |
| 3 | Michael | Johnson | N/A |
| 4 | Emily | Brown | Finance |
| 5 | James | Lee | HR |

## Utilizing the BETWEEN Operator for Range Queries

In SQL, the `BETWEEN` operator allows you to retrieve data that falls within a specified range of values. This operator is particularly useful for querying data that meets specific criteria based on numerical or date ranges. Let's explore how to use the `BETWEEN` operator with examples using sample tables.

### Sample Table

Consider a sample table `employees` with the following data:

| emp\_id | first\_name | last\_name | hire\_date | salary |
| --- | --- | --- | --- | --- |
| 1 | John | Smith | 2021-03-15 | 50000 |
| 2 | Jane | Doe | 2020-12-01 | 60000 |
| 3 | Michael | Johnson | 2022-02-10 | 55000 |
| 4 | Emily | Brown | 2023-01-05 | 75000 |
| 5 | James | Lee | 2023-06-20 | 48000 |

### 32\. Basic BETWEEN

The basic usage of the `BETWEEN` operator is to find records that fall within a specified range. For example, to retrieve employees with salaries between $50,000 and $60,000, you can use the following query:

```sql
SELECT emp_id, first_name, last_name, salary
FROM employees
WHERE salary BETWEEN 50000 AND 60000;
```

Output:

| emp\_id | first\_name | last\_name | salary |
| --- | --- | --- | --- |
| 1 | John | Smith | 50000 |
| 2 | Jane | Doe | 60000 |
| 3 | Michael | Johnson | 55000 |

### 33\. Using NOT BETWEEN

You can also use the `NOT BETWEEN` operator to retrieve records that fall outside the specified range. For example, to find employees with salaries not between $50,000 and $60,000, you can use the following query:

```sql
SELECT emp_id, first_name, last_name, salary
FROM employees
WHERE salary NOT BETWEEN 50000 AND 60000;
```

Output:

| emp\_id | first\_name | last\_name | salary |
| --- | --- | --- | --- |
| 4 | Emily | Brown | 75000 |
| 5 | James | Lee | 48000 |

### 34\. BETWEEN with Dates

The `BETWEEN` operator is also useful for querying date ranges. For example, to retrieve employees hired between January 1, 2022, and December 31, 2022, you can use the following query:

```sql
SELECT emp_id, first_name, last_name, hire_date
FROM employees
WHERE hire_date BETWEEN '2022-01-01' AND '2022-12-31';
```

Output:

| emp\_id | first\_name | last\_name | hire\_date |
| --- | --- | --- | --- |
| 3 | Michael | Johnson | 2022-02-10 |

### 35\. BETWEEN with Dates and Times

You can also use the `BETWEEN` operator with date and time data types. For example, to retrieve employees hired between January 1, 2022, 00:00:00, and January 1, 2022, 23:59:59, you can use the following query:

```sql
SELECT emp_id, first_name, last_name, hire_date
FROM employees
WHERE hire_date BETWEEN '2022-01-01 00:00:00' AND '2022-01-01 23:59:59';
```

Output:

| emp\_id | first\_name | last\_name | hire\_date |
| --- | --- | --- | --- |
| 3 | Michael | Johnson | 2022-02-10 |

## Using the LIKE Operator for Pattern Matching

In SQL, the `LIKE` operator allows you to perform pattern matching on string values. It is particularly useful for searching for specific patterns or substrings within columns. Let's explore how to use the `LIKE` operator with examples using sample tables.

### Sample Table

Consider a sample table `products` with the following data:

| product\_id | product\_name |
| --- | --- |
| 1 | Apples |
| 2 | Bananas |
| 3 | Oranges |
| 4 | Pineapple |
| 5 | Strawberry Jam |
| 6 | Orange Juice |
| 7 | Apple Pie |

### 36\. Basic LIKE

The basic usage of the `LIKE` operator is to find records that match a specific pattern. The `%` symbol represents zero, one, or multiple characters, and the `_` symbol represents a single character. For example, to retrieve products that start with "App", you can use the following query:

```sql
SELECT product_id, product_name
FROM products
WHERE product_name LIKE 'App%';
```

Output:

| product\_id | product\_name |
| --- | --- |
| 1 | Apples |
| 7 | Apple Pie |

### 37\. Using NOT LIKE

Similar to the `NOT BETWEEN` operator, you can use the `NOT LIKE` operator to retrieve records that do not match a specific pattern. For example, to find products that do not contain the word "Juice" in their name, you can use the following query:

```sql
SELECT product_id, product_name
FROM products
WHERE product_name NOT LIKE '%Juice%';
```

Output:

| product\_id | product\_name |
| --- | --- |
| 1 | Apples |
| 2 | Bananas |
| 3 | Oranges |
| 4 | Pineapple |
| 5 | Strawberry Jam |
| 7 | Apple Pie |

### 38\. Combining LIKE with other Operators

You can combine the `LIKE` operator with other operators to perform more complex searches. For example, to find products that start with "O" and have exactly seven characters in their name, you can use the following query:

```sql
SELECT product_id, product_name
FROM products
WHERE product_name LIKE 'O______' AND product_name NOT LIKE '% %';
```

Output:

| product\_id | product\_name |
| --- | --- |
| 3 | Oranges |

## Replacing NULL Values with COALESCE

In SQL, the `COALESCE` function is used to replace NULL values with a specified default value. This function is handy when you want to display meaningful data instead of NULLs in your query results. Let's explore how to use the `COALESCE` function with examples using sample tables.

### Sample Table

Consider a sample table `employees` with the following data:

| emp\_id | first\_name | last\_name | department | salary |
| --- | --- | --- | --- | --- |
| 1 | John | Smith | HR | 50000 |
| 2 | Jane | Doe | Marketing | 60000 |
| 3 | Michael | Johnson | NULL | NULL |
| 4 | Emily | Brown | Finance | NULL |
| 5 | James | Lee | HR | 48000 |

### 39\. Basic Usage of COALESCE

The basic usage of the `COALESCE` function is to replace NULL values with a default value of your choice. For example, to display the department name and replace NULL values with "Not Assigned," you can use the following query:

```sql
SELECT emp_id, first_name, last_name, COALESCE(department, 'Not Assigned') AS department
FROM employees;
```

Output:

| emp\_id | first\_name | last\_name | department |
| --- | --- | --- | --- |
| 1 | John | Smith | HR |
| 2 | Jane | Doe | Marketing |
| 3 | Michael | Johnson | Not Assigned |
| 4 | Emily | Brown | Finance |
| 5 | James | Lee | HR |

### 40\. Using COALESCE with Multiple Columns

You can use the `COALESCE` function with multiple columns. The function returns the first non-NULL value from the list. For example, if you have a backup email address column and you want to display the backup email if the primary email is NULL, you can use the following query:

```sql
SELECT emp_id, first_name, last_name, COALESCE(primary_email, backup_email) AS email
FROM employees;
```

Assuming we have an additional `email` table with the following data:

| emp\_id | primary\_email | backup\_email |
| --- | --- | --- |
| 1 | [john@example.com](mailto:john@example.com) | NULL |
| 2 | NULL | [jane.doe@example.com](mailto:jane.doe@example.com) |
| 3 | [michael@example.com](mailto:michael@example.com) | [mike@example.com](mailto:mike@example.com) |
| 4 | [emily@example.com](mailto:emily@example.com) | [emily.b@example.com](mailto:emily.b@example.com) |
| 5 | [james@example.com](mailto:james@example.com) | [jimmy@example.com](mailto:jimmy@example.com) |

Output:

| emp\_id | first\_name | last\_name | email |
| --- | --- | --- | --- |
| 1 | John | Smith | [john@example.com](mailto:john@example.com) |
| 2 | Jane | Doe | [jane.doe@example.com](mailto:jane.doe@example.com) |
| 3 | Michael | Johnson | [michael@example.com](mailto:michael@example.com) |
| 4 | Emily | Brown | [emily@example.com](mailto:emily@example.com) |
| 5 | James | Lee | [james@example.com](mailto:james@example.com) |

### 41\. Using COALESCE with Aggregate Functions

You can also use `COALESCE` in combination with aggregate functions to handle NULL values during calculations. For example, to find the average salary of employees and replace NULL values with 0, you can use the following query:

```sql
SELECT COALESCE(AVG(salary), 0) AS avg_salary
FROM employees;
```

Output:

| avg\_salary |
| --- |
| 42600 |

## Finding the Greatest and Least Values with GREATEST() and LEAST()

In SQL, the `GREATEST()` and `LEAST()` functions are used to find the highest and lowest values, respectively, from a list of expressions or column values. These functions are particularly useful when you need to compare multiple values and determine the maximum or minimum among them. Let's explore how to use the `GREATEST()` and `LEAST()` functions with examples using sample tables.

### Sample Table

Consider a sample table `employees` with the following data:

| emp\_id | first\_name | last\_name | age | salary |
| --- | --- | --- | --- | --- |
| 1 | John | Smith | 30 | 50000 |
| 2 | Jane | Doe | 28 | 60000 |
| 3 | Michael | Johnson | 35 | 55000 |
| 4 | Emily | Brown | 27 | 75000 |
| 5 | James | Lee | 32 | 48000 |

### 42\. Using GREATEST()

The `GREATEST()` function returns the highest value from a list of expressions or column values. For example, to find the highest salary among the employees, you can use the following query:

```sql
SELECT GREATEST(salary) AS highest_salary
FROM employees;
```

Output:

| highest\_salary |
| --- |
| 75000 |

### 43\. Using LEAST()

The `LEAST()` function, on the other hand, returns the lowest value from a list of expressions or column values. To find the lowest age among the employees, you can use the following query:

```sql
SELECT LEAST(age) AS lowest_age
FROM employees;
```

Output:

| lowest\_age |
| --- |
| 27 |

### 44\. Using GREATEST() and LEAST() Together

You can also use both `GREATEST()` and `LEAST()` functions in the same query to find multiple extreme values. For instance, to determine the oldest and youngest employees based on their ages, you can use the following query:

```sql
SELECT GREATEST(age) AS oldest_age, LEAST(age) AS youngest_age
FROM employees;
```

Output:

| oldest\_age | youngest\_age |
| --- | --- |
| 35 | 27 |

### 45\. Using GREATEST() with Multiple Columns

The `GREATEST()` function can be used with multiple columns to find the highest value among different columns. For example, to find the maximum age and salary among the employees, you can use the following query:

```sql
SELECT GREATEST(age, salary) AS max_age_or_salary
FROM employees;
```

Output:

| max\_age\_or\_salary |
| --- |
| 50000 |
| 60000 |
| 55000 |
| 75000 |
| 48000 |