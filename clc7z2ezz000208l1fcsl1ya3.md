# The Most Common .NET CLI Commands

The .NET Command Line Interface (CLI) is a powerful tool for building and running .NET applications. It allows developers to create, build, test, and publish .NET applications from the command line, making it an essential tool for any .NET developer.

Here are some of the most common .NET CLI commands that you will use in your development workflow:

## `dotnet new`

The `dotnet new` command is used to create a new .NET project or solution. It takes a template parameter, which specifies the type of project to create. For example, to create a new console application, you can use the following command:

```xml
dotnet new console
```

You can also specify a name for the project using the `--name` parameter:

```xml
dotnet new console --name MyConsoleApp
```

## `dotnet build`

The `dotnet build` command is used to build a .NET project or solution. It takes the path to the project or solution file as an argument. For example:

```xml
dotnet build MyProject.csproj
```

You can also specify the configuration (e.g. Debug or Release) using the `--configuration` parameter:

```xml
dotnet build MyProject.csproj --configuration Release
```

## `dotnet run`

The `dotnet run` command is used to run a .NET application. It takes the path to the project or solution file as an argument. For example:

```xml
dotnet run MyProject.csproj
```

You can also specify the configuration (e.g. Debug or Release) using the `--configuration` parameter:

```xml
dotnet run MyProject.csproj --configuration Release
```

## `dotnet test`

The `dotnet test` command is used to run unit tests in a .NET project. It takes the path to the test project as an argument. For example:

```xml
dotnet test MyTestProject.csproj
```

You can also specify the configuration (e.g. Debug or Release) using the `--configuration` parameter:

```xml
dotnet test MyTestProject.csproj --configuration Release
```

## `dotnet publish`

The `dotnet publish` command is used to publish a .NET application for deployment. It takes the path to the project or solution file as an argument. For example:

```xml
dotnet publish MyProject.csproj
```

You can also specify the configuration (e.g. Debug or Release) and the target runtime using the `--configuration` and `--runtime` parameters:

```xml
dotnet publish MyProject.csproj --configuration Release --runtime win-x64
```

## **Conclusion**

These are just a few of the most common .NET CLI commands that you will use in your development workflow. With the .NET CLI, you can easily create, build, test, and publish .NET applications from the command line, making it an essential tool for any .NET developer.