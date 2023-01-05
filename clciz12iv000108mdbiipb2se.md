# Observer Design Pattern

The observer design pattern is a behavioral design pattern that allows an object (called the subject) to notify a set of objects (called observers) when its state changes. This pattern is useful in scenarios where an object needs to be aware of changes in another object and react to those changes.

## **How does it work?**

The observer design pattern consists of three main components:

1. The subject: This is the object whose state is being observed. It maintains a list of its observers and provides an interface for attaching and detaching observers.
    
2. The observer: This is the object that is interested in the state of the subject. It implements an interface that allows it to receive updates from the subject.
    
3. The client: This is the object that creates the subject and observer objects and establishes the relationship between them.
    

When the state of the subject changes, it sends a notification to all of its observers. The observers can then use this notification to update their own state or perform some other action.

## **Example**

Here is an example of the observer design pattern in C#:

```csharp
public interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify();
}

public interface IObserver
{
    void Update(ISubject subject);
}

public class Subject : ISubject
{
    private List<IObserver> _observers;
    private int _state;

    public Subject()
    {
        _observers = new List<IObserver>();
    }

    public void Attach(IObserver observer)
    {
        _observers.Add(observer);
    }

    public void Detach(IObserver observer)
    {
        _observers.Remove(observer);
    }

    public void Notify()
    {
        foreach (var observer in _observers)
        {
            observer.Update(this);
        }
    }

    public int State
    {
        get { return _state; }
        set
        {
            _state = value;
            Notify();
        }
    }
}

public class Observer : IObserver
{
    private Subject _subject;
    private int _observerState;

    public Observer(Subject subject)
    {
        _subject = subject;
        _subject.Attach(this);
    }

    public void Update(ISubject subject)
    {
        if (subject == _subject)
        {
            _observerState = _subject.State;
        }
    }
}
```

To use the observer design pattern in this example, the client would create a `Subject` object and one or more `Observer` objects. The client would then set the state of the subject, which would trigger a notification to be sent to all of the observers. The observers would update their own state in response to this notification.

## Real-World Examples

The following are a few examples of the observer design pattern in the real world:

* A weather app that displays the current temperature and weather conditions. The app could be designed using the observer design pattern, with the weather service as the subject and the app as the observer. When the weather service receives updated data, it sends a notification to the app, which updates the displayed temperature and weather conditions.
    
* A stock ticker that displays the current price of a particular stock. The stock exchange could be the subject and the ticker could be the observer. When the stock exchange receives updated prices, it sends a notification to the ticker, which updates the displayed price.
    
* A social media platform that sends notifications to users when they receive a new message or someone replies to one of their posts. The platform could use the observer design pattern, with the user's profile as the subject and the notification system as the observer. When the user's profile receives a new message or reply, it sends a notification to the notification system, which sends a notification to the user.
    

### Weather App

The weather service is the subject in this example. It maintains a list of its observers (in this case, the weather app) and provides an interface for attaching and detaching observers. When the weather service receives updated data, it sends a notification to the weather app.

The weather app is the observer in this example. It implements an interface that allows it to receive updates from the weather service. When it receives a notification from the weather service, it updates the displayed temperature and weather conditions.

The client in this example is the code that creates the weather service and weather app objects and establishes the relationship between them. It sets up the weather app as an observer of the weather service and starts the app.

When the weather service receives updated data, it sends a notification to the weather app, which updates the displayed temperature and weather conditions for the user. The observer design pattern allows the weather app to be notified of changes in the weather service without being tightly coupled to it, making it easier to change and extend the system.

Here is an example of the weather app using the observer design pattern in C#:

```csharp
public interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify();
}

public interface IObserver
{
    void Update(ISubject subject);
}

public class WeatherService : ISubject
{
    private List<IObserver> _observers;
    private WeatherData _weatherData;

    public WeatherService()
    {
        _observers = new List<IObserver>();
    }

    public void Attach(IObserver observer)
    {
        _observers.Add(observer);
    }

    public void Detach(IObserver observer)
    {
        _observers.Remove(observer);
    }

    public void Notify()
    {
        foreach (var observer in _observers)
        {
            observer.Update(this);
        }
    }

    public WeatherData WeatherData
    {
        get { return _weatherData; }
        set
        {
            _weatherData = value;
            Notify();
        }
    }
}

public class WeatherApp : IObserver
{
    private WeatherService _weatherService;
    private WeatherData _displayedWeatherData;

    public WeatherApp(WeatherService weatherService)
    {
        _weatherService = weatherService;
        _weatherService.Attach(this);
    }

    public void Update(ISubject subject)
    {
        if (subject == _weatherService)
        {
            _displayedWeatherData = _weatherService.WeatherData;
            UpdateDisplay();
        }
    }

    private void UpdateDisplay()
    {
        // code to update the displayed temperature and weather conditions
    }
}

public class WeatherData
{
    public double Temperature { get; set; }
    public string Conditions { get; set; }
}
```

