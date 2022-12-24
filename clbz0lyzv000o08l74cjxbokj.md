# How to work with AutoMapper in C#?

## **Introduction to AutoMapper**

AutoMapper is a popular object-to-object mapping library for .NET that can be used to efficiently convert one object's structure into another's. It can save a lot of manual mapping code, especially when dealing with complex object hierarchies or large data sets. In this article, we'll look at how to use AutoMapper in a C# project.

## **Installation and Setup**

To use AutoMapper in your C# project, you'll need to install the AutoMapper NuGet package. You can do this using the NuGet Package Manager in Visual Studio, or by running the following command in the Package Manager Console:

```csharp
Install-Package AutoMapper
```

Once you've installed the package, you'll need to set up AutoMapper in your application. This typically involves creating a static `Mapper` class that contains your mapping configuration. Here's an example of how to set up AutoMapper in a C# console application:

```csharp
static class Mapper
{
    private static readonly IMapper _mapper;

    static Mapper()
    {
        var config = new MapperConfiguration(cfg =>
        {
            // Add your mappings here
        });

        _mapper = config.CreateMapper();
    }

    public static TDestination Map<TSource, TDestination>(TSource source)
    {
        return _mapper.Map<TSource, TDestination>(source);
    }
}
```

## **Creating Mappings**

With AutoMapper set up, you can start creating mappings between your source and destination objects. There are several ways to create mappings, but the most common way is to use the `CreateMap` method in the `MapperConfiguration` object. Here's an example of how to create a simple mapping between two classes:

```csharp
config.CreateMap<Source, Destination>();
```

This will create a default mapping that copies all the public properties from the `Source` object to the corresponding properties in the `Destination` object.

You can also specify custom mappings using the `ForMember` method. For example, if you want to map a property in the `Source` object to a different property in the `Destination` object, you can use the following code:

```csharp
config.CreateMap<Source, Destination>()
    .ForMember(dest => dest.CustomProperty, opt => opt.MapFrom(src => src.SourceProperty));
```

This will map the `Source.SourceProperty` property to the `Destination.CustomProperty` property.

## **Using Mappings**

Once you've created your mappings, you can use them to convert objects from one type to another. To do this, you can use the `Map` method of the `Mapper` class that you set up earlier. Here's an example of how to use a mapping to convert a `Source` object to a `Destination` object:

```csharp
var source = new Source();
var destination = Mapper.Map<Source, Destination>(source);
```

You can also use the `Map` method to convert a collection of objects. To do this, you can use the `ProjectTo` extension method, like this:

```csharp
var sourceList = new List<Source>();
var destinationList = sourceList.ProjectTo<Destination>().
```

## Real-World Example

Imagine you are building an e-commerce website and you want to display a list of products to the user. You have a database that stores all the product information, including the product name, price, and description. However, the database model and the view model for the product list are slightly different. The database model has a `Product` class with properties like `Name`, `Price`, and `Description`, while the view model has a `ProductViewModel` class with properties like `Title`, `Price`, and `Description`.

One way to handle this would be to manually map the properties from the `Product` class to the `ProductViewModel` class every time you need to display a list of products. However, this can be time-consuming and error-prone, especially if you have a large number of properties or a complex object hierarchy.

This is where AutoMapper comes in. With AutoMapper, you can set up a mapping between the `Product` and `ProductViewModel` classes and then use it to easily convert a list of `Product` objects into a list of `ProductViewModel` objects.

Here's an example of how you might set up the mapping using AutoMapper:

```csharp
config.CreateMap<Product, ProductViewModel>()
    .ForMember(dest => dest.Title, opt => opt.MapFrom(src => src.Name))
    .ForMember(dest => dest.Description, opt => opt.MapFrom(src => src.Description))
    .ForMember(dest => dest.Price, opt => opt.MapFrom(src => src.Price
```

Then, to convert a list of `Product` objects into a list of `ProductViewModel` objects, you can use the `ProjectTo` extension method like this:

```csharp
var productList = _productRepository.GetAll();
var productViewModelList = productList.ProjectTo<ProductViewModel>();
```

This will create a new list of `ProductViewModel` objects that have the same data as the original `Product` objects, but with the properties mapped according to the mapping configuration.

With AutoMapper, you can easily and efficiently convert between different object models, saving you time and effort and reducing the risk of errors.

## Mapping using Profiles

In some cases, you may want to group your mappings into logical units called "profiles". This can be useful if you have a large number of mappings and want to organize them in a more structured way.

