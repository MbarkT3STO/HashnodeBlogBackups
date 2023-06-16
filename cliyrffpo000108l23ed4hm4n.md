---
title: "ASP.NET Core: How to use Fluent Validation with CQRS (MediatR)?"
datePublished: Fri Jun 16 2023 16:04:07 GMT+0000 (Coordinated Universal Time)
cuid: cliyrffpo000108l23ed4hm4n
slug: aspnet-core-how-to-use-fluent-validation-with-cqrs-mediatr
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686931334491/d93f7863-a4d8-49bc-8500-cfc8d33aea15.webp
tags: csharp, net, cors, aspnet-core, dotnet

---

In modern web development, the CQRS (Command Query Responsibility Segregation) pattern and MediatR library have gained popularity for building scalable and maintainable applications. When it comes to validating input data, Fluent Validation provides a flexible and intuitive approach. In this article, we will explore how to integrate Fluent Validation with CQRS and MediatR in an [ASP.NET](http://ASP.NET) Core application.

## **Prerequisites**

Before we dive into the implementation, make sure you have the following prerequisites:

* Basic knowledge of [ASP.NET](http://ASP.NET) Core.
    
* Familiarity with CQRS and MediatR.
    
* Understanding of Fluent Validation.
    

## **Setting Up the Project**

Let's start by setting up a new [ASP.NET](http://ASP.NET) Core project. Open Visual Studio and follow these steps:

1. Select "Create a new project."
    
2. Choose the [ASP.NET](http://ASP.NET) Core Web Application template.
    
3. Set the project name and location.
    
4. Select the desired framework and click "Create."
    

Once the project is created, we can move on to installing the necessary NuGet packages.

## **Installing Required Packages**

To begin, install the following NuGet packages:

* **FluentValidation.AspNetCore**: This package provides integration with [ASP.NET](http://ASP.NET) Core for Fluent Validation.
    
* [**MediatR.Extensions.Microsoft**](http://MediatR.Extensions.Microsoft)**.DependencyInjection**: This package is required for setting up MediatR in the [ASP.NET](http://ASP.NET) Core application.
    

You can install these packages using the NuGet Package Manager or by running the following command in the Package Manager Console:

```sql
Install-Package FluentValidation.AspNetCore
Install-Package MediatR.Extensions.Microsoft.DependencyInjection
```

## **Commands and Queries**

Let's start by defining our commands and queries. In the CQRS pattern, commands are used to perform actions that modify the system's state, while queries are used to retrieve data without modifying the state.

```csharp
// CreateAccountCommand.cs

public class CreateAccountCommand : IRequest
{
    public string Username { get; set; }
    public string Email { get; set; }
    public string Password { get; set; }
}
```

```csharp
// GetAccountQuery.cs

public class GetAccountQuery : IRequest<AccountDto>
{
    public int AccountId { get; set; }
}
```

In the above code, we have defined a `CreateAccountCommand` that represents the action of creating a new user account. It contains properties for the username, email, and password. Additionally, we have a `GetAccountQuery` that retrieves an account by its ID and returns an `AccountDto` object.

## **Validators**

Next, we need to create validators for our commands and queries using Fluent Validation. Validators define the rules and constraints that the input data should adhere to.

```csharp
// CreateAccountValidator.cs

public class CreateAccountValidator : AbstractValidator<CreateAccountCommand>
{
    public CreateAccountValidator()
    {
        RuleFor(command => command.Username)
            .NotEmpty().WithMessage("Username is required.")
            .Length(3, 20).WithMessage("Username must be between 3 and 20 characters.");

        RuleFor(command => command.Email)
            .NotEmpty().WithMessage("Email is required.")
            .EmailAddress().WithMessage("Invalid email address.");

        RuleFor(command => command.Password)
            .NotEmpty().WithMessage("Password is required.")
            .MinimumLength(8).WithMessage("Password must be at least 8 characters long.");
    }
}
```

```csharp
// GetAccountQueryValidator.cs

public class GetAccountQueryValidator : AbstractValidator<GetAccountQuery>
{
    public GetAccountQueryValidator()
    {
        RuleFor(query => query.AccountId)
            .GreaterThan(0).WithMessage("Account ID must be greater than zero.");
    }
}
```

In the above code, we have defined a `CreateAccountValidator` that validates the `CreateAccountCommand` based on various rules. Similarly, the `GetAccountQueryValidator` validates the `GetAccountQuery` by ensuring the account ID is greater than zero.

## **Handlers**

Now that we have our commands, queries, and validators in place, let's implement the handlers that will process these requests and perform the necessary actions or data retrieval.

```csharp
//CreateAccountCommandHandler.cs

public class CreateAccountCommandHandler : IRequestHandler<CreateAccountCommand, Unit>
{
    private readonly IValidator<CreateAccountCommand> _validator;

    public CreateAccountCommandHandler(IValidator<CreateAccountCommand> validator)
    {
        _validator = validator;
    }

    public async Task<Unit> Handle(CreateAccountCommand command, CancellationToken cancellationToken)
    {
        var validationResult = await _validator.ValidateAsync(command);

        if (!validationResult.IsValid)
        {
            throw new ValidationException(validationResult.Errors);
        }

        // Perform account creation logic

        return Unit.Value;
    }
}
```

```csharp
// GetAccountQueryHandler.cs

public class GetAccountQueryHandler : IRequestHandler<GetAccountQuery, AccountDto>
{
    private readonly IValidator<GetAccountQuery> _validator;

    public GetAccountQueryHandler(IValidator<GetAccountQuery> validator)
    {
        _validator = validator;
    }

    public async Task<AccountDto> Handle(GetAccountQuery query, CancellationToken cancellationToken)
    {
        var validationResult = await _validator.ValidateAsync(query);

        if (!validationResult.IsValid)
        {
            throw new ValidationException(validationResult.Errors);
        }

        // Retrieve account logic

        return accountDto;
    }
}
```

In the handlers, we inject the corresponding validator via the constructor using the `IValidator<T>` interface. This allows us to access the validator instance and validate the command or query by calling the `ValidateAsync` method. If the validation fails, we throw a `ValidationException` and provide the validation errors.

## **Validation using IPipelineBehavior**

In addition to manual validation inside the handlers, Fluent Validation can also be integrated into the MediatR pipeline using the `IPipelineBehavior` interface. This approach allows for automatic validation of commands and queries before they reach the corresponding handlers. Let's explore how to set up and use this approach.

### **Setting up Fluent Validation Pipeline Behavior**

To use Fluent Validation with MediatR pipeline behavior, we need to create a pipeline behavior class that implements the `IPipelineBehavior` interface. This behavior class will intercept incoming commands and queries and perform the validation before passing them to the handlers.

```csharp
// ValidationPipelineBehavior.cs

public class ValidationPipelineBehavior<TRequest, TResponse> : IPipelineBehavior<TRequest, TResponse>
    where TRequest : IRequest<TResponse>
{
    private readonly IValidator<TRequest> _validator;

    public ValidationPipelineBehavior(IValidator<TRequest> validator)
    {
        _validator = validator;
    }

    public async Task<TResponse> Handle(TRequest request, CancellationToken cancellationToken, RequestHandlerDelegate<TResponse> next)
    {
        var validationResult = await _validator.ValidateAsync(request);

        if (!validationResult.IsValid)
        {
            throw new ValidationException(validationResult.Errors);
        }

        return await next();
    }
}
```

In the above code, we have created a `ValidationPipelineBehavior` class that implements the `IPipelineBehavior` interface. The behavior class takes an `IValidator<TRequest>` as a dependency via the constructor. It intercepts the request, validates it using the injected validator, and throws a `ValidationException` if the validation fails. If the validation succeeds, it calls the next handler in the pipeline using the `next()` delegate.

### **Registering Fluent Validation Pipeline Behavior**

To register the Fluent Validation pipeline behavior, we need to add it to the MediatR pipeline configuration in the `Startup.cs` file of our [ASP.NET](http://ASP.NET) Core application.

```csharp
// Startup.cs

public void ConfigureServices(IServiceCollection services)
{
    // ...

    services.AddValidatorsFromAssembly(Assembly.GetExecutingAssembly());
    services.AddTransient(typeof(IPipelineBehavior<,>), typeof(ValidationPipelineBehavior<,>));
    services.AddMediatR(Assembly.GetExecutingAssembly());

    // ...
}
```

In the above code, we use the `AddValidatorsFromAssembly` method to scan and register all the validator classes from the current assembly. Then, we use `AddTransient` to register the `ValidationPipelineBehavior` as a transient service for all request and response types. Finally, we call `AddMediatR` to register MediatR and its dependencies.

### **Using Validators with Handlers**

With the Fluent Validation pipeline behavior set up, we can now remove the manual validation from our handlers since the validation will be automatically performed by the pipeline behavior.

```csharp
// CreateAccountCommandHandler.cs

public class CreateAccountCommandHandler : IRequestHandler<CreateAccountCommand, Unit>
{
    public async Task<Unit> Handle(CreateAccountCommand command, CancellationToken cancellationToken)
    {
        // Perform account creation logic

        return Unit.Value;
    }
}
```

```csharp
// GetAccountQueryHandler.cs

public class GetAccountQueryHandler : IRequestHandler<GetAccountQuery, AccountDto>
{
    public async Task<AccountDto> Handle(GetAccountQuery query, CancellationToken cancellationToken)
    {
        // Retrieve account logic

        return accountDto;
    }
}
```

In the above code, we no longer manually validate the commands and queries within the handlers. Instead, the validation is automatically triggered by the pipeline behavior, which intercepts the requests before they reach the handlers.

## **Conclusion**

In this article, we have learned how to leverage Fluent Validation in combination with the CQRS pattern and MediatR in an [ASP.NET](http://ASP.NET) Core application. We started by defining commands and queries to represent actions and data retrieval requests. Then, we created validators using Fluent Validation to enforce validation rules on the input data.

We explored two approaches for incorporating validation into our application. First, we manually validated the commands and queries within the handlers by injecting the corresponding validators and invoking the `ValidateAsync` method. This approach provided us with fine-grained control over the validation process.

Next, we looked at using the `IPipelineBehavior` interface to integrate Fluent Validation into the MediatR pipeline. By creating a validation pipeline behavior, we could automatically validate the incoming commands and queries before they reached the handlers. This approach allowed for a more centralized and automated validation process.

To further assist you in understanding the concepts discussed in this article, you can take a look at the following demo project: [**FluentValidationDemo**](https://github.com/MbarkT3STO/FluentValidationDemo). The demo project provides a simple practical example and implementation for using Fluent Validation with CQRS and MediatR in an [ASP.NET](http://ASP.NET) Core application.