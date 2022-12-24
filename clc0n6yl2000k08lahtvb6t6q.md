# What is C#’s CancellationToken?

## **Introduction**

CancellationToken is a struct in the System.Threading namespace of the .NET Framework that is used to request cancellation of a long-running operation. It is commonly used in asynchronous programming to allow the calling code to cancel an operation that is currently in progress.

Here is an example of how to use a CancellationToken in C#:

```csharp
using System.Threading;

public async Task DoWorkAsync(CancellationToken cancellationToken)
{
    while (!cancellationToken.IsCancellationRequested)
    {
        // Perform some work here...

        // Check for cancellation every iteration
        cancellationToken.ThrowIfCancellationRequested();
    }
}
```

In this example, the `DoWorkAsync` method is an async task that performs some work in a loop. The `cancellationToken` parameter is used to check for a cancellation request at the beginning of each loop iteration. If a cancellation request has been made, the `ThrowIfCancellationRequested` method is called, which throws a `OperationCanceledException` to signal that the operation has been cancelled.

## **CancellationTokenSource**

CancellationTokenSource is a class in the System.Threading namespace that is used to create a CancellationToken. It has a `Cancel` method that can be called to request cancellation of the associated CancellationToken.

Here is an example of how to use a CancellationTokenSource to cancel a long-running operation:

```csharp
using System.Threading;

public async Task DoWorkAsync()
{
    CancellationTokenSource cts = new CancellationTokenSource();

    // Start the long-running operation in a separate task
    Task workTask = Task.Run(() => DoWorkAsync(cts.Token));

    // Cancel the operation after 5 seconds
    await Task.Delay(5000);
    cts.Cancel();

    // Wait for the operation to complete
    await workTask;
}

private async Task DoWorkAsync(CancellationToken cancellationToken)
{
    while (!cancellationToken.IsCancellationRequested)
    {
        // Perform some work here...

        // Check for cancellation every iteration
        cancellationToken.ThrowIfCancellationRequested();
    }
}
```

In this example, the `DoWorkAsync` method creates a CancellationTokenSource and passes its associated CancellationToken to the `DoWorkAsync` method in a separate task. After a delay of 5 seconds, the `Cancel` method is called on the CancellationTokenSource, which sets the cancellation request on the CancellationToken. The `DoWorkAsync` method then checks for a cancellation request at the beginning of each loop iteration, and throws a `OperationCanceledException` if one has been made.

## **Linking CancellationTokenSources**

CancellationTokenSource also has a `CreateLinkedTokenSource` method that allows you to link multiple CancellationTokenSources together. When any of the linked CancellationTokenSources are cancelled, all of the linked CancellationTokens will be cancelled as well.

Here is an example of how to use the `CreateLinkedTokenSource` method:

```csharp
using System.Threading;
CancellationTokenSource cts1 = new CancellationTokenSource();
CancellationTokenSource cts2 = new CancellationTokenSource();

CancellationTokenSource linkedCts = CancellationTokenSource.CreateLinkedTokenSource(cts1.Token, cts2.Token);

// Start a long-running operation using the linked CancellationToken
Task workTask = Task.Run(() => DoWorkAsync(linkedCts.Token));

// Cancel the operation by cancelling one of the linked CancellationTokenSources
cts1.Cancel();

// Wait for the operation to complete
await workTask;
```

In this example, the `CreateLinkedTokenSource` method is used to create a linked CancellationTokenSource from `cts1` and `cts2`. If either of these CancellationTokenSources are cancelled, the linked CancellationToken will also be cancelled.

## Real-World Examples

### Web Application

One real-world example of using CancellationToken in C# is in a web application that allows users to perform long-running tasks, such as importing a large amount of data or generating a report.

In this scenario, the user initiates the long-running task by making a request to the web server. The server then starts the task in a separate thread, and returns a unique identifier to the client to track the progress of the task.

The client can then make periodic requests to the server to check the status of the task, and can also request cancellation of the task by sending a cancellation request to the server with the unique identifier.

