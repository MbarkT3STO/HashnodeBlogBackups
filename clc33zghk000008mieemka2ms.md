# Multithreading in C#

Multithreading is a technique that allows a program to execute multiple threads concurrently. This can be useful in scenarios where a program needs to perform multiple tasks simultaneously, such as downloading multiple files or processing data in parallel. In C#, multithreading is implemented using the `System.Threading` namespace, which provides classes and methods for creating and managing threads.

## **Creating and Starting a Thread**

To create a new thread in C#, you can use the `Thread` class from the `System.Threading` namespace. The `Thread` class has a constructor that takes a `ThreadStart` delegate as an argument. The `ThreadStart` delegate represents the method that will be executed by the thread.

Here is an example of how to create and start a new thread in C#:

```csharp
using System.Threading;

namespace ThreadExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create a new thread
            Thread t = new Thread(new ThreadStart(ThreadMethod));

            // Start the thread
            t.Start();
        }

        static void ThreadMethod()
        {
            // The code in this method will be executed in the new thread
            Console.WriteLine("Hello from the new thread!");
        }
    }
}
```

In this example, the `ThreadMethod` method will be executed in the new thread.

## **Stopping a Thread**

To stop a thread, you can use the `Abort` method of the `Thread` class. This method raises a `ThreadAbortException` in the thread, which can be caught and handled. However, it is generally not recommended to use the `Abort` method to stop a thread, as it can leave resources in an undefined state and may cause unintended side effects.

A better way to stop a thread is to use a flag variable that the thread can check periodically. When the flag is set to a certain value, the thread can exit gracefully by cleaning up any resources it is using and terminating.

Here is an example of how to stop a thread using a flag variable:

```csharp
using System.Threading;

namespace ThreadExample
{
    class Program
    {
        static bool stopThread = false;

        static void Main(string[] args)
        {
            // Create a new thread
            Thread t = new Thread(new ThreadStart(ThreadMethod));

            // Start the thread
            t.Start();

            // Wait for the user to press a key
            Console.ReadKey();

            // Set the stopThread flag to true
            stopThread = true;
        }

        static void ThreadMethod()
        {
            while (!stopThread)
            {
                // The code in this loop will be executed until the stopThread flag is set to true
                Console.WriteLine("Hello from the new thread!");
                Thread.Sleep(1000);
            }
        }
    }
}
```

In this example, the `ThreadMethod` method will be executed in a loop until the `stopThread` flag is set to `true`. When the user presses a key, the `stopThread` flag is set to `true` and the thread will exit the loop and terminate.

## **Thread Synchronization**

When multiple threads access shared resources, there is a potential for race conditions to occur. A race condition occurs when two or more threads try to access the same resource at the same time, and the outcome of the program depends on the order in which the threads access the resource. To prevent race conditions, you can use synchronization techniques to ensure that only one thread can access the shared resource at a time.

One way to synchronize threads in C# is to use the `lock` statement. The `lock` statement acquires a mutual-exclusion lock on an object, allowing only one thread to execute the code inside the lock block at a time.

Here is an example of how to use the `lock` statement to synchronize threads:

```csharp
using System.Threading;

namespace ThreadExample
{
    class Program
    {
        static readonly object _lock = new object();
        static int sum = 0;

        static void Main(string[] args)
        {
            // Create two new threads
            Thread t1 = new Thread(new ThreadStart(ThreadMethod));
            Thread t2 = new Thread(new ThreadStart(ThreadMethod));

            // Start the threads
            t1.Start();
            t2.Start();

            // Wait for the threads to finish
            t1.Join();
            t2.Join();

            // Print the result
            Console.WriteLine("Sum: " + sum);
        }

        static void ThreadMethod()
        {
            for (int i = 0; i < 100; i++)
            {
                lock (_lock)
                {
                    // Only one thread can execute this code at a time
                    sum++;
                }
            }
        }
    }
}
```

In this example, the `ThreadMethod` method is executed by two separate threads, and both threads increment the `sum` variable by 1 in a loop. However, only one thread can execute the code inside the `lock` block at a time, so the `sum` variable is incremented by 1 for each iteration of the loop, rather than by 2.

## **Thread Priority**

In C#, you can set the priority of a thread using the `Priority` property of the `Thread` class. The priority of a thread determines the order in which the operating system schedules the thread to be executed. Higher-priority threads are more likely to be executed before lower-priority threads.

The possible values for the `Priority` property are `Lowest`, `BelowNormal`, `Normal`, `AboveNormal`, and `Highest`. The default value is `Normal`.

Here is an example of how to set the priority of a thread:

```csharp
using System.Threading;

namespace ThreadExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create a new thread
            Thread t = new Thread(new ThreadStart(ThreadMethod));

            // Set the priority of the thread to Highest
            t.Priority = ThreadPriority.Highest;

            // Start the thread
            t.Start();
        }

        static void ThreadMethod()
        {
            // The code in this method will be executed with highest priority
            Console.WriteLine("Hello from the high-priority thread!");
        }
    }
}
```

In this example, the `ThreadMethod` method will be executed with the highest priority.