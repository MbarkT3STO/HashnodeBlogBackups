# The Most Common CLI Commands in Angular

One of the key tools for developing Angular applications is the Angular CLI (Command Line Interface), which provides a set of commands for generating, testing, and deploying Angular code. In this article, we will take a look at some of the most common Angular CLI commands and provide examples of how to use them.

## **ng new**

The `ng new` command is used to create a new Angular project. It generates the basic project structure, installs the required dependencies, and sets up the configuration files.

To create a new Angular project, open a terminal and navigate to the directory where you want to create the project. Then, run the `ng new` command followed by the name of the project:

```typescript
ng new my-project
```

## **ng serve**

The `ng serve` command builds and serves the Angular application in development mode. It compiles the code, starts a development server, and opens the application in a browser.

To start the development server, navigate to the root directory of the project and run the `ng serve` command:

```typescript
ng serve
```

By default, the development server runs on port 4200. You can specify a different port by using the `--port` option:

```typescript
ng serve --port 8080
```

## **ng generate**

The `ng generate` command, also known as `ng g`, is used to generate various types of files and code snippets in an Angular project. It has a variety of subcommands for generating different types of files, such as components, services, pipes, and modules.

For example, to generate a new component, you can use the `component` subcommand:

```typescript
ng generate component my-component
```

This will create a new component with the name `my-component` in the `src/app` directory.  
Here are some common `ng g` (`ng generate`) subcommands that you can use to generate different types of files and code snippets in an Angular project:

* `component`: generates a new component
    
* `service`: generates a new service
    
* `pipe`: generates a new pipe
    
* `module`: generates a new module
    
* `class`: generates a new class
    
* `guard`: generates a new route guard
    
* `interface`: generates a new interface
    
* `enum`: generates a new enum
    
* `directive`: generates a new directive
    

### **ng g component**

The `component` subcommand generates a new component.

```typescript
ng g component my-component
```

This will create a new component in the `src/app` directory with the following files:

* `my-component.component.ts`: the component class
    
* `my-component.component.html`: the template file
    
* `my-component.component.css`: the stylesheet file
    
* `my-component.component.spec.ts`: the unit test file
    

### **ng g service**

The `service` subcommand generates a new service.

```typescript
ng g service my-service
```

This will create a new service in the `src/app` directory with the following file:

* `my-service.service.ts`: the service class
    

### **ng g pipe**

The `pipe` subcommand generates a new pipe.

```typescript
ng g pipe my-pipe
```

This will create a new pipe in the `src/app` directory with the following files:

* `my-pipe.pipe.ts`: the pipe class
    
* `my-pipe.pipe.spec.ts`: the unit test file
    

### **ng g module**

The `module` subcommand generates a new module.

```bash
ng g module my-module
```

This will create a new module in the `src/app` directory with the following files:

* `my-module.module.ts`: the module class
    

### **ng g class**

The `class` subcommand generates a new class.

```bash
ng g class my-class
```

This will create a new class in the `src/app` directory with the following file:

* `my-class.ts`: the class file
    

### **ng g guard**

The `guard` subcommand generates a new route guard.

```typescript
ng g guard my-guard
```

This will create a new route guard in the `src/app` directory with the following files:

* `my-guard.guard.ts`: the route guard class
    
* `my-guard.guard.spec.ts`: the unit test file
    

### **ng g interface**

The `interface` subcommand generates a new interface.

```bash
ng g interface my-interface
```

This will create a new interface in the `src/app` directory with the following file:

* `my-interface.ts`: the interface file
    

### **ng g enum**

The `enum` subcommand generates a new enum.

```bash
ng g enum my-enum
```

This will create a new enum in the `src/app` directory with the following file:

* `my-enum.ts`: the enum file
    

### **ng g directive**

The `directive` subcommand generates a new directive.

```typescript
ng g directive my-directive
```

This will create a new directive in the `src/app` directory with the following files:

* `my-directive.directive.ts`: the directive class
    
* `my-directive.directive.spec.ts`: the unit test file
    

## **ng test**

The `ng test` command runs the unit tests for an Angular project. It compiles the code, runs the tests, and displays the test results in the terminal.

To run the tests, navigate to the root directory of the project and run the `ng test` command:

```typescript
ng test
```

## **ng build**

The `ng build` command is used to build the Angular application for production. It compiles the code, minifies the assets, and generates the production-ready files in the `dist` directory.

To build the application for production, navigate to the root directory of the project and run the `ng build` command:

```typescript
ng build
```

You can also specify the build target and environment by using the `--target` and `--environment` options:

```typescript
ng build --target=production --environment=prod
```

## **ng lint**

The `ng lint` command is used to lint the code in an Angular project. It checks the code for style and syntax errors, and displays a list of any issues found.

To lint the code, navigate to the root directory of the project and run the `ng lint` command:

```typescript
ng lint
```

You can specify the files or directories to lint by using the `--files` option:

```typescript
ng lint --files src/app/**/*.ts
```