# TT500 Classes, Objects, and Methods

INTRO

‍

- abstractions for aggregating and composing data
- methods and modularising code
- constructs previously seen or by using standard library functions

‍

TOPICS

‍

part of composing more complex types and organising code effectively

aggregates constructs (Arrays) -- in addition Classes and Objects --> ability to assemble more complex objects and code

‍

- classes
- objects
- methods
- parameters and return values
- indirection and references
- algorithms

‍

---

‍

# 1. TT501 Classes

‍

## 1.1 Classes Structure

Within C#, defining a class is also defining a type

- acts as a template for objects that are instantiated as this type
- more than one object can be instantiated as a type --> can guarantee certain behaviours and fields from that object instance

‍

### 1.1.1 Class anatomy

‍

A class can range from being complex to simple, depending on the scenario through which it is being used.

Classes can be composed with other classes, objects and types (similar with other objects and constructs)

‍

### 1.1.2 Constructing our own class

‍

Already:

- interacted with classes through Array and other builtins such as `int`​ and `string`.

‍

Not yet:

- instantiating an object from a type using the `new` keyword

‍

#### 1.1.2.1 Most critical components (not all demonstrated tho)

‍

```cs
class ClassName
{
    // Field just not initialised; needs to be in the constructor or given a default value
    datatype instanceField;

    // Field initialised but with a string value
    datatype initialisedField = "Hello!";

    // Field for the class, but not initialised yet; will be set to default value
    static datatype staticField;

    // Field for the class, this time initialised
    static datatype staticInitialisedField = "And goodbye!";

    // Optional; when omitted, a constructor exists but without parameters
    ClassName() { }

    // Similar to a function but operations within the method are performed on the object
    // Can have a return statement or not
    ReturnType AMethod()
    {

    }

    // Method that is associated with the class but not the object
    // Similar to a function and method, can have a return statement
    static ReturnType AStaticMethod()
    {

    }
}
```

‍

Note:

- `field`​ specifiers do not **need** to exist in class scope;

  - they can be created and assigned during execution of the constructor

‍

### 1.1.3 Field initialisation

‍

Below example:

- constructor our own class (example - `Cupcake` class)
- focus on the field variations that can be utilised

‍

```cs
class Cupcake
{
    bool delicious = false;
    string name = "Chocolate Cupcake";
}

Cupcake cupcake1 = new Cupcake();

Console.WriteLine(cupcake1.name); // Output: Chocolate Cupcake
Console.WriteLine(cupcake1.delicious); // Output: False
```

‍

Problems / errors:

```
v C* class Cupcake.cs 3

Top-level statements must precede namespace and type declarations. (CS8803) [Ln 7, Col 1] 'Cupcake.name' is inaccessible due to its protection level (CS0122) [Ln 9, Col 28] x 'Cupcake.delicious' is inaccessible due to its protection level (CS0122) [Ln 10, Col 28]
```

‍

Errors explained:

1/ Fields are `private` by default

- in C#, class members (fields, methods) default to `private` unless stated otherwise
- must explicitly mark a field `public` to access it from outside the class

‍

```cs
class Cupcake
{
    public bool delicious = false;
    public string name = "Chocolate Cupcake";
}
```

‍

2/ Top-level statements must come BEFORE class declarations

- C# lets you skip writing a `Main` method and just write statements directly in the file (called "top-level statements")

  - what `Cupcake cupcake 1...`​ and `Console.WriteLine...` lines are { ❓ I guess these are within the "Main" scope? }
- However, the compiler requires all of these loose statements to appear BEFORE any `class`​/`struct` declarations in the file, not after.
- --> solution: move the class to the bottom

‍

Extra notes:

