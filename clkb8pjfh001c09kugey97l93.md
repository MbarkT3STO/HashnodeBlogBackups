---
title: "PostgreSQL: Creating a Database"
datePublished: Thu Jul 20 2023 14:20:49 GMT+0000 (Coordinated Universal Time)
cuid: clkb8pjfh001c09kugey97l93
slug: postgresql-creating-a-database
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689862545847/0cdf17a2-bedb-48da-b806-cb2e2178ae8c.png
tags: postgresql, databases, sql

---

In this blog post, we'll walk you through the process of creating a database in PostgreSQL.

## Creating a Database

To create a new database in PostgreSQL, we'll use the `CREATE DATABASE` SQL command. This command allows you to specify the name of the new database and any additional options you want to set. The basic syntax for creating a database is as follows:

```sql
CREATE DATABASE database_name;
```

Let's create a simple example database named "blog\_db":

```sql
CREATE DATABASE blog_db;
```