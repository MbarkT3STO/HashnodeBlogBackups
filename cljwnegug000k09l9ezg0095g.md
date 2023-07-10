---
title: "DDD: Difference between Aggregate and Aggregate Root"
datePublished: Mon Jul 10 2023 09:15:34 GMT+0000 (Coordinated Universal Time)
cuid: cljwnegug000k09l9ezg0095g
slug: ddd-difference-between-aggregate-and-aggregate-root
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688980233535/fa241a38-bcd7-4f6a-a18d-6d2d6f9ceaf6.webp
tags: csharp, software-architecture, net, ddd, domain-driven-design

---

## **Aggregate**

An aggregate is a cluster of related objects that are treated as a single unit within a domain model. It represents a transactional consistency boundary and ensures that all changes within the aggregate are applied atomically. Aggregates encapsulate a set of entities and value objects and provide a controlled and consistent way to modify them.

Let's consider an example of a blogging platform where we have a `BlogPost` entity and a `Comment` entity. These entities can be part of the same aggregate if they share transactional boundaries and are closely related. Here's an example implementation in C#:

```csharp
public class BlogPost
{
    public Guid Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public List<Comment> Comments { get; set; }

    public void AddComment(string author, string content)
    {
        // Create a new Comment entity and add it to the Comments collection
    }
}

public class Comment
{
    public Guid Id { get; set; }
    public string Author { get; set; }
    public string Content { get; set; }
}
```

In this example, the `BlogPost` and `Comment` entities are part of the same aggregate. Any modifications to the comments of a blog post should be performed through the `BlogPost` aggregate to maintain consistency.

## **Aggregate Root**

An aggregate root is a specific entity within the aggregate that acts as the primary access point to the aggregate. It is responsible for enforcing the aggregate's invariants and maintaining its integrity. The aggregate root encapsulates the internal structure of the aggregate and provides methods for modifying its state.

In our blogging platform example, the `BlogPost` entity can act as the aggregate root since it is the primary entity within the aggregate. Here's an updated version of the code:

```csharp
public class BlogPost : IAggregateRoot
{
    public Guid Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public List<Comment> Comments { get; set; }

    public void AddComment(string author, string content)
    {
        // Create a new Comment entity and add it to the Comments collection
    }

    public void DeleteComment(Guid commentId)
    {
        // Remove the specified comment from the Comments collection
    }
}

public class Comment
{
    public Guid Id { get; set; }
    public string Author { get; set; }
    public string Content { get; set; }
}
```

In this updated example, the `BlogPost` class implements the `IAggregateRoot` interface to indicate its role as the root of the aggregate. It provides methods such as `AddComment` and `DeleteComment` to modify the state of the aggregate. Other operations related to the aggregate would also be defined within the `BlogPost` class.