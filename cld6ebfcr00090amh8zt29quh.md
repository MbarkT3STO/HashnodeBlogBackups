# Getting Started with FakeItEasy in .NET Core Tests

FakeItEasy is a popular library for creating test doubles (mocks, stubs, and fakes) in .NET. It is a powerful tool for creating lightweight, flexible, and easy-to-use test doubles that can be used to isolate the code under test.

## **Installing FakeItEasy**

To use FakeItEasy in your .NET Core project, you will first need to install the FakeItEasy NuGet package. You can do this by running the following command in your project's root directory:

```csharp
dotnet add package FakeItEasy
```

## **Creating a Test Double**

A test double is a stand-in object that can be used in place of a real object during testing. In FakeItEasy, test doubles are created using the `A.Fake<T>()` method, where `T` is the type of the object you want to create a test double for. For example, to create a test double for an interface called `IMyService`, you would use the following code:

```csharp
var myService = A.Fake<IMyService>();
```

## **Configuring the Test Double**

Once you have created a test double, you can configure it to behave in a specific way when its methods are called. For example, you can configure a test double to return a specific value when a certain method is called, or to throw an exception when another method is called.

Here is an example of configuring a test double to return a specific value when the `GetData()` method is called:

```csharp
A.CallTo(() => myService.GetData()).Returns("Hello, World!");
```

## **Using the Test Double**

Once you have created and configured your test double, you can use it in your test just like you would use the real object. For example, you can call methods on the test double, and assert that they return the expected values.

Here is an example of using a test double in a test:

```csharp
[Fact]
public void Test_MyService_GetData()
{
    // Arrange
    var myService = A.Fake<IMyService>();
    A.CallTo(() => myService.GetData()).Returns("Hello, World!");

    // Act
    var result = myService.GetData();

    // Assert
    Assert.Equal("Hello, World!", result);
}
```

## **Conclusion**

FakeItEasy is a powerful library for creating test doubles in .NET. It is easy to use, highly configurable, and provides a wide range of features that make it an excellent choice for any .NET developer.