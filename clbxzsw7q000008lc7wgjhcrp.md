# How to create a simple logger in C#

A logger is a utility class that is used to write log messages to a specific destination, such as a file, the console, or a database. Loggers are useful for debugging, monitoring, and troubleshooting applications.

In C#, there are several ways to create a logger. One common approach is to use a logging library, such as `log4net` or `NLog`, which provides a set of APIs for logging messages with different levels of severity (e.g., debug, info, warning, error).

In this article, we will demonstrate how to create a simple logger using the `System.IO.File` class and the `System.Text.StringBuilder` class.

## **Step 1: Create a Logger Class**

First, let's create a `Logger` class that will contain the logic for writing log messages to a file. We will define the following methods:

*   `Log`: This method will take a log message and a log level as arguments, and write the message to the file with a timestamp and the log level.
    
*   `Debug`: This method will write a debug-level log message to the file.
    
*   `Info`: This method will write an info-level log message to the file.
    
*   `Warning`: This method will write a warning-level log message to the file.
    
*   `Error`: This method will write an error-level log message to the file.
    

Here is the code for the `Logger` class:

```csharp
public class Logger
{
    private readonly string _filePath;

    public Logger(string filePath)
    {
        _filePath = filePath;
    }

    public void Log(string message, string level)
    {
        var logMessage = $"[{DateTime.Now}] [{level}] {message}";

        using (var writer = File.AppendText(_filePath))
        {
            writer.WriteLine(logMessage);
        }
    }

    public void Debug(string message)
    {
        Log(message, "DEBUG");
    }

    public void Info(string message)
    {
        Log(message, "INFO");
    }

    public void Warning(string message)
    {
        Log(message, "WARNING");
    }

    public void Error(string message)
    {
        Log(message, "ERROR");
    }
}
```

The `Log` method takes a log message and a log level as arguments, and writes the message to the file with a timestamp and the log level using the `File.AppendText` method. The `Debug`, `Info`, `Warning`, and `Error` methods simply call the `Log` method with the appropriate log level.

## **Step 2: Use the Logger**

Now that we have created the `Logger` class, we can use it to write log messages to a file.

Here is an example of how to use the logger:

```csharp
var logger = new Logger("log.txt");

logger.Debug("This is a debug message");
logger.Info("This is an info message");
logger.Warning("This is a warning message");
logger.Error("This is an error message");
```

This code will create a new `Logger` instance with a file path of "log.txt", and write four log messages to the file with the appropriate log levels.

## **Step 3: Customize the Logger (continued)**

*   Adding log levels: You can add additional log levels, such as `Trace`, `Fatal`, and `Off`, by creating new methods or by using an enumeration.
    
*   Adding log destinations: You can add support for writing log messages to other destinations, such as the console or a database, by using additional classes or libraries.
    
*   Adding filtering: You can add filtering to only log messages that meet certain criteria, such as log level or message content.
    
*   Adding exception handling: You can add exception handling to the `Log` method to handle any errors that may occur when writing log messages to the file.
    

## **Logger with Multiple Targets**

In C#, you can use an interface to create a logger that supports multiple targets and allows you to specify the target to log to. This can be useful if you want to have more flexibility in choosing the destination for your log messages.

Here is an example of how to create a logger with multiple targets using an interface and the `System.IO.File` class and the `System.Console` class:

```csharp
public interface ILogger
{
    void Log(string message, string level);
}

public class FileLogger : ILogger
{
    private readonly string _filePath;

    public FileLogger(string filePath)
    {
        _filePath = filePath;
    }

    public void Log(string message, string level)
    {
        var logMessage = $"[{DateTime.Now}] [{level}] {message}";

        using (var writer = File.AppendText(_filePath))
        {
            writer.WriteLine(logMessage);
        }
    }
}

public class ConsoleLogger : ILogger
{
    public void Log(string message, string level)
    {
        var logMessage = $"[{DateTime.Now}] [{level}] {message}";
        Console.WriteLine(logMessage);
    }
}

public class Logger
{
    private readonly ILogger _logger;

    public Logger(ILogger logger)
    {
        _logger = logger;
    }

    public void Debug(string message)
    {
        _logger.Log(message, "DEBUG");
    }

    public void Info(string message)
    {
        _logger.Log(message, "INFO");
    }

    public void Warning(string message)
    {
        _logger.Log(message, "WARNING");
    }

    public void Error(string message)
    {
        _logger.Log(message, "ERROR");
    }
}
```

In this code, we define an `ILogger` interface with a `Log` method that takes a log message and a log level as arguments. We then create two classes, `FileLogger` and `ConsoleLogger`, that implement the `ILogger` interface and define their own `Log` method for writing log messages to a file and the console, respectively.

Finally, we create a `Logger` class that has a property of type `ILogger` and delegates the task of writing log messages to the `Log` method of the contained logger. The `Debug`, `Info`, `Warning`, and `Error` methods of the `Logger` class simply call the `Log` method of the contained logger with the appropriate log level.

To use the logger, you can create a new instance of either the `FileLogger` or the `ConsoleLogger` class, and pass it to the `Logger` constructor:

```csharp
// Log to file
var fileLogger = new FileLogger("log.txt");
var logger = new Logger(fileLogger);

logger.Debug("This is a debug message");
logger.Info("This is an info message");
logger.Warning("This is a warning message");
logger.Error("This is an error message");
```

```csharp
// Log to console
var consoleLogger = new ConsoleLogger();
var logger = new Logger(consoleLogger);

logger.Debug("This is a debug message");
logger.Info("This is an info message");
logger.Warning("This is a warning message");
logger.Error("This is an error message");
```

This code will create a new `Logger` instance with either a `FileLogger` or a `ConsoleLogger`, and write four log messages to the file or the console with the appropriate log levels.

By using an interface and separating the logger implementation from the target, you can easily switch between different log destinations and have more control over where your log messages are written.

You can also add additional targets by creating additional classes that implement the `ILogger` interface and adding them to the `Logger` constructor. For example, you can create a `DatabaseLogger` class that writes log messages to a database table using the `System.Data.SqlClient` namespace.

```csharp
public class DatabaseLogger : ILogger
{
    private readonly SqlConnection _connection;

    public DatabaseLogger(SqlConnection connection)
    {
        _connection = connection;
    }

    public void Log(string message, string level)
    {
        using (var command = _connection.CreateCommand())
        {
            command.CommandText = "INSERT INTO Logs (Message, Level) VALUES (@message, @level)";
            command.Parameters.AddWithValue("@message", message);
            command.Parameters.AddWithValue("@level", level);
            command.ExecuteNonQuery();
        }
    }
}
```

To use the `DatabaseLogger`, you can create a new instance with a database connection and pass it to the `Logger` constructor:

```csharp
var connection = new SqlConnection("connection string");
connection.Open();

var databaseLogger = new DatabaseLogger(connection);
var logger = new Logger(databaseLogger);

logger.Debug("This is a debug message");
logger.Info("This is an info message");
logger.Warning("This is a warning message");
logger.Error("This is an error message");

connection.Close();
```

By using an interface and creating separate classes for each log target, you can create a flexible and extensible logger that can support multiple targets and adapt to changing needs.

## **Conclusion**

In this article, we demonstrated how to create a simple logger in C# using the `System.IO.File` class and the `System.Text.StringBuilder` class. We also discussed some ways to customize the logger to fit your needs.

Loggers are a useful tool for debugging, monitoring, and troubleshooting applications, and can help you identify and resolve issues more efficiently. By creating your own logger, you can tailor it to your specific requirements and needs.