
Class - a reference type that defines a blueprint for objects.
	Stored on the heap.
	Supports inheritance and polymorphism.

Struct - a value type that defines a small, lightweight object.
	Usually stored on the stack.
	No inheritance.
	Can implement Interfaces.

Record - reference type that provides built-in functionality for encapsulating data.
	Usually immutable.

Example:
```C#
public record Person(string Name, int Age);

var p1 = new Person("Alice", 30);
var p2 = new Person("Alice", 30);

Console.WriteLine(p1 == p2); // True (compares values)
```


## Object-Oriented Programming

Four basic principles of Object-oriented programming are:
	Abstraction - modelling the relevant attributes and interactions of entities as classes to define an abstract representation of a system.
	Encapsulation - hiding the internal state and functionality of an object and only allowing access through a public set of functions.
	Inheritance - ability to create new abstractions based on existing abstractions.
	Polymorphism - ability to implement inherited properties or methods in different ways across multiple abstractions.