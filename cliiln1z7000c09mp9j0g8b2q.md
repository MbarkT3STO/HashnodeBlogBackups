---
title: "POSA: The Broker Pattern"
datePublished: Mon Jun 05 2023 08:37:46 GMT+0000 (Coordinated Universal Time)
cuid: cliiln1z7000c09mp9j0g8b2q
slug: posa-the-broker-pattern
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685954209518/b31dce64-b513-4635-8879-2b2a1cc50a86.png
tags: design-patterns, csharp, software-development, software-architecture

---

In software engineering, patterns provide reusable solutions to common design problems. One such pattern is the Broker pattern, also known as the Publish-Subscribe pattern or the Event-Driven architecture. The Broker pattern promotes loose coupling and decoupling between components in a system, enabling better scalability and maintainability. In this article, we will explore the Broker pattern in the context of C#, along with examples to demonstrate its usage.

## **What is the Broker Pattern?**

The Broker pattern facilitates communication between different components of a system by introducing a central entity known as the broker. The broker acts as an intermediary, allowing publishers and subscribers to interact without having direct knowledge of each other's existence. This decoupling enables a highly modular and extensible system, as publishers and subscribers can be added or removed without affecting other components.

## **Implementation in C#**

Let's delve into the implementation details of the Broker pattern in C#.

### **Components of the Broker Pattern**

The Broker pattern consists of the following components:

1. **Broker**: The central entity responsible for coordinating the communication between publishers and subscribers. It receives messages from publishers and routes them to the appropriate subscribers.
    
2. **Publisher**: A component that generates messages or events and sends them to the broker. Publishers are unaware of the subscribers and do not have direct knowledge of their existence.
    
3. **Subscriber**: A component that registers with the broker to receive messages or events of interest. Subscribers can subscribe to specific message types or categories.
    

### **Example Scenario**

Let's consider a scenario where we have a messaging system with multiple publishers and subscribers. Publishers can send messages of different types, such as "OrderPlaced," "PaymentCompleted," or "ProductShipped." Subscribers are interested in receiving specific types of messages and performing corresponding actions.

### **Example Implementation**

Here's a simplified implementation of the Broker pattern in C# for our messaging system scenario:

```csharp
// 1. Define the message class
public class Message
{
    public string Type { get; set; }
    public string Content { get; set; }
}

// 2. Implement the broker
public class MessageBroker
{
    private readonly Dictionary<string, List<Action<Message>>> _subscribers;

    public MessageBroker()
    {
        _subscribers = new Dictionary<string, List<Action<Message>>>();
    }

    public void Publish(Message message)
    {
        if (_subscribers.ContainsKey(message.Type))
        {
            foreach (var subscriber in _subscribers[message.Type])
            {
                subscriber.Invoke(message);
            }
        }
    }

    public void Subscribe(string messageType, Action<Message> handler)
    {
        if (!_subscribers.ContainsKey(messageType))
        {
            _subscribers[messageType] = new List<Action<Message>>();
        }

        _subscribers[messageType].Add(handler);
    }
}

// 3. Implement publishers and subscribers
public class OrderPlacedPublisher
{
    private readonly MessageBroker _broker;

    public OrderPlacedPublisher(MessageBroker broker)
    {
        _broker = broker;
    }

    public void PlaceOrder(string orderDetails)
    {
        // Perform order placement logic...

        // Publish the message
        _broker.Publish(new Message { Type = "OrderPlaced", Content = orderDetails });
    }
}

public class ShippingSubscriber
{
    public ShippingSubscriber(MessageBroker broker)
    {
        // Subscribe to the "OrderPlaced" message type
        broker.Subscribe("OrderPlaced", HandleOrderPlaced);
    }

    private void HandleOrderPlaced(Message message)
    {
        // Perform shipping logic...

        Console.WriteLine($"Shipping: Order placed - {message.Content}");
    }
}

// 4. Usage example
var broker = new MessageBroker();
var orderPlacedPublisher = new OrderPlacedPublisher(broker);
var shippingSubscriber = new ShippingSubscriber(broker);

orderPlacedPublisher.PlaceOrder("12345");
```

In this example, we define a `Message` class to encapsulate the message type and content. The `MessageBroker` class acts as the central broker and maintains a dictionary of subscribers for different message types. The `Publish` method in the broker receives messages from publishers and forwards them to the corresponding subscribers. Subscribers can register their interest by calling the `Subscribe` method with the desired message type and a handler function.

We then create an `OrderPlacedPublisher` and a `ShippingSubscriber`. The publisher generates an "OrderPlaced" message and publishes it via the broker. The subscriber registers itself to handle "OrderPlaced" messages and performs the necessary shipping logic upon receiving such messages.

### A detailed explanation of the code

Let's break down the code and provide a detailed explanation for each component:

```csharp
// 1. Define the message class
public class Message
{
    public string Type { get; set; }
    public string Content { get; set; }
}
```

Here, we define a simple `Message` class that represents a message being passed between publishers and subscribers. It has two properties: `Type` to indicate the type of the message, and `Content` to hold the actual content of the message.

```csharp
// 2. Implement the broker
public class MessageBroker
{
    private readonly Dictionary<string, List<Action<Message>>> _subscribers;

    public MessageBroker()
    {
        _subscribers = new Dictionary<string, List<Action<Message>>>();
    }

    public void Publish(Message message)
    {
        if (_subscribers.ContainsKey(message.Type))
        {
            foreach (var subscriber in _subscribers[message.Type])
            {
                subscriber.Invoke(message);
            }
        }
    }

    public void Subscribe(string messageType, Action<Message> handler)
    {
        if (!_subscribers.ContainsKey(messageType))
        {
            _subscribers[messageType] = new List<Action<Message>>();
        }

        _subscribers[messageType].Add(handler);
    }
}
```

