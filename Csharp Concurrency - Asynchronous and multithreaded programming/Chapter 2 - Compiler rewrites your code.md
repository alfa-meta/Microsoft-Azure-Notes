



### Summary
The C# compiler will rearrange and rewrite your code to add features that do not exist in .NET.

For lambda functions, the compiler moves code into a new method and shared data into a new class.

For yield return, the compiler also divides your code into chunks and wraps them in a class that runs the correct chunk at the correct time to simulate a function that can be suspended and resumed.