On the server side, the task would be implemented using a CancellationToken to check for a cancellation request at regular intervals. Here is an example of how this might look:

```csharp
using System.Threading;
using System.Web.Mvc;

public class TaskController : Controller
{
    // Dictionary to track the status of tasks
    private static Dictionary<string, CancellationTokenSource> _tasks = new Dictionary<string, CancellationTokenSource>();

    [HttpPost]
    public ActionResult StartTask()
    {
        // Generate a unique identifier for the task
        string taskId = Guid.NewGuid().ToString();

        // Create a CancellationTokenSource for the task
        CancellationTokenSource cts = new CancellationTokenSource();

        // Add the CancellationTokenSource to the dictionary
        _tasks.Add(taskId, cts);

        // Start the task in a separate thread
        Task.Run(() => DoTaskAsync(cts.Token));

        // Return the task identifier to the client
        return Json(new { taskId = taskId });
    }

    [HttpPost]
    public ActionResult CancelTask(string taskId)
    {
        // Look up the CancellationTokenSource for the task
        CancellationTokenSource cts = _tasks[taskId];

        // Cancel the task
        cts.Cancel();

        // Remove the CancellationTokenSource from the dictionary
        _tasks.Remove(taskId);

        return Json(new { success = true });
    }

    private async Task DoTaskAsync(CancellationToken cancellationToken)
    {
        while (!cancellationToken.IsCancellationRequested)
        {
            // Perform some work here...

            // Check for cancellation every iteration
            cancellationToken.ThrowIfCancellationRequested();
        }
    }
}
```

In this example, the `StartTask` action creates a CancellationTokenSource for the task and starts the task in a separate thread. The `CancelTask` action cancels the task by calling the `Cancel` method on the CancellationTokenSource, and removes the CancellationTokenSource from the dictionary. The `DoTaskAsync` method performs the work for the task and checks for a cancellation request at regular intervals using the CancellationToken.

This approach allows the client to cancel the long-running task at any time, and allows the server to communicate the cancellation request to the task in progress.

### Console Application

Another example of using CancellationToken in C# is in a console application that performs a long-running operation, such as downloading a large file from the internet.

In this scenario, the console application can use CancellationToken and CancellationTokenSource to allow the user to cancel the operation by pressing a key on the keyboard. Here is an example of how this might look:

```csharp
using System;
using System.Threading;

class Program
{
    static void Main(string[] args)
    {
        CancellationTokenSource cts = new CancellationTokenSource();

        // Start the long-running operation in a separate task
        Task downloadTask = Task.Run(() => DownloadFileAsync(cts.Token));

        // Wait for the user to press a key to cancel the operation
        Console.WriteLine("Press any key to cancel the operation...");
        Console.ReadKey();

        // Cancel the operation
        cts.Cancel();

        // Wait for the operation to complete
        downloadTask.Wait();
    }

    private static async Task DownloadFileAsync(CancellationToken cancellationToken)
    {
        while (!cancellationToken.IsCancellationRequested)
        {
            // Perform some work here...

            // Check for cancellation every iteration
            cancellationToken.ThrowIfCancellationRequested();
        }
    }
}
```

In this example, the `DownloadFileAsync` method is a long-running operation that is started in a separate task. The `Main` method waits for the user to press a key on the keyboard, and then cancels the operation by calling the `Cancel` method on the CancellationTokenSource. The `DownloadFileAsync` method checks for a cancellation request at regular intervals using the CancellationToken.

This approach allows the user to cancel the long-running operation at any time by pressing a key on the keyboard, and allows the console application to communicate the cancellation request to the operation in progress.

### WPF Application

Another example of using CancellationToken in C# is in a WPF application that performs a long-running operation, such as searching a large database.

In this scenario, the WPF application can use CancellationToken and CancellationTokenSource to allow the user to cancel the operation by clicking a cancel button on the user interface. Here is an example of how this might look:

