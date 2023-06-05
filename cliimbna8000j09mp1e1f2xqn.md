---
title: "POSA: The Event-Based Pattern"
datePublished: Mon Jun 05 2023 08:56:54 GMT+0000 (Coordinated Universal Time)
cuid: cliimbna8000j09mp1e1f2xqn
slug: posa-the-event-based-pattern
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685954679983/8a0b10de-01b4-42a7-bd35-e77c3b9e917b.png
tags: design-patterns, csharp, software-development, software-engineering

---

## **Understanding the Event-Based Pattern**

The Event-Based Pattern is based on the concept of events, which are messages or notifications that are emitted by a sender component and received by one or more subscriber components. This pattern promotes loose coupling between components, as the sender does not need to have any knowledge of its subscribers, and subscribers do not need to have any knowledge of other subscribers.

In the Event-Based Pattern, the sender component, also known as the publisher, is responsible for raising events. Subscribers, on the other hand, listen for specific events and respond accordingly when those events occur. This allows for a highly modular and extensible system, where new components can be added as subscribers without impacting existing components.

## **Implementing the Event-Based Pattern in C#**

To implement the Event-Based Pattern in C#, we utilize delegates and events. Delegates are similar to function pointers and define the signature of the event handler methods. Events are used to encapsulate the delegate and provide a mechanism for subscribing and unsubscribing to events.

Let's take a look at an example to better understand the implementation of the Event-Based Pattern in C#.

```csharp
// Define the delegate for the event handler
public delegate void EventHandlerDelegate(string message);

// Define the publisher class
public class Publisher
{
    // Define the event
    public event EventHandlerDelegate EventOccurred;

    // Method to raise the event
    public void RaiseEvent(string message)
    {
        // Check if any subscribers exist
        if (EventOccurred != null)
        {
            // Raise the event
            EventOccurred(message);
        }
    }
}

// Define the subscriber class
public class Subscriber
{
    // Event handler method
    public void HandleEvent(string message)
    {
        Console.WriteLine("Event occurred: " + message);
    }
}

// Usage example
public class Program
{
    static void Main(string[] args)
    {
        Publisher publisher = new Publisher();
        Subscriber subscriber = new Subscriber();

        // Subscribe to the event
        publisher.EventOccurred += subscriber.HandleEvent;

        // Raise the event
        publisher.RaiseEvent("Example event message");
    }
}
```

In the above example, we have a `Publisher` class that defines the event named `EventOccurred`. The `RaiseEvent` method is responsible for raising the event by invoking the event delegate if there are any subscribers. On the other hand, the `Subscriber` class defines the `HandleEvent` method, which serves as the event handler.

In the `Program` class, we create an instance of the `Publisher` and `Subscriber` classes. We then subscribe the `HandleEvent` method of the `Subscriber` instance to the `EventOccurred` event of the `Publisher` instance. Finally, we raise the event using the `RaiseEvent` method of the `Publisher` instance, which triggers the execution of the `HandleEvent` method in the `Subscriber` instance.

## **Advantages of the Event-Based Pattern**

The Event-Based Pattern offers several advantages when used in software development:

1. **Decoupling**: Components are loosely coupled, as the publisher does not need to know about its subscribers, and subscribers do not need to know about each other.
    
2. **Scalability**: New components can be easily added as subscribers without impacting the existing components.
    
3. **Maintainability**: Modifications or updates to the publisher do not require changes in subscriber implementations.
    
4. **Extensibility**: The pattern allows for easy addition of new events and event handlers without affecting existing code.
    
5. **Reusability**: Subscribers can be reused in different contexts, as they are independent of the publisher.
    

## **Real-World Example**

In the previous section, we discussed the Event-Based Pattern and its implementation in C#. To further illustrate its usage, let's explore a real-world example where the Event-Based Pattern can be applied effectively.

### **Scenario: Online Marketplace**

Consider an online marketplace where users can buy and sell various products. In this scenario, the Event-Based Pattern can be utilized to enhance the system's functionality and provide a seamless user experience.

### **Implementation**

To implement the Event-Based Pattern in the online marketplace, we can identify two key components: the ProductListingService (publisher) and the NotificationService (subscriber).

The ProductListingService is responsible for managing product listings, including creating new listings and updating existing ones. Whenever a new product listing is created or an existing listing is updated, the ProductListingService raises an event to notify interested parties, such as the NotificationService.

The NotificationService, as a subscriber, listens for these events and triggers appropriate actions, such as sending notifications to interested users. For example, when a new product listing is created, the NotificationService can send a notification to subscribed users who have expressed interest in similar products or categories.

### **Code Example**

Let's take a look at a simplified code example to demonstrate the Event-Based Pattern implementation in the online marketplace scenario:

```csharp
// Define the delegate for the event handler
public delegate void ProductListingEventHandler(ProductListing listing);

// Define the product listing class
public class ProductListing
{
    public string Title { get; set; }
    public string Description { get; set; }
    // Other properties...

    // Define the event
    public static event ProductListingEventHandler ListingCreated;

    // Method to create a new listing
    public static void CreateListing(ProductListing listing)
    {
        // Perform listing creation logic...

        // Raise the event
        ListingCreated?.Invoke(listing);
    }

    // Method to update an existing listing
    public void UpdateListing()
    {
        // Perform listing update logic...

        // Raise the event
        ListingCreated?.Invoke(this);
    }
}

// Define the notification service class
public class NotificationService
{
    // Event handler method
    public void HandleNewListing(ProductListing listing)
    {
        // Send notification to interested users about the new listing
        Console.WriteLine("New product listing: " + listing.Title);
    }
}

// Usage example
public class Program
{
    static void Main(string[] args)
    {
        // Create instances of the ProductListingService and NotificationService
        ProductListingService.ListingCreated += new NotificationService().HandleNewListing;

        // Create a new product listing
        ProductListing.CreateListing(new ProductListing
        {
            Title = "Example Product",
            Description = "This is an example product."
        });

        // Update an existing product listing
        ProductListing existingListing = new ProductListing
        {
            Title = "Existing Product",
            Description = "This is an existing product."
        };
        existingListing.UpdateListing();
    }
}
```

In this example, the `ProductListing` class represents a product listing in the online marketplace. It defines the `ListingCreated` event of type `ProductListingEventHandler`. The `CreateListing` method is responsible for creating a new product listing and raising the event, while the `UpdateListing` method updates an existing listing and raises the event.

The `NotificationService` class represents a service that handles notifications to interested users. Its `HandleNewListing` method serves as the event handler, which is triggered when a new product listing event occurs. In this example, it simply prints a notification message to the console.

In the `Program` class, we subscribe an instance of `NotificationService` to the `ListingCreated` event of the `ProductListingService`. Then, we create a new product listing using the `CreateListing` method, and update an existing listing using the `UpdateListing` method. These actions raise the `ListingCreated` event, which, in turn, triggers the `HandleNewListing` method of the `NotificationService`, resulting in a notification being sent.