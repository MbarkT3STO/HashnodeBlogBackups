---
title: "MongoDB: Essential Shell Commands"
datePublished: Wed Jul 12 2023 14:39:02 GMT+0000 (Coordinated Universal Time)
cuid: cljztu5bp000209jk18i8gsks
slug: mongodb-essential-shell-commands
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689172148172/02cc5a47-1963-475a-9c9b-c49932da0e1e.webp
tags: nosql, mongodb, databases

---

The MongoDB Shell, also known as the MongoDB command-line interface (CLI), is a powerful tool for interacting with MongoDB databases. In this blog post, we will explore a wide range of MongoDB shell essential commands and provide examples to help you understand their usage.

## Prerequisites

Before we dive into the MongoDB shell commands, make sure you have MongoDB installed on your system.

For this purpose take a look at the previous posts:

[MongoDB: How to Install?](https://mbarkt3sto.hashnode.dev/mongodb-how-to-install)

[MongoDB: How to Install Mongo Shell?](https://mbarkt3sto.hashnode.dev/mongodb-how-to-install-mongo-shell)

## Connecting to a MongoDB Database

To begin using MongoDB shell commands, you need to establish a connection to a MongoDB server. Open your command-line interface and enter the following command:

```shell
mongo
```

This command will connect to the local MongoDB server running on the default port (27017). If you're connecting to a remote server or using a different port, you can specify the hostname and port as follows:

```shell
mongo <hostname>:<port>
```

Replace `<hostname>` with the server's hostname or IP address and `<port>` with the MongoDB server's port number.

## Database Operations

### Creating a Database

To create a new database, use the `use` command followed by the database name. For example, to create a database named "mydb," enter the following command:

```shell
use mydb
```

### Switching Databases

To switch to an existing database, use the `use` command followed by the database name. For instance, to switch to the "mydb" database, enter the following command:

```shell
use mydb
```

### Listing Databases

To list all the databases available on the MongoDB server, use the `show dbs` command. This command will display a list of database names along with their sizes. For example:

```shell
show dbs
```

### Dropping a Database

To drop (delete) a database, ensure that you are using the correct database and then execute the `db.dropDatabase()` command. For example, to drop the "mydb" database, use the following command:

```shell
use mydb
db.dropDatabase()
```

## Collection Operations

### Creating a Collection

To create a new collection within a database, use the `db.createCollection()` command. For instance, to create a collection named "users" in the "mydb" database, use the following command:

```shell
use mydb
db.createCollection("users")
```

### Listing Collections

To list all the collections within the current database, use the `show collections` command. For example:

```shell
show collections
```

### Dropping a Collection

To drop (delete) a collection, execute the `db.<collection_name>.drop()` command. For example, to drop the "users" collection, use the following command:

```shell
db.users.drop()
```

## Document Operations

### Inserting Documents

To insert documents into a collection, use the `db.<collection_name>.insertOne()` or `db.<collection_name>.insertMany()` commands. For instance, to insert a single document into the "users" collection, use the following command:

```shell
db.users.insertOne({ name: "John Doe", age: 30 })
```

### Querying Documents

To query documents from a collection, use the `db.<collection_name>.find()` command. You can pass a query filter to retrieve specific documents. For example, to find all documents in the "users" collection, use the following command:

```shell
db.users.find()
```

### Updating Documents

To update documents in a collection, use the `db.<collection_name>.updateOne()` or `db.<collection_name>.updateMany()` commands. For instance, to update a single document in the "users" collection, use the following command:

```shell
db.users.updateOne({ name: "John Doe" }, { $set: { age: 35 } })
```

### Deleting Documents

To delete documents from a collection, use the `db.<collection_name>.deleteOne()` or `db.<collection_name>.deleteMany()` commands. For example, to delete a single document from the "users" collection, use the following command:

```shell
db.users.deleteOne({ name: "John Doe" })
```