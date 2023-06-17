---
title: "C#: The Priority Queue Pattern"
datePublished: Fri Jun 16 2023 16:43:40 GMT+0000 (Coordinated Universal Time)
cuid: cliysua4j000q09mp0h4j8187
slug: c-the-priority-queue-pattern
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686932835700/94adada8-3f8f-40d1-b25b-f230e6dec0d1.png
tags: design-patterns, csharp, net

---

In software development, there are often scenarios where you need to process items in a specific order based on their priority. One commonly used data structure for this purpose is a priority queue. A priority queue is an abstract data type that allows you to insert elements with associated priorities and retrieve them in a specific order. In C#, you can implement the priority queue pattern using various approaches. In this article, we will explore a simple implementation of the priority queue pattern in C#.

## **Implementation Using SortedList**

One way to implement a priority queue in C# is by using the `SortedList` class from the .NET framework. The `SortedList` class maintains a sorted collection of key-value pairs, where the keys represent the priorities, and the values represent the items. Here's an example of how you can create a basic priority queue using `SortedList`:

```csharp
using System;
using System.Collections.Generic;

public class PriorityQueue<T>
{
    private SortedList<int, Queue<T>> queue;
	public int Count => _queue.Count;

    public PriorityQueue()
    {
        queue = new SortedList<int, Queue<T>>();
    }

    public void Enqueue(T item, int priority)
    {
        if (!queue.ContainsKey(priority))
        {
            queue.Add(priority, new Queue<T>());
        }

        queue[priority].Enqueue(item);
    }

    public T Dequeue()
    {
        if (queue.Count == 0)
        {
            throw new InvalidOperationException("Priority queue is empty.");
        }

        var highestPriority = queue.Keys[0];
        var highestPriorityQueue = queue[highestPriority];
        var item = highestPriorityQueue.Dequeue();

        if (highestPriorityQueue.Count == 0)
        {
            queue.RemoveAt(0);
        }

        return item;
    }
}
```

In this implementation, we use a `SortedList<int, Queue<T>>` to store the items. The keys represent the priorities, and the values are `Queue<T>` objects that hold the items with the same priority. When you enqueue an item, it's added to the corresponding queue based on its priority. The `Dequeue` method retrieves the item with the highest priority by dequeuing it from the queue with the lowest priority value.

## **Using the Priority Queue**

Let's see an example of how to use the `PriorityQueue` class:

```csharp
public static void Main()
{
    var priorityQueue = new PriorityQueue<string>();

    priorityQueue.Enqueue("Task 1", 3);
    priorityQueue.Enqueue("Task 2", 1);
    priorityQueue.Enqueue("Task 3", 2);

    while (priorityQueue.Count > 0)
    {
        var item = priorityQueue.Dequeue();
        Console.WriteLine(item);
    }
}
```

In this example, we enqueue three tasks with different priorities into the priority queue. When we dequeue the items, they are returned in the order of their priorities (i.e., from the highest priority to the lowest).

## **Task Scheduling with Priority Queue**

In addition to managing items based on their priorities, the priority queue pattern is also commonly used in task scheduling scenarios. By assigning priorities to tasks, you can ensure that higher-priority tasks are executed before lower-priority ones. Let's expand our previous `PriorityQueue` implementation to include task scheduling capabilities.

Now, let's see an example of how to use the `PriorityQueue` class for task scheduling:

```csharp
using System;
using System.Threading.Tasks;

public class Program
{
    public static void Main()
    {
        var priorityQueue = new PriorityQueue<Task>();

        var task1 = new Task(() => Console.WriteLine("Task 1 completed."));
        var task2 = new Task(() => Console.WriteLine("Task 2 completed."));
        var task3 = new Task(() => Console.WriteLine("Task 3 completed."));

        priorityQueue.Enqueue(task1, 3);
        priorityQueue.Enqueue(task2, 1);
        priorityQueue.Enqueue(task3, 2);

        while (priorityQueue.Count > 0)
        {
            var task = priorityQueue.Dequeue();
            task.Start();
            task.Wait();
        }
    }
}
```

