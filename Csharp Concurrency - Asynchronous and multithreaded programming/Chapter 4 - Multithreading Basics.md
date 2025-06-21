When a process starts, it begins with one thread that runs the `Main` method. Referred as the main thread.

### Different ways to run in another thread

In .NET, a thread is represented by `System.Threading.Thread` class.
This class lets you inspect and control existing threads, and create new ones.

```C#
public void RunInBackground()
{
	var newThread = new Thread(CodeTuRunInBackgroundThread); // Creates thread object
	newThread.IsBackground = true; // Configures thread
	newThread.Start(); // Starts running
}

private void CodeToRunInBackgroundThread() // Code to run in new thread
{
	Console.WriteLine("Do Stuff");
}
```

If you add a parameter to `Thread.Start(anyParamHere)` it will pass an object to the Thread.

```C#
public void RunLotsOfThreads()
{
	var threads = new Thread[100];
	for(int i=0; i < 100; i++)
	{
		threads[i] = new Thread(MyThread);
		threads[i].Start(i);
	}
}
private void MyThread(object? parameter)
{
	Console.WriteLine($"Hello from thread {parameter}");
}
```

If you use parameterised method of `Thread` constructor and then use non-parameterised `Thread.Start` it will return null.

If you use non-parameterised delegate and the parameterised `Thread.Start`, value is ignored.

`Thread.Join` is a Computer Science common term meaning wait.
	It makes the thread wait.
	Good for parallel programming including combining results.
	Returns immediately if the thread has already finished.

```C#
public void RunAndWait()
{
	var threads = new Thread[3];
	for (int i=0; i < 3; i++) // Runs all threads
	{
		threads[i] = new Thread(DoWork);
		threads[i].Start();
	}
	foreach(var current in threads) // Waits for threads to finish
	{
		current.Join();
	}
	Console.WriteLine("Finished");
}

private void DoWork()
{
	Console.WriteLine("Doing Work");
}
```

Synchronisation - the issue of starting and waiting for threads repeatedly.

Loop waiting with `Join` only cares about the longest waiting thread and returns the other values immediately.

Creating and destroying threads is relatively resource intensive.

### When to use Thread.Start?
Use `Thread.Start` for:
	Long-running code.
	If you need to change thread settings.
Do not use `Thread.Start` for:
	Asynchronous code.
	Short tasks.

### Thread pool
With the thread pool, the system keeps a small number of threads waiting in the background, and whenever you have something to run, you can use one of those pre-existing threads to run it.

Thread pool is controlled by `System.Threading.ThreadPool` class.
To run something on the thread pool, use `QueueUserWorkItem`.

```C#
public void RunInBackground()
{
	ThreadPool.QueueUserWorkItem(RunInPool); // Queues code to run in thread pool
}

private void RunInPool(object? state) // Code to run
{
	Console.WriteLine("Do stuff");
}
```

Like `Thread.Start`, `QueueUserWorkItem` also has a parameterised and non-parameterised version. But unlike the `Thread`class, the method that runs on the thread pool always accepts an object parameter. Returns null if no parameter has been passed.

```C#
public void RunInBackground()
{
	for(int i=0; i < 10; i++)
	{
		ThreadPool.QueueUserWorkItem(RunInPool, i); // Passes a value to the thread
	}
}

private void RunInPool(object? parameter)
{
	Console.WriteLine($"Hello from thread {parameter}");
}
```

`ThreadPool.QueueUserWorkItem` pre-dates `async/await` and `Task` by about a decade.
### When to use `ThreadPool.QueueUserWorkItem`
Use it for:
	Short-running tasks
Do not use `ThreadPool.QueueUserWorkItem`
	For long-running tasks
	When you need to change the thread properties.
	With Task-based asynchronous operations.
	With `async/await`.

`Task.Run` method runs code on the thread pool, just like `ThreadPool.QueueUserWorkItem`, but it has a nicer interface that works well with `async/await`.

```C#
public void RunInBackground()
{
	Task.Run(RunInPool); // Queue code to run in thread pool
}

private void RunInPool() // Code to run
{
	Console.WriteLine("Do stuff");
}
```

This example creates multiple threads with the `Thread` class and waits for all of them to finish.

```C#
public async Task RunInBackground()
{
	var tasks = new Task[10];
	for(int i=0; i<10; i++)
	{
		tasks[i] = Task.Run(RunInPool); // Queues tasks to run on thread pool
	}
	await Task.WhenAll(tasks); // Waits for tasks to complete.
	Console.WriteLine("All finished");
}

private void RunInPool()
{
	Console.WriteLine("Do stuff");
}
```

Compiler generates a warning if Task.Run is not awaited. But you don't want to await it in order to gain the benefits of asynchronous programming.

Do this to remove the warning:

```C#
Task.Run(MethodToRunInBackground); // Might generate a warning.
_ = Task.Run(MethodToRunInBackground);_// No warning.
```

### When to use `Task.Run`
Use for:
	Code that uses async-await.
	Short running tasks.
Do not use `Task.Run` for:
	Non-asynchronous long running tasks.


## Accessing the same variables from multiple threads.

Simplest example:
```C#
int n = 0;
n++;
```

What happens in this example:
- Read the value from the memory location allocated for the n variable into the CPU.
- Increment the value inside the CPU.
- Save the new value from the CPU back into the memory location allocated for the n variable.

Atomic operation - an operation that cannot be interrupted in a multithreaded application, due to being a single operation at the hardware level.

![[Pasted image 20250618060804.png]]

Figure 4.1 shows only single-threaded applications or applications that don't share data.

Incorrect value when accessing shared data without locking:
```C#
public void GetIncorrectValue()
{
	int theValue = 0;

	var threads = new Thread[2];
	for(int i=0; i<2; i++)
	{
		threads[i] = new Thread(() =>
		{
			for(int j=0; j<500000; j++)
				++theValue;
		});
		threads[i].Start();
	}

	foreach(var current in threads)
	{
		current.Join();
	}
	Console.WriteLine(theValue);
}
```

There is no way to guarantee our value to be any specific value. Due to `theValue` not being locked.

Lock - a general mechanism to enforce mutual exclusion.
	Provides methods like lock() and unlock().

Mutex (short for mutual exclusion) - using a variable only by one thread at a time.
	Usually includes ownership.
	A specific type of lock.
## Summary 
You can run multiple things in parallel. Each one of these things is called a thread.
The program starts with one thread running the `Main` method. This thread is called the main thread.
You can create Threads that are completely under your control with the `Thread` class by creating a `Thread` object, optionally reconfiguring it, and calling the `Thread.Start` method to start running it.
The thread pool is a collection of threads managed by the system and is available for use when you have code to run. It is optimised for short-running tasks and can create new threads as needed.
Traditionally, you run code in the thread pool by using the `ThreadPool.QueueUserWorkItem` method.
A simpler and more `async/await` friendly way to run code in the thread pool is using `Task.Run`.
The main thread and threads you create with the `Thread` class are the only threads that are completely under your control and that you can reconfigure any way you want. However, you should never reconfigure threads created by other components, especially threads managed by the threads pool.
When you access data shared by more than one thread, you have to use a lock; otherwise, different threads may overwrite data written by other threads, leading to incorrect results.
The same also applies to reading shared data. If you don't use locks to synchronise reads as well as writes, you may get stale data and even the results of incomplete writes.
In native UI applications, the thread running the UI is called the UI thread. It is typically the main thread, but it can be a different thread if needed. The UI thread is the only thread that may access windows and other UI controls.
You should avoid blocking the UI thread because this makes the UI freeze.