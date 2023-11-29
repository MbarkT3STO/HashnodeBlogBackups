---
title: "PostgreSQL: Functions"
datePublished: Wed Nov 29 2023 08:08:35 GMT+0000 (Coordinated Universal Time)
cuid: clpjhjamd000j09l35utx6aoq
slug: postgresql-functions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701244871256/89581808-d738-4bb0-b772-969db01dcb15.png
tags: postgresql, databases, sql

---

## **What are Functions in PostgreSQL?**

In PostgreSQL, functions are named blocks of code that perform specific tasks. These functions can accept input parameters, process data, and return results. By encapsulating logic into functions, developers can create modular code, improve code reusability, and simplify complex operations. PostgreSQL supports various types of functions, including scalar functions, aggregate functions, and table-valued functions.

### **Scalar Functions**

Scalar functions are functions that operate on a single row and return a single value. These functions are commonly used to perform calculations, manipulate strings, and transform data on a row-by-row basis. Let's look at an example of a simple scalar function that calculates the square of a given number:

```pgsql
CREATE OR REPLACE FUNCTION calculate_square(input_num INTEGER) RETURNS INTEGER AS
$$
BEGIN
  RETURN input_num * input_num;
END;
$$
LANGUAGE plpgsql;
```

To use the `calculate_square` function, you can execute the following SQL query:

```pgsql
SELECT calculate_square(5); -- Output: 25
```

### **Aggregate Functions**

Aggregate functions operate on a set of rows and return a single value that summarizes the data. These functions are often used in combination with the `GROUP BY` clause to perform calculations on groups of data. A classic example is the `SUM` aggregate function:

```pgsql
SELECT department, SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

In the above query, the `SUM` function calculates the total salary for each department from the `employees` table.

### **Table-Valued Functions**

Table-valued functions, as the name suggests, return a set of rows, which can be used as a virtual table. These functions are valuable when you need to use the result set in further queries or joins. Here's a basic example of a table-valued function that returns a list of employees with a specific job title:

```pgsql
CREATE OR REPLACE FUNCTION get_employees_by_job(job_title TEXT) RETURNS TABLE (employee_id INT, name TEXT) AS
$$
BEGIN
  RETURN QUERY SELECT id, full_name FROM employees WHERE job = job_title;
END;
$$
LANGUAGE plpgsql;
```

You can use the `get_employees_by_job` function in the following manner:

```pgsql
SELECT * FROM get_employees_by_job('Software Engineer');
```

## **Benefits of Using Functions in PostgreSQL**

Using functions in PostgreSQL provides several advantages:

1. **Modularity**: Functions enable you to break down complex operations into smaller, manageable units, making your codebase more organized and maintainable.
    
2. **Code Reusability**: Once a function is defined, it can be called from multiple places within your database, reducing code duplication and promoting consistency.
    
3. **Performance Optimization**: By moving complex calculations or data manipulations into functions, you can optimize query performance and reduce database load.
    
4. **Security**: Functions can be used to control data access by defining them with `SECURITY DEFINER` or `SECURITY INVOKER` privileges, ensuring data security and integrity.
    
5. **Abstraction**: Functions provide a layer of abstraction, allowing you to modify the underlying logic without affecting the application's functionality.