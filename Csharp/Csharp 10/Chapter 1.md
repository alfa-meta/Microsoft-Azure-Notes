
New feature of .NET 10 is to support executing a C# code file without .csproj project file.


## History of .NET

C# and .NET are closely related technologies.
C# is a programming language that compiles to Common Intermediate Language (CIL)

IL code can then be loaded by the Common Language Runtime (CLR) that is part of the .NET Runtime and Just In Time (JIT) compiled to native CPU instructions.

Visual Basic .NET and F# can also be compiled to IL code.

![[Pasted image 20251123030454.png]]


In theory C# someone could create a C# compiler that builds projects without .NET however no one has done this.


### .NET Framework

.NET Framework is a development platform that includes a Common Language Runtime (CLR), which manages the execution of code, and a Base Class Library (BCL), which provides a rich library of classes to build applications from.

For .NET Framework 4.0 or later all apps on a computer written for .NET Framework share the same version of the CLR and libraries in the Global Assembly Cache (GAC).


Patch Tuesday, every second Tuesday of the month.


## .NET Support

Long-Term Support - 3 years or 1 year after the next LTS release ships, whichever is longer.
Standard-Term Support - 2 Years after General Availability, or 1 year after the next STS or LTS release ships, whichever is longer.
Preview - releases for public testing for bleeding edge users.

Patch Tuesday - second Tuesday of each month.

.NET updates are released every month.

### .NET Support Phases

- Preview - Not supported at all. .NET 10 Preview 1 to 7 were supported from February 2025 to August 2025.
- Go Live - These are supported until General Availability. Upgrade ASAP when GA is released.
- Active - .NET 10 will be in support from November 2025 to May 2028.
- Maintenance - Supported only with security fixes for the last six months of its lifetime. .NET 10 will be in this support phase from May 2028 to November 2028.
- End of Life (EOL) - Software version no longer supported. .NET 10 will reach its EOL in November 2028.


## Listing and removing versions of .NET

All future versions of .NET SDK maintain the ability to build projects that target previous versions.

Lists all SDKs
```cmd
dotnet --list-sdks
```

![[Pasted image 20251126033547.png]]

![[Pasted image 20251126033626.png]]

## .NET Compiler and Runtime

Runtime - is the environment where a program actually executes.

C# compiles to Common Intermediate Language (CIL)
Intermediate Language (IL) code can be loaded by the Common Language Runtime (CLR)
And Just In Time (JIT) Compiled to native CPU instructions.

C# compiler is called Roslyn.

Dotnet CLI converts C# source code into IL, it stores the IL in an assembly (a DLL or EXE file)
IL is used by .NET's Virtual Machine called CoreCLR.
CoreCLR loads the IL code from the assembly.
JIT compiler compiles it into native CPU instructions.
Executes the CPU instructions on the machine.


.NET Framework is a Windows only CLR
Modern .NET has CLR for all Operating Systems.


## Building Applications

Solution - is a project management file

Attaching a debugger slows the project down.
Attaching a debugger only allows you to run one project.

Compiling a project created `obj` and bin directories

`obj` - contains one compiled object file for each source code file (this hasn't been linked).
`bin` - contains binary executable for the application or class library

dotnet clean - command deletes generated files.

This generates a console app without top level statement.
```pwsh
dotnet new console --user-program-main
```

### Requirements for top-level programs
- Only one file for top-level program
- Any `using` statements must be at the top of the file
- Classes and other type declaration must be at the bottom of the file.
- If Main method is explicitly defined then the entry point will become `<Main>$`


### Implicitly imported namespaces

Global namespace imports:
- A feature that imports some commonly used namespaces like System for use in all code files.
- Introduced in C# 10 and .NET 6

```C#
string name = typeof(Program).Namespace ?? "<null>";
Console.WriteLine($"Namespace: {name}");
```

`??` - is the null-coalescing operator.
	First statement means "if the namespace of Program is `null`, then return `<null>`; otherwise, return the actual name"


If you write `throw new Exception();` in `Program.cs`, you will be able to see `<main>$` method.


When Visual Studio runs a console app, it executes it from the `<projectname>\bin\Debug\net10.0` folder.


C# 14 adds file-based apps. allowing developers to execute single.cs files directly

```pwsh
dotnet run app.cs
```


```C#
#: package Humanizer@2.14.1 // NuGet Package
#:project ../MyClassLib/MyClassLib.csproj // Project reference
#:property LangVersion=preview // MS Build property, setting C# language to preview.

// On Linux
#!/usr/bin/dotnet run
```

`#` - at the top of a .cs file, allows for enhancing the functionality of your .cs file.


```C#
dotnet project convert app.cs
```

```C#
dotnet publish app.cs
```

By default, app is published as a native-compiled AOT app.