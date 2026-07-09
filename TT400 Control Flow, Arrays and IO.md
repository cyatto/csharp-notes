# TT400 Control Flow, Arrays and IO

*Estimated Time Required:*   ***10 hours***📅🕙

‍

# 1. TT401 Scope and If-Else

‍

22:46:31

Conditional logic and loop structures and patterns associated with them.

(Making decisions, iteration)

Core concepts:

- Sequence
- Selection
- Iteration

These are critical building blocks for programs and building algorithms.

Also will look at Arrays which are a natural extension from scalar types.

‍

## 1.1 /Topics

The take away from this chapter is that you should understand the following and where to appropriately apply it:

- Variable lifetime
- `if`​, `else`​ and `else if`
- `while`​, `do...while`​ and `for`
- `{ }` usage and scope rules
- Boolean logic being utilised for control flow.
- Arrays and aggregation
- Designing and implementing an algorithm

Where we left off was building simple programs that went through a **sequence** of instructions, we are now going to introduce `if-else`​ statements as part of **selection** and `while`​, `do...while`​ and `for`​ as part of **iteration**.

‍

# 2. TT402 Scope and If-Else

13:42:19

## 2.1 Scope and Variable Lifetimes

Where a variable is declared/scoped, within curly braces, determines the lifetime/existence/how long a variable lives for.

‍

Code example

```C#
int x = 10;
Console.WriteLine(x);
{
    int y = 20 + x;
    Console.WriteLine(y);
    {
        int z = x + y;
        Console.WriteLine(z);
    }

    int z = 100 + y;
    Console.WriteLine(z);
}

// Error?
// CS0136 A local or parameter named 'z' cannot be declared in this scope because that name is used in an enclosing local scope to define a local or parameter
```

‍

Observations:

- `x` is declared and assigned to 10
- `y` is declared and assigned to 20 + x
- `z` is declared and assigned x + y
- `z` is dropped after printing 40
- `z` is declared and assigned 100 + 30
- `z`​ and `y` are both dropped

‍

Explanation:

- `y`​ and `z`​ are declared in the *inner scope*, which means that when they are dropped, they become inaccessible.

  - dropped: they exist between the time they are created and the closing curly brace `}`
- a new declaration is effectively creating a brand new variable with the same name as a previously existing variable (that no longer exists)

‍

"Lifetime" of a variable:

- the idea of **lifetime** is applied when the variable starts existing and when it is dropped
- this is different to the lifetime of data, particularly **reference types**

‍

## 2.2 Making decisions

‍

If statements:

To provide branching paths/decisions, use `if` statements.

The `if`​ statement's role is to evaluate a boolean expression and execute the code within the branch if it evaluates as `true`.

‍

### 2.2.1 if-else block pattern

`if-else` block pattern:

```C#
if (boolean_expression)
{
    // If the boolean expression evaluates as true
}
else
{
    // If the expression evaluates as false
}
```

‍

Code example - comparing numbers:

```C#
int x = 10;

if (x > 5)
{
    Console.WriteLine("x is greater than 5");
}
else
{
    Console.WriteLine("x is smaller or equal to 5");
}
```

‍

Code breakdown:

- On line 3, the expression `x > 5`​ uses a logical operator that compares the left operand with the right. Since `x`​ is a variable, it will read the value it is currently set to, which is 10 --> `x`​ evaluates to `10`​, so the expression `x > 5` evaluates as true --> the first branch between curly braces on lines 4-6 executes and "x is greater than 5" is output to the console.

‍

### 2.2.2 else if condition

¿ What if we want to check if x < 5 but greater than 2? => add an additional condition using `else if`

‍

Code example:

```C#
int x = 3;    // Output: x is greater than 2 but less than 5
// int x = 2; // Output: x is smaller than 2 

if (x > 5)
{
    Console.WriteLine("x is greater than 5");
}
else if (x > 2)
{
    Console.WriteLine("x is greater than 2 but less than 5");
}
else
{
    Console.WriteLine("x is smaller than 2");
}

```

‍

### 2.2.3 Nested `if` statements

if statements can be nested within other if statements and have multiple conditions with `else if(...)` after the first if statement.

‍

The above code can be restructured with nested if statements:

```C#
int x = 3;    // Output: x is greater than 2 but less than 5
// int x = 2; // Output: x is smaller than 2 

if (x > 5)
{
    Console.WriteLine("x is greater than 5");
}
else
{
    if (x > 2)
    {
        Console.WriteLine("x is greater than 2 but less than 5";
    }
    else
    {
        Console.WriteLine("x is smaller than 2");
    }
}

```

‍

## 2.3 Logical Operators and Evaluation

‍

### 2.3.1 Exercise 📒💻

What occurs in different expressions?

‍

Use the following snippet for an experiment and note your observations 📒 💻

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (/* input your expressions here */)
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}
```

‍

Use the following expressions and replace the commented component with it.

Note down what you expected and what matched those expectations and what didn't.

‍

- `a`

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (a)
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}

// Output:
// if branch is executed
```

