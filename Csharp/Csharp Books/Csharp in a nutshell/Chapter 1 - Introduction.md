
C# is a general-purpose, type-safe, object-oriented programming language.
Goal of the language is programmer productivity.

C# is platform neutral.

## Object Orientation

Encapsulation - means creating a boundary around an object to separate its external (public) behaviour from its internal (private) implementation.

#### Unified Type System

`type` - fundamental encapsulated unit of data and fundamental building block.
C# has a unified type system in which all types ultimately share a common base type.
An instance of any type can be converted to a string by calling its `ToString` method.


#### Classes and interfaces

Interface is like a class that cannot hold data. We can define behaviour but not state.
Allows for multiple inheritance as well as a separation between specification and implementation.

#### Properties, methods, and events

In pure object-oriented paradigm, all functions are methods.
In C#, methods are only one kind of function member, which also includes properties and events.
Properties are function members that encapsulate a piece of an object's state such as a button's colour or a label's text.
Events are function members that simplify acting on object state changes.


## Type Safety

C# is `type-safe` - meaning that instances of types can interact only through protocols they define, thereby ensuring each type's internal consistency.
	For instance, C# prevents you from interacting with a string type as though it were an integer type.

C# supports `Static typing` - meaning that the language enforces type safety at compile time. Also during runtime.

C# is `strongly typed language` - because its type rules are strictly enforced.
	For example you cannot pass a float to a function that requires an int. You need to cast an int first.

## Memory Management

The `Common Language Runtime` has a `garbage collector` that executes as part of your program, reclaiming memory for objects that are no longer referenced.

C# does not eliminate pointers.
For performance-critical hot spots and interoperability, pointers and explicit memory allocation is permitted in blocks that are marked`unsafe`.

## CLRs, BCLs, and Runtimes

![[Pasted image 20250827215041.png]]

Runtime support for C# programs consists of a Common Language Runtime and a Base Class Library.

### Common Language Runtime (CLR)

CLR - Provides essential runtime services such as automatic memory management and exception handling.
	`Common` - refers to the fact that the same runtime can be shared by other managed programming languages, such as F#, Visual Basic, and Managed C++.

C# is called a managed language because it compiles source code into managed code, which is represented in Intermediate Language (IL).
The CLR converts the IL into the native code of the machine, such as `X64` or `X86`, usually just prior to execution. This is referred to as Just-In-Time (JIT) compilation.
Ahead-of-time compilation is also available to improve startup time with large assemblies or resource-constrained devices.
Ahead-of-time compilation is also available to improve startup time with large assemblies or resource-constrained devices.


A program can query its own metadata (reflection).


### Base Class Library (BCL)

A CLR always ships with a set of assemblies called a Base Class Library (BCL).
BCL - provides core functionality to programmers, such as collections, I/O, text processing, XML/JSON handling, Networking, encryption, interop, concurrency, and parallel programming.

### Runtimes

A runtime (also called a framework) - is a deployable unit that you download and install.
	A runtime consists of a CLR (with its BCL), plus an optional application layer specific to the kind of application that you're writing.
	You don't need an application layer if you are writing a command-line console application.

![[Pasted image 20250827231707.png]]


### .NET 8

.NET 8 is Microsoft's flagship open-source runtime.
	You can write web and console apps that run on Windows, Linux, and macOS.
	Rich-client applications that run on Windows 10+ and macOS.
	And mobile apps that run on iOS and Android.



![[Pasted image 20250827231934.png]]


### Windows Desktop and WinUI 3
For writing rich-client applications that run on Windows 10 and above.
You can choose between the classic Windows Desktop API's (Windows Forms and WPF) and WinUI 3.
Windows Desktop APIs are part of the .NET Desktop runtime.
WinUI 3 is part of the Windows App SDK.

### MAUI
Multi-platform App UI - is designed primarily for creating mobile apps for iOS and Android.
MAUI is an evolution of Xamarin and allows a single project to target multiple platforms.

Avalonia is Linux version similar to MAUI.
### .NET Framework
.NET Framework - is Microsoft's original Windows-only runtime for writing web and rich-client applications.
	Microsoft will continue to support and maintain the current 4.8 release due to the wealth of existing applications.
	

.NET Framework is preinstalled with Windows and is automatically patched via Windows Update.
When targeting .NET Framework 4.8, you can use the features of C# 7.3 and earlier.

### Niche Runtimes

Unity is a game development platform that allows game logic to be scripted with C#.
.NET Micro Framework is for running .NET code on highly resource constrained embedded devices (under one megabyte)

Its also possible to run managed code within SQL Server.
With SQL Server CLR integration, you can write custom functions, stored procedures, and aggregations in C# and then call them from SQL.

## A Brief History of C#


### Whats new with C# 12

C# 12 ships with Visual Studio 2022, and is used when you target .NET 8.

#### Collection Expressions

From:
```C#
char[] vowels = {'a', 'e', 'i'};
```

