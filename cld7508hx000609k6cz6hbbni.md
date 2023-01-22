# Argument Constraints with FakeItEasy

One of the features of FakeItEasy is the ability to specify constraints on arguments passed to mocked methods. This can be useful for testing specific scenarios or edge cases in your code.

## **Using Argument Constraints**

To use argument constraints with FakeItEasy, you will first need to create a mock of the class or interface you want to test. Once you have your mock, you can use the `A.CallTo()` method to specify constraints on the arguments passed to a particular method.

For example, let's say we have a class called `Calculator` with a method called `Add` that takes two integers as arguments and returns their sum. We can create a mock of this class and use argument constraints to test that the `Add` method is called with specific arguments.

```csharp
var calculator = A.Fake<Calculator>();

A.CallTo(() => calculator.Add(A<int>.That.IsEqualTo(2), A<int>.That.IsEqualTo(3))).Returns(5);

var result = calculator.Add(2, 3);

Assert.AreEqual(5, result);
```

In this example, we are using the `IsEqualTo` constraint to specify that the `Add` method should be called with the arguments 2 and 3. If the method is called with any other arguments, the constraint will not be met and the test will fail.

## **Available Constraints**

FakeItEasy provides a number of different constraints that can be used to specify the arguments passed to a mocked method. Some of the most commonly used constraints include:

* `IsEqualTo`: checks that the argument is equal to a specific value
    
* `IsSameAs`: checks that the argument is the same object as a specific value
    
* `IsNotNull`: checks that the argument is not null
    
* `IsNull`: checks that the argument is null
    

**For more, you can check the official documentation article about the** [**Argument Constraints**](https://fakeiteasy.github.io/docs/7.3.1/argument-constraints/)

## **Conclusion**

Argument constraints are a powerful feature of FakeItEasy that can be used to test specific scenarios or edge cases in your code. By specifying constraints on the arguments passed to mocked methods, you can ensure that your tests are more precise and less prone to false positives. With a variety of available constraints and the ability to combine them in a single call, developers can test almost any edge cases they might want to.