To use the observer design pattern in this example, the client would create a `WeatherService` object and a `WeatherApp` object. The client would then set the `WeatherData` property of the weather service, which would trigger a notification to be sent to the weather app. The weather app would update its displayed weather data in response to this notification.

Here is an example of how the client might use these classes:

```csharp
WeatherService weatherService = new WeatherService();
WeatherApp weatherApp = new WeatherApp(weatherService);

WeatherData weatherData = new WeatherData
{
    Temperature = 72.5,
    Conditions = "Sunny"
};

weatherService.WeatherData = weatherData;
```

### Stock Ticker

The example of the stock ticker using the observer design pattern works in a similar way to the weather app example. The stock exchange is the subject and the stock ticker is the observer. When the stock exchange receives updated stock prices, it sends a notification to the stock ticker, which updates the displayed price.

Here is an example of the stock ticker using the observer design pattern in C#:

```csharp
public interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify();
}

public interface IObserver
{
    void Update(ISubject subject);
}

public class StockExchange : ISubject
{
    private List<IObserver> _observers;
    private StockData _stockData;

    public StockExchange()
    {
        _observers = new List<IObserver>();
    }

    public void Attach(IObserver observer)
    {
        _observers.Add(observer);
    }

    public void Detach(IObserver observer)
    {
        _observers.Remove(observer);
    }

    public void Notify()
    {
        foreach (var observer in _observers)
        {
            observer.Update(this);
        }
    }

    public StockData StockData
    {
        get { return _stockData; }
        set
        {
            _stockData = value;
            Notify();
        }
    }
}

public class StockTicker : IObserver
{
    private StockExchange _stockExchange;
    private StockData _displayedStockData;

    public StockTicker(StockExchange stockExchange)
    {
        _stockExchange = stockExchange;
        _stockExchange.Attach(this);
    }

    public void Update(ISubject subject)
    {
        if (subject == _stockExchange)
        {
            _displayedStockData = _stockExchange.StockData;
            UpdateDisplay();
        }
    }

    private void UpdateDisplay()
    {
        // code to update the displayed stock price
    }
}

public class StockData
{
    public string StockSymbol { get; set; }
    public double Price { get; set; }
}
```

To use the observer design pattern in this example, the client would create a `StockExchange` object and a `StockTicker` object. The client would then set the `StockData` property of the stock exchange, which would trigger a notification to be sent to the stock ticker. The stock ticker would update its displayed stock data in response to this notification.

Here is an example of how the client might use these classes:

```csharp
StockExchange stockExchange = new StockExchange();
StockTicker stockTicker = new StockTicker(stockExchange);

StockData stockData = new StockData
{
    StockSymbol = "GOOG",
    Price = 1417.32
};

stockExchange.StockData = stockData;
```

## **Advantages**

* Loose coupling: The observer design pattern allows objects to communicate with each other without requiring a tight coupling between them. This makes it easier to change the implementation of one object without affecting the other objects that depend on it.
    
* Extensibility: The observer design pattern makes it easy to add new observers to a system without requiring any changes to the subject or other observers. This can be useful when you need to extend the functionality of a system but do not want to make changes to the existing code.
    
    * Reusability: The observer design pattern allows you to reuse the subject and observer classes in other systems. This can be useful when you have similar requirements in different parts of a system or in different systems altogether.  
        

* ## **Disadvantages**
    
    * Performance: Notifying all of the observers when the state of the subject changes can have a negative impact on the performance of a system, especially if there are many observers. This can be mitigated by using techniques such as lazy evaluation or using weak references to avoid keeping unnecessary references to the observers.
        
    * Complexity: The observer design pattern can add complexity to a system, especially if there are many observers and/or subjects. This can make it harder to understand and maintain the code.
        
    
    ## **Conclusion**
    
    The observer design pattern is a useful tool for creating flexible and extensible systems. It allows objects to communicate with each other without requiring a tight coupling between them, making it easier to change and extend the system. However, it is important to consider the potential performance and complexity issues when using this pattern.