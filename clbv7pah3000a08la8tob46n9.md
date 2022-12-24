# How To Implement the Mediator Design Pattern in C#

## What is the Mediator Design Pattern?

The Mediator design pattern is a behavioral design pattern that defines an object that encapsulates how a set of objects interact with each other. It promotes loose coupling by allowing objects to communicate indirectly, through the mediator object, rather than directly with one another. This can help to reduce the complexity of object interactions and promote flexibility in the design of a system.

## Benefits of using the Mediator Design Pattern

There are several benefits to using the Mediator design pattern in your C# code:

1.  It promotes loose coupling: By allowing objects to communicate indirectly through the mediator, the Mediator design pattern promotes loose coupling between objects. This means that the objects are less dependent on each other, which can make it easier to change or modify the system.
    
2.  It reduces complexity: The Mediator design pattern helps to reduce the complexity of object interactions by centralizing the communication logic in a single place. This can make it easier to understand and maintain the codebase.
    
3.  It promotes flexibility: The Mediator design pattern allows you to change the behavior of the system by simply changing the mediator object. This can make it easier to add new functionality or make changes to the system without having to modify the underlying objects.
    

## Implementing the Mediator Design Pattern in C#

To implement the Mediator design pattern in C#, you will need to define a mediator interface that defines the methods for communication between objects. Then, you will need to create a concrete mediator class that implements the mediator interface and provides the logic for communication between objects.

Here is an example of a mediator interface in C#:

```csharp
public interface IMediator
{
    void Send(string message, Colleague colleague);
}
```

To implement the concrete mediator class, you will need to define a constructor that accepts a reference to each of the objects that will communicate through the mediator. You will also need to implement the Send method, which will contain the logic for communication between the objects.

Here is an example of a concrete mediator class in C#:

```csharp
public class ConcreteMediator : IMediator
{
    private Colleague1 colleague1;
    private Colleague2 colleague2;

    public ConcreteMediator(Colleague1 colleague1, Colleague2 colleague2)
    {
        this.colleague1 = colleague1;
        this.colleague2 = colleague2;
    }

    public void Send(string message, Colleague colleague)
    {
        if (colleague == colleague1)
        {
            colleague2.Notify(message);
        }
        else
        {
            colleague1.Notify(message);
        }
    }
}
```

You will also need to define the Colleague classes, which will be the objects that communicate through the mediator. Each Colleague class should have a reference to the mediator object and a method for sending messages to the mediator.

**Here is an example of a Colleague class in C#:**

```csharp
public class Colleague1
{
    private IMediator mediator;

    public Colleague1(IMediator mediator)
    {
        this.mediator = mediator;
    }

    public void Send(string message)
    {
        mediator.Send(message, this);
    }

    public void Notify(string message)
    {
        Console.WriteLine("Colleague1 received message: " + message);
    }
}
```

To use the Mediator design pattern in your code, you will need to create an instance of the `ConcreteMediator` class and pass it to the constructors of each of the Colleague objects. Then, you can use the Send method of the Colleague objects to send messages to the mediator, which will then be forwarded to the appropriate Colleague object.

**Here is an example of using the Mediator design pattern in C#:**

```csharp
IMediator mediator = new ConcreteMediator(new Colleague1(mediator), new Colleague2(mediator));
Colleague1 colleague1 = new Colleague1(mediator);
Colleague2 colleague2 = new Colleague2(mediator);

colleague1.Send("Hello, how are you?");
colleague2.Send("I'm doing well, thanks for asking.");
```

## Real implementation example

This example demonstrates how the Mediator design pattern can be used to build a chat application.

First, we will define the IMediator interface and the ConcreteMediator class:

```csharp
public interface IMediator
{
    void Send(string message, User sender);
}

public class ChatMediator : IMediator
{
    private List<User> users;

    public ChatMediator()
    {
        this.users = new List<User>();
    }

    public void AddUser(User user)
    {
        this.users.Add(user);
    }

    public void Send(string message, User sender)
    {
        foreach (User user in this.users)
        {
            if (user != sender)
            {
                user.Receive(message);
            }
        }
    }
}
```

Next, we will define the User class, which represents a user in the chat application:

```csharp
public class User
{
    private IMediator mediator;
    private string name;

    public User(IMediator mediator, string name)
    {
        this.mediator = mediator;
        this.name = name;
    }

    public string Name
    {
        get { return this.name; }
    }

    public void Send(string message)
    {
        Console.WriteLine("{0} sending message: {1}", this.name, message);
        this.mediator.Send(message, this);
    }

    public void Receive(string message)
    {
        Console.WriteLine("{0} received message: {1}", this.name, message);
    }
}
```

Finally, we can use the ChatMediator and User classes to create a chat application:

```csharp
IMediator mediator = new ChatMediator();

User john = new User(mediator, "John");
User jane = new User(mediator, "Jane");
User bob = new User(mediator, "Bob");

mediator.AddUser(john);
mediator.AddUser(jane);
mediator.AddUser(bob);

john.Send("Hi everyone!");
jane.Send("Hello, John!");
bob.Send("Hey, what's going on?");
```

This will output the following messages:

```json
John sending message: Hi everyone!
Jane received message: Hi everyone!
Bob received message: Hi everyone!
Jane sending message: Hello, John!
John received message: Hello, John!
Bob received message: Hello, John!
Bob sending message: Hey, what's going on?
John received message: Hey, what's going on?
Jane received message: Hey, what's going on?
```

As you can see, the Mediator design pattern allows the users to communicate indirectly through the ChatMediator, rather than directly with each other. This promotes loose coupling and makes it easier to add new users or modify the behavior of the chat application.

## Conclusion

The Mediator design pattern is a useful way to promote loose coupling and reduce complexity in the design of a system by centralizing the communication logic in a single place. By implementing a mediator interface and concrete mediator class, and creating Colleague classes that communicate through the mediator, you can use the Mediator design pattern in your C# code to improve the flexibility and maintainability of your system.