```csharp
using System.Threading;
using System.Windows;
using System.Windows.Controls;

public partial class MainWindow : Window
{
    private CancellationTokenSource _cts;

    public MainWindow()
    {
        InitializeComponent();
    }

    private void SearchButton_Click(object sender, RoutedEventArgs e)
    {
        // Disable the search button and show the cancel button
        SearchButton.IsEnabled = false;
        CancelButton.Visibility = Visibility.Visible;

        // Create a CancellationTokenSource for the search
        _cts = new CancellationTokenSource();

        // Start the search in a separate task
        Task searchTask = Task.Run(() => SearchAsync(_cts.Token));
    }

    private void CancelButton_Click(object sender, RoutedEventArgs e)
    {
        // Cancel the search
        _cts.Cancel();

        // Enable the search button and hide the cancel button
        SearchButton.IsEnabled = true;
        CancelButton.Visibility = Visibility.Collapsed;
    }

    private async Task SearchAsync(CancellationToken cancellationToken)
    {
        while (!cancellationToken.IsCancellationRequested)
        {
            // Perform the search here...

            // Check for cancellation every iteration
            cancellationToken.ThrowIfCancellationRequested();
        }
    }
}
```

In this example, the `SearchButton_Click` event handler starts the search operation in a separate task when the user clicks the search button. The `CancelButton_Click` event handler cancels the search operation by calling the `Cancel` method on the CancellationTokenSource, and disables the search button and hides the cancel button. The `SearchAsync` method performs the search and checks for a cancellation request at regular intervals using the CancellationToken.

This approach allows the user to cancel the long-running operation at any time by clicking the cancel button, and allows the WPF application to communicate the cancellation request to the operation in progress.

### Desktop Application

Another example of using CancellationToken in C# is in a desktop application that performs a long-running operation, such as importing a large amount of data from a file.

In this scenario, the desktop application can use CancellationToken and CancellationTokenSource to allow the user to cancel the operation by pressing a cancel button on the user interface. Here is an example of how this might look in WPF:

```csharp
using System.Threading;
using System.Windows;
using System.Windows.Controls;

public partial class MainWindow : Window
{
    private CancellationTokenSource _cts;

    public MainWindow()
    {
        InitializeComponent();

        // Create the cancel button
        Button cancelButton = new Button { Content = "Cancel" };
        cancelButton.Click += CancelButton_Click;

        // Add the cancel button to the grid
        Grid.SetRow(cancelButton, 1);
        MainGrid.Children.Add(cancelButton);
    }

    private void CancelButton_Click(object sender, RoutedEventArgs e)
    {
        // Cancel the import
        _cts.Cancel();

        // Disable the cancel button
        ((Button)sender).IsEnabled = false;
    }

    private void ImportButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a CancellationTokenSource for the import
        _cts = new CancellationTokenSource();

        // Start the import in a separate task
        Task importTask = Task.Run(() => ImportAsync(_cts.Token));
    }

    private async Task ImportAsync(CancellationToken cancellationToken)
    {
        while (!cancellationToken.IsCancellationRequested)
        {
            // Perform the import here...

            // Check for cancellation every iteration
            cancellationToken.ThrowIfCancellationRequested();
        }
    }
}
```

In this example, the `CancelButton_Click` event handler cancels the import operation by calling the `Cancel` method on the CancellationTokenSource, and disables the cancel button. The `ImportAsync` method performs the import and checks for a cancellation request at regular intervals using the CancellationToken.

This approach allows the user to cancel the long-running operation at any time by pressing the cancel button, and allows the desktop application to communicate the cancellation request to the operation in progress.

# **Conclusion**

CancellationToken and CancellationTokenSource are useful tools for requesting cancellation of long-running operations in C#. They allow you to cancel an operation that is currently in progress, and provide a mechanism for communicating the cancellation request to the operation itself.

I hope this article has helped you understand how to use CancellationToken and CancellationTokenSource in your C# code. Happy coding!