In this example, we create three `Task` objects representing different tasks. We assign priorities to each task and enqueue them into the `PriorityQueue`. The tasks are then dequeued and executed in order of their priorities. The `Start()` method is used to begin the execution of each task, and the `Wait()` method is used to ensure that a task completes before moving on to the next one.

## **Real-World Example: Task Scheduling in a Job Queue**

To illustrate the practical application of task scheduling with a priority queue, let's consider a real-world example of a job queue system. Imagine a server application that receives incoming jobs from multiple clients and needs to process them efficiently based on their priorities.

### **Job Class**

First, let's define a `Job` class that represents an individual job in the system. Each job will have a unique identifier, a priority level, and some associated data or instructions to be processed.

```csharp
public class Job
{
    public int Id { get; set; }
    public int Priority { get; set; }
    public string Data { get; set; }
}
```

### **Job Processing**

Next, we'll create a `JobProcessor` class responsible for managing the job queue and executing the jobs based on their priorities. This class will utilize the `PriorityQueue` we implemented earlier to prioritize and process the jobs.

```csharp
public class JobProcessor
{
    private PriorityQueue<Job> jobQueue;

    public JobProcessor()
    {
        jobQueue = new PriorityQueue<Job>();
    }

    public void EnqueueJob(Job job)
    {
        jobQueue.Enqueue(job, job.Priority);
    }

    public void ProcessJobs()
    {
        while (jobQueue.Count > 0)
        {
            var job = jobQueue.Dequeue();
            Console.WriteLine($"Processing Job {job.Id} with priority {job.Priority}: {job.Data}");
            // Perform actual job processing here...
        }
    }
}
```

### **Using the Job Processor**

Now, let's see how we can use the `JobProcessor` class to handle incoming jobs and process them based on their priorities.

```csharp
public class Program
{
    public static void Main()
    {
        var jobProcessor = new JobProcessor();

        var job1 = new Job { Id = 1, Priority = 2, Data = "Process sales data" };
        var job2 = new Job { Id = 2, Priority = 1, Data = "Generate monthly report" };
        var job3 = new Job { Id = 3, Priority = 3, Data = "Backup database" };

        jobProcessor.EnqueueJob(job1);
        jobProcessor.EnqueueJob(job2);
        jobProcessor.EnqueueJob(job3);

        jobProcessor.ProcessJobs();
    }
}
```

In this example, we create three `Job` objects with different priorities and enqueue them in the `JobProcessor` instance. The `ProcessJobs()` method is then called, which dequeues the jobs from the priority queue and processes them based on their priorities. The specific job processing logic is not included in this example, but you can perform the actual job tasks, such as data processing, report generation, or database backups, within the processing loop.

## **When to Use the Priority Queue Pattern?**

The priority queue pattern is beneficial in various situations where you need to manage items or tasks based on their priorities. Here are a few scenarios where you can consider using the priority queue pattern:

### **Task Scheduling**

When you have a set of tasks that need to be executed in a specific order based on their priorities, the priority queue pattern allows you to efficiently schedule and process the tasks.

### **Job Processing**

In job queue systems, where incoming jobs arrive with different priorities, the priority queue pattern enables you to prioritize and process jobs based on their importance or urgency.

### **Event-driven Systems**

When handling events in an event-driven system, the priority queue pattern can help manage event callbacks or actions based on their priority levels, ensuring that higher-priority events are processed first.

### **Resource Allocation**

For resource allocation scenarios, such as managing requests for shared resources or handling customer service tickets, the priority queue pattern allows you to prioritize and address requests based on their importance or severity.

Remember, the priority queue pattern is especially valuable when you need to order items or tasks based on their priorities and process them efficiently. It provides a flexible and scalable solution for managing prioritized data in various application domains.

## Conclusion

In conclusion, the priority queue pattern in C# provides a powerful mechanism for managing items or tasks based on their priorities. By using a priority queue, you can efficiently prioritize and process data or tasks in the desired order. Whether it's task scheduling, job processing, or any other scenario that requires prioritization, the priority queue pattern is a valuable tool in your C# programming arsenal.