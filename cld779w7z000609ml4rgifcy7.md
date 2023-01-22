# Assertion with FakeItEasy

Assertion is an important part of testing. It allows you to check that your code is behaving as expected. In this article, we'll look at how to use assertion with the FakeItEasy library.

## **Basic Assertions**

FakeItEasy provides several basic assertion methods that you can use to check the behavior of your fake objects. The most commonly used method is `MustHaveHappened()`, which asserts that a method or property was accessed.

```csharp
var fakeObject = A.Fake<IMyInterface>();
fakeObject.MyMethod();
A.CallTo(() => fakeObject.MyMethod()).MustHaveHappened();
```

In this example, we're creating a fake object that implements the `IMyInterface` interface and calling the `MyMethod` method. We're then asserting that the method was called by using the `MustHaveHappened()` method.

Another basic assertion method is `MustNotHaveHappened()`, which asserts that a method or property was not accessed.

```csharp
var fakeObject = A.Fake<IMyInterface>();
A.CallTo(() => fakeObject.MyMethod()).MustNotHaveHappened();
```

In this example, we're creating a fake object that implements the `IMyInterface` interface and asserting that the `MyMethod` method was not called by using the `MustNotHaveHappened()` method.

## **Advanced Assertions**

In addition to the basic assertion methods, FakeItEasy also provides several advanced assertion methods. One of these is `MustHaveHappenedOnceExactly()`, which asserts that a method or property was accessed exactly one time.

```csharp
var fakeObject = A.Fake<IMyInterface>();
fakeObject.MyMethod();
fakeObject.MyMethod();
A.CallTo(() => fakeObject.MyMethod()).MustHaveHappenedOnceExactly();
```

In this example, we're creating a fake object that implements the `IMyInterface` interface and calling the `MyMethod` method twice. We're then asserting that the method was called exactly one time by using the `MustHaveHappenedOnceExactly()` method.

Another advanced assertion method is `MustHaveHappened(n,Times.Exactly)`, which asserts that a method or property was accessed a specific number of times.

```csharp
var fakeObject = A.Fake<IMyInterface>();
fakeObject.MyMethod();
fakeObject.MyMethod();
A.CallTo(() => fakeObject.MyMethod()).MustHaveHappened(1,Times.Exactly);
```

In this example, we're creating a fake object that implements the `IMyInterface` interface and calling the `MyMethod` method twice. We're then asserting that the method was called exactly two times by using the `MustHaveHappened(1,Times.Exactly)` method.

## **Asserting Arguments**

FakeItEasy also provides several assertion methods that you can use to check the arguments passed to a method. The most commonly used method is `WithAnyArguments`, which asserts that a method was called with any arguments.

```csharp
var fakeObject = A.Fake<IMyInterface>();
fakeObject.MyMethod("arg1", "arg2");
A.CallTo(() => fakeObject.MyMethod(A<string>.Ignored, A<string>.Ignored)).WithAnyArguments().MustHaveHappened();
```

In this example, we're creating a fake object that implements the `IMyInterface` interface and calling the `MyMethod` method with two string arguments. We're then asserting that the method was called with any arguments by using the `WithAnyArguments()` method.