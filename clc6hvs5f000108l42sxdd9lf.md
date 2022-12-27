# Best Practices and Tips for Good Routing in ASP.Net Core

Routing is a key component of any [ASP.Net](http://ASP.Net) Core application, as it determines how users access the various pages, resources, and functionality of your application. Proper routing can improve the user experience, make your application easier to maintain, and enhance the overall performance of your application. In this article, we will discuss some best practices and tips for good routing in [ASP.Net](http://ASP.Net) Core.

## **Use Attribute Routing**

One of the best practices for routing in [ASP.Net](http://ASP.Net) Core is to use attribute routing. Attribute routing allows you to specify the routes directly in the action methods of your controllers, rather than in a separate route configuration file. This can make your routing more concise and easier to read, as the routes are defined in the same place as the code they are associated with.

To use attribute routing, you simply decorate your action methods with the `[Route]` attribute, and specify the route template as the parameter. For example:

```csharp
[Route("/products")]
public IActionResult Index()
{
    // code to display a list of products
}

[Route("/products/{id}")]
public IActionResult Details(int id)
{
    // code to display the details of a specific product
}
```

## **Use Conventional Routing for Standard CRUD Operations**

Conventional routing is another useful feature in [ASP.Net](http://ASP.Net) Core that allows you to define routes based on the naming conventions of your controllers and action methods. This can be a useful way to define routes for standard CRUD (create, read, update, delete) operations, as it allows you to use intuitive and predictable URL patterns.

For example, the following controller and action methods would automatically be mapped to the following routes:

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Index()
    {
        // code to display a list of products
    }

    [HttpGet]
    public IActionResult Details(int id)
    {
        // code to display the details of a specific product
    }

    [HttpGet]
    public IActionResult Create()
    {
        // code to display a form for creating a new product
    }

    [HttpPost]
    public IActionResult Create(Product product)
    {
        // code to create a new product
    }

    [HttpGet]
    public IActionResult Edit(int id)
    {
        // code to display a form for editing a product
    }

    [HttpPost]
    public IActionResult Edit(int id, Product product)
    {
        // code to update a product
    }

    [HttpDelete]
    public IActionResult Delete(int id)
    {
        // code to delete a product
    }
}
```

```csharp
/products
/products/details/{id}
/products/create
/products/edit/{id}
/products/delete/{id}
```

## **Use URL Parameters and Query Strings Effectively**

URL parameters and query strings can be useful tools for passing data between pages and resources in your application. It is important to use these tools effectively in order to maintain clean and readable URLs, and to ensure that your application is able to handle the data it receives in a reliable and secure manner.

Here are a few tips for using URL parameters and query strings effectively:

* Use URL parameters for required data that is used to uniquely identify a specific resource or page. For example, you might use a URL parameter to specify the ID of a product that you want to display the details of.
    
* Use query strings for optional data that is used to filter or sort resources, or to specify options or preferences. For example, you might use a query string to specify a search term, or to specify the sort order of a list of products.
    
* Use descriptive and meaningful parameter and query string names, as these will be visible in the URL and can help users understand what the URL represents.
    
* Validate and sanitize all input data to ensure that your application is only receiving expected and safe data. This is particularly important for URL parameters, as they are part of the URL and can be easily manipulated by users.
    

## **Use Route Constraints and Defaults**

Route constraints and defaults are features of [ASP.Net](http://ASP.Net) Core routing that allow you to specify rules and default values for the parameters in your routes. This can be useful for ensuring that your application only receives valid and expected data, and for providing a consistent and user-friendly experience.

To use route constraints, you can specify regular expressions or custom constraint classes as the parameters of the `[Route]` attribute. For example:

```csharp
[Route("/products/{id:int}")]
public IActionResult Details(int id)
{
    // code to display the details of a specific product
}
```

This route constraint specifies that the `id` parameter must be an integer. If a user tries to access this route with a non-integer value for `id`, they will receive a 404 error.

To use route defaults, you can specify default values for the parameters in the `[Route]` attribute. For example:

```csharp
[Route("/products/{id:int?}", Name = "product_details", DefaultValues = new { id = 1 })]
public IActionResult Details(int id)
{
    // code to display the details of a specific product
}
```

This route default specifies that the `id` parameter has a default value of `1`, which means that if a user accesses this route without specifying an `id` value, they will be shown the details of product with ID `1`.

## **Use Named Routes and Link Generation**

Named routes and link generation are useful features in [ASP.Net](http://ASP.Net) Core that allow you to reference routes by name, rather than by URL template. This can make your code more maintainable and easier to read, as you don't have to hardcode URLs into your application.

To use named routes, you can specify a name for the route using the `Name` parameter of the `[Route]` attribute. For example:

```csharp
[Route("/products/{id:int}", Name = "product_details")]
public IActionResult Details(int id)
{
    // code to display the details of a specific product
}
```

To generate a link to a named route, you can use the `Url.RouteUrl` method in your views and controllers. For example:

```csharp
<a href="@Url.RouteUrl("product_details", new { id = 1 })">View Product</a>
```

This will generate a link to the `/products/1` URL.

## Use HTTP Redirects and Status Codes Appropriately

HTTP redirects and status codes are important tools for managing the flow of traffic within your application and communicating with users. It is important to use these tools appropriately in order to provide a good user experience and to ensure that your application is functioning properly.

Here are a few tips for using HTTP redirects and status codes appropriately:

* Use a permanent redirect (HTTP status code 301) when you have permanently moved a resource to a new URL. This will tell search engines and other clients that the old URL should no longer be used, and will help preserve your SEO rankings.
    
* Use a temporary redirect (HTTP status code 302) when you need to redirect a user to a different URL for a short period of time. This is often used in situations where a resource is temporarily unavailable, or where a user needs to be redirected to a login page before they can access a protected resource.
    
* Use an HTTP status code of 404 (Not Found) when a user tries to access a resource that does not exist. This will help users understand that they have requested a resource that cannot be found, and will also help search engines understand the structure of your application.
    
* Use an HTTP status code of 403 (Forbidden) when a user tries to access a protected resource that they do not have permission to access. This will help users understand that they do not have the necessary permissions to access the resource, and will also help protect the security of your application.
    

## **Conclusion**

In this article, we have discussed some best practices and tips for good routing in [ASP.Net](http://ASP.Net) Core. By following these best practices, you can create a well-organized and user-friendly application that is easy to maintain and performs well.