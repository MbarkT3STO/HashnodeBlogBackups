# How to Implement the Command Design Pattern in C#

## Command Design Pattern

The Command design pattern is a behavioral design pattern that encapsulates a request as an object, allowing you to treat requests as first-class objects. This can be useful for implementing undo/redo functionality, for instance, or for implementing deferred execution of operations.

In this article, we will cover how to implement the Command design pattern in C#.

## Implementation Steps

1.  Create a command interface with a single `Execute` method. The `Execute` method should contain the logic for executing the command.
    

```csharp
public interface ICommand
{
    void Execute();
}
```

2.  Create concrete command classes that implement the `ICommand` interface. Each concrete command class should contain the specific logic for a particular request.
    

```csharp
public class ConcreteCommandA : ICommand
{
    public void Execute()
    {
        // Logic for executing command A
    }
}

public class ConcreteCommandB : ICommand
{
    public void Execute()
    {
        // Logic for executing command B
    }
}
```

3.  Create an `Invoker` class that has a method for setting and executing a command.
    

```csharp
public class Invoker
{
    private ICommand _command;

    public void SetCommand(ICommand command)
    {
        _command = command;
    }

    public void ExecuteCommand()
    {
        _command.Execute();
    }
}
```

4.  Create a `client` class that instantiates the `invoker` and **concrete commands**, and sets the commands on the invoker.
    

```csharp
public class Client
{
    public void Main()
    {
        var invoker = new Invoker();
        var commandA = new ConcreteCommandA();
        var commandB = new ConcreteCommandB();

        invoker.SetCommand(commandA);
        invoker.ExecuteCommand();

        invoker.SetCommand(commandB);
        invoker.ExecuteCommand();
    }
}
```

## Conclusion

In this article, we have covered the basic steps for implementing the Command design pattern in C#. By encapsulating requests as objects, the Command pattern allows us to treat requests as first-class objects and to implement deferred execution of operations and undo/redo functionality.