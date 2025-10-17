

## Fundamental Data Types

Variables - a way to store data in a program.
Data Types - classifications of data in programming that define the type of value a variable can hold.

Integers:
- Used to store whole numbers
- Both positive and negative
- Do not include fractions or decimals
- Are used for counting and arithmetic operations

Double:
- Floating-point number
- Used for precise calculations involving precision

Strings:
- Sequences of characters used to represent text
- Include letters, numbers, symbols, and spaces
- Numbers as strings cannot be used for calculations

Boolean:
- True or False statement


Type-safe Languages - are programming languages that enforce strict data type rules.

Non-type-safe Languages - are programming languages that are more flexible about data type rules than type-safe languages.

Function (Programming) - blocks of code designed to take input, process it, and return an output.

Type (C#) - is used to hold the type of a variable.

Data Type Conversion - the process of changing data stored in a variable from one data type to another.

Implicit Conversion - the ability to convert data types automatically without specific instructions from the programmer. 

Explicit Conversion - the process of manually converting a value from one data type to another.
### Identifying data types

```C#
string myVariable = "This fruti is an apple";
Type dataType = myVariable.GetType();
Console.WriteLine(dataType);

Result
System.String
```

### Checking data type compatibility

```C#
object myVariable = 123;

if (myVariable.GetType() == typeof(int))
{
	Console.WriteLine($"The variable {myVariable} is an integer.");
}
else
{
	Console.WriteLine($"The variable {myVariable} is not an integer.");
}
```

### Converting data types

`int.Parse()` - tries to convert the object into an int.
	Returns and error on failure.

```C#
string myVariable = "123";
int number = int.Parse(myVariable);
Console.WriteLine(number);
```

### Validating conversion

`int.TryParse()` - attempts to check if the variable can be converted successfully, returns boolean value.

```C#
string myVariable = "123";
if (int.TryParse(myVariable, out int number))
{
	Console.WriteLine($"Conversion successful: {number}");
} else {
	Console.WriteLine("Conversion failed");
}
```


### Explicit  Conversion Methods

Casting - method of converting a variable from one type to another type, explicitly stating the desired type.

```C#
double piVar = 3.14;
int piInt = (int)piVar;
Console.WriteLine(piInt);
```

Parsing - interprets a string representation of data and converts it to another data type


## Variables

Variables - named storage locations in memory used for storing data that allow programmers to store, retrieve, and manipulate data during a program's execution.


Syntax - the set of rules that defines the structure and arrangement of elements in a programming language.

Array - a list of values used to represent a list of items.


```C#
int[] numbers = {1, 2, 3, 4, 5};
```

Variable Declaration Keywords - define how variables, behave particularly in terms of mutability and immutability

Mutability - the ability of a variable to be changed or modified after it has been created.

Immutability - the characteristic of a variable that prevents it from being changed or modified after it has been created.


`const` and `readonly` keywords make variables immutable in C#

`const` - can assign objects or arrays but cannot be reassigned

`readonly` - assigned at declaration or in a constructor.

`let` and `const` keywords make variable immutable in JavaScript

Attempting a reassign will cause a compile time error.



