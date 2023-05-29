---
title: "ASP.Net Core: FromForm, FromBody, and FromQuery"
datePublished: Mon May 29 2023 11:07:52 GMT+0000 (Coordinated Universal Time)
cuid: cli8qx3tm000r09jn4rm7g0hm
slug: aspnet-core-fromform-frombody-and-fromquery
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685358327815/dfd5fb19-bede-4519-86af-faa0965fd22e.webp
tags: aspnet-core

---

When working with APIs, it's common to receive data from different sources such as form submissions, request bodies, or query parameters. In [ASP.Net](http://ASP.Net) Core, you can use the `FromForm`, `FromBody`, and `FromQuery` attributes to bind incoming data to action method parameters easily. In this article, we'll explore these attributes and provide examples to illustrate their usage.

## **FromForm**

The `FromForm` attribute is used to bind data from form submissions to action method parameters. It tells the [ASP.Net](http://ASP.Net) Core framework to look for data in the request's form collection and bind it to the corresponding parameters. Let's see an example:

```csharp
[HttpPost]
public IActionResult SubmitForm([FromForm] MyFormModel model)
{
    // Process the submitted form data
    // ...
    return View();
}
```

In this example, the `SubmitForm` action method is decorated with the `HttpPost` attribute, indicating that it handles POST requests. The `FromForm` attribute is applied to the `MyFormModel` parameter, indicating that the model should be populated with data from the form submission.

## **FromBody**

The `FromBody` attribute is used to bind data from the request body to action method parameters. It allows you to receive data in various formats such as JSON, XML, or plain text. Here's an example:

```csharp
[HttpPost]
public IActionResult ProcessData([FromBody] MyDataModel model)
{
    // Process the received data
    // ...
    return Ok();
}
```

In this example, the `ProcessData` action method is also decorated with the `HttpPost` attribute to handle POST requests. The `FromBody` attribute is applied to the `MyDataModel` parameter, indicating that the model should be populated with data from the request body.

## **FromQuery**

The `FromQuery` attribute is used to bind data from query parameters to action method parameters. It allows you to extract values directly from the URL's query string. Consider the following example:

```csharp
[HttpGet]
public IActionResult Search([FromQuery] string searchTerm)
{
    // Perform a search based on the provided query parameter
    // ...
    return View();
}
```

In this example, the `Search` action method is decorated with the `HttpGet` attribute to handle GET requests. The `FromQuery` attribute is applied to the `searchTerm` parameter, indicating that the value should be extracted from the query string.

## **Combining Attributes**

You can also combine these attributes to handle scenarios where you expect data from multiple sources. For instance, you might have an action method that accepts data from both the request body and query parameters. Here's an example:

```csharp
[HttpPost]
public IActionResult ProcessData([FromBody] MyDataModel model, [FromQuery] string searchTerm)
{
    // Process the received data and perform a search
    // ...
    return Ok();
}
```

In this example, the `ProcessData` action method is decorated with both `FromBody` and `FromQuery` attributes. The `MyDataModel` parameter is bound to the request body, while the `searchTerm` parameter is extracted from the query string.

## **Conclusion**

The `FromForm`, `FromBody`, and `FromQuery` attributes in [ASP.Net](http://ASP.Net) Core provide convenient ways to bind incoming data to action method parameters. By utilizing these attributes, you can easily handle form submissions, request bodies, and query parameters in your API endpoints. Remember to use the appropriate attribute based on the data source you expect and combine them as needed for more complex scenarios.