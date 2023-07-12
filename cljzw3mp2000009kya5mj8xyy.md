---
title: "MongoDB: Sort Documents"
datePublished: Wed Jul 12 2023 15:42:23 GMT+0000 (Coordinated Universal Time)
cuid: cljzw3mp2000009kya5mj8xyy
slug: mongodb-sort-documents
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689175790772/621d31d9-6efb-411c-96c8-d6a16fa76329.webp
tags: nosql, mongodb, databases

---

One of the key features of MongoDB is its ability to sort documents based on various criteria. In this blog post, we will explore how to sort documents in MongoDB and provide examples to demonstrate the sorting functionality.

## Sorting Documents in MongoDB

MongoDB provides the `sort()` method to sort documents in a collection. This method takes one or more sorting criteria as input and returns a cu rsor pointing to the sorted documents. The sorting criteria can be specified using the `sort()` method in the following format:

```javascript
db.collection.find().sort({ field1: 1, field2: -1 });
```

In the above example, `collection` represents the name of the collection where the documents are stored. The `find()` method is used to retrieve documents from the collection, and the `sort()` method is chained to it to specify the sorting criteria. In this case, the documents will be sorted in ascending order based on `field1` and in descending order based on `field2`.

## Sorting in Ascending Order

To sort documents in ascending order, you can specify the field to sort by with a value of `1`. Let's consider an example where we have a collection named `books` with documents representing books and their corresponding publication years. To sort the books in ascending order based on their publication years, we can use the following query:

```javascript
db.books.find().sort({ year: 1 });
```

This query will return a cursor pointing to the documents in the `books` collection, sorted in ascending order based on the `year` field.

## Sorting in Descending Order

To sort documents in descending order, you can specify the field to sort by with a value of `-1`. Let's continue with our previous example and sort the books in descending order based on their publication years:

```javascript
db.books.find().sort({ year: -1 });
```

This query will return a cursor pointing to the documents in the `books` collection, sorted in descending order based on the `year` field.

## Sorting on Multiple Fields

MongoDB also allows sorting on multiple fields. When sorting on multiple fields, the sorting criteria are applied in the order they are specified. Let's consider the `books` collection again and assume that each document contains both a `year` field and a `title` field. To sort the books first by publication year in ascending order and then by title in descending order, we can use the following query:

```javascript
db.books.find().sort({ year: 1, title: -1 });
```

This query will return a cursor pointing to the documents in the `books` collection, sorted first in ascending order based on the `year` field, and for documents with the same year, sorted in descending order based on the `title` field.