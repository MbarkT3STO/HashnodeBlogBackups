---
title: "MongoDB: Update Arrays"
datePublished: Wed Jul 12 2023 18:29:51 GMT+0000 (Coordinated Universal Time)
cuid: clk022ze4000e09mp01clcswx
slug: mongodb-update-arrays
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689185121706/16ea2b9c-f3c0-42d0-8a47-a58e29f81783.webp
tags: nosql, mongodb, databases

---

One of the features of MongoDB is its ability to handle arrays and perform powerful operations on them. In this blog post, we will explore how to update arrays in MongoDB using various methods and provide examples to illustrate their usage.

## Prerequisites

Before diving into array updates in MongoDB, make sure you have the following:

* MongoDB installed and running on your system.
    
* A basic understanding of MongoDB queries and operations.
    

## Updating Arrays using $push

The `$push` operator is commonly used to add elements to an existing array. It appends the specified value(s) to the array field. Here's an example:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $push: { skills: "MongoDB" } }
);
```

In the above example, we use the `updateOne` method to update the `users` collection. The `$push` operator is used to add the value `"MongoDB"` to the `skills` array of a specific user identified by their `_id`.

## Updating Arrays using $addToSet

The `$addToSet` operator is similar to `$push`, but it only adds elements to an array if they don't already exist. This ensures that each element in the array is unique. Let's see an example:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $addToSet: { skills: { $each: ["MongoDB", "Node.js"] } } }
);
```

In this example, the `$addToSet` operator adds multiple elements to the `skills` array: `"MongoDB"` and `"Node.js"`. If any of these values already exist in the array, they will not be added again.

### **$each**

The `$each` modifier is used in conjunction with `$push` or `$addToSet` operators to add multiple elements to an array at once. It allows you to specify an array of values that should be added individually.

## Updating Arrays using $pull

The `$pull` operator is used to remove specific elements from an array. It removes all occurrences of the specified value(s) from the array field. Here's an example:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $pull: { skills: "MongoDB" } }
);
```

In the above example, we remove all occurrences of `"MongoDB"` from the `skills` array of the user identified by their `_id`.

## Updating Arrays using $pop

The `$pop` operator allows you to remove the first or last element from an array. It accepts a value of either `-1` or `1` to indicate whether to remove the first or last element, respectively. Here's an example:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $pop: { skills: 1 } }
);
```

In the above example, the `$pop` operator removes the last element from the `skills` array of the user identified by their `_id`.

## Updating Arrays using $each and $position

In addition to the array update operators we discussed earlier, MongoDB also provides the `$each` and `$position` modifiers, which allow for more granular control over array updates.

The `$each` modifier is used in conjunction with `$push` or `$addToSet` to add multiple elements to an array at once. Here's an example:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $push: { skills: { $each: ["React", "Python"], $position: 0 } } }
);
```

In this example, we use `$each` to add the elements `"React"` and `"Python"` to the beginning of the `skills` array. The `$position` modifier specifies the index at which the elements should be inserted. In this case, the elements will be added at index 0.

## Updating Arrays using $slice

The `$slice` operator allows you to update an array by specifying the number of elements to keep or discard from the beginning or end of the array. It is useful when you want to limit the size of an array or retrieve a subset of elements. Here's an example:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $push: { skills: { $each: ["Java", "C++"], $slice: -3 } } }
);
```

In this example, we use `$slice` with a negative value `-3`, indicating that only the last three elements of the `skills` array should be kept. If the array already has more than three elements, the oldest elements will be discarded.

## Updating Arrays using Positional Operator ($)

The positional operator `$` allows you to update specific elements within an array based on a query condition. It is particularly useful when you want to update a specific element without knowing its index. Consider the following example:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a"), "skills": "JavaScript" },
  { $set: { "skills.$": "TypeScript" } }
);
```

In this example, we use the positional operator `$` in the query condition to find a user who has `"JavaScript"` in their `skills` array. We then use `$set` to update the matched element to `"TypeScript"`. The positional operator ensures that only the matching element is updated.

## **$slice in Details**

MongoDB provides the `$slice` operator, which allows you to update an array by specifying the number of elements to keep or discard from the beginning or end of the array. This operator is useful when you want to limit the size of an array or retrieve a subset of elements.

The `$slice` operator can be used in conjunction with the `$push` or `$addToSet` operators to update an array. It accepts a numerical value, which determines the number of elements to keep or discard. The behavior of `$slice` depends on whether the value is positive or negative:

* Positive Value: If the value is positive, it specifies the number of elements to keep from the beginning of the array. The remaining elements are discarded.
    
* Negative Value: If the value is negative, it specifies the number of elements to discard from the end of the array. The specified number of elements from the beginning of the array is retained.
    

Let's illustrate the usage of `$slice` with a few examples:

### Example 1: Keeping Elements from the Beginning

Consider the following scenario. You have a `skills` array with the following elements: `["JavaScript", "Python", "Java", "C++", "Ruby"]`. You want to keep only the first three elements and discard the rest. Here's how you can achieve that using `$slice`:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $push: { skills: { $each: ["JavaScript", "Python", "Java", "C++", "Ruby"], $slice: 3 } } }
);
```

In the above example, we use `$slice: 3` to specify that we want to keep only the first three elements from the `skills` array. MongoDB will discard `"C++"` and `"Ruby"` from the end of the array, resulting in the updated array: `["JavaScript", "Python", "Java"]`.

### Example 2: Discarding Elements from the End

Now, let's say you have the same `skills` array as in the previous example: `["JavaScript", "Python", "Java", "C++", "Ruby"]`. This time, you want to discard the last two elements and retain the rest. Here's how you can achieve that using `$slice`:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $push: { skills: { $each: ["JavaScript", "Python", "Java", "C++", "Ruby"], $slice: -3 } } }
);
```

In this example, we use `$slice: -3` to specify that we want to discard the last two elements from the `skills` array. MongoDB will keep the first three elements and discard `"Java"`, `"C++"`, and `"Ruby"`, resulting in the updated array: `["JavaScript", "Python"]`.

### Example 3: Handling Existing Array Size

When updating an array with `$slice`, it's important to consider the existing size of the array. If the array already has fewer elements than the specified slice value, MongoDB will either keep all existing elements or add null values to meet the required size.

Let's consider the following scenario. You have a `skills` array with two existing elements: `["JavaScript", "Python"]`. You want to keep the first four elements from a new array: `["Java", "C++", "Ruby", "Go"]`. Here's how you can update the array using `$slice`:

```javascript
db.users.updateOne(
  { _id: ObjectId("609d1c209d7e6e245468e09a") },
  { $push: { skills: { $each: ["Java", "C++", "Ruby", "Go"], $slice: 4 } } }
);
```

In this case, since the existing array has only two elements, MongoDB will add null values for the missing elements and keep the first four elements from the new array. The resulting updated array will be: `["JavaScript", "Python", "Java", "C++"]`.