‍

- `!b`

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (!b)
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}

// Output:
// if branch is executed
```

‍

- `a && b`

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (a && b) // both a and b must be true for the if expression to evaluate as true
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}

// Output:
// else branch is executed
```

‍

- `b`
- expected: ✅

  - if statement evaluates as false so else branch is executed

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (b)
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}

// Output:
// else branch is executed
```

‍

- `b || a`
- expected: ✅

  - logical OR operator will evaluate both expressions to the left to right of the operator; if the expression on the left evaluates as true, short-circuiting occurs, which means that the entire expression will evaluate as true even without evaluating the expression to the right of the logical OR operator.
  - since the expression on the left, `b`​, evaluates as false, the expression on the right, `a`​ is also evaluated, which is `true`​, so the entire logical OR expression `b || a` evaluates as true --> resulting in the code in the if branch being executed.
  - Output: if branch is executed

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (b || a)
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}

// Output:
// if branch is executed
```

‍

- `c > d`
- expected: ✅

  - expression evaluates as false, so else branch is executed

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (c > d)
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}

// Output:
// else branch is executed
```

‍

- `d > c`
- expected:

  - expression evaluates as true, so if branch is executed

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (d > c)
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}

// Output:
// if branch is executed
```

‍

- `c == d`
- expected: ✅

  - the value of c does not equal to the value of d (i.e. the expression evaluates as false), so the else branch is executed.

```C#
bool a = true;
bool b = false;
int c = 10;
int d = 20;

if (a && b) // both a and b must be true for the if expression to evaluate as true
{
    Console.WriteLine("if branch is executed");
}
else
{
    Console.WriteLine("else branch is executed");
}

// Output:
// else branch is executed
```

‍

### 2.3.2 Booleans forever 📒

‍

Logical operators:

- `&&` - Logical AND operator
- `||` - Logical OR operator
- `!` - Logical NOT operator; unary operator

‍

|Operator name|Symbol|evaluate as true?|evaluate as false?|
| ---------------------| --------| ----------------------------------------------| ---------------------------------------------|
|logical AND|`&&`|both left and right operands must be true|either operand is false|
|logical OR|`\|\|`|only one operand must be true|both operands are false|
|logical NOT (unary)|`!`|operand is false (evaluates to the opposite)|operand is true (evaluates to the opposite)|

‍

Truth tables:

Logical AND

|x && y|||
| -------------| ------------| -------------|
||y\=true|y\=false|
|x\=true|**true**|**false**|
|x\=false|**false**|**false**|

‍

Logical OR

|x \|\| y|||
| ----------------| ------------| -------------|
||y\=true|y\=false|
|x\=true|**true**|**true**|
|x\=false|**true**|**false**|

‍

Logical NOT

|!x||
| -------------| --|
|x\=true|**false**|
|x\=false|**true**|

‍

For further information, refer to the MDN documentation.

- `if and switch statements`​ - [Microsoft Developer Network if and switch statements](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/selection-statements)

‍

14:38:46

‍

# 3. TT403 Loops

‍

Loops are a construct used for iteration, allowing operations to be executed repeatedly until a certain condition is met. Similar to `if` statements, the condition is based on the evaluation of a boolean expression.

‍

## 3.1 Where and when are loops used?

‍

**Iteration:**

- able to change values inside the scope of a loop itself

‍

16:08:18

Consider the following scenarios:

- Outputting numbers 1 to 10
- Keeping a game running until only 1 player is left
- Finding a character in a string
- A program that requires input from a user until they write `QUIT`

These scenarios outline a pattern we want to encode and perpetuate until a condition is no longer met.

‍

### 3.1.1 while loop

‍

- layout is similar to an if statement; simply needs to evaluate a boolean expression

‍

```C#
while (boolean_expression)
{

}
```

‍

What happens when the expression evaluates as true?

- If it's the first time evaluating the expression, it will enter the loop block `{ ... }` and execute the code in the block.
- If it's the second or third... or nth iteration and the expression remains true, it will continue executing the statements within the block (scope).

‍

Example: print numbers 0 to 9

```C#
int x = 0;

while (x < 10)
{
    Console.WriteLine(x);
    x += 1;
}
```

‍

Output:

```output
0
1
2
3
4
5
6
7
8
9
```

‍

Code breakdown:

- Prints numbers 0 to 9
- On line 6, the value of `x` is incremented by 1 on each iteration that evaluates true.
- When `x` reaches the value 10, it will exit the loop.

‍

To change the loop to output from 9 to 0:

Method 1:

```C#
int x = 0;

while (x < 10)
{
    Console.WriteLine(9 - x);
    x += 1;
}
```

```Output
9
8
7
6
5
4
3
2
1
0
```

- The condition remains the same, but the **console.log** statement is changed to subtract from 9. In spirit though, `x` is still counting upwards.

