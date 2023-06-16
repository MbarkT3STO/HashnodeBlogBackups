---
title: "SQL: Constraints"
datePublished: Thu Jun 08 2023 09:09:41 GMT+0000 (Coordinated Universal Time)
cuid: climx3n88000009js98da8und
slug: sql-constraints
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686933948476/972e58fa-dfcf-4fca-ac3c-932236861f3a.jpeg
tags: sql-server, databases, sql

---

When working with relational databases, maintaining data integrity is of utmost importance. SQL, offers a variety of constraints to enforce rules and restrictions on the data stored within its tables. These constraints play a crucial role in maintaining the accuracy, consistency, and reliability of the data. In this article, we will explore some of the most commonly used constraints in SQL/SQL Server, along with relevant examples.

## **1\. Primary Key Constraint**

The primary key constraint ensures that each record in a table has a unique identifier. It enforces the uniqueness and non-nullability of a column or a combination of columns. Consider the following example:

```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(50),
    Email VARCHAR(100)
);
```

In this case, the `CustomerID` column is designated as the primary key. It guarantees that each customer record will have a unique identifier, preventing duplicate entries and facilitating efficient data retrieval.

## **2\. Foreign Key Constraint**

The foreign key constraint establishes a relationship between two tables based on a common key. It ensures referential integrity, enforcing that values in the foreign key column exist in the primary key column of another table. Let's illustrate this with an example:

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

Here, the `Orders` table has a foreign key constraint (`CustomerID`) that references the primary key of the `Customers` table. This constraint guarantees that each order is associated with a valid customer, preventing orphaned or inconsistent data.

## **3\. Unique Constraint**

The unique constraint ensures that the values in a column or a combination of columns are unique across the table. Unlike the primary key constraint, a unique constraint allows null values. Consider the following example:

```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(50),
    SKU VARCHAR(20) UNIQUE,
    Price DECIMAL(10,2)
);
```

In this case, the `SKU` column has a unique constraint. It ensures that each product is uniquely identified by its SKU, preventing duplicate entries while still allowing null values.

## **4\. Check Constraint**

The check constraint allows defining custom rules that the values in a column must adhere to. It ensures that only valid data is inserted or updated in the table. Let's see an example:

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Age INT CHECK (Age >= 18 AND Age <= 65),
    Department VARCHAR(50)
);
```

Here, the `Age` column has a check constraint that ensures the age of an employee falls within a specified range (between 18 and 65 years). This constraint guarantees that only valid age values are stored in the table.

## **5\. Not Null Constraint**

The not null constraint ensures that a column does not contain any null values. It guarantees that each record must have a non-null value in the specified column. Consider the following example:

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    Age INT,
    Grade CHAR(2) NOT NULL
);
```

In this case, the `Name` and `Grade` columns have the not null constraint. It ensures that both the name and grade fields are always populated, preventing the insertion of incomplete or missing data.

## **Conclusion**

Constraints in SQL Server are vital tools for maintaining data integrity and enforcing business rules. The primary key, foreign key, unique, check, and not null constraints provide powerful mechanisms to validate and control the data stored in tables. By utilizing these constraints effectively, database administrators and developers can ensure the accuracy and consistency of their data, leading to reliable and robust database systems.