To:
```C#
char vowels = ['a', 'e', 'i'];
```

You can now use square brackets allowing for the same syntax across other collection types, such as lists and sets.

```C#
List<char> list = ['a', 'e', 'i'];
HashSet<char> set = ['a', 'e', 'i'];
ReadOnlySpan<char> span = ['a', 'e', 'i'];
```

They are also target-typed, meaning you can omit the type in other scenarios where the compiler can infer it.

```C#
Foo(['a', 'e', 'i']);

void Foo (char[] letters) {...}
```


#### Primary constructors in classes and structs

From C# 12, you can include a parameter list directly after a class (or struct) declaration.

```C#
class Person (string firstName, string lastname)
{
	public void Print() => Console.WriteLine(firstName + " " + lastName);
}
```

This instructs the compiler to automatically build a primary constructor, allowing the following:
```C#
Person p = New Person ("Alice", "Jones")l
p.Print(); // Alice Jones
```

#### Default lambda parameters

Just as ordinary methods can define parameters with default values, so, too, can lambda expressions:
```C#
void Print(string message = "") => Console.WriteLine(message);

var print = (string message = "") => Console.WriteLine(message);

print("Hello");
print();
```

#### Alias any type

C# has always allowed you to alias a simple or generic type via the using directive:
```C#
using ListofInt = System.Collections.Generic.list<int>;

var list = new ListOfInt();
```

From C# 12, this approach works with other kinds of types, too, such as arrays and tuples:

```C#
using NumberList = double[];
using Point = (int X, int Y);

NumberList numbers = { 2.5, 3.5 };
Point p = (3, 4);
```


### What's New in C# 11

C# 11 is used by default when you target .NET 7.

#### Raw string literals

Wrapping a string in three or more quote characters creates a raw string literal, which can contain almost any character sequence without escaping or doubling up.

Makes it easy to represent JSON, XML, and HTML literals.

```C#
string raw = """<file path = "c:\temp\test.txt"></file>""";
```

Using two or more $ characters in a raw string literal prefix changes the interpolation sequence from one brace to two braces, allowing you to include braces in the string itself:

```C#
Console.WriteLine($$"""{ "TimeStamp": "{{DateTime.Now}}"}""");
// Output: {"TimeStamp": "01/01/2024 12:13:25 PM" }
```
#### UTF-8 strings

With u8 suffix, you create string literals encoded in UTF-8 rather than UTF-16.

```C#
ReadOnlySpan<byte> utf8 = "ab->cd"u8; // Arrow symbol consumes 3 bytes
Console.WriteLine(utf8.Length); // 7
```

Underlying type is `ReadOnlySpan<byte>`, which you can convert to a byte array by calling its `ToArray()` method.

#### List patterns 

List patterns match a series of elements in square brackets, and work with any collection type that is countable and indexable.

```C#
int[] numbers = {0, 1, 2, 3, 4};
Console.WriteLine(numbers is [0, 1, 2, 3, 4]); // True
```

An underscore matches a single element  of any value, and two dots match zero or more elements (a slice):
```C#
Console.WriteLine (numbers is [_, 1, .., 4]); // True
```

#### Required members

Applying the required modifier to a field or property forces consumers of that class or struct to populate that member via an object initialiser when constructing it:

```C#
Asset a1 = new Asset { Name = "House" }; // OK
Asset a2 = new Asset();  // Error: will not compile

class Asset { public required string Name; }
```


#### Static virtual/abstract interface members

C# 11, interfaces can declare members as static virtual or static abstract:

```C#
public interface IParsable<TSelf>
{
	static abstract TSelf Parse (string s);
}
```

These members are implementes as static functions in classes or structs, and can be called polymorphically via a constrained type parameter:
`
```C#
T ParseAny<T> (string s) where T : IPasable<T> => T.Parse (s);
```

#### Generic maths

`System.Numerics.INumber<TSelf>` interface unifies arithmetic operations across all numeric types, allowing generic methods such as the following to be written:

```C#
T Sum<T> (T[] numbers) where T : INumber<T>
{
	T total = T.Zero;
	foreach (T n in numbers)
		total += n; // Invokes addition operator for any numeric type
	return total;
}

int intSum = Sum(3, 5, 7)'
double doubleSum = Sum(3.2, 5.3, 7.1);
decimal decimalSum = Sum(3.2m, 5.3m, 7.1m);
```

# Important

The rest was skipped due to needing to know more of the basics before diving in the deep end.

#### File-scoped namespaces (C#10)

In the common case that all types in a file are defined in a single namespace, a file-scoped namespace declaration in C# 10 reduces clutter and eliminates an unnecessary level of indentation:

```C#
namespace MyNamespace; // Applies to everything that follows in the file.

class Class1 {}
class Class2 {}
```

#### Global using directive (C#10)

When you prefix a `using` directive with the `global` keyword, it applies the directive to all files in the project:

```C#
global using System;
global using System.Collection.Generic;
```

