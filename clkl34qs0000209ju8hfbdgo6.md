---
title: "PostgreSQL: Data Types"
datePublished: Thu Jul 27 2023 11:42:22 GMT+0000 (Coordinated Universal Time)
cuid: clkl34qs0000209ju8hfbdgo6
slug: postgresql-data-types
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690457277977/5e8fa064-459d-4939-8034-9c7f81cf9a39.png
tags: postgresql, sql

---

## Introduction

In this blog post, we will explore some of the most commonly used data types in PostgreSQL, along with examples to demonstrate their usage.

## 1\. Numeric Data Types

PostgreSQL provides several numeric data types to handle different ranges and precision requirements. Here are some of the most commonly used numeric data types:

### 1.1. INTEGER

The `INTEGER` data type is used to store whole numbers within the range of -2,147,483,648 to 2,147,483,647. It is ideal for storing values like a person's age, number of items, etc.

Example:

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    age INTEGER
);

INSERT INTO employees (age) VALUES (28), (35), (42);
```

### 1.2. DECIMAL

The `DECIMAL` data type is used to store numbers with high precision. It is suitable for financial and monetary data, where accuracy is crucial.

Example:

```sql
CREATE TABLE invoices (
    invoice_id SERIAL PRIMARY KEY,
    amount DECIMAL(10, 2)
);

INSERT INTO invoices (amount) VALUES (123.45), (987.65), (543.21);
```

## 2\. Text Data Types

PostgreSQL offers various text data types to store character-based information. Below are some widely used text data types:

### 2.1. VARCHAR

The `VARCHAR` data type is used to store variable-length character strings. It is commonly employed to store text that can vary in length.

Example:

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100)
);

INSERT INTO products (product_name) VALUES ('PostgreSQL Book'), ('Laptop'), ('Smartphone');
```

### 2.2. TEXT

The `TEXT` data type is used for storing unlimited-length character strings. It is suitable for storing large textual content like descriptions or comments.

Example:

```sql
CREATE TABLE articles (
    article_id SERIAL PRIMARY KEY,
    content TEXT
);

INSERT INTO articles (content) VALUES ('Lorem ipsum dolor sit amet...'), ('A long text...'), ('More text...');
```

## 3\. Date and Time Data Types

PostgreSQL provides data types to handle date and time information. Let's look at two common ones:

### 3.1. DATE

The `DATE` data type is used to store calendar dates (year, month, and day).

Example:

```sql
CREATE TABLE events (
    event_id SERIAL PRIMARY KEY,
    event_name VARCHAR(100),
    event_date DATE
);

INSERT INTO events (event_name, event_date) VALUES ('Meeting', '2023-07-15'), ('Conference', '2023-08-25');
```

### 3.2. TIMESTAMP

The `TIMESTAMP` data type stores both date and time information, down to microseconds precision.

Example:

```sql
CREATE TABLE log_entries (
    entry_id SERIAL PRIMARY KEY,
    log_message TEXT,
    log_timestamp TIMESTAMP
);

INSERT INTO log_entries (log_message, log_timestamp) VALUES ('Error occurred.', '2023-07-27 12:34:56.789'), ('Informational log.', '2023-07-27 14:22:33.000');
```

## 4\. Boolean Data Type

The `BOOLEAN` data type is used to store true or false values.

Example:

```sql
CREATE TABLE tasks (
    task_id SERIAL PRIMARY KEY,
    task_name VARCHAR(100),
    is_completed BOOLEAN
);

INSERT INTO tasks (task_name, is_completed) VALUES ('Write blog post', false), ('Review code', true);
```

## 5\. Array Data Type

PostgreSQL supports the `ARRAY` data type, allowing you to store a collection of values of the same data type in a single column. This can be useful when dealing with lists, tags, or multiple related values.

Example:

```sql
CREATE TABLE books (
    book_id SERIAL PRIMARY KEY,
    book_title VARCHAR(100),
    authors TEXT[],
    genres VARCHAR(50)[]
);

INSERT INTO books (book_title, authors, genres)
VALUES ('PostgreSQL Handbook', ARRAY['John Doe', 'Jane Smith'], ARRAY['Technology', 'Database']);
```

## 6\. JSON Data Type

The `JSON` data type in PostgreSQL enables you to store and manipulate JSON (JavaScript Object Notation) data directly in the database. JSON is a widely used data interchange format, making it convenient for storing semi-structured or flexible data.

Example:

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    user_data JSON
);

INSERT INTO users (user_data)
VALUES ('{"name": "John Doe", "email": "john@example.com", "age": 30}');
```

## 7\. Enumerated Data Type

The `ENUM` data type allows you to define a set of named values, and a column using this data type can only store one of the specified values.

Example:

```sql
CREATE TYPE gender_enum AS ENUM ('Male', 'Female', 'Other');

CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    employee_name VARCHAR(100),
    gender gender_enum
);

INSERT INTO employees (employee_name, gender)
VALUES ('Alice', 'Female'), ('Bob', 'Male');
```

## 8\. Composite Data Type

PostgreSQL supports creating custom data types called composite types. A composite type can have multiple fields with different data types, allowing you to represent more complex data structures within a single column.

Example:

```sql
CREATE TYPE address AS (
    street VARCHAR(100),
    city VARCHAR(50),
    zip_code VARCHAR(10)
);

CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_address address
);

INSERT INTO customers (customer_name, customer_address)
VALUES ('John Smith', ROW('123 Main St', 'New York', '10001'));
```

## 9\. Geometric and Geographical Data Types

PostgreSQL offers support for geometric and geographical data types, which are particularly useful for applications dealing with location-based information.

### 9.1. Point

The `POINT` data type represents a point in a two-dimensional Cartesian coordinate system.

Example:

```sql
CREATE TABLE locations (
    location_id SERIAL PRIMARY KEY,
    location_name VARCHAR(100),
    coordinates POINT
);

INSERT INTO locations (location_name, coordinates)
VALUES ('Central Park', POINT(40.7829, -73.9654));
```

### 9.2. Polygon

The `POLYGON` data type represents a closed polygon defined by a set of points.

Example:

```sql
CREATE TABLE regions (
    region_id SERIAL PRIMARY KEY,
    region_name VARCHAR(100),
    area POLYGON
);

INSERT INTO regions (region_name, area)
VALUES ('Park Area', '(0, 0), (0, 100), (100, 100), (100, 0)');
```

## 10\. Network Address Data Types

PostgreSQL provides specialized data types for working with network addresses, such as IP addresses.

### 10.1. INET

The `INET` data type is used to store IPv4 and IPv6 addresses.

Example:

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    user_name VARCHAR(100),
    user_ip INET
);

INSERT INTO users (user_name, user_ip)
VALUES ('Jane Doe', '192.168.0.1'), ('John Smith', '2001:0db8:85a3:0000:0000:8a2e:0370:7334');
```

### 10.2. CIDR

The `CIDR` data type represents an IP address range using CIDR notation.

Example:

```sql
CREATE TABLE networks (
    network_id SERIAL PRIMARY KEY,
    network_name VARCHAR(100),
    subnet CIDR
);

INSERT INTO networks (network_name, subnet)
VALUES ('Internal Network', '192.168.0.0/24'), ('VPN Network', '10.0.0.0/16');
```