# Noob Introduction to Writing Tests in .NET Core

One of the best features of .NET Core is its built-in support for unit testing. In this article, we will explore how to write tests in .NET Core using the built-in testing framework and the popular xUnit testing library.

## **Setting up a Test Project**

To get started with writing tests in .NET Core, we first need to create a test project. This can be done using the `dotnet new` command.

```csharp
dotnet new xunit -n MyTestProject
```

This command creates a new xUnit test project called "MyTestProject". You can also create a test project using Visual Studio by selecting "xUnit Test Project" from the "New Project" template.

## **Defining the Calculator Class**

Here's an example of how the `Calculator` class might be defined:

```csharp
public class Calculator
{
    public int Add(int a, int b) => a + b;

    public int Subtract(int a, int b) => a - b;

    public int Multiply(int a, int b) => a * b;

    public decimal Divide(int a, int b) => a / b;
}
```

This is a very basic implementation, and in a real-world scenario, the class could have many other methods and properties.

## **Writing Tests**

Once the test project is set up, you can start writing tests. Tests in .NET Core are written using test methods, which are defined using the `[Fact]` attribute. Here's an example of a test method that tests a simple method that adds two numbers:

```csharp
using Xunit;

public class CalculatorTests
{
    [Fact]
    public void Add_TwoNumbers_ReturnsSum()
    {
        // Arrange
        var calculator = new Calculator();
        var x = 1;
        var y = 2;

        // Act
        var result = calculator.Add(x, y);

        // Assert
        Assert.Equal(3, result);
    }
}
```

In this example, the `Add_TwoNumbers_ReturnsSum` test method tests the `Add` method of the `Calculator` class. The test method is divided into three sections: the arrange section, the act section, and the assert section.

In the arrange section, we create an instance of the `Calculator` class and set up the test input. In the act section, we call the `Add` method and store the result in a variable. In the assert section, we use the `Assert.Equal` method to check that the result is equal to the expected value.

## **Running Tests**

Once you have written your tests, you can run them using the `dotnet test` command. This command runs all the tests in the project and displays the results in the console.

```csharp
dotnet test
```

You can also run tests using Visual Studio by right-clicking on the test project and selecting "Run Tests".

## **Test Organization**

As your test suite grows, it becomes important to organize your tests in a logical and maintainable way. The xUnit testing library provides a few attributes to help with this:

* `[Theory]`: This attribute is used to define a test that is parameterized, meaning that it can be run with multiple sets of input. For example, if you have a `Divide` method in your calculator class, you can test it with different inputs and expected results.
    

```csharp
[Theory]
[InlineData(1,2,0.5)]
[InlineData(2,4,0.5)]
[InlineData(4,4,1)]
public void Divide_TwoNumbers_ReturnsQuotient(int x, int y, double expected)
{
    //Arrange
    var calculator = new Calculator();
    //Act
    var result = calculator.Divide(x, y);
    //Assert
    Assert.Equal(expected, result);
}
```

## Note

It is a common practice to separate the test project from the original project in software development. This is known as a separation of concerns, where the test project is focused solely on testing the functionality of the original project, while the original project is focused on implementing the functionality.

One of the main benefits of separating the test project from the original project is that it allows for easier maintenance and scalability. By keeping the test project separate, you can make changes to the original project without affecting the test project and vice versa. This makes it easier to update and maintain both the original project and the test project independently.

Another benefit of separating the test project is that it allows for better organization of the code. The test project can be structured in a way that is optimized for testing, while the original project can be structured in a way that is optimized for functionality. This improves the overall readability and maintainability of the code.

Additionally, separating the test project from the original project also allows for better collaboration among team members. Developers can work on the original project while testers work on the test project, without interfering with each other's work.