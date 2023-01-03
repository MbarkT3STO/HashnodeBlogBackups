# How to Create a Simple Cache System in C#?

In computer science, a cache is a high-speed data storage layer which stores a subset of data, temporarily, from a slower data store or a remote server. It allows faster access to data, as it is stored in the cache memory which is faster to access than the main memory or a remote server.

Caching can significantly improve the performance of a system by reducing the number of expensive data access operations. It is commonly used in web development, operating systems, and databases to speed up the access to data.

In this article, we will see how to create a simple cache system in C#.

## **Implementation**

There are different ways to implement a cache in C#. In this section, we will use a `Dictionary` object to store the data in the cache.

First, let's define the `Cache` class:

```csharp
public class Cache
{
    private readonly Dictionary<string, object> _cacheData;

    public Cache()
    {
        _cacheData = new Dictionary<string, object>();
    }
}
```

The `Cache` class has a private field `_cacheData` of type `Dictionary<string, object>` which will store the data in the cache. The key of the dictionary will be the identifier of the data, and the value will be the actual data.

We can add a method to the `Cache` class to add data to the cache:

```csharp
public void Add(string key, object data)
{
    if (_cacheData.ContainsKey(key))
    {
        _cacheData[key] = data;
    }
    else
    {
        _cacheData.Add(key, data);
    }
}
```

The `Add` method takes a `key` and the actual `data` as arguments. It checks if the `key` already exists in the cache. If it does, it updates the value associated with the `key`, otherwise it adds a new entry to the cache.

We can also add a method to retrieve data from the cache:

```csharp
public object Get(string key)
{
    if (_cacheData.ContainsKey(key))
    {
        return _cacheData[key];
    }

    return null;
}
```

The `Get` method takes a `key` as an argument and returns the value associated with the `key` if it exists in the cache, otherwise it returns `null`.

## **Expiry**

In a cache system, we usually want to store data temporarily and discard it after a certain period of time. We can add an expiry mechanism to the `Cache` class to achieve this.

First, let's add a field to the `Cache` class to store the expiry time of the data:

```csharp
private readonly Dictionary<string, (object, DateTime)> _cacheData;
```

Now, the value of the dictionary will be a tuple containing the actual data and the expiry time.

We can update the `Add` method to accept an expiry time as an argument:

```csharp
public void Add(string key, object data, TimeSpan expiry)
{
    if (_cacheData.ContainsKey(key))
    {
        _cacheData[key] = (data, DateTime.Now.Add(expiry));
    }
    else
    {
        _cacheData.Add(key, (data, DateTime.Now.Add(expiry)));
    }
}
```

We also need to update the `Get` method to check the expiry time of the data before returning it:

```csharp
public object Get(string key)
{
    if (_cacheData.ContainsKey(key))
    {
        var (data, expiry) = _cacheData[key];
        if (expiry > DateTime.Now)
        {
            return data;
        }
    }

    return null;
}
```

The `Get` method first checks if the `key` exists in the cache. If it does, it retrieves the data and the expiry time and checks if the expiry time is in the future. If it is, it returns the data, otherwise it returns `null`.

## **Example**

Let's see an example of how to use the `Cache` class.

First, let's create an instance of the `Cache` class:

```csharp
var cache = new Cache();
```

Now, let's add some data to the cache:

```csharp
cache.Add("key1", "value1", TimeSpan.FromMinutes(10));
cache.Add("key2", "value2", TimeSpan.FromMinutes(20));
cache.Add("key3", "value3", TimeSpan.FromMinutes(30));
```

We can retrieve the data from the cache using the `Get` method:

```csharp
var value1 = cache.Get("key1");
var value2 = cache.Get("key2");
var value3 = cache.Get("key3");
```

## **Conclusion**

In this article, we saw how to create a simple cache system in C# using a `Dictionary` object and an expiry mechanism. We also saw an example of how to use the `Cache` class to add and retrieve data from the cache.

Caching can significantly improve the performance of a system by reducing the number of expensive data access operations. It is a useful technique to have in your toolkit as a developer.