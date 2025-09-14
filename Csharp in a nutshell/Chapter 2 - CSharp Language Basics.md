
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

When using `==`, a `NaN` value is never equal to another value, even another `NaN` value:
```C#
Console.WriteLine(0.0 / 0.0 == double.NaN); // False
```

To test whether a value is `NaN`, you must use the `float.IsNaN` or `double.IsNaN` method:
```C#
Console.WriteLine(double.IsNaN( 0.0 / 0.0 )); // True
```

### double Versus decimal

`double` is useful for scientific computations.
`decimal` is useful for financial computations and values that are manufactured rather than the result of real-world measurements.

![[Pasted image 20250902174224.png]]

### Real Number Rounding Errors

`float` and `double` internally represent numbers in base 2.
Only numbers expressible in base 2 are represented precisely.
	Most literals with a fractional component will not be represented precisely. Due to them being base 10.

`decimal` works in base 10, and so can precisely represent numbers expressible in base 2, 5 and 10. 
Because literals are in base 10, `decimal` can precisely represent numbers such as 0.1.

NEITHER `double` nor `decimal` can precisely represent a fractional number whose base 10 representation is recurring:
```C#
decimal m = 1M / 6M;   // 0.16666666666666666666666666667M
double d = 1.0 / 6.0;  // 0.1666666666666666
```

This leads to accumulated rounding errors:
```C#
decimal notQuiteWholeM = m+m+m+m+m+m; // 1.0000000000000000000000002M
double notQuiteWholeD = d+d+d+d+d+d;  // 0.99999999999999999999989

Console.WriteLine(notQuiteWholeM == 1M); // False
Console.WriteLine(notQuiteWholeD < 1.0); // True
```

## Boolean Type and Operators

`System.Boolean` type - is a logical value that can be assigned the literal `true` or `false`.

Boolean only requires one bit of storage.
Runtime uses 1 byte of memory because this is the minimum chunk that the runtime and processor can efficiently work with.

To avoid space inefficiency in the case of arrays, .NET provides a `BitArray` class in the `System.Collections` namespace.
Designed to use just one bit per Boolean value


#### bool Conversions

No casting conversions can be made from the `bool` type to numeric types, or vice versa.


== and != test for equality and inequality of any type but always return a bool value.

For reference types, equality, by default, is based on `reference`, as opposed to the actual `value` of the underlying object.

```C#
Dude d1 = new Dude ("John");
Dude d2 = new Dude ("John");
Console.WriteLine(d1 == d2); // False
Dude d3 = d1;
Console.WriteLine(d1 == d3); // True

public class Dude
{
	public string Name;
	public Dude (string n) { Name = n; }
}
```

`==`, `!=`, `<`, `>`, `>=`, and `<=` work for all numeric types.

### Conditional Operators

`!` - not operator
`&&` - and conditional.
`||` - or conditional.

`&&` and `||` operators short-circuit evaluation when possible.
`&` and `|` operators are bitwise operators and do not short circuit.

### Ternary Operator

Has the form q ? a : b - thus if condition q is true, a is evaluated; otherwise b is evaluated.

```C#
static int Max (int a, int b)
{
	return (a > b) ? a : b;
}
```


## Strings and Characters

C#'s char type represents a Unicode character and occupies 2 bytes (UTF-16). A Char literal is specified within single quotes:
```C#
char c = 'A'; // Simple character
```

Escape character - is a backslash followed by a character with a special meaning.
Escape Sequences express characters that cannot be expressed or interpreted literally.

![[Pasted image 20250907134342.png]]

![[Pasted image 20250907134348.png]]

The `\u` or `\x` escape sequence lets you specify any Unicode character via its four-digit hexadecimal code.

```C#
char copyrightSymbol = '\u00A9';
char omegaSymbol = '\u03A9';
char newLine = '\u000A';
```

#### Char Conversions

An implicit conversion from a char to a numeric type works for the numeric types that can accommodate an unsigned short.
For other numeric types, and explicit conversion is required.


### String Type

C#'s string type represents an immutable sequence of Unicode characters.
A string literal is specified within double quotes:
```C#
string a = "Heat";
```

Escape sequences that are valid for char literals also work inside strings:
```C#
string a = "Here's a tab:\t";
```

The cost of this is that whenever you need a literal backslash, you must write it twice:
```C#
string a1 = "\\\\server\\fileshare\\helloworld.cs";
```

@ - verbatim string literal - does not support escape sequences.
```C#
string a2 = @"\\server\fileshare\helloworld.cs";
```

```C#
string escaped = "First Line\r\nSecond Line";
string verbatim = @"First Line
Second Line";

// True if your text editor uses CR-LF line separators:
Console.WriteLine (escaped == verbatim);
```

