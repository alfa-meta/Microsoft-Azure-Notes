
Asynchrony is essential for activities that are potentially blocking, such as web access.

Blocking - a type of characteristic of code that is synchronous, and must be finished before any other task can be executed.

Task Asynchronous Programming (TAP) model - provides a layer of abstraction over typical asynchronous coding.
	The compiler reads your task-based code as it processes it, each statement and before it starts processing the next statement.

Goal of task based asynchronous programming is to enable code that reads like a sequence of statements, but executes in a more complicated order.

When an async task is started, it returns a Task or coroutine object.
The task will run in the background concurrently with other tasks in the event loop.
It will complete independently, but if you await it, your code will pause until it is done.

### Exceptions

When code running asynchronously throws an exception, the exception is stored in the Task object.

### Apply await expressions to tasks efficiently

```C#
Task.WhenAll(eggsTask, baconTask, toastTask);
Console.WriteLine("Eggs are ready");
Console.WriteLine("Bacon is ready");
Console.WriteLine("Toast is ready");
Console.WriteLine("Breakfast is ready!");
```