The `MessageBroker` class represents the central broker responsible for managing the communication between publishers and subscribers. It maintains a dictionary called `_subscribers`, where the keys are the message types and the values are lists of subscribers (represented as `Action<Message>` delegates).

The `Publish` method receives a `Message` and checks if there are any subscribers registered for that message type. If there are, it iterates through the list of subscribers and invokes their respective handlers, passing the message as an argument.

The `Subscribe` method allows subscribers to register themselves for a specific message type. If there are no subscribers registered for that message type, it creates a new list to store the subscribers. It then adds the provided handler function to the list of subscribers for that message type.

```csharp
// 3. Implement publishers and subscribers
public class OrderPlacedPublisher
{
    private readonly MessageBroker _broker;

    public OrderPlacedPublisher(MessageBroker broker)
    {
        _broker = broker;
    }

    public void PlaceOrder(string orderDetails)
    {
        // Perform order placement logic...

        // Publish the message
        _broker.Publish(new Message { Type = "OrderPlaced", Content = orderDetails });
    }
}

public class ShippingSubscriber
{
    public ShippingSubscriber(MessageBroker broker)
    {
        // Subscribe to the "OrderPlaced" message type
        broker.Subscribe("OrderPlaced", HandleOrderPlaced);
    }

    private void HandleOrderPlaced(Message message)
    {
        // Perform shipping logic...

        Console.WriteLine($"Shipping: Order placed - {message.Content}");
    }
}
```

Next, we define two concrete implementations: `OrderPlacedPublisher` and `ShippingSubscriber`.

The `OrderPlacedPublisher` class represents a publisher that generates an "OrderPlaced" message when the `PlaceOrder` method is called. It takes an instance of the `MessageBroker` as a dependency, which is used to publish the message by calling the `Publish` method.

The `ShippingSubscriber` class represents a subscriber that is interested in receiving "OrderPlaced" messages. In its constructor, it subscribes to the "OrderPlaced" message type by calling the `Subscribe` method of the broker and providing a handler function (`HandleOrderPlaced`). When a message of type "OrderPlaced" is received, the `HandleOrderPlaced` method is invoked, and it performs the shipping logic.

```csharp
// 4. Usage example
var broker = new MessageBroker();
var orderPlacedPublisher = new OrderPlacedPublisher(broker);
var shippingSubscriber = new ShippingSubscriber(broker);

orderPlacedPublisher.PlaceOrder("12345");
```

In the usage example, we create an instance of the `MessageBroker` as the central broker. Then, we create instances of the `OrderPlacedPublisher` and `ShippingSubscriber`, passing the broker instance as a dependency to both.

Finally, we call the `PlaceOrder` method of the `OrderPlacedPublisher` instance, which triggers the placement of an order. This, in turn, generates an "OrderPlaced" message and publishes it via the broker. As a result, the `HandleOrderPlaced` method of the `ShippingSubscriber` is invoked, and it performs the shipping logic, printing a message to the console.

Overall, this code demonstrates the Broker pattern in action. The publisher and subscriber components are decoupled from each other and communicate through the central broker, allowing for flexible and extensible system design.

## **Benefits of the Broker Pattern**

The Broker pattern provides several benefits in software development:

1. **Loose coupling**: Publishers and subscribers are decoupled from each other, as they only interact with the broker. This loose coupling allows for better modularity and extensibility.
    
2. **Scalability**: The broker acts as a centralized point for message distribution, allowing for easy scaling of publishers and subscribers without impacting other components.
    
3. **Flexibility**: New publishers and subscribers can be added or removed independently, facilitating system evolution and maintenance.
    
4. **Event-driven architecture**: The pattern promotes an event-driven architecture, which is suitable for scenarios where components need to react to specific events or messages.
    

## When to use the Broker Pattern

The Broker pattern *(also known as the Publish-Subscribe pattern or the Event-Driven architecture)* is useful in several scenarios. The following are some situations where you might consider using the Broker pattern:

1. **Decoupling components**: If you have multiple components in your system that need to communicate with each other, but you want to minimize direct dependencies between them, the Broker pattern can help. By introducing a central broker, components can interact indirectly, reducing coupling and promoting modularity.
    
2. **Event-driven systems**: When your system is event-driven, meaning that different components react to specific events or messages, the Broker pattern fits well. The broker acts as the event hub, receiving events from publishers and distributing them to interested subscribers.
    
3. **Scalability and extensibility**: If you anticipate the need to scale your system by adding or removing publishers or subscribers dynamically, the Broker pattern provides a flexible solution. New components can be easily integrated into the system without affecting existing ones, allowing for better scalability and extensibility.
    
4. **Asynchronous communication**: When you require asynchronous communication between components, where the sender does not need to wait for a response from the receiver, the Broker pattern can be beneficial. Publishers can send messages to the broker and continue their execution, while subscribers handle the messages independently.
    
5. **Loose coupling and maintenance**: If you aim to achieve loose coupling between components, making it easier to maintain and evolve the system over time, the Broker pattern can be a valuable design choice. Components can be developed and modified independently, as long as they adhere to the message types defined by the broker.
    

It's important to note that while the Broker pattern provides advantages in certain scenarios, it also introduces additional complexity due to the introduction of the central broker. Therefore, it's essential to carefully evaluate your system's requirements and trade-offs before deciding to adopt the Broker pattern.

## **Conclusion**

The Broker pattern, also known as the Publish-Subscribe pattern or the Event-Driven architecture, is a valuable design pattern for promoting loose coupling and modularity in software systems. In this article, we explored the implementation of the Broker pattern in C# and demonstrated its usage through an example scenario. By leveraging the Broker pattern, you can build scalable and maintainable systems that can easily accommodate new components and functionalities.