- public fields like this are fine for learning, but later likely will switch to **properties** (`public string Name { get; set; }` --> gives controlled access instead of exposing raw fields

‍

**Corrected code:**

‍

```cs
Cupcake cupcake1 = new Cupcake();

Console.WriteLine(cupcake1.name);
Console.WriteLine(cupcake1.delicious);

class Cupcake
{
    public bool delicious = false;
    public string name = "Chocolate Cupcake";
}

// Output:
// Chocolate Cupcake
// False
```

‍

{ Back to the book ... }

‍

Code example explained:

- defines a class with two instance fields
- the fields are already assigned to values, so the output is printed
- (may seem redundant to have a whole concept that doesn't differentiate itself from objects that much...)

‍

### 1.1.4 Class constructor

‍

- parameterise the fields themselves through a constructor

‍

```cs
Cupcake cupcake1 = new Cupcake("Chocolate Cupcake", false);

Console.WriteLine($"Is the {cupcake1.name} delicious?");
Console.WriteLine($"Answer: {cupcake1.delicious}!");

class Cupcake
{
    public string name;
    public bool delicious;

    public Cupcake(string name, bool delicious)
    {
        this.name = name;
        this.delicious = delicious;
    }
}

// Output:
// Is the Chocolate Cupcake delicious?
// Answer: False!
```

‍

Observations:

- can parameterise what the field will be, similar to a **function**
- `this` keyword:

  - has a lot of utility within C#;
  - within above context, both addresses the ambiguity and refers to the current reference
  - NOTE: re: memory addresses;

    - ṃ (mental model): can think of `this` as the memory address of the current object executing the method.

‍

### 1.1.5 Class methods

‍

- Logic can be incorporated into a class method, which can be referred to by calling the method name.

‍

Likely cannot see too much difference between a function and method:

‍

```cs
// Assume inside class scope

ReturnType TheMethod(parameter)
{
    // If not void, needs to return a value
}
```

‍

Example - requirements:

- want to have some function whose purpose is to print out the name of the cupcake and if it it's delicious or not --> use a method { ?? should separate methods --> i.e. a method should only be doing ONE thing? }

‍

```cs
// Usage in another method

Cupcake cupcake1 = new Cupcake("Chocolate Cupcake", false);
cupcake1.PrintDetails();

class Cupcake
{
    public string name;
    public bool delicious;

    public Cupcake(string name, bool delicious)
    {
        this.name = name;
        this.delicious = delicious;
    }

    public void PrintDetails()
    {
        Console.WriteLine(this.name);
        Console.WriteLine(this.delicious);
    }
}
```

‍

## 1.2 Topic review - key concepts & learnings

‍

- classes
- instantiation of objects / instances
- fields
- constructors - allows for parameterisation of fields
- methods
- order of classes and top-level statements
- public and private classes/methods/etc

‍

---

‍

# 2. TT502 Objects

‍

Overview

- common to refer values assigned to variables as objects -- this is a generalisation
- specifically, objects refer to an *aggregate type* composed of fields, properties and methods

‍

## 2.1 Initialisation and indirection objects

‍

Within C#, an object is constructed as an instance of a class

- `new` keyword and calling the constructor

‍

Code demonstration:

- use the type `Box`
- demonstrates the concept of indirection

‍

Code from book:

```cs
class Box
{
    string label;
    Box? nestedBox = null;

    Box(string label)
    {
        this.label = label;
        nestedBox = null;
    }
}

Box obj = new Box("empty");
```

‍

Errors:

```
CS8803 Top-level statements must precede namespace and type declarations.

CS8805 Program using top-level statements must be an executable.

CS0122 'Box.Box(string)' is inaccessible due to its protection level
```

‍

Refactored code (so that the compiler actually runs):

```cs
Box obj = new Box("empty");

class Box
{
    public string label;
    public Box? nestedBox = null;

    public Box(string label)
    {
        this.label = label;
        nestedBox = null;
    }
}
```

‍

Observations & notes:

- Can update the fields of `obj` afterwards but new fields cannot be added to the object

‍

## 2.2 Reference types, indirection and aliasing

‍

- (already discussed) Arrays and Objects are *reference types*
- Arrays are assigned to a memory address
- Objects are also assigned to a similar value or modelled as such
- a powerful feature of objects is that other objects can be nested inside parent objects

‍

### 2.2.1 Indirection

‍

Code demo:

```cs
Box x = new Box("BoxX");
Box a = new Box("BoxA");

a.nestedBox = x;
```

‍

- challenge: retrieve the string "BoxX" without explicitly using the variable `x`​ --> want to use `a`​ and the fields associated with `a` to do this

  - able to refer to the field inside `x`​ via `a.nestedBox.label`

    - this is because the object that `x`​ is assigned to is assigned to `a.nestedBox` --> grants access to the nested field

‍

### 2.2.2 Nesting and more indirection

‍

Indirection with nesting

- using objects as a way to nest and group data
- can also group it up quite a lot where it has layers

‍

Onion examination! requirements:

- redefine the `Box` constructor to help with crafting "an absurd statement"

‍

Code demo:

```cs
class Box {

  string label;
  Box? nestedBox = null;

  Box(string label, Box nestedBox) {
    this.label = label;
    this.nestedBox = nestedBox;
  }
  
}

Box onionBox = new Box("1",
  new Box("2",
    new Box("3",
      new Box("4", null))));

// To get 3

string label3 = onionBox.nestedBox.nestedBox.label;
Console.WriteLine(label3);
```

‍

Refactored code demo:

```cs
Box onionBox = new Box("1",
    new Box("2",
        new Box("3",
            new Box("4", null))));

// To get 3

string label3 = onionBox.nestedBox.nestedBox.label;
Console.WriteLine(label3);


class Box
{
    public string label;
    public Box? nestedBox = null;

    public Box(string label, Box nestedBox)
    {
        this.label = label;
        this.nestedBox = nestedBox;
    }
}
```

‍

Observations:

- demonstrates how to nest objects inside other objects
- do not necessarily need to have separate variables for each layer or an array

  - instead, can actively place another object inside of another

‍

Question:

How to get the layers from the `onion` variable?

- Remember, `onionBox`​ has the fields `label`​ and `nestedBox`
- `label`: allows us to know what the layer number is
- to get layer 2:

  - `onionBox.nestedBox.label`
  - retrieves the value of layer 2 by using `.label`

‍

```cs
Box onionBox = new Box("1",
    new Box("2",
        new Box("3",
            new Box("4", null))));

// To get 3

string label3 = onionBox.nestedBox.nestedBox.label;
Console.WriteLine(label3); // Outputs 3

// get value of label of layer #2
Console.WriteLine(onionBox.nestedBox.label); // Outputs 2


class Box
{
    public string label;
    public Box? nestedBox = null;

    public Box(string label, Box nestedBox)
    {
        this.label = label;
        this.nestedBox = nestedBox;
    }
}
```

‍

### 2.2.3 Aliasing

‍

Concept features:

- can have 2 or more **references** to the same object (or allocation)

‍

Code example:

```cs
int[] a = [1, 2, 3, 4, 5];

int[] b = a;

b[2] = 100;

Console.WriteLine(a[2]);
Console.WriteLine(b[2]); //What's the print out here?
```

‍

```cs
int[] a = [1, 2, 3, 4, 5];

int[] b = a;

b[2] = 100;

Console.WriteLine(a[2]); // Output: 100
Console.WriteLine(b[2]); // Output: 100
```

‍

Observations:

- if the same operations were performed on a `number`​ or `boolean`, the above output would not be observable because those types are not reference types
- difference between reference types and value types:

  - a reference type (like an object or array) will copy the address, while value types will copy the value
- the output above for both arrays `a`​ and `b` are the same because both arrays are mapped to the same allocation (or memory address)

‍

{ my own question }

- { is it correct to say that they hold references to the same array object held in the same memory address? }

‍

Claude explainer:

Yes, that's exactly right.

`a`​ and `b`​ aren't separate arrays — they're two variables holding the *same reference* (pointer) to one array object living on the heap. When you write `int[] b = a;`​, you're copying the reference itself, not the data it points to. So after that line, both `a`​ and `b`​ point at the identical memory location. Mutating the array through `b[2] = 100`​ changes the only array that exists, which is why reading it back through either variable shows `100`.

A useful way to picture it: think of the array object as a box out in memory, and `a`​ and `b`​ as two sticky notes with that box's address written on them. Assigning `b = a`​ copies the ​*address*, not the box.

This lines up with your Ruby background, actually — Ruby arrays behave the same way:

ruby

```ruby
a = [1, 2, 3, 4, 5]
b = a
b[2] = 100
puts a[2]  # => 100
```

Same reference-type behavior. Where it'd differ is with value types (`int`​, `bool`​, `struct`​, etc.) — those get *copied* on assignment in C​, so they follow the "sticky note" behavior you just described.

‍

# 3. TT503.1 Method Overloading and Constructor Overloading

‍

In C#, it is possible to use the same method name but with a different method **signature** (([6.0.2 Method signature](appendxck/everything,%20or%20a%20glossary%20of%20mostly%20everything,%20aka%20please%20explain%20all%20these%20things%20to%20me.md#20260708220153-u5nzlkb))).

--> can define a method with different versions (e.g. a method called `Add()` where one version accepts two parameters, and another version that accepts three parameters)

‍

> ### 6.0.2 Method signature
>
> from the **Yellow Book (p. 206):**
>
> A given C# method has a particular signature which allows it to be uniquely identified in a program. The signature is the name of the method and the type and order of the parameters to that method:
>
> - `void Silly(int a, int b)` – has the signature of the name Silly and two int parameters.
> - `void Silly(float a, int b)` – has the signature of the name Silly and an float parameter followed by an integer parameter.
>
> This means that the code:
>
> - `Silly(1, 2) ;` - would call the first method, whereas:
> - `Silly(1.0f, 2) ;` - would call the second.
>
> Note that the type of the method has no effect on the signature.
>
> ---

‍

## 3.1 [3.1 Method overloading](TT500%20Classes,%20Objects,%20and%20Methods/3.1%20Method%20overloading.md)

‍

Console.WriteLine()

- ... leverages overloading --> datatypes are converted to strings without needing to be explicitly converted (does not apply to custom objects)

‍

How to?

- create a method with the same name but ensure that it has a different signature

‍

```cs
// Create two methods with the same name Add but with different signatures (differing parameter lists)

int Add(int a, int b);

int Add(int a, int b, int c);
```

‍

How to methods that accept that same parameters but have different method signatures?

‍

First, an invalid case, where

- method overloading cannot be applied because the callsite of the function would have difficulties creating a distinction between the two, especially if the type being assigned is not apparent

‍

```cs
// invalid case

int[] Add(int[] a, int[] b);
float[] Add(int[] a, int[] b);

// nulls added instead

// For 1 and 2, it isn't clear what is being called, although the type it is being assigned to could give-away some more information about that.

int[] x = Add(null, null);
float[] y = Add(null, null);

// For 3, this is why the return type isn't too helpful, since the datatype could used to assign either float[] or int[], it isn't too useful.

object o = Add(null, null);
```

‍

### 3.1.1 Definition & features

- A method is overloaded one with the same name but a different set of parameters is declared in the same class.
- Methods are overloaded when there is more than one way of providing information for a particular action

  - e.g. a date can be set by providing day/month/year information, or by a text string, or by a single integer which is the number of days since 1st Jan
  - ∴ Three different, overloaded, methods could be provided to set the date, each with the same name like `SetDate`
  - ≡ the `SetDate` method could be said to have been overloaded

‍

### 3.1.2 Quick guide - mental models (vs Ruby)

Ruby doesn't have method overloading in this sense because Ruby just uses whatever args are passed at runtime (duck typing, no compile-time type checks).

- i.e. you cannot define `def add(a, b)`​ and `def add(a, b, c)`

C# is statically typed, so the compiler needs to figure out **at compile time** which method is intended to be used, just from what was typed at the call site.

‍

### 3.1.3 Overloading: what the actual...

Same method name, different **signature** (parameter list).

The compiler picks the right one based on what arguments are passed in.

```cs
int Add(int a, int b);
int Add(int a, int b, int c);
```

- Step by step:

  - the compiler looks at the method call, counts/checks the argument types, and matches it to the overload whose parameters fit.
  - e.g. where parameter *count* differs (no ambiguity possible)

    - `Add(1, 2)`​ --> first method; `Add(1, 2, 3)` --> second method

‍

### 3.1.4 Why `Console.WriteLine` just works

- internally, it's just way overloaded (`WriteLine(int)`​, `WriteLine(string`​, `WriteLine(double)`, etc)
- when `Console.WriteLine(myInt)`​ is called, the compiler matches it to the `int` version automatically
- this is why it is not necessary to call `.ToString()` everywhere.

‍

### 3.1.5 Invalid case - return type isn't enough

```cs
int[] Add(int[] a, int[] b);
float[] Add(int[] a, int[] b);
```

Step by step:

- both methods have *identical* parameter types (`int[], int[]`). The only difference is the return type.
- <span data-type="text" style="background-color: var(--b3-card-success-background); color: var(--b3-card-success-color);">C# overload resolutions </span>**only looks at parameters**<span data-type="text" style="background-color: var(--b3-card-success-background); color: var(--b3-card-success-color);">, never the return type. So the compiler has no way of knwoing which one is intended to run based on the call alone.</span>
- Proof:

  - `Add(null, null)` --> literally nothing in this call tells the compiler which overload to use
  - `null`​ is not typed to `int[]`​ or `float[]`​ on its own #prog_core#​ [3. Data types](appendxck/everything,%20or%20a%20glossary%20of%20mostly%20everything,%20aka%20please%20explain%20all%20these%20things%20to%20me.md#20260624183008-8itvsx1)
  - This is illegal in C#; will not even compile.

‍

#### 3.1.5.1 Why assignment targets do not save you!

```csharp
int[] x = Add(null, null);
float[] y = Add(null, null);
object o = Add(null, null);
```

 **👣 Step by step:**

- Although C# can let use return-type context to help distinguish some overloading scenarios, it does not do so reliably enough to base a the whole design on it.

  - Proof:

    - line 3: `object` could hold either return type so there is zero information to disambiguate.
- Method signatures themselves have to be distinguishable, not only how the result is used.

‍

### 3.1.6 What does work/fixing ambiguity

‍

#### 3.1.6.1 differing parameter types

```csharp
int[] CrossProduct(int[] a, int[] b);
int[] CrossProduct(float[] a, float[] b);
```

 **👣 Step by step:**

- Now the parameters differ (`int[]`​ vs `float[]`), not just the return turn --> valid overloading
- However, calling it with `CrossProduct(null, null)`​ is still ambiguous since `null`​ fits either `int[]`​ and `float[]` parameter --> the compiler will not be able to pick

‍

#### 3.1.6.2 casting

```csharp
int[] x = CrossProduct((int[])null, (int[])null);
int[] y = CrossProduct((float[])null, (float[])null);
```

 **👣 Step by step:**

- casting `null`​ to `(int[])`​ or `(float[])`​ explicitly tells the compiler the *argument's* type, not just the result's.
- --> the compiler now has concrete parameter types to match against the signatures, resolving the ambiguity, and allowing it to pick the right overload

‍

### 3.1.7 TL;DR / review

Overload resolution in C# is decided by **parameter types (and count)**  at the [call site](appendxck/everything,%20or%20a%20glossary%20of%20mostly%20everything,%20aka%20please%20explain%20all%20these%20things%20to%20me.md#20260709140726-krktr6u) — never by return type or by what you're assigning the result to.

If two overloads could match a given call equally, i.e. method overloading is ambiguous, to resolve this, requires either:

- (1) [casting](appendxck/everything,%20or%20a%20glossary%20of%20mostly%20everything,%20aka%20please%20explain%20all%20these%20things%20to%20me.md#20260709121757-02532je);
- (2) a distinct parameter list (including count).

‍

## 3.2 TT503.2 Constructor Overloading & Constructor Chaining

‍

### definition & features

The same overloading concept can be applied to constructors:

- multiple constructors can be defined in one class, each with a different *signature* (different number/type of parameters).
- C# picks the right one based on what args are passed in when `new` is called.
- why useful?

  - flexibility in ways to create an object: with full info, partial info, or none at all; do not need to write separate factory methods for each case

‍

### book example

```cs
public class Person {
  private static int DEFAULT_AGE = 21;
  private string name;
  private int age;
  
  public Person()
  {
    name = "Jeff";
    age = DEFAULT_AGE;
  }

  public Person(string name)
  {
    this.name = name;
    this.age = DEFAULT_AGE;
  }

  public Person(string name, int age)
  {
    this.name = name;
    this.age = age;
  }
}
```

‍

explained:

- **How the first example works**:

  - `Person()`​ — no-arg constructor, hardcodes `name = "Jeff"`​ and `age = DEFAULT_AGE`.
  - `Person(string name)` — takes just a name, defaults the age.
  - `Person(string name, int age)` — takes both, sets them directly.
  - When you write `new Person("Janice")`​, <span data-type="text" style="background-color: var(--b3-card-info-background); color: var(--b3-card-info-color);">the compiler looks at the argument list (one string) and matches it to the constructor with that exact signature. This matching happens at </span>**compile time**<span data-type="text" style="background-color: var(--b3-card-info-background); color: var(--b3-card-info-color);">, not runtime.</span>
- **The problem with that version**​: <span data-type="text" style="background-color: var(--b3-card-error-background); color: var(--b3-card-error-color);">each constructor duplicates field-assignment logic</span>. If you ever change how `age` gets set, you'd have to update it in multiple places.

‍

#### `this`!

similar concepts: inheritance, using `base`

- use the `this` keyword in similar position...
- observe that constructor 1 and 2 are using Constructor 3, since 3 is setting the values for both fields.

```cs
public class Person {
  private static int DEFAULT_AGE = 21;
  private string name;
  private int age;
  
  public Person() : this("Jeff", DEFAULT_AGE) { }

  public Person(string name): this(name, DEFAULT_AGE) { }

  public Person(string name, int age)
  {
    this.name = name;
    this.age = age;
  }
} 
```

‍

### { 🦑+ } refactored example - C# statements

 { refactored code to run C# statements correctly: }

```cs
Person p1 = new Person();
Person p2 = new Person("Matt");
Person p3 = new Person("Cat", 33);

Console.WriteLine($"{p1.name}, {p1.age}"); // output: Jeff, 21
Console.WriteLine($"{p2.name}, {p2.age}"); // output: Matt, 21
Console.WriteLine($"{p3.name}, {p3.age}"); // output: Cat, 33

public class Person
{
    public static int DEFAULT_AGE = 21;
    public string name;
    public int age;

    public Person()
    {
        name = "Jeff";
        age = DEFAULT_AGE;
    }

    public Person(string name)
    {
        this.name = name;
        this.age = DEFAULT_AGE;
    }

    public Person(string name, int age)
    {
        this.name = name;
        this.age = age;
    }
}
```

‍

with comments

```cs
// overloading in action
// calls the no-arg constructor where field values are hardcoded
Person p1 = new Person();
// compiler sees one string arg --> matches it with 2nd constructor definition on line 25
Person p2 = new Person("Matt");
// compiler sees two args matching the 3rd constructor --> both fields get set directly from the args
Person p3 = new Person("Cat", 33);

// strring interpolation pulls each instance's public fields straight into the string
// Console.WriteLine calls can access public fields from the Person class directly via dot notation; no getter methods needed here
Console.WriteLine($"{p1.name}, {p1.age}"); // output: Jeff, 21
Console.WriteLine($"{p2.name}, {p2.age}"); // output: Matt, 21
Console.WriteLine($"{p3.name}, {p3.age}"); // output: Cat, 33

// defines the blueprint
public class Person
{
    // could be private fields --> would not be accessible outside of the class
    public static int DEFAULT_AGE = 21; // static means the field belongs to the class itself, not to any individual instance
    public string name;
    public int age;

    // each constructor has a different signature --> C# resolves which one to call based on the args list
    // 1st constructor
    public Person()
    {
        name = "Jeff";
        age = DEFAULT_AGE;
    }
    // 2nd constructor
    public Person(string name)
    {
        this.name = name;
        this.age = DEFAULT_AGE;
    }
    // 3rd constructor
    public Person(string name, int age)
    {
        this.name = name;
        this.age = age;
    }
}
```

‍

#### 3.2.1 Line-by-Line Breakdown

- **​`Person p1 = new Person();`​** 

  - Calls the no-arg constructor. Inside it, `name`​ is hardcoded to `"Jeff"`​ and `age`​ gets set to `DEFAULT_AGE`​ (21). `p1`​ now points to a `Person` object with those values.
- **​`Person p2 = new Person("Matt");`​** 

  - Compiler sees one `string`​ argument, so it matches the `Person(string name)`​ constructor. `this.name = name`​ sets `name`​ to `"Matt"`​, and `age`​ defaults to `DEFAULT_AGE` (21).
- **​`Person p3 = new Person("Cat", 33);`​** 

  - Two arguments (`string`​, `int`​) match the `Person(string name, int age)`​ constructor. Both fields get set directly from the args: `name = "Cat"`​, `age = 33`.
- **​`Console.WriteLine($"{p1.name}, {p1.age}");`​** 

  - String interpolation (the `$"..."`​ syntax) pulls `p1`​'s public fields straight into the string. Prints `Jeff, 21`.
- **​`Console.WriteLine($"{p2.name}, {p2.age}");`​** 

  - Same idea, reads `p2`​'s fields. Prints `Matt, 21`.
- **​`Console.WriteLine($"{p3.name}, {p3.age}");`​** 

  - Reads `p3`​'s fields. Prints `Cat, 33`.
- **​`public class Person { ... }`​** 

  - Defines the blueprint. Since `name`​ and `age`​ are `public`​, code outside the class (like your `Console.WriteLine` calls) can access them directly via dot notation — no getter methods needed here.
- **​`public static int DEFAULT_AGE = 21;`​** 

  - `static`​ means this field belongs to the ​*class itself*​, not to any individual instance. All three `Person`​ objects share the exact same `DEFAULT_AGE` — there's only one copy in memory, not one per object. That's why every constructor can reference it without needing an instance first.
- **The three constructors**

  - Each has a different signature (no params / one `string`​ / `string`​ + `int`), so C# resolves which one to call based on the argument list at the call site — this is the overloading in action you saw earlier.
- **Quick Ruby-lens note**: the `static`​ field here is like Ruby's class-level variables/constants (`@@default_age`​ or a constant `DEFAULT_AGE`​) — shared across all instances rather than duplicated per object. And `$"{p1.name}, {p1.age}"`​ is C#'s string interpolation, functionally the same as Ruby's `"#{p1.name}, #{p1.age}"`.

‍

### `this (...)` for constructor chaining

‍

#### syntax

```cs
public Person() : this("Jeff", DEFAULT_AGE) { }
```

‍

#### refactoring the class code above:

```cs
public class Person
{
    private static int DEFAULT_AGE = 21;
    private string name;
    private int age;

    public Person() : this("Jeff", DEFAULT_AGE) { }

    public Person(string name) : this(name, DEFAULT_AGE) { }

    public Person(string name, int age)
    {
        this.name = name;
        this.age = age;
    }
}
```

Explained:

- isn't just "the current instance" here;
- when `this`​ is used right after the constructor's parameter list with `:`, it means 👄 "call another constructor on this same class first"
- runs the *targeted* constructor's body **before** the current constructor's own body runs (though here the bodies are empty, so nothing extra happens after)

‍

👣 tracing. step by step, breakdown:

- line 7: `Person()`​ calls `this("Jeff", DEFAULT_AGE)` --> forwards to the 2-arg constructor (line 11)
- line 9: `Person(string name)`​ calls `this(name, DEFAULT_AGE)` --> also forwards to the 2-arg constructor
- line 11: `Person(string name, int age)` is the "base" constructor that actually does the field assignment. All roads lead here.

‍

end result:

- one single source of truth for initialisation logic.
- change it once, everything else benefits (as opposed to the previous version which had **duplication** of field-assignment logic, e.g. changing how `age`​ gets set in one place means it has to be manually updated in multiple places. #notyet#

‍

#### memo

- related but different: `base(...)`​ works the same way but calls a constructor on the **parent class** instead of another constructor in the same class --> useful in inheritance chains

‍

## extra conceptual

new, not yet, nevertheless... these things.

- private vs public fields
- duplication
- base constructor: `base(...)` and inheritance chains { Ruby equivalent: "super constructor" }
- ["targeted constructor's body"](appendxck/language%20precision.md#20260709154958-z8dhuth)

‍

### ([private vs public fields](appendxck/everything,%20or%20a%20glossary%20of%20mostly%20everything,%20aka%20please%20explain%20all%20these%20things%20to%20me.md#20260709150120-27azof9))

> ### 7.1.1 public vs private fields
>
> - **​`public`​**​ — accessible from ​*anywhere*​: inside the class, from other classes, from your `Main`​ method, wherever. That's why in your earlier version, `Console.WriteLine($"{p1.name}, {p1.age}")` worked fine — outside code could reach in and read the fields directly via dot notation.
> - **​`private`​**​ — only accessible from *inside the class itself*. No other class (including your `Main`​/top-level code where `p1`​, `p2`​, `p3`​ live) can touch `name`​ or `age` directly.

- **This actually breaks your current code.**  You've marked `name`​ and `age`​ as `private`​, but you're still doing `p1.name`​ and `p1.age`​ in `Console.WriteLine`​ from outside the class. That won't compile anymore — you'd get an error like `'Person.name' is inaccessible due to its protection level`. Your comment even flags this ("could be private... would not be accessible outside of the class") but the rest of the code hasn't caught up to that change yet.
- **How you'd actually fix it**​: add a way for outside code to *read* the private fields without letting them be freely overwritten. Two common options:

  - **Properties** (the idiomatic C# way):

    ```csharp
    private string name;public string Name => name; // read-only property, exposes value safely
    ```

    Then outside code uses `p1.Name`​ (capital N) instead of `p1.name`.
  - **Getter methods** (more explicit, closer to what you might do in other languages):

    ```csharp
    public string GetName() { return name; }
    ```

    Then you'd call `p1.GetName()`.
- **Why bother with** **​`private`​**​ **at all if it's more work?**  — This is ​**encapsulation**​, one of the core OOP principles. Keeping fields `private` and controlling access through properties/methods means:

  - You can validate data before it's set (e.g. reject a negative `age`).
  - You can change the internal implementation later without breaking code that depends on the class.
  - You prevent outside code from randomly mutating state in ways that leave the object in a broken/invalid condition.

‍

### duplication

‍

# 4. TT504 Properties and Encapsulation

‍

# 5. TT505 Exercises

‍

# 6. TT506 Review

‍