#### Raw String Literals (C# 11)
Wrapping a string in three or more quote characters (""") creates a `raw string literal`.
Raw string literals can contain almost any character sequence, without escaping or doubling up:
```C#
string raw = """<file path="c:\temp\test.txt"></file>""";
```

Compiler will generate an error if each line in a multiline raw string literal is not prefixed with the common indentation specified by the closing quotes.

#### String Concatenation
The + operator concatenates two strings.
If one of the values is a non-string value, `ToString` is called on that value.

```C#
string s = "a" + "b";
string sn = "a" + 5; // a5
```
#### String Interpolation
A string preceded with the `$` character is called an `interpolated string`. Interpolated strings can include expressions enclosed in braces:
```C#
int x = 4;
Console.Write($"A square has {x} sides"); // Prints: A square has 4 sides.
```

Any valid C# expression of any type can appear within the braces. C# will convert the object to `ToString` automatically.

If you need to use a colon for other purpose than formatting:
```C#
bool b = true;
Console.WriteLine($"The Answer in binary is {(b ? 1 : 0)}");
```
#### String comparisons
`==` operator is supported
`>` and `<`  - are not supported for strings
Use `CompareTo` method instead.
#### UTF-8 Strings
From C# 11, you can use the u8 suffix to create string literals encoded in UTF-8 rather than UTF-16. 
```C#
ReadOnlySpan<byte> utf8 = "ab->cd"u8; // Arrow symbol consumes 3 bytes
Console.WriteLine(utf8.Lenght); // 7
```

## Arrays

Array - represents a fixed number of variables (called elements) of a particular type.
	Elements are always stored in a contiguous block of memory, providing highly efficient access.

```C#
char[] vowels = new char[5]; // Declare an array of 5 characters
```

Square brackets also `index` the array, accessing a particular element by position:
```C#
vowels[0] = 'a';
vowels[1] = 'e';
vowels[2] = 'i';
Console.WriteLine(vowels[1]); // e
```

Array indexes start at 0

```C#
for (int i = 0; i < vowels.Length; i++)
	Console.Write(vowels[i]); // aei
```

`Length` property of an array returns the number of elements in the array.
After an array has been created, you cannot change its length.

An array initialisation expression lets you declare and populate an array in a single step:
```C#
char[] vowels = new char[] {'a', 'e', 'i', 'o', 'u'};
// OR
char[] vowels = {'a', 'e', 'i', 'o', 'u'};
```

#### Collection Expression
From C# 12, you can use square brackets instead of curly braces:
```C#
char[] vowels = ['a','e','i','o','u'];

void Foo (char[] letters) { ... }
```

### Default Element Initialisation
Creating an array always preinitialises the elements with default values.
The default value for a type is the result of a bitwise zeroing of memory.

```C#
int[] a = new int[1000];
Console.Write(a[123]); // 0
```

#### Value types versus reference types
When an element is a value type, each element value is allocated as part of the array, as shown here:
```C#
Point[] a = new Point[1000];
int x = a[500].X; // 0

public struct Point { public int X, Y; }
```

Had the `Point` been a class, creating the array would have merely allocated 1,000 null references:
```C#
Point[] a = new Point[1000];
int x = a[500].X; // Runtime error, NullReferenceException

public class Point ( public int X, Y;)
```

To avoid this error, we must explicitly instantiate 1,000 Points after instantiating the array:
```C#
Point[] a = new Point[1000];
for (int i= 0; i < a.Length; i++) // Iterate i from 0 to 999
	a[i] = new Point();           // Set array element i with new point.
```

An array is always a reference type object, regardless of the element type.
```C#
int[] a = null;
```
### Indices and Ranges

#### Indices
Indices let you refer to elements relative to the end of an array, with the `^` operator.
`^1` refers to the last element, `^2` refers to the second-to-last element, and so on:
```C#
char[] vowels = new char[] {'a', 'e', 'i', 'o', 'u'};
char lastElement = vowels[^1];  // 'u'
char secondToLast = vowels[^2]; // 'o'
```

^0 equals the length of the array, so `vowels[^0]` generates an error.

C# implements indices with the help of the `Index` type, so you can also do the following:
```C#
Index first = 0;
Index last = ^1;
char firstElement = vowels[first];  // 'a'
char lastElement = vowels[last];    // 'u'
```
#### Ranges
Ranges let you slice an array by using the .. operator:
```C#
char[] firstTwo = vowels[..2]; // 'a', 'e'
char[] lastThree = vowerls[2..]; // 'i', 'o', 'u'
char[] middleOne = vowels[2..3]; // 'i'
```

### Multidimensional Arrays
Multidimensional arrays come in two varieties: rectangular and jagged.
Rectangular arrays represent an n-dimensional block of memory, and jagged arrays are arrays of arrays.
#### Rectangular arrays
Rectangular arrays are declared using commas to separate each dimension.
```C#
int[,] matrix = new int[3, 3];
```

The `GetLength` method of an array returns the length for a given dimension
```C#
for (int i = 0; i < matrix.GetLength(0); i++)
	for (int j = 0; j < matrix.GetLength(1); j++)
		matrix[i, j] = i * 3 + j;

/// Identical to

int[,] matrix = new int[,]
{
	{0, 1, 2},
	{3, 4, 5},
	{6, 7, 8}
};
```

#### Jagged arrays
Jagged arrays are declared using successive square brackets to represent each dimension.

Here is an example of declaring a jagged two-dimensional array for which the outermost dimension is 3:
```C#
int[][] matrix = new int[3][];
```

Inner dimensions are not in the declaration because they can be an arbitrary length.
Each inner array is implicitly initialised to null rather than an empty array.
You must manually create each inner array:
```C#
int[][] matrix new int[][]
{
	new int[] {0, 1, 2},
	new int[] {3, 4, 5},
	new int[] {6, 7, 8, 9}
};
```
### Simplified Array initialisation Expressions
There are two ways to shorten array initialisation expressions.
The first is to omit the `new` operator and type qualifications.
```C#
char[] vowels = {'a', 'e', 'i', 'o', 'u'};

int[,] rectangularMatrix = 
{
	{0, 1, 2},
	{3, 4, 5},
	{6, 7, 8}
};

int[][] jaggedMatrix =
{
	new int[] {0, 1, 2},
	new int[] {3, 4, 5},
	new int[] {6, 7, 8, 9}
};
```

The second approach is to use the `var` keyword, which instructs the compiler to implicitly type a local variable.
```C#
var i = 3;         // i is implicitly of type int
var s = "sausage"; // s is implicitly of type string
```

```C#
var vowels = new[] {'a', 'e', 'i', 'o', 'u'}; // Compiler infers char[]
```

```C#
var rectMatrix = new [,]     // rectMatrix is implicitly of type int[,]
{
	{0, 1, 2},
	{3, 4, 5},
	{6, 7, 8}
};
var jaggedMat = new int[][]  // jaggedMat is implicitly of type int[][]
{
	new[] {0, 1, 2},
	new[] {3, 4, 5},
	new[] {6, 7, 8, 9}
};
```

For this to work, the elements must all be implicitly convertible to a single type:
```C#
var x = new[] {1, 1000000000}; // all convertible to long.
```

### Bounds Checking

All array indexing is bounds checked by the runtime. An `IndexOutOfRangeException` is thrown if you use an invalid index:
```C#
int[] arr = new int[3];
arr[3] = 1;
```

## Variables and Parameters

### The Stack and the Heap
The stack and heap are the places where variables reside. Each has very different lifetime semantics.
#### Stack
Stack - is a block of memory for storing local variables and parameters.
	Logically grows and shrinks as a method or function is entered and exited.

```C#
static int Factorial (int x)
{
	if (x == 0) return 1;
	return x * Factorial (x - 1);
}
```

This method is recursive, meaning that it calls itself.
Each time the method is entered, a new `int` is allocated on the stack, and each time the method exits, the `int` is deallocated.
#### Heap
Heap - is the memory in which `Objects` reside.
Whenever a new object is created, it is allocated on the heap, and a reference to that object is returned.
During program's execution the heap begins filling up as new objects are created. 
The runtime has a garbage collector that periodically deallocates objects from the heap, so your program does not run out of memory.
An object is eligible for deallocation as soon as it's not referenced by anything that's itself "alive"

```C#
using System;
using System.Text;

StringBuilder ref1 = new StringBuilder("object1");
Console.WriteLine(ref1);
// The StringBuilder referenced by ref1 is now eligible for GC.

StringBuilder ref2 = new StringBuilder("object2");
StringBuilder ref3 = ref2;
// The StringBuilder referenced by ref2 is NOT yet eligible for GC.

Console.WriteLine(ref3); // object2
```

Value-type instances (and object references) live wherever the variable was declared.
If the instance was declared as a field within a class type, or as an array element, that instance lives on the heap.

The heap also stores static fields.
Unlike objects allocated on the heap, these live until the process ends.

### Definite Assignment
C# enforces a definite assignment policy.
It means that outside of an `unsafe` or interop context, you can't accidentally access uninitialised memory.

Definite assignment has three implications:
- Local variables must be assigned a value before they can be read.
- Function arguments must be supplied when a method is called. (Unless optional)
- All other variables (such as fields and array elements) are automatically initialised by the runtime.

### Default Values

### Parameters

#### Passing arguments by value

#### The ref modifier

#### The out modifier

#### Out variables and discard

#### Implications of passing by reference

#### The in modifier

#### The params modifier

#### Optional parameters

#### Named arguments

### Ref Locals

### Ref Returns

### var--Implicitly Typed Local Variables

### Target-Typed new Expressions
## Expressions and Operators

## Null Operators

## Statements

## Namespaces

