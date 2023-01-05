# IComparable vs IComparer in C#

In C#, the `IComparable` and `IComparer` interfaces are used to sort and compare objects. These interfaces are defined in the `System` namespace and provide a means to compare objects of a specific type.

## **IComparable**

The `IComparable` interface is implemented by a class to allow its objects to be compared to other objects of the same type. It defines a single method called `CompareTo`, which takes an object of the same type and returns an integer indicating the relative order of the two objects.

For example, consider a `Person` class with properties `FirstName` and `LastName`:

```csharp
public class Person : IComparable<Person>
{
    public string FirstName { get; set; }
    public string LastName { get; set; }

    public int CompareTo(Person other)
    {
        if (other == null)
        {
            return 1;
        }

        int result = LastName.CompareTo(other.LastName);
        if (result == 0)
        {
            result = FirstName.CompareTo(other.FirstName);
        }

        return result;
    }
}
```

In this example, the `CompareTo` method compares the `LastName` properties of the two `Person` objects. If the last names are the same, the `FirstName` properties are compared. The method returns a positive integer if the current object is greater than the `other` object, a negative integer if the current object is less than the `other` object, and 0 if they are equal.

The `IComparable` interface is often used with the `Array.Sort` method to sort an array of objects. For example:

```csharp
Person[] people =
{
    new Person { FirstName = "John", LastName = "Doe" },
    new Person { FirstName = "Jane", LastName = "Doe" },
    new Person { FirstName = "Bob", LastName = "Smith" }
};

Array.Sort(people);
```

This will sort the `people` array in ascending order based on the comparison implemented in the `CompareTo` method of the `Person` class.

## **IComparer**

The `IComparer` interface is similar to `IComparable`, but it allows you to define a separate class to compare objects of a specific type. This can be useful if you want to define multiple ways to compare the same type of object, or if you don't have control over the implementation of the class you want to compare.

The `IComparer` interface defines two methods: `Compare` and `Equals`. The `Compare` method is similar to the `CompareTo` method of `IComparable`, but it takes two objects of the type being compared as arguments and returns an integer indicating their relative order. The `Equals` method is used to determine if two objects are equal.

Here is an example of a class that implements the `IComparer` interface for the `Person` class:

```csharp
public class PersonComparer : IComparer<Person>
{
    public int Compare(Person x, Person y)
    {
        if (x == null && y == null)
        {
            return 0;
        }
        if (x == null)
        {
            return -1;
        }
        if (y == null)
        {
        return 1;
        }
        
        int result = x.LastName.CompareTo(y.LastName);
        if (result == 0)
        {
        result = x.FirstName.CompareTo(y.FirstName);
        }
        
        return result;        
    }

    public bool Equals(Person x, Person y)
    {
        if (x == null && y == null)
        {
        return true;
        }
        if (x == null || y == null)
        {
        return false;
        }
        return x.LastName == y.LastName && x.FirstName == y.FirstName;
        }
    }
```

The `PersonComparer` class can then be used with the `Array.Sort` method to sort an array of `Person` objects:

```csharp
Person[] people =
{
    new Person { FirstName = "John", LastName = "Doe" },
    new Person { FirstName = "Jane", LastName = "Doe" },
    new Person { FirstName = "Bob", LastName = "Smith" }
};

Array.Sort(people, new PersonComparer());
```

## **Conclusion**

In summary, the `IComparable` and `IComparer` interfaces provide a means to compare and sort objects of a specific type in C#. `IComparable` is implemented by the class itself, while `IComparer` allows you to define a separate class to compare objects of a specific type. Both interfaces are commonly used with the `Array.Sort` method to sort arrays of objects.