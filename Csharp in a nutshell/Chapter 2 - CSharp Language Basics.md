
## A First C# Program

```C#
int x = 12 * 30;             // Statement 1
System.Console.WriteLine(x); // Statement 2
```

Our program consists of two statements.
Statements in C# execute sequentially and are terminated by a semicolon.
Types are organised into `namespaces`.


The `using` directive lets you avoid this clutter by importing a namespace:
```C#
using System;  // Import the System namespace

int x = 12 * 30;
Console.WriteLine(x); // No need to specify System.
```

Statement block - series of statements surrounded by a pair of braces.
A method can receive input data from the caller by specifying parameters and output data back to the caller by specifying a return type.

### Compilation

C# Compiler compiles source code into an assembly.
Assembly - is the unit of packaging and deployment in .NET.
	Can be either an application or a library.

A normal console or Windows application has an entry point, whereas a library does not.
Purpose of a library is to be called upon (referenced) by an application or by other libraries.

![[Pasted image 20250831034822.png]]

The `dotnet` tool helps you manage .NET source code and binaries from the command line.

Scaffolding new console project:
```Console
dotnet new Console -n MyFirstProgram
```

This creates a subfolder called `MyFirstProgram` containing a project file called `MyFirstProgram.csproj` and a C# file called Program.cs that prints "Hello world".

To Build and Run or to just build your dotnet program.
```Console
dotnet run MyFirstProgram
dotnet build MyfirstProgram.csproj
```

Output assembly will be written to a subdirectory under bin\debug

## Syntax

### Identifiers and Keywords

Identifiers are names that programmers choose for their classes, methods, variables, and so on.

By convention parameters, local variables, and private fields should be in camel case.
All other identifiers should be in Pascal case.

![[Pasted image 20250831035514.png]]

If you really want to use an identifier that clashes with a reserved keyword, you can do so by qualifying it with the @prefix
```C#
int using = 123; // Illegal
int @using = 123; // Legal
```
### Contextual keywords

![[Pasted image 20250831035639.png]]
### Literals, Punctuators, and Operators

Literals - are primitive pieces of data lexically embedded into the program.

Punctuators - help demarcate the structure of the program.


Period . - denotes a member of something.
Parentheses () - used when declaring or calling a method.
	Empty parentheses are used when the method accepts no arguments.
Equal Sign = - performs assignment.
	Double equals sign == perform equality comparison.
### Comments

C# Offers two different styles of source-code documentation:
	Single-line comments //
	Multi-line comments `/* */`

## Type Basics

`type` - defines the blueprint for a value.
variable - denotes a storage location that can contain different values over time.
constant - storage location that always represents the same value

All values in C# are instances of a type.

### Predefined Type Examples

Predefined types - are types that are specially supported by the compiler.

int – a 32-bit integer number.  
	Between –2<sup>31</sup> to 2<sup>31</sup> – 1

```C#
string message = "Hello world";
string upperMessage = message.ToUpper();
Console.WriteLine(upperMessage);    // HELLO WORLD

int x = 2022;
message = message + x.ToString();
Console.WriteLine(message); // Hello World2022
```

`x.ToString()` was used to obtain a string representation of the integer x.
You can call `ToString()` on a variable of almost any type.

`bool` - type has exactly two possible values: true and false.

### Custom Types

```C#
public class UnitConverter
{
	int ratio: // Field
	
	public UnitConverter (int unitRatio) // Constructor
	{
		ratio = unitRatio;
	}
	
	public int Convert (int unit) // Method
	{
		return unit * ratio;
	}
}
```

#### Members of a type

A type contains data members and function members.
Data member of `UnitConverter` is the field called ratio.
Function members of `UnitConverter` are the Convert and the `UnitConverter's` constructor.

#### Constructors and instantiation

Data is created by instantiating a type.
Predefined types can be instantiated simply by using a literal such as 12 or "Hello World".

We create and declared an instance of the `UnitConverter` type with this statement:
```C#
UnitConverter feetToInchesConverter = new UnitConverter(12);
```

Immediately after the new operator instantiates an object, the object's constructor is called to perform initialisation.
```C#
ppublic UnitConverter (int unitRatio) { ratio = unitRatio;}
```

public keyword exposes members to other classes.
In OO terms, we say that the public members `encapsulate` the private members of the class.


#### Defining a Main method

In the absence of top-level statements, C# looks for a static method called Main, which becomes the entry point.
Can be defined in any class.
Only one main method can exist.

Main method can also be declared `async` and return a `Task` or `Task<int>`

The CLR doesn't explicitly support top-level statements, the compiler translates your code into something like this:

From:
![[Pasted image 20250831180924.png]]

![[Pasted image 20250831180937.png]]

#### Types and Conversions

C# can convert between instances of compatible types.
Conversion always creates a new value from an existing one.
Conversions can be either `implicit` or `explicit`.
`implicit` conversions happen automatically.
`explicit` conversions require a cast.

