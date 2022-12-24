# How to use IDisposable in ASP.NET Core

Dispose and Finalize are two common ways to release resources held by  .NET and .NET Core applications running in the context of the CLR. Most importantly, if your application has unmanaged resources, you must explicitly release the resources occupied by such resources.

Due to the non-deterministic nature of finalization, and because finalizers are performance intensive, the Dispose method is used much more often than finalizers. Additionally, he can use the Dispose method on types that implement the IDisposable interface.

In this article, we'll explore the different ways an object that implements the IDisposable interface can be disposed in ASP.NET Core 6.

## Create an ASP.NET Core Web API project in Visual Studio 2022

First, let's create an ASP.NET Core project in Visual Studio 2022. Following these steps will create a new ASP.NET Core Web API 6 project in Visual Studio 2022.

1. Launch the Visual Studio 2022 IDE.
2. Click on “Create new project.”
3. In the “Create new project” window, select “ASP.NET Core Web API” from the list of templates displayed.
4. Click Next.
5. In the “Configure your new project” window, specify the name and location for the new project.
6. Optionally check the “Place solution and project in the same directory” check box, depending on your preferences.
7. Click Next.
8. In the “Additional Information” window shown next, select .NET 6.0 (Preview) as the target framework from the drop-down list at the top. Leave the “Authentication Type” as “None” (default). Ensure that the option “Use controllers …” is checked.
9. Ensure that the check boxes “Enable Docker,” “Configure for HTTPS,” and “Enable Open API Support” are unchecked as we won’t be using any of those features here.
10. Click Create.
11. This will create a new ASP.NET Core 6 Web API project in Visual Studio 2022. We’ll use this project to work with objects that implement the IDisposable interface in the subsequent sections of this article.

## Create a class that implements the IDisposable interface

We’ll now create a class that implements the IDisposable interface as shown in the code snippet given below.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666019255634/FhAr-I4qn.png align="left")

```cs
public class FileManager: IDisposable {
      FileStream fileStream = new FileStream(@"C:\Test.txt",
      FileMode.Append);
      public async Task Write(string text) {
            byte[] buffer = Encoding.Unicode.GetBytes(text);
            int offset = 0;
            try {
                  await fileStream.WriteAsync(buffer, offset,
                  buffer.Length);
            }
            catch {
                  //Write code here to handle exceptions.
            }
      }
      public void Dispose() {
            if (fileStream != null) {
                  fileStream.Dispose();
            }
      }
}
```

The FileManager class implements the IDisposable interface and contains two methods – Write and Dispose. While the former is used to write text to a file asynchronously, the latter is used to remove the FileStream instance from memory by calling the Dispose method of the FileStream class.

## Disposing IDisposable objects in ASP.NET Core 6

In this section we’ll examine various ways in which we can dispose IDisposable objects in ASP.NET Core 6.

Dispose IDisposable objects using the ‘using’ statement

The simplest way to dispose an IDisposable instance is by using the “using” statement, which calls the Dispose method on the instance automatically. The following code snippet illustrates this.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666019316509/XOUGMASFO.png align="left")

```cs
using(FileManager fileManager = new FileManager())
{
      await fileManager.Write("This is a text");
}
```

## Dispose IDisposable objects at the end of a request

When working in ASP.NET Core or ASP.NET Core MVC applications, you might often need to dispose objects at the end of an HTTP request. The HttpResponse.RegisterForDispose method can be used to register IDisposable objects for disposal in this way. It accepts an instance of a class that implements the IDisposable interface and makes sure that the IDisposable object passed to it as a parameter is disposed automatically with each request.

The following code snippet illustrates how you can use the HttpResponse.RegisterForDispose method to register an instance of the FileManager class at the end of each HTTP request.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666019355850/6jr9Dl6Yd.png align="left")

```cs
public class DefaultController: ControllerBase {
      readonly IDisposable _disposable;
      public DefaultController() {
            _disposable = new FileManager();
      }
}
```

## Dispose IDisposable objects using the built-in IoC container

Another approach to dispose IDisposable objects automatically is by using the built-in IoC (inversion of control) container in ASP.NET Core. You can take advantage of either Transient, Scoped, or Singleton instances to created services and add them to the built-in IoC container.

Add IDisposable objects to the IoC container in the ConfigureServices method of the Startup class so that those objects are disposed automatically with each HTTP request.

## Dispose IDependency objects using IHostApplicationLifetime events

ASP.NET Core has an interface called IHostApplicationLifetime that allows you to run custom code when the application is started or shut down. You can take advantage of the Register method of this interface to register to events.

The Configure method of the Startup class can accept the following parameters:

- IApplicationBuilder
- IHostingEnvironment
- ILoggerFactory
- IHostApplicationLifetime

The following code snippet shows how you can use the IHostApplicationLifetime interface to register objects for disposal when the application shuts down.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666019429798/qP8X0NKZo.png align="left")

```cs
public void Configure(IApplicationBuilder app, IHostApplicationLifetime hostApplicationLifetime) {
      hostApplicationLifetime.ApplicationStopping.Register(OnShutdown);
}
private void OnShutdown() {
      //Write your code here to dispose objects
}
```

Finally, note that Startup.cs is not created by default in ASP.NET Core 6. You’ll need to create one manually and then write the following code in the Program.cs file to specify the Startup class you’ll be using in the application.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666019453537/CzcYk2JLf.png align="left")

```cs
var builder = WebApplication.CreateBuilder(args);
builder.Host.ConfigureWebHostDefaults(webBuilder =>
{
    webBuilder.UseStartup<Startup>();
});
using var app = builder.Build();
app.Run();
```

Unlike with Finalize, we use the Dispose method explicitly to free unmanaged resources. You should call the Dispose method explicitly on any object that implements it to free any unmanaged resources for which the object may be holding references.

*This article is fully based on InfoWorld's *[article](https://www.infoworld.com/article/3637150/how-to-use-idisposable-in-aspnet-core.html)