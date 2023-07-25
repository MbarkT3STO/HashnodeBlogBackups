---
title: "PostgreSQL: Delete Data in a Table"
datePublished: Tue Jul 25 2023 20:08:25 GMT+0000 (Coordinated Universal Time)
cuid: clkiqbtaw00020ajsevpq84sq
slug: postgresql-delete-data-in-a-table
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690315610088/711a4512-b306-446e-a026-12e7da661cc1.png
tags: postgresql, sql

---

In this blog post, we'll focus on the DELETE command, which allows us to remove rows from a table based on specified conditions.

## Understanding the DELETE Command Syntax

The DELETE command in PostgreSQL has a straightforward syntax:

```sql
DELETE FROM table_name
WHERE condition;
```

* `DELETE FROM` indicates that we want to delete data from a table.
    
* `table_name` is the name of the table from which we want to delete the data.
    
* `WHERE` is an optional clause that specifies the condition for deleting rows. If omitted, all rows in the table will be deleted.
    

## Examples of Using DELETE in PostgreSQL

### Example 1: Deleting All Rows

Let's start by deleting all rows from a table named "employees," which contains information about employees in a company. To delete all rows, we can use the following SQL command:

```sql
DELETE FROM employees;
```

**Note:** Be cautious when using this command, as it will delete all records from the "employees" table without any conditions.

### Example 2: Deleting Rows Based on a Condition

Suppose we want to delete employees who no longer work for the company and have a "resigned" status in the "employment\_status" column. The following SQL command achieves this:

```sql
DELETE FROM employees
WHERE employment_status = 'resigned';
```

This command will remove all rows from the "employees" table where the value of the "employment\_status" column is 'resigned.'

### Example 3: Deleting Rows Using Multiple Conditions

We can also delete rows based on multiple conditions. Let's say we want to delete employees who are both marked as "resigned" and have not logged in for more than a year. We can use the following SQL command:

```sql
DELETE FROM employees
WHERE employment_status = 'resigned' AND last_login_date < NOW() - INTERVAL '1 year';
```

This command deletes rows from the "employees" table that meet both conditions: "employment\_status" is 'resigned,' and "last\_login\_date" is more than a year ago.