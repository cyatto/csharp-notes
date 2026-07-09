# TT300 Source Files, Output, Variables and Expressions

# 1. Timekeeping

🕐 ETA: 10 hours

📆 2026.06.23 - 

|block|section|begin|end|total|
| -------| ------------------| -------| -----| -------|
|1|3.1 Source Files||||
|2|||||
|3|||||

---

# 2. [Topics](https://tafe-tthom.github.io/programming1cs-book/03basics.html#topics)

The chapter will go into detail with the following.

- Source Files
- Variables and Values
- Datatypes and Operators
- Output and Formatting
- Execution

‍

---

# 3. 1 Source Files

- Source file: typically a text file that contains **source code** (saved using the `.cs` extension)
- What to do with source files? [📒](https://tafe-tthom.github.io/programming1cs-book/021source.html#are-source-files-the-program-)

  - In this current context: write code, compile and then execute it using `dotnet` (or you can run them directly once compiled).
- The tooling in the C# ecosystem:

  - Retarget the platform; make binaries for Mac, Linux and Windows
  - Build cross-platform applications using AvaloniaUI with XML/XAML
  - Much more...

‍

*Are source files the program?*  [📒](https://tafe-tthom.github.io/programming1cs-book/021source.html#are-source-files-the-program-)

- C# and Java are compiled languages.
- The compiler assists with catching issues a programmer may have unintentionally left in
- The C# compilation process will typically emit a native executable when compared to Java, but they will still contain bytecode that will need to be interpreted.

‍

## 3.1 Walkthrough [💻](https://tafe-tthom.github.io/programming1cs-book/021source.html#walkthrough-)

```cs
class Program
{
    static void Main(string[] args)
    {
        int answer = 50;

        if (answer >= 50)
        {
            Console.WriteLine("You passed!");
        }
        else
        {
            Console.WriteLine("Oh no, something went wrong.");
        }
    }
}
```

- a source file simply testing a variable (called `answer`​), checking if it is greater than `50`​, and outputting the appropriate string (printing `You passed!`​ if the value is greater than 50, otherwise going down the other branch/path and printing `Oh no, something went wrong.`
- Instructions:

  - Create a project using `dotnet`
  - Use `dotnet run` and observe the output

‍

### 3.1.1 Steps via CLI

```terminal
// Create a dotnet project
dotnet new console -n HelloWorld --use-program-main

// Build project
dotnet build

// Run program
dotnet run

// NOTE: calling `dotnet run` will recompile the program again if the build process has not already been triggered
```

‍

## 3.2 Structuring your source file

- Execution of the program starts at the **top** of the file and goes down
- Best to form some good habits and reserve some space for when certain things are to be appropriately placed and clustered:

‍

```cs
class Program
{
    // Formatting output and printing it
    static void outputFilePath(string filepath)
    {
        string output = $"This file that will be used is: {filepath}";
        Console.WriteLine(output);
    }

    static void Main(string[] args)
    {
        string filepath = "example.txt";

        if (args.Length > 1)
        {
            filepath = args[0];
        }

        outputFilePath(filepath);
    }
    
}
```

‍

Structuring conventions

- Class definition:

  - normally named `Program`​ if it contains an entry point method (such as `Main`)
  - (C# is an OOP language influenced by Java --> has a class definition)
- Method definitions:

  - placed inside the Class
  - not strictly the only way of doing this, but is a common program structuring convention
- Variable declarations and initialisation (along with other statements):

  - within method definitions or are considered class fields/properties
  - top-level variables and statements are different to those made inside functions and classes; normally considered globals.

‍

---

# 4. 3.2 Variables and Values

## 4.1 Variables

- use variables to record and hold data within programs
- a variable is a symbol within a C# program that we can read and write to
- different keywords that can be used in combination with a variable...

‍

### 4.1.1 Declaring variables:

- start by specifying the **data type**:

  ```cs
  int number = 5;
  string text = "hello!";
  boolean b = true;
  ```

‍

#### 4.1.1.1 Declaration pattern:

```terminal
(qualifier) <data type> <symbol> = <initial value>;
```

- `qualifier` - (this is optional)

  - the variable could be declared as `const`, meaning that it is a read-only variable,
  - or `static` which would mean the lifetime of the variable exceeds that of the class and method
- `data type`​ - not strictly necessary as **implicitly-typed** variables can be used, although it is necessary here. Typically use data types such as `int`​, `float`​, `double`​, `long`​, `string`​, `boolean` for now. More complex types will be used later.
- `symbol` - refers to the name of the variable itself; how the variable is able to be referred to throughout the code; without a name, it will not be valid.
- `initial value` - it is possible to simply declare an (empty?) variable; but also may want to initialise variables to a sensible value; this value must also resemble the same data type.

‍

### 4.1.2 Implicitly-typed variables

It is possible to omit the `data type`​ and just have the `symbol`​ --> use prefix `var`

```cs
var message = "Hello World!";
```

- this is an example of a declaration of a variable without specifying the data type;
- the data type is derived from the assigned value.

‍

### 4.1.3 Variable naming rules

- Must be mindful of how variables are named

  - Should be meaningful in the context of the problem that is being solved
  - By convention, C# leans into `camelCase`​ naming for variables (meanwhile `CapitalCamelCase` is used for classes and methods)
  - Variables must start with a non-numeric character and not use any symbols that could be confused with an **operator**

‍

<span data-type="text" style="color: var(--b3-card-info-color); background-color: var(--b3-card-info-background);">Exercise</span>

Consider the following scenarios and what variable name would be appropriate. 🖊️

- Number of soccer balls in a garage - `totalSoccerBalls`
- Players part of Red Team - `redTeamPlayers`
- If a light is on or off - `isLightOn`
- Blood type - `bloodType`
- Email message - `emailMessage`

‍

### 4.1.4 Variable values and assignments

- use the `let`​ or `const`​ keyword followed by the **name** of the symbol
- to initialise a variable to a **value**, use the `=` operator to assign it to the value/operand on the right hand side
- a **binary** operator has two operands to the left and right hand side of the operator; in this case the assignment operator `=` has two operands, the variable name on the left of the operator, and the String value on the right

‍

Example:

```cs
string message1 = "Hello!";
var message2 = "Hello!";
```

- code breakdown:

  - on line 1, the data type keyword is used to intialise the variable called `message1`​ and specify its data type as a `string`​; this variable undergoes an assignment operation via the `=`​ operator; it is assigned to the string value `"Hello!"`
  - on line 2, the `var`​ keyword is used to declare a variable called `message2`​, which is assigned to the value `"Hello!"`​ using the `=` operator. This type of variable declaration is implicitly typed, which means that the data type is not explicitly specified but instead is derived from the assigned value, i.e. a string value therefore the variable is a string data type.

‍

---

# 5. 3.3 Datatypes

19:14:43

19:15:08

‍

## 5.1 Primitive and reference datatypes

C# supports a primitive set of data types/categorisation similar to Java, C and C++.

‍

### 5.1.1 Primitive datatypes 📒

- `int` - integer datatype, represents whole numbers
- `bool`​ - represents boolean `true`​ or `false`
- `float` - floating point value, represents decimal numbers like 20.5, 1.22, 9.69
- `double` - similar to float but with more precision
- `byte` - integer value between -128 and 127

‍

### 5.1.2 Reference datatypes 📒

- `Array`​ - holds many objects; a contiguous block of data that can hold other kinds of data; usually indicated with `[]` after the datatype in a variable declaration
- `Object` - an object is composed of fields and/or methods; used to compose and group data together;

  - normally a generalisation used to refer to variables or to the value assigned to a variable;
  - but more accurately in C#, it is usually an instance of a class.

‍

### 5.1.3 Both types?

- A `string` datatype falls into both categories...

  - while it is a reference datatype, its semantics are similar to that of a primitive datatype
- A major distinction between the two kinds of datatypes is how assignment works.

‍

## 5.2 But why do we have datatypes?

- Each data type has operators associated with it
- These are operations that allow for the data to be used with another piece of data or expressed by itself.

‍

Example:

```cs
int x = 10;
int y = 20; 
int z = x + y;
```

- Code breakdown:

  - line 1: the variable `x` is set to the integer value 10;
  - line 2: the variable `y` is set to the integer value 20;
  - line 3: adds the values of variables `x`​ and `y`​, which will compute 30; also sets `z` to the number 30 (the result of the calculation)
- Code explanation:

  - There are 4 operators being used:

    - we can observe 3 uses of the `=` operator, used for variable assignment
    - the `+` operator is used to perform addition with its operands

‍

## 5.3 Assigning the correct data type to a variable

Remember the pattern `<data type> <variable name> = <value>`.

How C# derives the type of the variable:

```cs
var numberX = 10;       // int
var numberY = 20.5;     // double
var stringX = "Ten";    // string
var stringY = "Twenty"; // string
var booleanX = true;    // boolean
var booleanY = false;   // boolean is only true or false
```

‍

## 5.4 Categorising and converting data [🖊️](https://tafe-tthom.github.io/programming1cs-book/023_datatypes.html#categorising-data-and-converting-data-)

When developing a program, you must know what data is being used and how to process it. Choosing a suitable datatype assists with the kind of operations that will be performed with that data.

*Exercise* [🖊️](https://tafe-tthom.github.io/programming1cs-book/023_datatypes.html#categorising-data-and-converting-data-)

Consider what would be an appropriate data type for the following data:

- The name of a colour - `string`
- The number of participants in a survey - `int`
- Recording the grade as A, B, C, D, E, F - `string`
- If a subscription is active or not - `bool`
- Representing tax from an income statement - `double`
- Recording the age of a person - `int`

‍

### 5.4.1 Data conversion

Commonly, data is represented as text itself and then converted into something more appropriate like a number or a boolean value.

Conversion methods are associated with the following **types:**

- int, long, byte
- float, double
- string

It is common to observe conversion methods from text/string to its own relevant representation:

- `Int32.Parse`​ or `Float.Parse` are common methods used to convert a string to a number datatype.
- For `bool`​, if the string represents the text as `true`​ or `false`, a string comparison can be performed to resolve this conversion.
- Use the `ToString` method to convert from a numeric type to a string type; however, each type usually can be converted into a string when used with a string.

‍

19:33:40

‍

---

# 6. 3.4 Operators and Expressions

2026.06.24 - 12:20:23

‍

## 6.1 Types of operators

- Binary operator
- Unary operator
- Ternary operator

‍

The logic behind each operator refers to the number of elements being operated on.

‍

### 6.1.1 Unary operator

- needs one variable

‍

Example:

```cs
bool r = !true; // will return False

int i = 10;

i++;

Console.WriteLine(r); // False
Console.WriteLine(i); // 11
```

                                  Code explanation:

- `!` is a common unary operator, applied to get the opposite result of a boolean expression.
- the `++`​ unary operator increments the value by 1 (i.e. `i` is being changed from 10 to 11)

‍

#### 6.1.1.1 Binary operator

- uses the element (/operand) on the left and the element on the right.
- they take on different behaviours depending on the data type

‍

Example:

```cs
int y = 10 + 20;
int z = y * 2;

Console.WriteLine(y); // 30
Console.WriteLine(z); // 60
```

Code explanation:

- `+`​ addition operator for numbers and `*` multiplication operator
- both work on the left and right operands

‍

#### 6.1.1.2 Ternary operator

- use the `?`​ and `:` symbols together

‍

Example:

```cs
int mark = 61;
string result = mark >= 50 ? "Pass" : "Fail";

Console.WriteLine(result); // Pass


```

Code explanation:

- the ternary expression above evaluates as "Pass" since `mark`​ is greater than or equal to 50, therefore the value of `result` is assigned to the string "Pass"

‍

## 6.2 Data types changing operator behaviour [📒](https://tafe-tthom.github.io/programming1cs-book/021source.html#are-source-files-the-program-)

- the data type of a variable will change what kind of operation is performed or is even supported

‍

Example:

```cs
var numberX = 10;
var numberY = 20.5;
var stringX = "Ten";
var stringY = "Twenty";
var booleanX = true;
var booleanY = false;

// number + number = number
Console.WriteLine(numberX + numberY); // 30.5

// number + string = string
Console.WriteLine(numberX + stringY); // 10Twenty

// string + number = string
Console.WriteLine(stringX + numberY); // Ten20.5

// string + string = string
Console.WriteLine(stringX + stringY); // TenTwenty

// boolean + boolean = number?
// CS0019 Operator '+' cannot be applied to operands of type 'bool' and 'bool'
// Console.WriteLine(booleanX + booleanY);

// boolean || boolean = boolean (logical OR) // True
Console.WriteLine(booleanX || booleanY);

// boolean && boolean = boolean (logical AND) // False
Console.WriteLine(booleanX && booleanY);

// !boolean = boolean (logical NOT) // False
Console.WriteLine(!booleanX);
```

Explanation:

- number + number will result in a number (an addition operation)
- number + string, or string + string will result in a `string` => string concatenation

‍

#### 6.2.0.1 Logical OR and Logical AND

- ==Short circuiting:==

‍

## 6.3 ==Common Operators and Expressions 📒==

Refer to MDN (Mozilla Developer Network) --> get a breakdown of some common operators, expressions, and literals

‍

- `+`
- `-`
- `/`
- `*`
- `"..."`​ or `'...'`
- `true`​ or `false`
- `<`
- `>`
- `<=`​ or `>=`
- `==`
- `!=`

‍

## 6.4 Printing and Interpreting Data

We will want to print data to standard output and interpret strings into integers and floats.

C# provides the capabilities with the following functions:

- `Console.WriteLine()` - print to standard output (terminal screen)
- `int.Parse`​ - attempt to convert a `string`​ into an `int`​; convert to the type `int`
- `float.Parse`​ - attempt to convert a `string`​ into a `float`​; convert to the type `Number`

‍

## 6.5 <span data-type="text" style="color: var(--b3-card-info-color); background-color: var(--b3-card-info-background);">Exercise </span>🖊️ 💻

Notice that there are two different ways to interpret the string and translate it to a number. Consider what the difference between an `int`​ and a `float`. 📒

**Note:**  Write an experiment and use the following text: 

- `"2"`
- `"2.5"`
- `"2.33"`
- `"1.99"`
- `"5.0"`

Consider what the behaviour is between the two and what **number sets** they relate to.

12:45:54

---

# 7. 3.5 Command Line Arguments

12:47:11

## 7.1 Intro

- This is specifically a desktop application component.
- As observed in chapter 1, programs can be built that can use command line arguments as well.
- When executing code with `dotnet run`, command line arguments can be input that can be used with your program.

‍

## 7.2 Using command line arguments

To retrieve command line arguments from your application, use the following code:

```cs
static void Main(string[] args)
{
    string arg1 = args[0];
    string arg2 = args[1];

    Console.WriteLine(arg1);
    Console.WriteLine(arg2);
}
```

- the 0th and 1st index of the `args` array is where the arguments are stored for our program
- example: `dotnet run Monday Tuesday`

  - `arg1`​ will map to `Monday`
  - `arg2`​ will map to `Tuesday`
- keep in mind: the data type that `arg1`​ and `arg2`​ are associated with is `string`

‍

Exercise 📒

- Note down some of the concepts that may be new here

‍

12:52:24

‍

---

# 8. 3.6 Standard Input, Output and Error

‍

## 8.1 Intro

- if we want simple programs to be interactive...
- use standard IO buffers: `stdin`​, `stdout`​ and `stderr`
- within C#, these are normally accessed through a method line `Read`​, `ReadLine`​, `Write`​, `WriteLine`​ or `Error.WriteLine`

‍

## 8.2 Input and output

- simple outputting to the shell: when interacting with `stdout`​, we use the method `Console.WriteLine`​ or `Console.Write`

  - both methods interpret input data and print out to the screen (output)
  - to achieve this, it must be able to convert the input data to a string

‍

Example:

```cs
string name = "Cat";
Console.WriteLine(name);
```

- the above snippet accepts the argument `name`​, which is assigned to the value `"Cat"`
- the `WriteLine` method then writes this to the terminal
- there is very little processing or translation it will need to do

‍

Example 2:

```cs
int n = 10;
Console.WriteLine(n);
```

- before the value 10 is printed to the screen, `WriteLine` needs to convert it to a string
- `Console.WriteLine`​ has a way to handle `int` data: it converts each digit to the relevant ASCII digit.
- refer to the ASCII table --> [ASCII Table](https://en.wikipedia.org/wiki/ASCII#Printable_character_table)

  - the integer 10 needs to be translated to `0x6160`​, where `61`​ is `1`​ and `60`​ is `0`

‍

### 8.2.1 Console.Read and Console.ReadLine

- to handle data that the user can input, you will typically interact with reading `stdin`​ by using `Console.Read`​ or `Console.ReadLine`, which return a character (read) or a string (readline)
- since `ReadLine` returns a string, it can be readily assigned to a string variable:

```cs
string line = Console.ReadLine();

// Do something with the line
Console.WriteLine(line);
```

- the above code is essentially a simple `echo` program: whatever is typed in is repeated back out by the program
- ¿ what if `**` is added around the line being read?

  - => still echos the program but a little bit prettier looking..!

```cs
string line = Console.ReadLine();

Console.WriteLine("**" + line + "**");
```

‍

### 8.2.2 What if I want to convert data?

- a common issue; refer to section 3.3
- => conversion methods
- if we can convert data to a string, we are able to convert it back, assuming it has a pattern that can be followed.

‍

## 8.3 `stderr` / Console.Error

Although it isn't common to use this buffer, it is valuable when implementing/leveraging error handling, or immediately flushing the data to the shell.

‍

- `Console.Error`​ has similar functionality to `Console.WriteLine`​ or `Console.Write` which may make it seem somewhat meaningless...
- However, what's not obvious --> `Console.WriteLine`​ / `Console.Write` do not print to the screen immediately
- `Console.Error.*` will satisfy this constraint; allows you to have fine control over the buffers when using the program as well

‍

Example:

a program that contains the following code:

```cs
Console.WriteLine("This is a line to be outputted");
```

... can be run like so:

```terminal
dotnet run > program.output
```

- the string is not outputted to the shell, but instead has been written to `program.output`
- however, by leveraging `Console.Error`​, we can print to the terminal and also redirect output that we may want to **log**.

```cs
Console.WriteLine("This is a line to be outputted to the file");
Console.Error.WriteLine("This is a line to be outputted to the shell");
```

‍

Observations:

- the `program.output`​ file holds the data from `WriteLine`​, not `Error.WriteLine`

‍

13:08:41

---

# 9. 3.7 Exercises

13:20:40

‍

---

# 10. 3.8 Review

N/A 2026.06.24

---

# 11. 3.9 Supplementary - Debugging

N/A 2026.06.24

‍