```C#
int x = 12345;      // int is a 32-bit integer
long y = x;         // Implicit conversion to 64-bit integer
short z = (short)x; // Explicit conversion to 16-bit integer
```

If the compiler can determine that a conversion will `always` fail, both kinds of conversions are prohibited.

#### Value Types Versus Reference Types

ALL C# types fall into the following categories:
	- Value Types
	- Reference Types
	- Generic Type parameters
	- Pointer Types

Value types - comprise most built-in types
	Examples include:
		All numeric types
		Char type
		bool type
		Custom structs.
		Enum types.

Reference types - comprise all class, array, delegate, and interface types.
	Includes string type.

Fundamental difference between value types and reference types is how they are handled in memory.

##### Value Types

The content of a `value type` variable or constant is simply a value.
	Content of the built-in value type, `int`, is 32 bits of data.

You can define a custom value type with the `struct` keyword.

```C#
public struct Point { public int X; public int Y; }
public struct Point { public int X, Y; } // More terse
```

![[Pasted image 20250831223857.png]]

The assignment of a value-type instance always copies the instance; for example:

```C#
Point p1 = new Point();
p1.X = 7;

Point p2 = p1; // Assignment causes a copy

Console.WriteLine(p1.X); // 7
Console.WriteLine(p2.X); // 7

p1.X = 9; // Change p1.X

Console.WriteLine(p1.X); // 9
Console.WriteLine(p2.X); // 7
```

`p1` and `p2` have independent storage.

![[Pasted image 20250831223917.png]]

##### Reference Types

Reference type - holds two parts:
	An object
	And the reference to that object.

![[Pasted image 20250831224057.png]]

```C#
Point p1 = new Point();
p1.X = 7;

Point p2 = p1; // Copies p1 reference

Console.WriteLine(p1.X); // 7
Console.WriteLine(p2.X); // 7

p1.X = 9; // Change p1.X

Console.WriteLine(p1.X); // 9
COnsole.WriteLine(p2.X); // 9
```

![[Pasted image 20250831224413.png]]

##### Null

A reference can be assigned the literal `null`, indicating that the reference points to no object:
```C#
Point p = null;
Console.WriteLine( p == null ); // True

// The following line generates a runtime error
// (a NullReferenceException is thrown):
Console.WriteLine(p.X);

class Point {...}
```

```C#
Point p = null; // Compile-time error
int x = null; // Compile-time error

struct Point {...}
```


#### Storage overhead

Value-type instances occupy precisely the memory required to store their fields.
In this example, Point takes 8 bytes of memory:

```C#
struct Point
{
	int x; // 4 bytes
	int y; // 4 bytes
}
```

Technically, the CLR positions fields within the type at an address that's a multiple of the field's size, up to a maximum of 8 bytes. Thus, the following actually consumes 16 bytes of memory.

```C#
struct A { byte b; long l; } // 16 bytes
```

byte b --> 1 byte
long l --> 8 bytes (requires 8-byte alignment)

Because `long` must start at an 8-byte boundary, the compiler inserts 7 bytes of padding after `b`.
So layout in memory is the following:
```sql
Offset 0: b (1 byte)
offset 1-7: padding (7 bytes)
Offset 8-15: l (8 bytes)
```

--> Total size = 16 bytes

If you add `[StructLayout(LayoutKind.Sequential, Pack = 1)]`, padding is suppressed:
1 + 8 = 9 bytes

#### Predefined Type Taxonomy

Value types:
- Numeric
	- Signed Integer (`sbyte`, `short`, `int`, `long`)
	- Unsigned integer (`byte`, `ushort`, `uint`, `ulong`)
	- Real number (`float`, `double`, `decimal`)
- Logical (bool)
- Character (char)

Reference types:
- String (string)
- Object (object)

Underlying hexadecimal representation of primitive types:
```C#
int i = 7; //0.7
bool b = true; // 0x1
char c = 'A' // 0x41
float f = 0.5f; // uses IEEE floating-point encoding.
```
## Numeric types

![[Pasted image 20250901011409.png]]

Of the integral types, int and long are first-class citizens and are favoured by both C# and the run time.

Of the real number types, float and double are called floating-point types and are typically used for scientific and graphical calculations.
`decimal` type is typically used for financial calculations, for which base-10-accurate arithmetic and high precision are required.

### Numerical Literals

Integral-type literals use decimal or hexadecimal notation;
hexadecimal is denoted with the `0x` prefix.
```C#
int x = 127;
long y = 0x7F;

int million = 1_000_000;

var b = 0b1010_1011_1100_1101_1111;

double d = 1.5;
double million = 1E06;
```

### Numerical literal type inference

By default, the compiler `infers` a numeric literal to be either double or an integral type:
	If the literal contains a decimal point or the exponential symbol (E), it is a double.
	Otherwise, the literal's type is the first type in this list that can fit the literal's value: `int`, `uint`, `long`, and `ulong`.