‍

Method 2:

```C#
int x = 9;

while (x >= 0)
{
    Console.WriteLine(x);
    x -= 1;
}
```

- `x` now starts at 9
- the conditional statement is `x >= 0`: if x is greater than or equal to 10, continue executing the code in the loop block
- decrement the value of x by 1

‍

Experiment!

What happens when you forget to either increment or decrement the value of `x`? Does the loop terminate? 💻

```C#
int x = 9;

while (x >= 0)
{
    Console.WriteLine(x);
    // x -= 1;
}
```

‍

⚠️ an infinite loop occurs, printing either 0 or 9 depending on the starting value of x and the conditional statement.

🛑 if there is a non-terminating loop, use the shortcut `CTRL+C` to send an interrupt signal to the process in the terminal.

‍

### 3.1.2 for loop

‍

The for loop pattern - comprised of three parts:

```C#
for (initialisation; boolean_expression; updates)
{
    
}
```

‍

The structure of a `for`​ loop maps onto the following `while` loop construction:

```C#
initialisation;

while (boolean_expression)
{
    
    updates;
}
```

‍

To create a similar loop to the previous `while` loop section:

```C#
// print numbers 0 to 9

// for loop
for (int x = 0; x < 10; x += 1)
{
    Console.WriteLine(x);
}

// while loop
int y = 0;

while (y < 10)
{
    Console.WriteLine(y);
    y += 1;
}
```

‍

Observations:

- The initialisation that was outside of the while loop is not in the initialisation position (first part of for loop construct);
- the boolean expression is similar;
- the update position is different.

‍

**Incrementation statements:**

- `x += 1` is not usually used
- the shorthand version `x++` is typically preferred (simply, shorter)

‍

### 3.1.3 do-while loop

While this construct does exist, it is less common in practice.

Normally this is because the `do-while` loop provides a guarantee that it will execute at least one iteration.

However, some optimisers may transform some `for`​ and `while`​ loops into a `do...while` loop, assuming it can find a guarantee that the loop executes once.

The block code is always executed once.

‍

```C#
// do-while loop pattern

do {


} while (boolean_expression)
```

‍

Example - print numbers 0 to 9:

```C#
int x = 0;

do
{
    Console.WriteLine(x);
    x += 1;
} while (x < 10);
```

‍

‍

### 3.1.4 [Equivalance of loops 🖊️ 💻](https://tafe-tthom.github.io/programming1cs-book/032_loops.html#equivalance-of-loops--)

For any loop that can be constructed with a `while`​ keyword, it can also be constructed with a `for` loop, and vice-versa.

‍

Exercise: Convert these loops into an equivalent version but using a different loop keyword.

Just make sure you construct the opposite of what is there. No point redoing the `for` loop when one is right there.

‍

Loop 0 (so that I can visualise the output per iteration...)

```C#
bool keepRunning = true;
int counter = 10;

while(keepRunning)
{

  if(counter <= 0)
  {
    keepRunning = false;
  }
  Console.WriteLine(keepRunning);
  counter--;
}
```

‍

Output

```Output
True
True
True
True
True
True
True
True
True
True
False
```

‍

2026.06.25 - 16:48:36 //

2026.06.26 - 16:14:27

Refactored loop 0 (using for loop):

```C#
for (int counter = 10; counter > 0; counter --)
{
    Console.WriteLine(true);
}

Console.WriteLine(false);
```

‍

Observations:

- the `for` loop lets you fold the counter setup, condition check, and decrement all into one line, removing the need for the keepRunning flag entirely.

‍

### 3.1.5 For loop (structure/pattern)

‍

**For loop (again) --> three parts separated by semicolons:**

1. `int counter = 10` --> runs once at the start (initialisation)
2. `counter > 0` --> checked before each iteration, loop stops when condition evaluates as false (i.e. is no longer true)
3. `counter--` --> runs after each iteration

‍

‍

**Loop 1**

```cs
bool keepRunning = true;
int counter = 10;

while(keepRunning){
  if(counter <= 0)  {    keepRunning = false;  }  counter--;}
```

‍

Refactored loop 1:

```C#
for (int counter = 10; counter > 0; counter--)
{
    // Console.WriteLine(true);
}

// This code runs ten times and stops (without doing anything, since the body code does not contain any logic or operations)
```

16:37:10

‍

**Loop 2**

```cs
int x = 10;
int y = 10;
int mx = 2;
double my = 2.003;

for(int i = 0; Math.Floor(x) == Math.Floor(y); i++){  x = x * mx;  y = y * my;  Console.WriteLine(x);  Console.WriteLine(y);}
```

‍

Refactored loop 2:

```C#

```

‍

‍

‍

# 4. TT403 Nesting Control Flow

# 5. TT404 Break and Continue

# 6. TT405 Arrays

# 7. TT406 Files and IO

# 8. TT407 Exercises

# 9. TT408 Review

‍
