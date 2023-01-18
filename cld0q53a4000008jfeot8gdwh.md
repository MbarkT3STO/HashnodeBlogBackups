# Introduction to Serilog

One of the key features of [ASP.NET](http://ASP.NET) Core is its built-in support for logging, which allows developers to easily track and diagnose issues within their applications. One of the most popular logging frameworks for [ASP.NET](http://ASP.NET) Core is Serilog.

## **What is Serilog?**

Serilog is a popular open-source logging framework for .NET applications. It is designed to be simple and flexible, making it easy to integrate with a wide variety of applications and platforms. Serilog provides a number of powerful features, including support for structured logging, custom formatting, and automatic log filtering.

## **Why Use Serilog in** [**ASP.NET**](http://ASP.NET) **Core?**

There are a number of reasons why developers might choose to use Serilog in their [ASP.NET](http://ASP.NET) Core applications. Some of the key benefits of using Serilog include:

* Structured logging: Serilog allows developers to log data in a structured format, which makes it easy to search, filter, and analyze log data. This can be particularly useful when trying to diagnose issues in a large and complex application.
    
* Custom formatting: Serilog allows developers to customize the format of their log messages, making it easy to include additional information such as timestamps, log levels, and custom properties.
    
* Automatic log filtering: Serilog provides built-in support for filtering log messages based on various criteria, such as log level or message template. This can help to reduce the amount of noise in the log data, making it easier to find the information you need.
    

# **Getting Started with Serilog in** [**ASP.NET**](http://ASP.NET) **Core**

To get started with Serilog in an [ASP.NET](http://ASP.NET) Core application, you will need to install the Serilog package and configure it to work with your application. Here's a basic example of how to do this:

1. Install the following Serilog packages
    

* `Serilog.AspNetCore`: This package provides the necessary integration between Serilog and [ASP.NET](http://ASP.NET) Core. It enables you to use Serilog as your logging provider and provides support for middleware and configuration.
    
* `Serilog.Sinks.Console`: This package enables you to write log messages to the console. It can be useful for local development and debugging.
    

```csharp
Install-Package Serilog.AspNetCore
Install-Package Serilog.Sinks.Console
```

1. In your `Startup.cs` file, add the following code to the `Configure` method:
    

```csharp
Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Debug()
    .WriteTo.Console()
    .CreateLogger();
```

1. In the `Configure` method, add the following line to the `app.UseSerilog()` method:
    

```csharp
app.UseSerilog();
```

1. In any class that you want to log data from, add the following line at the top of the class:
    

```csharp
private static readonly ILogger Log = Log.Logger.ForContext<MyClass>();
```

1. In any method that you want to log data from, add the following line:
    

```csharp
Log.Information("This is a log message");
```

## Simpler Method to Create a Logger Instance

There is a simpler method to create a logger instance in your class. Instead of using the `Log.ForContext<MyClass>()` method, you can use the `Log.Logger` property to create a logger instance.

```csharp
private static readonly ILogger Log = Log.Logger;
```

This will create a logger instance with the default settings. This method is useful when you don't need to specify additional context or when you want to use the same logger for all of your classes.

Another way is to use the `Log.ForContext<T>()` method with the `this` keyword instead of the class name

```csharp
private static readonly ILogger Log = Log.ForContext<this>();
```

This will create a logger instance with the context of the current class and it's more helpful when you have several classes and you want to track the origin of the log.

## Log Without Creating an Instance

You can directly log a message without using an object by using the `Log` class's static methods such as `Information`, `Error`, `Debug`, `Warning` etc.. For example:

```csharp
Log.Information("This is an information message");
Log.Error("This is an error message");
Log.Debug("This is a debug message");
Log.Warning("This is a warning message");
```

These methods accept a string as the message and an optional set of properties.

It's important to note that in order to use these methods, you should have already created a logger instance as described in the previous lines.

You could also use the `Log.Write` method which takes the log level as the first parameter, and the message as the second parameter.

```csharp
Log.Write(LogEventLevel.Information, "This is a log message");
Log.Write(LogEventLevel.Error, "This is an error message");
```

This method is useful when you need to specify a log level that is not covered by the other methods.

Please note that you should check that the log level you want to use is enabled in the logger configuration otherwise you will not see any log.

## **Conclusion**

Serilog is a powerful logging framework that makes it easy to track and diagnose issues in your [ASP.NET](http://ASP.NET) Core applications. Its structured logging, custom formatting, and automatic log filtering features make it a great choice for developers looking for a flexible and easy-to-use logging solution. With just a few lines of code, you can get started with Serilog and start logging data from your [ASP.NET](http://ASP.NET) Core application.