To create a profile, you can create a new class that derives from the `Profile` class and override the `Configure` method. Inside the `Configure` method, you can add your mappings using the same `CreateMap` and `ForMember` methods that you would use in the regular mapping configuration.

Here's an example of how you might create a profile for the product mapping example from the previous section:

```csharp
public class ProductProfile : Profile
{
    public ProductProfile()
    {
        CreateMap<Product, ProductViewModel>()
            .ForMember(dest => dest.Title, opt => opt.MapFrom(src => src.Name))
            .ForMember(dest => dest.Description, opt => opt.MapFrom(src => src.Description))
            .ForMember(dest => dest.Price, opt => opt.MapFrom(src => src.Price));
    }
}
```

To use the profile, you'll need to add it to your mapping configuration. You can do this by passing it to the `AddProfile` method of the `MapperConfiguration` object, like this:

```csharp
var config = new MapperConfiguration(cfg =>
{
    cfg.AddProfile<ProductProfile>();
});
```

With this configuration, AutoMapper will use the mappings defined in the `ProductProfile` class when performing object conversions.

Using profiles can help you organize your mappings and make it easier to maintain your mapping configuration as your project grows. You can create as many profiles as you need, and you can even create profiles that depend on other profiles to further modularize your mapping configuration.

## Configure AutoMapper from DI in [ASP.net](http://asp.net) Core

To use AutoMapper with dependency injection (DI) in an [ASP.NET](http://ASP.NET) Core project, you'll need to follow a few steps.

1.  Install the AutoMapper NuGet package:
    

```csharp
Install-Package AutoMapper
```

2.  Create your mapping profiles:
    

```csharp
public class ProductProfile : Profile
{
    public ProductProfile()
    {
        CreateMap<Product, ProductViewModel>()
            .ForMember(dest => dest.Title, opt => opt.MapFrom(src => src.Name))
            .ForMember(dest => dest.Description, opt => opt.MapFrom(src => src.Description))
            .ForMember(dest => dest.Price, opt => opt.MapFrom(src => src.Price));
    }
}
```

3.  Add the AutoMapper service to the `ConfigureServices` method in the `Startup` class:
    

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Other service configuration goes here

    services.AddAutoMapper(typeof(Startup));
}
```

4.  Inject the `IMapper` interface into your controllers or services where you need to use AutoMapper:
    

```csharp
public class ProductController : Controller
{
    private readonly IMapper _mapper;

    public ProductController(IMapper mapper)
    {
        _mapper = mapper;
    }

    // Controller actions go here
}
```

With these steps, you'll be able to use AutoMapper with DI in your [ASP.NET](http://ASP.NET) Core project. You can then use the `IMapper` interface to perform object conversions, just like in the non-DI example.

```csharp
var productList = _productRepository.GetAll();
var productViewModelList = _mapper.Map<List<ProductViewModel>>(productList);
```

Using AutoMapper with DI can make it easier to manage your mapping configuration and make it more flexible and testable.

## Explain of `services.AddAutoMapper(typeof(Startup))`

In the `ConfigureServices` method of the `Startup` class in an [ASP.NET](http://ASP.NET) Core project, the `AddAutoMapper` method is used to register the AutoMapper service with the DI container. This allows you to inject the `IMapper` interface into your controllers or services and use it to perform object conversions using AutoMapper.

The `AddAutoMapper` method takes an optional parameter that specifies the assembly where the mapping profiles are located. In the example you provided, `typeof(Startup)` is passed as the parameter, which means that AutoMapper will search for mapping profiles in the same assembly as the `Startup` class.

By default, AutoMapper will look for classes that derive from the `Profile` class and have a default constructor. It will then use these classes to configure the mappings for the `IMapper` interface.

You can also pass an array of `Type` objects to the `AddAutoMapper` method to specify which types should be used as mapping profiles. This can be useful if you have multiple assemblies with mapping profiles and you want to specify which ones should be used.

For example:

```csharp
services.AddAutoMapper(typeof(ProductProfile), typeof(OrderProfile));
```

This will register the AutoMapper service with the DI container and configure it to use the `ProductProfile` and `OrderProfile` classes as mapping profiles.

Using the `AddAutoMapper` method in the `ConfigureServices` method is a convenient way to set up AutoMapper with DI in an [ASP.NET](http://ASP.NET) Core project. It allows you to easily inject the `IMapper` interface into your controllers or services and use it to perform object conversions with AutoMapper.