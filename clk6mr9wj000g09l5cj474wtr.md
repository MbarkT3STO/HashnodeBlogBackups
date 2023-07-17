---
title: "MongoDB: Relationships"
datePublished: Mon Jul 17 2023 08:55:13 GMT+0000 (Coordinated Universal Time)
cuid: clk6mr9wj000g09l5cj474wtr
slug: mongodb-relationships
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689583948049/4f8b5fa4-4587-435a-a5de-bcba4b8368f4.webp
tags: nosql, mongodb, databases

---

One of the key differences between MongoDB and traditional relational databases is the handling of relationships between data. In this blog post, we will explore how to implement relationships in MongoDB and examine different strategies to model related data effectively.

## Understanding MongoDB Relationships

In MongoDB, relationships between data are not established through traditional foreign keys as seen in relational databases. Instead, MongoDB offers two primary techniques to represent relationships:

1. **Embedding**: Embedding involves nesting related data within a single document. It is suitable for one-to-one and one-to-many relationships where the related data is relatively small and doesn't frequently change.
    
2. **Referencing**: Referencing creates a link between separate documents, allowing data to be spread across collections. It is ideal for modeling one-to-many and many-to-many relationships where related data can grow significantly or change independently.
    

Now, let's dive into each method with examples.

## 1\. Embedding Relationships

### One-to-One Relationship

Consider a scenario where you have a MongoDB database for an e-commerce platform, and you want to store customer and address information. Since one customer has only one address, you can embed the address directly into the customer document.

```json
{
  "_id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com",
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "country": "USA"
  }
}
```

### One-to-Many Relationship

Let's extend the e-commerce example to include order information. Since a customer can have multiple orders, we can embed an array of orders within the customer document.

```json
{
  "_id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com",
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "country": "USA"
  },
  "orders": [
    {
      "_id": 101,
      "date": "2023-07-17",
      "total": 59.99
    },
    {
      "_id": 102,
      "date": "2023-07-16",
      "total": 42.50
    }
  ]
}
```

## 2\. Referencing Relationships

### One-to-Many Relationship

In this scenario, we'll separate customers, addresses, and orders into individual collections and reference the related data using ObjectId.

#### Customer Collection

```json
{
  "_id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com"
}
```

#### Address Collection

```json
{
  "_id": 101,
  "customerId": 1,
  "street": "123 Main St",
  "city": "Anytown",
  "country": "USA"
}
```

#### Orders Collection

```json
{
  "_id": 501,
  "customerId": 1,
  "date": "2023-07-17",
  "total": 59.99
}
```

### Many-to-Many Relationship

Consider a case where you have a MongoDB database for a blogging platform, and you want to handle tags for each blog post. Multiple blog posts can have the same tag, and a blog post can have multiple tags. In this situation, we can use referencing to model the relationship.

#### Blog Posts Collection

```json
{
  "_id": 201,
  "title": "Getting Started with MongoDB",
  "content": "Learn how to implement relationships in MongoDB.",
  "tags": [301, 302, 303]
}
```

#### Tags Collection

```json
{
  "_id": 301,
  "name": "MongoDB"
}
{
  "_id": 302,
  "name": "NoSQL"
}
{
  "_id": 303,
  "name": "Databases"
}
```