```C#
Console.WriteLine(1.0.GetType()); // Double (double)
Console.WriteLine(1E06.GetType()); // Double (double)
Console.WriteLine(1.GetType()); // Int32 (int)
Console.WriteLine(0xF0000000.GetType()); // UInt32 (uint)
Console.WriteLine(0x100000000.GetType()); // Int64 (long)
```

#### Numeric Suffixes

Numeric suffixes explicitly define the type of a literal. Suffixes can be either lowercase or uppercase, and are as follows:

| Category | C# type | Example           |
| -------- | ------- | ----------------- |
| F        | float   | float f = 1.0F;   |
| D        | double  | double d = 1D;    |
| M        | decimal | decimal d = 1.0M; |
| U        | uint    | uint i = 1U;      |
| L        | long    | long i = 1L;      |
| UL       | ulong   | ulong i = 1UL;    |
The suffixes U and L are rarely necessary because the `uint`, `long`, and `ulong` types can nearly always be either inferred or implicitly converted from int:
```C#
long i = 5; // Implicit lossless conversion from int literal to long
```

The D suffix is technically redundant in that all literals with a decimal point are inferred to be double. And you can always add a decimal point to a numeric literal:
```C#
double x = 4.0;
```

The F and M suffixes are most useful and should always be applied when specifying `float` or `decimal` literals.

### Numeric Conversions

```C#
int x = 12345; // int is a 32-bit integer
long y = x; // Implicit conversion to 64-bit integral type
short z = (short)x; // Explicit conversion to 16-bit integral type
```

A `float` can be implicitly converted to a `double` given that a double can represent every possible value of a `float`.
The reverse conversion must be explicit.

All integral types can be implicitly converted to all floating-point types:
```C#
int i = 1;
float f = i;

// Reverse conversion must be explicit
int i2 = (int)f;
```

When you cast from a floating-point number to an integral type, any fractional portion is truncated; no rounding is performed.

implicitly converting a large integral type to a floating-point type preserves `magnitude` but can occasionally lose precision.

```C#
int i1 = 100000001;
float f = i1; // Magnitude preserved, precision lost
int i2 = (int)f;
```

### Arithmetic Operators

The arithmetic operators are defined for all numeric types except the 8-bit and 16-bit integral types:
- `+` Addition
- `-` Subtraction
- `*` Multiplication
- `/` Division
- `%` Remainder after division

Increment operator `++` - increases numeric value by 1.
Decrement operator `--` - decreases numeric value by 1.

The operator can either follow or precede the variable, depending on whether you want its value `before` or `after` the increment/decrement; for example:
```C#
int x = 0, y = 0;
Console.WriteLine(x++); // Outputs 0; x is now 1
Console.WriteLine(++y); // Outputs 1; y is not 1
```

Integral types are:
- int
- uint
- long
- ulong
- short
- ushort
- byte
- sbyte

#### Specialised Operations on Integral Types

Division operations on integral types always eliminates the remainder (round toward zero).
Dividing by a variable whose value is zero generates a runtime error (a `DivideByZeroExeption`):

```C#
int a = 2 / 3; // 0
int b = 0;
int c = 5 / b; // throws DivideByZeroException
```

Dividing by the literal or constant 0 generates a compile-time error.
##### Overflow

At runtime, arithmetic operations on integral types can overflow.
Meaning they can go from a min value to a max value and vice versa.

```C#
int a = int.MinValue;
a--;
Console.WriteLine(a == int.MaxValue); // True
```


`checked` operator instructs the runtime to generate an `OverflowException` rather than overflowing silently when an integral-type expression or statement exceeds the arithmetic limits of that type.
	`checked` operator has no effect on the `double` and `float` types.
		They both overflow to special infinite values.
	`decimal` type is always checked.

```C#
int a = 1000000;
int b = 1000000;

int c = checked (a * b); // Checks just the expression

checked
{
	...
	c = a * b;
	...
}
```

Overflow checking can be made by default at the project Level.
`unchecked` operator similar to `checked` makes sure that an `OverflowException` is not thrown.

Regardless of the "checked" project setting, expressions evaluated at compile time are always overflow-checked:
```C#
int x = int.MaxValue + 1; // Compile-time error
int y = unchecked (int.MaxValue + 1); // No errors
```

![[Pasted image 20250902073355.png]]

#### 8- and 16-Bit Integral Types
Include `byte`, `sbyte`, `short`, and `ushort`.
	These types lack their own arithmetic operators, so C# implicitly converts them to larger types as required.

```C#
// Implicit conversion to int
short x = 1. y = 1;
short z = x + y; // Compile-time error


// Explicit conversion to short
short z = (short) (x + y); // OK
```

#### Special Float and Double Values

![[Pasted image 20250902074421.png]]

## Boolean Type and Operators






## Strings and Characters

## Arrays

## Variables and Parameters

## Expressions and Operators

## Null Operators

## Statements

## Namespaces

