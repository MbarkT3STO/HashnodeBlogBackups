# Running Multiple ASP.NET Core Projects in VS Code at Once

When working on multiple projects at the same time (for example let's say that you [Building Microservices Architecture with ASP.Net Core](https://mbarkt3sto.hashnode.dev/building-microservices-architecture-with-aspnet-core)), it can be tedious to run each project individually. In this article, we will show you how to use the compounds feature in VS Code to run multiple [ASP.NET](http://ASP.NET) Core projects at the same time.

## **Setting up the launch.json file**

The first step is to set up the `launch.json` file, which is used to configure the debug settings for your projects. In this file, you will define each project that you want to run and specify the settings for that project.

Here is an example of a `launch.json` file that configures three different projects:

```json
 "version": "0.2.0",
  "configurations": [
    {
      "name": "Customer Service",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "build",
      "program": "${workspaceFolder}/Customer.Service/bin/Debug/net7.0/Customer.Service.dll",
      "args": [],
      "cwd": "${workspaceFolder}/Customer.Service",
      "stopAtEntry": false,
      "serverReadyAction": {
        "action": "openExternally",
        "pattern": "\\bNow listening on:\\s+(https?://\\S+)"
      },
      "env": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "sourceFileMap": {
        "/Views": "${workspaceFolder}/Views"
      }
    },
    {
      "name": "Product Service",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "build",
      "program": "${workspaceFolder}/Product.Service/bin/Debug/net7.0/Product.Service.dll",
      "args": [],
      "cwd": "${workspaceFolder}/Product.Service",
      "stopAtEntry": false,
      "serverReadyAction": {
        "action": "openExternally",
        "pattern": "\\bNow listening on:\\s+(https?://\\S+)"
      },
      "env": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "sourceFileMap": {
        "/Views": "${workspaceFolder}/Views"
      }
    },
    {
      "name": "Gateway API",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "build",
      "program": "${workspaceFolder}/Gateway.API/bin/Debug/net7.0/Gateway.API.dll",
      "args": [],
      "cwd": "${workspaceFolder}/Gateway.API",
      "stopAtEntry": false,
      "serverReadyAction": {
        "action": "openExternally",
        "pattern": "\\bNow listening on:\\s+(https?://\\S+)"
      },
      "env": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "sourceFileMap": {
        "/Views": "${workspaceFolder}/Views"
      }
    }
  ],
  "compounds": [
    {
      "name": "Run All",
      "configurations": ["Customer Service", "Product Service", "Gateway API"]
    }
  ]
}
```

In this example, three different projects are defined: "Customer Service", "Product Service", and "Gateway API". For each project, the `type`, `request`, `preLaunchTask`, `program`, `args`, `cwd`, `stopAtEntry`, `serverReadyAction`, `env` and `sourceFileMap` properties are set.

## Using the compounds feature

Once you have defined your projects in the `launch.json` file, you can use the compounds feature to run all of the projects at once. To do this, you need to add a `compounds` property to the `launch.json` file, and define a new compound that includes all of the projects that you want to run.

In the example above, we have created a compound called "Run All" and included all three projects in the `configurations` property.

To run all of the projects, you can simply select the "Run All" compound from the debug drop-down menu in VS Code. This will start all of the projects at once, and you can easily switch between them as needed.