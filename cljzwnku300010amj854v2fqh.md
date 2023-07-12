---
title: "MongoDB: Update Documents"
datePublished: Wed Jul 12 2023 15:57:54 GMT+0000 (Coordinated Universal Time)
cuid: cljzwnku300010amj854v2fqh
slug: mongodb-update-documents
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689176896952/60bd614f-b03a-457d-a204-d65754d1a645.webp
tags: nosql, mongodb, databases

---

MongoDB, the popular NoSQL database, provides powerful capabilities for updating documents in a flexible and efficient manner. Whether you need to modify a single field or perform complex updates across multiple documents, In this blog post, we'll explore different ways to update documents in MongoDB, accompanied by examples to illustrate the concepts.

## Update Operators

MongoDB utilizes update operators to specify the modifications to be applied to the documents. These operators allow you to perform a wide range of updates, including setting field values, incrementing or decrementing numeric values, adding or removing elements from arrays, and more. Here are a few commonly used update operators:

### $set

The `$set` operator updates the value of a field in a document. It can create a new field if it doesn't exist or overwrite the existing value. Let's say we have a collection called `users` with the following document:

```json
{
  "_id": ObjectId("60e2070e18d5d339cc5f1699"),
  "name": "John Doe",
  "age": 30
}
```

To update the `age` field to 35, we can use the `$set` operator as follows:

```javascript
db.users.updateOne(
  { "_id": ObjectId("60e2070e18d5d339cc5f1699") },
  { "$set": { "age": 35 } }
)
```

### $inc

The `$inc` operator increments the value of a numeric field by a specified amount. Consider the following document in the `inventory` collection:

```json
{
  "_id": ObjectId("60e2070e18d5d339cc5f16a0"),
  "item": "Apple",
  "quantity": 10
}
```

To increase the `quantity` field by 5, we can use the `$inc` operator:

```javascript
db.inventory.updateOne(
  { "_id": ObjectId("60e2070e18d5d339cc5f16a0") },
  { "$inc": { "quantity": 5 } }
)
```

### $push

The `$push` operator appends an element to an array field. Let's say we have a document in the `products` collection representing a user's shopping cart:

```json
{
  "_id": ObjectId("60e2070e18d5d339cc5f16b0"),
  "user": "jdoe",
  "cart": ["item1", "item2"]
}
```

To add a new item, "item3," to the `cart` array, we can use the `$push` operator:

```javascript
db.products.updateOne(
  { "_id": ObjectId("60e2070e18d5d339cc5f16b0") },
  { "$push": { "cart": "item3" } }
)
```

## Updating Multiple Documents

MongoDB provides options to update multiple documents that match a given filter condition. This can be accomplished using the `updateMany()` method instead of `updateOne()`:

```javascript
db.users.updateMany(
  { "age": { "$gte": 30 } },
  { "$inc": { "age": 5 } }
)
```

The above query increases the `age` field by 5 for all documents where the `age` is greater than or equal to 30.

## Upserts

In addition to updating existing documents, MongoDB supports upserts, which combine the operations of update and insert. An upsert operation allows you to update a document if it exists or insert a new document if it doesn't. This is achieved using the `upsert` option in the update operations.

Consider a collection called `books` with the following document:

```json
{
  "_id": ObjectId("60e2070e18d5d339cc5f16c0"),
  "title": "The Great Gatsby",
  "author": "F. Scott Fitzgerald"
}
```

To update the document with a new field called `year` and set its value to 1925, you can use the `updateOne()` method with the `upsert` option:

```javascript
db.books.updateOne(
  { "title": "The Great Gatsby" },
  { "$set": { "year": 1925 } },
  { "upsert": true }
)
```

If a document matching the filter condition exists, the `$set` operation updates the `year` field. Otherwise, MongoDB inserts a new document with the specified fields.

## Updating Nested Fields

MongoDB allows updating nested fields within documents using the dot notation. This notation helps specify the exact field to update within a nested structure. Let's say we have a collection called `employees` with the following document:

```json
{
  "_id": ObjectId("60e2070e18d5d339cc5f16d0"),
  "name": "Jane Smith",
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY"
  }
}
```

To update the `city` field within the `address` nested object, you can use the dot notation in the update operation:

```javascript
db.employees.updateOne(
  { "_id": ObjectId("60e2070e18d5d339cc5f16d0") },
  { "$set": { "address.city": "San Francisco" } }
)
```

This operation only modifies the `city` field while leaving other fields within the `address` object unchanged.