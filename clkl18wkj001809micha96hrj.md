---
title: "PostgreSQL: Common Table Expressions (CTEs)"
datePublished: Thu Jul 27 2023 10:49:37 GMT+0000 (Coordinated Universal Time)
cuid: clkl18wkj001809micha96hrj
slug: postgresql-common-table-expressions-ctes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690454539164/2db8c873-85e5-486f-8e63-d56b6f390bfc.png
tags: postgresql, sql

---

## Introduction

When it comes to working with relational databases, One of the powerful features of PostgreSQL that often confuses developers, especially those new to database programming, is Common Table Expressions (CTEs). CTEs provide a way to create temporary result sets that can be referenced within a query.

## What is a Common Table Expression (CTE)?

A Common Table Expression, commonly known as a CTE, is a temporary result set that can be referenced within the context of a single SQL statement. It allows you to break down complex queries into simpler, more manageable parts. CTEs improve query readability, maintainability, and performance by enabling the creation of reusable and self-contained subqueries.

## The Anatomy of a CTE

A CTE is defined using the `WITH` keyword, followed by the CTE name and a query that defines the CTE itself. The syntax for a CTE looks like this:

```sql
WITH cte_name (column1, column2, ...) AS (
    -- CTE query goes here
    SELECT ...
)
```

Let's go through each component of the CTE definition:

* **cte\_name:** This is the name given to the CTE. It acts as a reference to the temporary result set, and you can use it in the main query as if it were a table name.
    
* **(column1, column2, ...):** Optionally, you can specify column names for the CTE. This step is useful when you intend to use the CTE to perform further calculations or join with other tables.
    
* **AS:** This keyword separates the CTE name and the CTE query.
    
* **CTE query:** This is where you define the query that generates the temporary result set. It can be any valid SQL query, including SELECT, INSERT, UPDATE, DELETE, or a combination of these statements.
    

## Benefits of Using CTEs

CTEs offer several advantages when it comes to SQL query construction:

1. **Improved Readability:** By breaking down complex queries into smaller, self-contained parts, CTEs make the SQL code more readable and easier to maintain.
    
2. **Code Reusability:** Once defined, you can reference the CTE multiple times in the same query or even across multiple queries within the same session.
    
3. **Recursive Queries:** CTEs can be used to create recursive queries, where a query refers to its own output, allowing you to work with hierarchical data structures.
    
4. **Optimized Query Execution:** PostgreSQL's query planner is optimized to handle CTEs efficiently, resulting in better query performance.
    

## Real-World Examples

Let's explore some practical examples to see how CTEs work in real-world scenarios.

### Example 1: Finding Employees with High Salaries

Suppose we have a table named `employees`, and we want to find employees with salaries higher than a certain threshold. We can achieve this using a CTE as follows:

```sql
WITH high_salary_employees AS (
    SELECT employee_id, first_name, last_name, salary
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary_employees;
```

In this example, we first define the CTE named `high_salary_employees`, which selects the desired columns from the `employees` table. The main query then references the CTE and retrieves all columns from it.

### Example 2: Calculating Employee Bonuses

Let's say we want to calculate bonuses for employees based on their salaries. The bonus formula is to provide 10% of the salary as the bonus amount. We can achieve this using the following query:

```sql
WITH employee_bonuses AS (
    SELECT employee_id, first_name, last_name, salary, salary * 0.1 AS bonus
    FROM employees
)
SELECT *
FROM employee_bonuses;
```

In this example, the CTE named `employee_bonuses` calculates the bonus amount for each employee and the main query retrieves all columns, including the calculated bonus.