---
title: "ASP.NET Core: ContentDispositionHeaderValue"
datePublished: Wed May 24 2023 15:24:23 GMT+0000 (Coordinated Universal Time)
cuid: cli1uvqgj000409ilfr0x48p0
slug: aspnet-core-contentdispositionheadervalue
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1684941567516/c44c2a01-1ec0-4a3d-9c7f-7f92833a7d99.webp
tags: aspnet-core

---

When it comes to handling file downloads and managing their disposition in [ASP.NET](http://ASP.NET) Core, the `ContentDispositionHeaderValue` class plays a vital role. This powerful class allows you to control how files are delivered to the client and how they should be treated by the browser. In this blog post, we'll explore the various features and usage scenarios of `ContentDispositionHeaderValue` in [ASP.NET](http://ASP.NET) Core, accompanied by practical examples.

## **What is** `ContentDispositionHeaderValue`**?**

`ContentDispositionHeaderValue` is a class provided by the [`System.Net`](http://System.Net)`.Http.Headers` namespace in [ASP.NET](http://ASP.NET) Core. It represents the value of the Content-Disposition header in an HTTP response. This header is typically used when serving files to specify whether the browser should display the file directly in the browser window or prompt the user to download it.

## **Working with** `ContentDispositionHeaderValue`

To work with `ContentDispositionHeaderValue`, you need to instantiate an object of this class and set its properties accordingly. The key properties of `ContentDispositionHeaderValue` include `DispositionType`, `FileName`, `FileNameStar`, `CreationDate`, `ModificationDate`, and `Size`.

Let's take a look at some common scenarios and how we can use `ContentDispositionHeaderValue` to achieve the desired behavior.

### **Scenario 1: Prompting a File Download**

Suppose we have an action in our [ASP.NET](http://ASP.NET) Core controller that returns a file to the client for download. We want to ensure that the file is downloaded by the user rather than displayed in the browser. Here's how we can achieve this using `ContentDispositionHeaderValue`:

```csharp
public IActionResult DownloadFile()
{
    // Get the file stream
    Stream fileStream = GetFileStream();

    // Set the response headers
    var contentDispositionHeader = new ContentDispositionHeaderValue("attachment")
    {
        FileName = "example.txt"
    };
    Response.Headers.Add("Content-Disposition", contentDispositionHeader.ToString());

    // Return the file stream as a FileResult
    return File(fileStream, "application/octet-stream");
}
```

In this example, we create a new instance of `ContentDispositionHeaderValue` and set the `DispositionType` property to "attachment" to indicate that the file should be downloaded. We also specify the `FileName` property to provide the name of the file to be saved by the user.

### **Scenario 2: Displaying a File in the Browser**

Sometimes, instead of prompting a download, we may want to display a file directly in the browser window, such as an image or a PDF document. In such cases, we can use `ContentDispositionHeaderValue` to specify the disposition type as "inline." Let's see an example:

```csharp
public IActionResult DisplayFile()
{
    // Get the file stream
    Stream fileStream = GetFileStream();

    // Set the response headers
    var contentDispositionHeader = new ContentDispositionHeaderValue("inline")
    {
        FileName = "image.jpg"
    };
    Response.Headers.Add("Content-Disposition", contentDispositionHeader.ToString());

    // Return the file stream as a FileResult
    return File(fileStream, "image/jpeg");
}
```

In this case, we set the `DispositionType` property of `ContentDispositionHeaderValue` to "inline" to indicate that the file should be displayed in the browser. Again, we provide the `FileName` property with the desired filename for the file.

### **Scenario 3: Providing Additional Information**

The `ContentDispositionHeaderValue` class also allows us to include additional information such as the creation date, modification date, and file size. This can be useful for displaying metadata alongside the file. Here's an example:

```csharp
public IActionResult DownloadWithMetadata()
{
    // Get the file stream
    Stream fileStream = GetFileStream();

    // Set the response headers
    var contentDispositionHeader = new ContentDispositionHeaderValue("attachment")
    {
        FileName = "example.txt",
        CreationDate = DateTime.UtcNow,
        ModificationDate = DateTime.UtcNow,
        Size = fileStream.Length
    };
    Response.Headers.Add("Content-Disposition", contentDispositionHeader.ToString());

    // Return the file stream as a FileResult
    return File(fileStream, "application/octet-stream");
}
```

In this example, we set the `CreationDate`, `ModificationDate`, and `Size` properties of `ContentDispositionHeaderValue` to provide additional information about the file being downloaded.

### The `GetFileStream()` method

The `GetFileStream()` method mentioned in the examples is a placeholder for a method that retrieves the file stream to be returned in the [ASP.NET](http://ASP.NET) Core action. You would need to implement this method according to your specific requirements and logic.

Here's an example of how the `GetFileStream()` method could be implemented to retrieve a file stream from a local file:

```csharp
private Stream GetFileStream()
{
    // Assuming you have the file path or some identifier to locate the file
    string filePath = "path/to/file/example.txt";

    // Open the file stream
    Stream fileStream = new FileStream(filePath, FileMode.Open);

    // You may want to perform additional operations or validations here

    return fileStream;
}
```

In this example, the `GetFileStream()` method takes a file path as an input parameter or any other identifier that helps locate the desired file. It then opens the file using the `FileStream` class with the specified file path and [`FileMode.Open`](http://FileMode.Open) to create a read-only stream.

You can customize this method based on your application's requirements. It could involve retrieving a file from a database, generating a file dynamically, or accessing files from a remote storage system. Adapt the implementation of `GetFileStream()` to suit your specific needs and integrate it into your [ASP.NET](http://ASP.NET) Core controller or service.

## **Conclusion**

The `ContentDispositionHeaderValue` class in [ASP.NET](http://ASP.NET) Core provides a convenient way to control how files are delivered to clients. Whether you need to prompt a file download, display a file in the browser, or include metadata, `ContentDispositionHeaderValue` offers the flexibility to achieve your desired behavior. By leveraging its properties and setting them appropriately, you can enhance the file delivery experience for your [ASP.NET](http://ASP.NET) Core applications.