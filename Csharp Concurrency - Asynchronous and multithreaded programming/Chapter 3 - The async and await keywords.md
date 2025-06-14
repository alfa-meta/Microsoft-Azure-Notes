
`async/await` is a feature that lets us write asynchronous code as if it were normal synchronous code.

Reading a file is a blocking operation, making a thread wait for data to arrive from the hard disk.

`Stream.Read` method has an asynchronous version called `Stream.ReadAsync`.

If a `Read` method returns an `int`. `ReadAsync` method returns `Task<int>`.

`Task` - represents an event that may happen in the future.
`Task<T>` - represents a value that may be available in the future.
	Usually referred to a future, promise, deferred value.

`Task` or `Task<T>` objects themselves do not let you manage the background operation and do not carry context related to it.

`Task` has no result value.
`Task<T>` has a result value.

4 States of completion in C#:
	`Task` happens.
	Value by `Task<T>` is available.
	`Task` is cancelled or faulted.


### Are we there yet?
Polling - asking the program are we there yet?
	In Asynchronous programming this happens in a loop asking if the `Task` is complete.
	Done my `Task.IsCompleted` - True even if the task has errored out or cancelled

`Task` also has `Status` property. The task is completed if `Status` is `RanToCompletion`, `Canceled`, `Faulted`.

You should not be checking `IsCompleted` or `Status` in a loop if possible. Wastes CPU cycles and negates Asynchronous programming.


Not very useful way of programming:
```C#
var readCompleted = File.ReadAllBytesAsync("example.bin");
while(!readCompleted.IsCompleted)
{
	UpdateCounter();
}
var bytes = readCompleted.Result;
// do something with bytes
```

### Wake me up when we get there
In this model you can pass a callback method to the task.
It will call you when its complete, errored out or cancelled.

A useful way of programming:
```C#
var readCompleted = File.ReadAllBytesAsync("example.bin");
readCompleted.ContinueWith( t =>
	{
		if(t.IsCompletedSuccessfully)
		{
		byte[] bytes = t.Result;
		// do something with bytes
		}
	});
```

`Task.ContinueWith` - runs after the preceding task completes.
	Can be used to chain tasks.
	Does not propagate exceptions automatically.


`Task.Result` method will cause your code to block a thread. Use it only if you want the code to run synchronously.

Both `Task` and `Task<T>` have `IsFaulted`, `IsCanceled`, `IsCompleted`.
Should be used at the end of the task to check the status.


`Task.Wait` - blocks the thread until the task has completed execution.
Calling `Wait` after the task is finished will not block the thread, since the task has already happened.
It will either do nothing, return immediately or throw an exception.

To check if the task has errored out:
```C#
if(task.IsFaulted)
	HandleError(task.Exception);
```


if you want to check whether the task has errored out and check the exception object without throwing you would use:
```C#
task.Wait();
```

`Task<T>.Result` will block the thread if the task has not completed. Equivalent to calling `Wait` . 

Catching exceptions after using `ContinueWith` guarantees that nice, fast and safe operation has occurred.

`await` keyword does not wait, it ends the current chunk and returns control to the caller.

### Async void methods

`async void` is only used for event handlers.
Avoid throwing exceptions from async void methods.

Correct way of using `async void`:
```C#
private async void Button1_Click(object sender, EventArgs ea)
{
	try
	{
		var text = await File.ReadAllTextAsync("source.txt");
		await File.WriteAllTextAsync("dest.txt", text);
	}
		catch(Exception ex)
	{
		// Do something with the exception
	}
}
```

`Dictionary` is not thread safe and is unsuitable to be used with `async` methods.

`GetValue` method first checks whether the requested value is in the cache.
	If so it will return the value before the first time it uses await.
	If value is in the cache it will be returned immediately.

`ValueTask` and `ValueTask<T>` are structs, that contain the value directly if the value is available immediately and a reference to a `Task`/`Task<T>` object otherwise.

`ValueTask.AsTask()` method allows you to get the `Task` stored inside `ValueTask`

`ValueTask` less efficient if used on slower operations.
	Much better when using cache or repeatedly used data or methods.
### Summary
`Task` represents an event that may happen in the future.
`Task<T>` represents a value that may be available in the future.
When the event happens or the value is available, we say that the `Task` or `Task<T>` has completed.
Use `ContinueWith` to make the task call you when it completes.
You can call `Wait` or `Result` to make the task synchronous, but that's inefficient and dangerous.
Calling `Wait` or `Result` after the task has completed is perfectly efficient and safe.
`async` is just a compiler flag. It tells the compiler that the method needs to be broken into chunks whenever there's an `await` keyword.
The `async` keyword does not make the code run in the background. Without await, it does nothing (except make the compiler generate an awful lot of boilerplate code).
The compiler breaks the method after each `await` and passes the next chunk to `ContinueWith` (conceptually).
`await` does not wait but ends the current chunk and returns to the caller.
`async` methods can be `void`, but then there's no way to know when the method has finished, and you should catch and handle all exceptions inside the method.
If an `async` method often returns a result immediately without doing anything asynchronous, you can improve the efficiency by returning `ValueTask` or `ValueTask<T>` instead of `Task` or `Task<T>`
