
Asynchronous Programming and Multithreading are both techniques to achieve concurrency, but they work differently.

Asynchronous programming - a programming model where tasks run without blocking the main thread, using `async`/`await` patterns.
	Uses a single thread + event loop to switch between tasks when waiting.
	Great for I/O-bound tasks.

Multithreading - runs multiple threads in parallel.
	Useful for CPU-bound tasks.
	Each thread runs independently; context switching is managed by the OS.

| Feature         | Async Programming                  | Multithreading                     |
| --------------- | ---------------------------------- | ---------------------------------- |
| Use Case        | I/O-bound tasks                    | CPU-bound tasks                    |
| Threads used    | Often single-threaded              | Multiple threads                   |
| Execution model | Event loop with callbacks(`await`) | True parallel execution            |
| Complexity      | Easier with `async/await`          | More complex: thread safety, locks |


Asynchronous reading of files:
```C#
public void Read10Files()
{
	var tasks = new Task[10];
	for(int i=0; i<10; i++)
	{
		tasks[i] = File.ReadAllBytesAsync($"{i}.txt");
	}
	Task.WaitAll(tasks);
}
```


Multithreading/Parallel reading of files:
```C#
public void Read10Files()
{
	var tasks = new Task[10];
	for(int i=0; i<10;i++)
	{
		var icopy = i;
		tasks[i] = Task.Run(() => File.ReadAllBytes($"{icopy}.txt"));
	}
	Task.WaitAll(tasks);
}
```

`Task.Run` creates new threads and therefor would either use 10 threads or create enough of them and add them to the pool

```C#
public void Process10Files()
{
	var tasks = new Task[10];
	for(int i=0; i<10; i++)
	{
		var icopy = i;
		tasks[i] = Task.Run(async () =>
		{
			await File.ReadAllBytesAsync($"{icopy}.txt");
			Console.WriteLine("Doing something with the file's content");
		});
	}
	Task.WaitAll(tasks);
}
```

Instead of using 10 threads `File.ReadAllBytesAsync()` will pick up a thread from the thread pool, use it to start the read operation and then free the thread from the thread pool.

By adding `async` to the method and await `Task.WhenAll` the method truly uses no threads when it doesn't need to.

Final Code:
```C#
public async Task Process10Files() /// Makes the method async
{
	var tasks = new Task[10];
	for(int i=0;i<10;i++)
	{
		var icopy = i;
		tasks[i] = Task.Run(async () =>
		{
			await File.ReadAllBytesAsync($"{icopy}.txt");
			Console.WriteLine("Doing something with the file's content");
		});
	}
	await Task.WhenAll(tasks); /// Changes WaitAll to WhenAll
}
```

### Where does code run after await?

Example A:
```C#
var buffer = await File.ReadAllBytesAsync("somefile.bin");
Console.WriteLine(buffer[0]);
```

A is translated to B

Example B:
```C#
File.ReadAllBytesAsync("somefile.bin").ContinueWith(task =>
{
	var buffer = task.Result;
	Console.WriteLine(buffer[0]);
})
```

`Task.ContinueWith` runs a continuation after a task completed.
- By default, it runs the continuation on a ThreadPool thread (not the original thread that created the task.)

`await` awaits the result of an async operation, then continues running code after it completes.
- If you're on a UI thread, `await` tries to resume on the same thread.
- If that thread is not available it uses a ThreadPool thread.

If the `await` task is complete no thread switching occurs and the code continues to run normally.

If `await` knows the current context it will use the previous thread, else it will use ThreadPool.

Using `Thread` class you can change the `Name` parameter, which is just a string.

Using `await` inside a `lock` statement leads to Compiler error. This is due to:
	Lock statements are being used by a specific thread, await statements can be used by any thread, requiring the lock to be lifted for await statement to work in this case.
	Await frees up the thread and potentially runs other code. Running code you do not control can cause deadlocks.

Example of Compiler error:
```C#
//// Compiler Error
private Dictionary<string, string> _cache = new();
private object _cacheLock = new();
public async Task<string> GetResult(string query)
{
	lock(_cacheLock)
	{
		if (_cache.TryGetValue(query, out var cacheResult)) return cacheResult;
		var http = new HttpClient();
		var result = await http.GetStringAsync("https://example.com?"+query);
		_cache[query] = result;
	}
	return result;
}_
```

Releasing the lock while awaiting:
```C#
private Dictionary<string, string> _cache = new();
private object _cacheLock = new();
public async Task<string> GetResult(string query)
{
	lock(_cacheLock) /// Lock while checking the cache
	{
		if (_cache.TryGetValue(query, out var cacheResult)) return cacheResult;
	}

	/// async HTTP call
	var http = new HttpClient();
	var result = await http.GetStringAsync("https://example.com?"+query);
	
	lock(_cacheLock) /// Lock while updating the cache
	{
		_cache[query] = result;
	}
	return result;
}
```

![[Pasted image 20250703063714.png]]


![[Pasted image 20250703063729.png]]

### UI threads

UI threads do not want to be blocked for any reason, therefore async methods need to be used. 

Example UI thread code that works:
```C#
private async void MyButtonClick(object sender, EventArgs ea)
{
	int result = 0;
	await Task.Run(() =>
	{
		result = LongCalculation();
	});
	MyLabel.Text = result.ToString();
}
```


## Summary
- The tools for using asynchronous operations are also good for using multithreaded operations.
- The high-performance code that benefits from multithreading is also likely to benefit from using asynchronous operations.
- In UI apps, when using await in the UI thread, the code after `await` will also run in the UI thread.
- In all other cases, the code after await will run in the thread pool.
- If the code calling await is running in the thread pool, the code after the `await` might run in a different thread in the thread pool.
- You can't use the `await` inside the code block of a `lock` statement. The best solution is to rearrange your code and move the `await` outside of the `lock` block.