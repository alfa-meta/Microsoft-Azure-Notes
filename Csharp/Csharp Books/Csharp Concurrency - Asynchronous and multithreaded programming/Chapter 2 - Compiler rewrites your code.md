
### Lambda's

Compiler modifies your code, which means that the output is not a direct representation of the source code.

async/await is implemented by the C# compiler rather than the .NET runtime.

Lambda function - code block you can write inline, inside a larger method, function or variable. That can also be used as a standalone method.

C# compiler rewrites lambda functions into regular methods since all functions have to be methods as part of a class.
	But this only happens at compile time.

Before Compiling
```C#
public class LambdaDemo1
{
private System.Timers.Timer? _timer;
	public void InitTimer()
	{
		_timer = new System.Timers.Timer(1000);
		_timer.Elapsed += (sender,args) => Console.WriteLine("Elapsed");
		_timer.Enabled = true;
	}
}
```

After Compiling
```C#
public class LambdaDemo2
{
	private System.Timers.Timer? _timer;
	private void HiddenMethodForLambda(
		object? sender, System.Timers.ElapsedEventArgs args)
	{
		Console.WriteLine("Elapsed");
	}
	public void InitTimer()
	{
		_timer = new System.Timers.Timer(1000);
		_timer.Elapsed += HiddenMethodForLambda;
		_timer.Enabled = true;
	}
}
```

If you want to store a local variable inside lambda the compiler creates a separate Class to store the local variable.

Before Compiling:
```C#
public class LambdaDemo3
{
	private System.Timers.Timer? _timer;
	
	public void InitTimer()
	{
		int aVariable = 5;
		_timer = new System.Timers.Timer(1000);
		_timer.Elapsed += (sender,args) => Console.WriteLine(aVariable);
		_timer.Enabled = true;
	}
}
```

After Compiling:
```C#
public class LambdaDemo4
{
	private System.Timers.Timer? _timer;
	private class HiddenClassForLambda
	{
		public int aVariable;
		public void HiddenMethodForLambda( object? sender, System.Timers.ElapsedEventArgs args)
		{
			Console.WriteLine(aVariable);
		}
	}
	public void InitTimer()
	{
		var hiddenObject = new HiddenClassForLambda();
		hiddenObject.aVariable = 5;
		_timer = new System.Timers.Timer(1000);
		_timer.Elapsed += hiddenObject.HiddenMethodForLambda;
		_timer.Enabled = true;
	}
}
```

If there are multiple lambda functions defined in the same method, they are placed in the same class so they can share local variables.

### Yield return

Yield return - lets you write functions that generate a sequence of values you can use in foreach loops directly without using a collection such as a list or an array.
	Called an iterator method.

Iterator method - returns data when needed.
	Saves CPU and Memory, data is returned on demand rather than pre-processed. Lazy evaluation.
	Does not execute immediately.
	Each iteration is resumed when needed.
	Eliminates boilerplate code.

Example code:
```C#
IEnumerable<int> GetNumbers()
{
    yield return 1;
    yield return 2;
    yield return 3;
}
```

Coroutine - code that can be suspended, resumed, and potentially return multiple values.

![[Pasted image 20250612151739.png]]

The IEnumerator interface is important because it lets us (and the compiler) write code that handles a sequence of items without knowing anything about that sequence. 
```C# 
IEnumerator<T>
```

## Summary
The C# compiler will rearrange and rewrite your code to add features that do not exist in .NET.

For lambda functions, the compiler moves code into a new method and shared data into a new class.

For yield return, the compiler also divides your code into chunks and wraps them in a class that runs the correct chunk at the correct time to simulate a function that can be suspended and resumed.

