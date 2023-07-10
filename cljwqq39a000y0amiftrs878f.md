---
title: "DDD: Repositories"
datePublished: Mon Jul 10 2023 10:48:35 GMT+0000 (Coordinated Universal Time)
cuid: cljwqq39a000y0amiftrs878f
slug: ddd-repositories
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688984535444/ea2a18d8-0c22-472f-89b1-68e037d58ec3.webp
tags: csharp, software-architecture, net, ddd, domain-driven-design

---

In DDD, repositories play a crucial role in facilitating access to the underlying data persistence system, such as a database, to read and write business objects, particularly aggregates.

## What is a Repository?

A repository can be seen as a collection-like interface that acts as a bridge between the Domain and Application Layers, providing a standardized way to interact with the data persistence layer. It encapsulates the logic required to perform CRUD (Create, Read, Update, Delete) operations on business entities.

## Repository Principles

To effectively implement repositories in DDD, it is essential to adhere to a set of common principles:

### 1\. Define a Repository Interface in the Domain Layer

Repositories should be defined as interfaces in the Domain Layer. By doing so, we ensure that repositories can be utilized by both the Domain and Application Layers. The interface serves as a contract that defines the operations available for accessing and manipulating business objects.

```csharp
namespace Domain.Repositories
{
    public interface IProductRepository
    {
        Product GetById(Guid productId);
        void Add(Product product);
        void Update(Product product);
        void Remove(Product product);
    }
}
```

### 2\. Implement the Repository in the Infrastructure Layer

The concrete implementation of the repository resides in the Infrastructure Layer, which typically includes data access technologies like Entity Framework Core. By separating the implementation from the domain logic, we maintain a clear separation of concerns and promote modularity.

```csharp
namespace Infrastructure.Repositories
{
    public class ProductRepository : IProductRepository
    {
        private readonly DbContext _context;

        public ProductRepository(DbContext context)
        {
            _context = context;
        }

        public Product GetById(Guid productId)
        {
            // Implementation details using EF Core
        }

        public void Add(Product product)
        {
            // Implementation details using EF Core
        }

        public void Update(Product product)
        {
            // Implementation details using EF Core
        }

        public void Remove(Product product)
        {
            // Implementation details using EF Core
        }
    }
}
```

### 3\. Avoid Including Business Logic in Repositories

Repositories should focus solely on data access and manipulation tasks. It is important to keep them free from any business logic. Business rules and validations should be handled by the domain entities and services, ensuring a clear separation of concerns.

### 4\. Repository Interface Should be Database Provider / ORM Independent

To promote flexibility and avoid coupling the repositories to a specific database provider or Object-Relational Mapping (ORM) framework, the repository interface should not expose provider-specific types. For example, it is recommended to avoid returning a `DbSet` (provided by EF Core) from a repository method.

### 5\. Create Repositories for Aggregate Roots

In DDD, repositories are typically created for aggregate roots rather than individual entities. An aggregate root represents a cluster of associated entities that should be treated as a single unit. Accessing sub-collection entities within an aggregate should be done through the aggregate root, ensuring consistency and transactional boundaries.

Remember, repositories should primarily focus on data access and manipulation, leaving the domain logic to the domain entities and services.