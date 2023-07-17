---
title: "MongoDB: Aggregation"
datePublished: Mon Jul 17 2023 09:03:55 GMT+0000 (Coordinated Universal Time)
cuid: clk6n2ggq000b09mt9pqscxde
slug: mongodb-aggregation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689584502332/87b05f19-3673-4e5e-970b-d2696b9f8668.webp
tags: nosql, mongodb, databases

---

As the world of technology constantly evolves, so does the need for powerful and efficient data processing and analysis. MongoDB, a leading NoSQL database, has gained popularity for its flexibility and scalability. One of its standout features is **aggregation**, a robust data transformation pipeline that empowers developers to perform complex data operations within the database.

In this blog post, we will explore the concept of MongoDB aggregation, understand its benefits, and walk through practical examples to demonstrate its capabilities.

## What is MongoDB Aggregation?

MongoDB aggregation is a framework for processing and transforming data within the database. It allows developers to perform various operations, such as grouping, filtering, sorting, and computing, on data stored in MongoDB collections. Aggregation pipelines consist of multiple stages, each representing a data transformation step. These stages are executed sequentially, creating a powerful and flexible data processing pipeline.

## The Advantages of Aggregation

MongoDB aggregation offers several advantages over traditional query methods:

1. **Efficiency**: Aggregation pipelines are designed to work with large datasets efficiently. They can utilize indexes and memory for faster processing, reducing the need for extensive client-side computation.
    
2. **Flexibility**: The aggregation framework supports a wide range of data transformation operations, making it highly adaptable to various data analysis scenarios.
    
3. **Reduced Data Transfer**: With aggregation, you can filter, project, and transform data directly within the database, reducing the amount of data transferred between the database and application.
    
4. **Scalability**: As MongoDB is built to scale horizontally, aggregation pipelines can handle significant data processing requirements while maintaining performance.
    

## Aggregation Stages and Examples

Let's dive into some essential aggregation stages and their practical applications.

### 1\. `$match`: Filtering Documents

The `$match` stage filters documents in the collection based on specified criteria. This allows you to work with only the data you need, reducing the pipeline's workload.

**Example**: Find all documents with "status" equal to "completed."

```json
db.orders.aggregate([
  { $match: { status: "completed" } }
])
```

### 2\. `$group`: Grouping Data

The `$group` stage groups documents based on a specific key and allows performing aggregate functions on grouped data.

**Example**: Calculate the total revenue for each product category.

```json
db.orders.aggregate([
  {
    $group: {
      _id: "$product_category",
      totalRevenue: { $sum: "$price" }
    }
  }
])
```

### 3\. `$project`: Reshaping Documents

The `$project` stage lets you reshape documents, including including, excluding, or renaming fields.

**Example**: Display only the customer name and order date.

```json
db.orders.aggregate([
  {
    $project: {
      _id: 0,
      customerName: 1,
      orderDate: 1
    }
  }
])
```

### 4\. `$sort`: Sorting Results

The `$sort` stage sorts the documents based on specified criteria.

**Example**: Sort the orders by order date in descending order.

```json
db.orders.aggregate([
  {
    $sort: { orderDate: -1 }
  }
])
```

### 5\. `$limit` and `$skip`: Pagination

The `$limit` and `$skip` stages allow pagination of results.

**Example**: Retrieve the first 10 orders sorted by order date.

```json
db.orders.aggregate([
  { $sort: { orderDate: 1 } },
  { $limit: 10 }
])
```

### 6\. `$unwind`: Flattening Arrays

The `$unwind` stage is used to unwind array fields, creating separate documents for each array element.

**Example**: Get a list of all products sold, with one product per document.

```json
db.orders.aggregate([
  { $unwind: "$products" }
])
```