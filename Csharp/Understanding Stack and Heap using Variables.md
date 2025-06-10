
## Variable

Variable - is a name in source code associated (bound) to a block of memory that holds a value.

In C# variables fall into specific types: value types and reference types.

By default, on assignment, passing an argument to a method, and returning a method result, variable values are copied.

Changes to structs out of scope would make no changes as shown with p2 in MutateAndDisplay below.

```C#
using System;

public struct MutablePoint
{
    public int X;
    public int Y;

    public MutablePoint(int x, int y) => (X, Y) = (x, y);

    public override string ToString() => $"({X}, {Y})";
}

public class Program
{
    public static void Main()
    {
        var p1 = new MutablePoint(1, 2);
        var p2 = p1;
        p2.Y = 200;
        Console.WriteLine($"{nameof(p1)} after {nameof(p2)} is modified: {p1}");
        Console.WriteLine($"{nameof(p2)}: {p2}");

        MutateAndDisplay(p2);
        Console.WriteLine($"{nameof(p2)} after passing to a method: {p2}");
    }

    private static void MutateAndDisplay(MutablePoint p)
    {
        p.X = 100;
        Console.WriteLine($"Point mutated in a method: {p}");
    }
}
// Expected output:
// p1 after p2 is modified: (1, 2)
// p2: (1, 200)
// Point mutated in a method: (100, 200)
// p2 after passing to a method: (1, 200)
```
### Value Type

Value type - is a variable that holds the value to which it is associated.
	Directly contains the data in memory instead of holding a reference to it.


Microsoft Docs explanation:
	Value type - is a variable that contains the instance of the type.

Example:
```C#
int x = 7;
```

Simple Types are value types such as:
	bool
	byte
	sbyte
	short
	ushort
	int 
	uint
	long
	ulong
	float
	double
	decimal
	char

Struct Types such as :
	Any user-defined struct
	Predefined Structs:
```C#
System.DateTime
System.TimeSpan
System.Guid
System.Nullable<T>
```

Enumeration Types (enums) - based on integral types like byte, int, etc.

void - technically a value type, used as a return type.

A const value has to be of Value type.
### Reference Type

Reference type - is a reference (or null), meaning the value of a reference type is the memory address of the object to which it refers.

Reference types store references to their data (objects).

Example:
```C#
Car car = new Car("Toyota");
```

The following keywords are used to declare reference types:
	class
	interface
	delegate
	record

C# also provides the following built-in reference types.

A const value cannot be of reference type.
## Stack

Call Stack - keeps track of the method that control should return to once the currently executing method has finished. And holds the values of local variables.
	If a variable is inside a method, they are pushed onto the stack.

Very fast memory type (just moves the stack pointer)
Access time is fast contiguous memory, is cache-friendly.
Overhead is minimal.
Usually managed by CPU

When a method stops executing all of the stack data is deleted.

Data Structure Stack - last in, first out.
## Heap

Heap Data Structure - tree based data structure that satisfies the following principle:
	Max Heap: parent >= child
	Min Heap: parent <= child

Heap Memory - purpose of the heap is to store data that needs to outlive specific methods. 
	Used to store reference type variables, or objects.


Garbage Collector (GC) - is a program that manages the objects on the heap, it allocates objects and reclaims objects that are no longer being used.

## So where do C# variables get stored?

Local variables - stack
Primitives (int, float, bool, char) - stack
Enumerations - stack
Struct - stack
Objects of reference type - heap
Instance variables that are part of a reference type instance (e.g. a field on a class) - heap
Instance variables such as structs - heap


Example:
```C#
public class Car
{
    public string manufacturer;
    public int price;

    public Car(string manufacturer, int price)
    {
        this.manufacturer = manufacturer;
        this.price = price;
    }
}
```