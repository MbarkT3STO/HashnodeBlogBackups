# Building Microservices Architecture with ASP.Net Core

## Microservices Architecture

Microservices architecture is a popular approach to building software systems that are composed of small, independent services that communicate with each other through APIs. This approach allows for greater flexibility, scalability, and maintainability of the system as a whole. In this article, we will discuss how to implement microservices architecture in [ASP.Net](http://ASP.Net) Core with API Gateway (using Ocelot).

## Microservices Architecture as Diagram

![](https://blog.varstreetinc.com/wp-content/uploads/2021/01/Blog_First-Image-what-is-Microservice-Architecture.png align="left")

The diagram shows the following components:

* **API Gateway**: This is the entry point for the client's requests, it acts as a reverse proxy that routes requests to the appropriate service based on the routing rules defined in the configuration file.
    
* **Services**: These are the different services that make up the application, each service is responsible for a specific functionality and communicates with other services through a well-defined API.
    

## **What is an API Gateway?**

An API Gateway is a server that acts as an intermediary between an application and a set of microservices. It is responsible for routing requests to the appropriate service, as well as handling tasks such as authentication, rate limiting, and caching. In a microservices architecture, an API Gateway is essential for managing and maintaining the communication between the different services.

## **Why Use Ocelot for an API Gateway?**

Ocelot is a popular open-source library that can be used to implement an API Gateway in [ASP.Net](http://ASP.Net) Core. It is lightweight, easy to use, and provides a wide range of features that are essential for managing microservices. Some of the key features of Ocelot include:

* Dynamic routing: Ocelot allows you to define routes for different services dynamically, which means that you can easily change the routing configuration without having to rebuild the entire application.
    
* Authentication and Authorization: Ocelot supports various authentication and authorization schemes, including JWT, OAuth, and Basic Auth.
    
* Caching and Rate Limiting: Ocelot allows you to cache responses and limit the number of requests that can be made to a service in a given time period.
    
* Load Balancing: Ocelot supports load balancing between multiple instances of a service, which helps to improve the scalability and availability of the system.
    

## **Setting up Ocelot in** [**ASP.Net**](http://ASP.Net) **Core**

To set up Ocelot in an [ASP.Net](http://ASP.Net) Core application, you first need to install the Ocelot package using the NuGet package manager. Once you have done that, you can add Ocelot to the `Program.cs` file by adding the following code:

```csharp
using Ocelot.DependencyInjection;
using Ocelot.Middleware;

var builder = WebApplication.CreateBuilder(args);

//Add Ocelot json file
builder.Configuration.AddJsonFile("ocelot.json");

// Add Ocelot services
builder.Services.AddOcelot(builder.Configuration);
```

Next, you need to configure the routes for your services. You can do this by creating a `ocelot.json` file in the root of your project and defining the routes for your services. For example, the following configuration will route requests for the `/customers` endpoint to a service called `customer-service`:

```csharp
{
    "Routes": [
        {
            "DownstreamPathTemplate": "/customers",
            "DownstreamHostAndPorts": [
                {
                    "Host": "customer-service",
                    "Port": 80
                }
            ],
            "UpstreamPathTemplate": "/customers"
        }
    ]
}
```

Finally, you need to configure the API Gateway to use the `ocelot.json` file as the configuration source. You can do this by adding the following code to the `Configure` method in the `Program.cs` file:

```csharp
app.UseOcelot().Wait();
```

## **Real-World Example**

Let's consider a scenario where we have a web application for an e-commerce store that allows customers to browse products, add items to their cart, and place orders. In this example, we will have 3 microservices:

* **Product Service**: This service is responsible for managing the products available in the store, including fetching product details, creating new products, and updating existing products.
    
* **Cart Service**: This service is responsible for managing customer cart items, including adding and removing items, and calculating the total cost of the cart.
    
* **Order Service**: This service is responsible for managing customer orders, including creating new orders, updating order status, and fetching order details.
    

Here's a breakdown of the structure of the solution:

```csharp
- ECommerceStore
  - ProductService (ASP.NET Core Web API project)
    - Controllers
      - ProductController.cs
    - Services
      - IProductService.cs
      - ProductService.cs
    - Repositories
      - IProductRepository.cs
      - ProductRepository.cs
  - CartService (ASP.NET Core Web API project)
    - Controllers
      - CartController.cs
    - Services
      - ICartService.cs
      - CartService.cs
    - Repositories
      - ICartRepository.cs
      - CartRepository.cs
  - OrderService (ASP.NET Core Web API project)
    - Controllers
      - OrderController.cs
    - Services
      - IOrderService.cs
      - OrderService.cs
    - Repositories
      - IOrderRepository.cs
      - OrderRepository.cs
  - ECommerceStore.API.Gateway (ASP.NET Core Web API project)
    - Ocelot.json (config file for Ocelot)
```

Each of the above services is an independent [ASP.NET](http://ASP.NET) Core Web API project that has its controllers, services, and repositories folders. The controllers handle the incoming HTTP requests and return the appropriate responses, the services handle the business logic, and the repositories handle the data access.

We will also have an API Gateway project, which is an [ASP.NET](http://ASP.NET) Core Web API project that uses Ocelot to route requests to the appropriate service. The `ocelot.json` file in this project will have the routing configuration for the different services.

For example, the following configuration will route requests for the `/api/products` endpoint to the Product Service:

```csharp
{
    "Routes": [
        {
            "DownstreamPathTemplate": "/api/products",
            "DownstreamHostAndPorts": [
                {
                    "Host": "product-service",
                    "Port": 80
                }
            ],
            "UpstreamPathTemplate": "/api/products"
        }
    ]
}
```

This way, when a customer makes a request to the `/api/products` endpoint, the API Gateway will forward the request to the Product Service, which will then handle the request and return the appropriate response.

## `ocelot.json`

The `ocelot.json` file is a configuration file used by Ocelot to define the routing rules for an API Gateway. This file is typically located in the root of the API Gateway project and contains a JSON object with different configuration options.

The most important configuration option in the `ocelot.json` file is the `Routes` array, which defines the routing rules for different services. Each object in the `Routes` array represents a single routing rule, and it has the following properties:

* **DownstreamPathTemplate**: This property defines the path template of the request that should be routed to the downstream service. For example, if this property is set to `/api/products`, then requests to the `/api/products` endpoint will be routed to the downstream service.
    
* **DownstreamHostAndPorts**: This property defines the host and port of the downstream service. For example, if this property is set to `{ "Host": "product-service", "Port": 80 }`, then requests will be routed to the `product-service` service running on port 80.
    
* **UpstreamPathTemplate**: This property defines the path template of the request that should be used to match the routing rule. For example, if this property is set to `/api/products`, then requests to the `/api/products` endpoint will match this routing rule.
    

In addition to the `Routes` array, the `ocelot.json` file can also contain other configuration options, such as `GlobalConfiguration`, `HttpHandlerOptions`, and `LoadBalancerOptions`. These options can be used to configure various features of Ocelot, such as authentication, rate limiting, and load balancing.

It's worth mentioning that the `ocelot.json` file is just a configuration file, so you don't need to worry about any logic or business rules. In fact, this file should be a static file and you should not use any code to change it during runtime.

## **Downstream and Upstream**

In the context of an API Gateway and microservices architecture, "downstream" and "upstream" refer to the direction of the communication between the different services.

* **Downstream** refers to the service that a request is being sent to. For example, in our e-commerce example, if a customer makes a request to the `/api/products` endpoint, the API Gateway will forward the request to the Product Service, which is considered the downstream service.
    
* **Upstream** refers to the service that a request is coming from. For example, in our e-commerce example, if a customer makes a request to the `/api/products` endpoint, the API Gateway is considered the upstream service.
    

In summary, downstream is the service that handles the request, and upstream is the service that sends the request. The API Gateway is the service that sits in the middle and routes the requests to the appropriate downstream service. These names are not fixed, you can use the name that you prefer, but it's important to understand the concept and the direction of the communication.

## Re-explanation of ocelot.json

The `ocelot.json` file is a configuration file that helps the API Gateway (Ocelot) to understand where to route a specific request. It contains a list of routing rules that the API Gateway uses to direct the requests to the correct service.

Each routing rule has three properties:

* **DownstreamPathTemplate**: This is the path that the client is trying to reach. For example, if the client is trying to reach `/gateway/products`, this property should be set to `/gateway/products`
    
* **DownstreamHostAndPorts**: This is the address of the service that will handle the request. For example, if the service that handles the request is located at [`http://product-service:80`](http://product-service:80), this property should be set to `{ "Host": "product-service", "Port": 80 }`
    
* **UpstreamPathTemplate**: This is the path that the service is listening to. For example, if the service is listening to `/api/products`, this property should be set to `/api/products`
    

So, when a client sends a request to `/gateway/products`, the API Gateway (Ocelot) will look for a routing rule that has `/gateway/products` as a `DownstreamPathTemplate`, it will then forward the request to the service that has the address [`http://product-service:80`](http://product-service:80)`/api/products` and the service will handle the request and sends the response back to the client.

## Run The Application

Once you have implemented the microservices architecture, you will need to run all the projects of the services and the API Gateway in order to test your application.

Here's how you can do it using Visual Studio:

1. Open the solution that contains all the projects of the services and the API Gateway in Visual Studio.
    
2. In the Solution Explorer, right-click on the solution and select the "Set Startup Projects" option.
    
3. In the "Multiple startup projects" section, select the "Start" option for the projects of the services and the API Gateway.
    
4. Press the "Apply" button to save the changes and then press the "OK" button to close the dialog.
    
5. Press the "Start" button or press F5 on your keyboard to run the application.
    
6. Visual Studio will start all the selected projects and run them in separate instances of the development server.
    
7. You can now test your application by making requests to the API Gateway using a tool like Postman or by using your application's front-end.
    

It's worth mentioning that you should make sure that the ports that the services are running on are not in use before running the application, if you are having any issues with that you can change the ports in the configuration file or in the `launchsettings.json` file.

Also, you should make sure that the `ocelot.json` file is correctly configured with the correct addresses of the services and the correct routing rules before running the application.

By following these steps, you should be able to run your microservices architecture with the API Gateway in Visual Studio and test your application.