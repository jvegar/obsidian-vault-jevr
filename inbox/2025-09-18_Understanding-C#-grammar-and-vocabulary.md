---
date: 2025-09-18
tags:
  -
hubs:
  - "[[]]"
urls:
  -
---
# 2025-09-18_Understanding-C#-grammar-and-vocabulary

## Understanding grammar
- The grammar includes statements and blocks. To document your code you can use comments.

## Statements
- Indicates the end of a statement with a semicolon.
- Can be composed of multiple types, variables and expressions made up of tokens.
- Each token is separated by white spaces or some other recognizably different token.

```cs
decimal totalPrice = subtotal + salesTax;
```

## Comments
- They are the primary method of documenting your code to enhance the Understanding of how it works.
- You can add comments to explain your code using a double slash `//`. 

```cs
// Sales taax must be added to the subtotal.
decimal totalPrice = subtotal + salesTax;
```

- To write multiline comment, use `/*` at the beginning and `*/` at the end of the comment.

```cs
/*
This is a
multi-line comment.
*/
```

- Can be also used for commenting in the middle of a statement

```cs
decimal totalPrice = subtotal /* for this item */ + salesTax;
```

- Try to make your code self-documenting. Avoid using too many comments.

## Blocks
- In C# we indicate a __block__ of code with the use of curly brackets `{ }`.
- Important concepts:
  - A *namespace* contains types like classes to group them together.
  - A *class* contains the members of an object, including methods.
  - A *method* contains statements that implement an action that an object can take.

## Regions
- Used to define your own labeled regions around any statement you want.
- Most editors will allow you to collapse and expand in the same way as blocks.

```cs
#region Three variables that store the number 2 million.
int decimalNotation = 2_000_000;
int binaryNotation = 0b_0001_1110_1000_0100_1000_0000;
int hexadecimalNotation = 0x_001E_8480;
```

In this way, regions can be treated ad commented blocks that can be collapsed.

## Examples of statements and blocks

```cs
using System; // A semicolon indicates the end of a statement.
namespace Basics
{ // An open brace indicates the start of a block.
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World"); // A statement.
        }
    }
} // A close brace indicates the end of a block.
```

Note that C# uses a brace style where both the open and close braces are on their own line and are at the same indentation level.
```cs
if (x < 3)
{
    // Do something if x is less than 3.
}
```
Other languages like JavaScript open curly brace at the end of the declaration statement.
```js
if (x < 3) {
    // Do something if x is less than 3.
}
```

## Formatting code using white space
It includes the space, tab, and newline characters.
```cs 
int sum = 1 + 2; // Most developers would prefer this format.
int
sum=1+
2; // One statement over three lines.
int         sum=    1   +2;int sum=1+2; // Two statements on one line.
```

## Understanding C# vocabulary
The C# vocabulary is made up of *keywords*, *symbols* *characters*, and *types*.
Some of the predefined, reserverd keywords include: `using`, `namespace`, `class`, `static`, `int`, `string`, `double`, `bool`, `if`, `switch`, `break`, `while`, `do`, `for`, `foreach`, `this`, and `true`.
Some of the symbols characters that you will see are `",` `',` `+,` `*,` `/,` `%,` `@,` and `$`.

### Types of brackets
- `()` are called parentheses. They are used to call a function, define an expression or condition, and cast between types.
- `{}` are called braces (curly brackets). They are used to define blocks and perform object and collection initialization.
- `[]` are called brackets (square brackets). They are used to access items in an array or collection, and around attributes decorating elements of code.
- `<>` are called angle brackets. They are used for generic types, in XML and HTML files, and individually as less than or greater than tokes in an expression.

## Importing namespaces
`System` is a namespace, which is like an address for a type. To refer to someone's location exactly, you might use `Oxford.HighStreet.BobSmith`, which tell us to look for a preson named Bob Smith on the High Stret in the city of Oxford.
`System.Console.WriteLine` tells the compiler to look for a method named `WriteLine` in a type named `Console` in a namespace named `System`.
To simplify our code, the `Console App` project template for every version of .NET before 6.0 added a statement at the top of the code file to tell the compiler to always look in the `System` namespace for types that haven't been prefixed with their namespace, as shown in the following code:

```cs
using System; // Import the System namespace.
```

## Implicitly and globally importing namespaces
Traditionally, every `.cs` file that needs to import namespaces would have to start with `using` statement to import those namespaces.
Namespaces like `System` and `System.Linq` are needed in almost all `.cs` files, so the first few lines of every `.cs` file often had at least a few using statements, as shown in the following code:

```cs
using System;
using System.Linq;
using System.Collections.Generic;
```

When creating websites and services using ASP.NET Core, there are often dozens of namespaces that each file would have to import.

### Global Importing
The `global using` keyword combination means you only need to import a namespace in one `.cs` file and it will be available throughout all `.cs` files instead of having to import the namespace at the top of every file that needs it.
The recommendation is to create a separate file for those statements like `GlobalUsings.cs`

```cs
global using System;
global using System.Linq;
global using System.Collections.Generic;
```

The list of implicitly imported namespaces depends onf which SDK you target

|SDK                      |Implicitly imported namespaces           |
|-------------------------|-----------------------------------------|
|Microsoft.NET.Sdk        |System                                   |
|                         |System.Collections.Generic               |
|                         |System.IO                                |
|                         |System.Linq                              |
|                         |System.Net.Http                          |
|                         |System.Threading                         |
|                         |System.Threading.Tasks                   |
|-------------------------|-----------------------------------------|
|Microsoft.NET.Sdk.Web    |Same a `Microsoft.NET.Sdk`, plus:          |
|                         |System.Net.Http.Json                     |
|                         |Microsoft.AspNetCore.Builder             |
|                         |Microsoft.AspNetCore.Hosting             |
|                         |Microsoft.AspNetCore.Http                |
|                         |Microsoft.AspNetCore.Routing             |
|                         |Microsoft.Extensions.Configuration       |
|                         |Microsoft.Extensions.DependencyIjection  |
|                         |Microsoft.Extensions.Hosting             |
|                         |Microsoft.Extensions.Logging             |
|-------------------------|-----------------------------------------|
|Microsoft.NET.Sdk.Worker |Same a `Microsoft.NET.Sdk`, plus:          |
|                         |Microsoft.Extensions.Configuration       |
|                         |Microsoft.Extensions.DependencyIjection  |
|                         |Microsoft.Extensions.Hosting             |
|                         |Microsoft.Extensions.Logging             |
|-------------------------|-----------------------------------------|


