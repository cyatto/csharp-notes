# everything, or a glossary of mostly everything, aka please explain all these things to me

# 1. Terms - language precision

‍

### <span id="20260709140726-krktr6u" style="display: none;"></span>1.1 call site

the specific place in the code where a method is called/invoked (as opposed to where a method is defined)

- C#: the compiler does real work *at the call site* (matching argument types to overload signatures)
- "the compiler looks at the call site to resolve overloads" ≡ it looks at *how the method call was actually written* to figure out which version of the method to run

‍

# 2. Core concepts

‍

## 2.1 Static typing vs duck typing

‍

### 2.1.1 Static typing (C#)

Every variable, parameter, and return value has a ​**fixed type declared in the code**​, and the compiler checks all of it ​*before your program ever runs*.

csharp

```csharp
int a = 5;
string b = "hello";
a = b; // ❌ compile error — caught immediately, program won't even build
```

- **Step by step:**  the compiler reads your code, knows `a`​ is an `int`​ forever, sees you're trying to shove a `string`​ into it, and refuses to compile. You find out about the bug *before* you ever run the program.
- <span data-type="text" style="background-color: var(--b3-card-success-background); color: var(--b3-card-success-color);">This is also exactly why overload resolution works the way it does — the compiler needs to know types in advance to pick the right method.</span>

‍

### 2.1.2 Duck typing (Ruby)

Ruby doesn't check types ahead of time. It just tries to run whatever you wrote, and figures out at that *exact moment* whether the object can do what you're asking. The name comes from "if it walks like a duck and quacks like a duck, treat it like a duck" — Ruby doesn't care what *class* something is, only whether it responds to the method you're calling.

ruby

```ruby
def add(a, b)
  a + b
end

add(1, 2)        # => 3
add("hi", "bye")  # => "hibye"   (strings respond to +)
add(1, "bye")     # 💥 crashes, but only when this line actually runs
```

- **Step by step:**  Ruby doesn't inspect `add`​'s parameters ahead of time. It runs the method, hits `a + b`​, and only *then* checks "does `1`​ respond to `+`​ with a `"bye"` argument?" It doesn't — boom, runtime error. If that line of code never executes, you'd never find out it was broken.

‍

### 2.1.3 Why this matters for what you just learned

This is exactly why C# needs overloading as a *language feature* and Ruby doesn't:

||Ruby|C#|
| ---------------------------------------------| ------------------------------------------| ------------------------------------------|
|When are types checked?|At runtime, only when code executes|At compile time, before running anything|
|Can one method name handle different types?|Yes, automatically (duck typing)|Only via explicit overloads|
|Catch a type mismatch bug|Only if you happen to run that code path|Compiler blocks the build|
|Cost|Flexible, less boilerplate|Slower to write, but errors caught early|

‍

So in Ruby, `def add(a, b); a + b; end`​ just *works* for ints, strings, arrays — anything with a `+`​ method — with zero extra code. In C#, if you want `Add`​ to work for `int`​ and `float`​ and `string`, you have to write out separate overloads, because the compiler needs to know in advance exactly which version to compile against.

One nuance worth knowing: <span data-type="text" style="background-color: var(--b3-card-success-background); color: var(--b3-card-success-color);">C# does have a runtime-typed escape hatch too</span> — `dynamic`​ — which behaves much more like Ruby (type errors show up only when that code runs). <span data-type="text" style="background-color: var(--b3-card-success-background); color: var(--b3-card-success-color);">But it's rarely used because it throws away the whole point of static typing</span>.

‍

# <span id="20260624183008-8itvsx1" style="display: none;"></span>3. Data types

‍

## 3.1 reference types vs value types

‍

### 3.1.1 value types

Value types **hold their data directly**.

Examples: `int`​, `float`​, `double`​, `bool`​, `char`​, `struct`.

```cs
int a = 5;
int b = a;   // b gets a COPY of 5
b = 10;      // changing b does NOT affect a
Console.WriteLine(a);  // still 5
```

- **Step by step:**

  - ELI5: `a`​ is a little box containing the value `5`​. When you assign `b = a`​, C# copies that value into a *new* box. `a`​ and `b` are now totally independent — no connection.
  - Intermediate: `a`​ holds the value `5`​ directly in its own spot in memory. When you write `b = a`​, C# duplicates that value and gives `b`​ its own separate copy — it's not linking to `a`​ in any way, just cloning the number. Since `a`​ and `b` now each have their own independent copy, changing one has zero effect on the other.
  - Advanced: `a`​ is a stack-allocated storage location holding the literal value `5`​ directly. The assignment `b = a`​ performs a bitwise copy of that value into a distinct storage location for `b`​. Because each variable owns its own independent memory, there is no aliasing between them — mutating `b`​ afterward has no effect on `a`, since they no longer share any underlying state.

‍

### 3.1.2 Reference types

Hold an **address pointing to data stored elsewhere** (on the heap).

Examples: `int[]`​, `string`​, `object`​, any `class`.

csharp

```csharp
int[] a = new int[] { 1, 2, 3 };
int[] b = a;      // b gets a COPY of the ADDRESS, not the array itself
b[0] = 99;
Console.WriteLine(a[0]);  // prints 99 — because a and b point to the SAME array!
```

- **Step by step:**  `a`​ doesn't contain `{1, 2, 3}`​ directly — it contains a *pointer* to where that array lives in memory. `b = a`​ copies the pointer, not the array. So `a`​ and `b`​ are two names pointing at the *same* underlying data. Change it through one, and you see it through the other.

‍

### 3.1.3 Why `null` only applies to reference types

- **This is exactly why the casting example used** **​`int[]`​** ​ **and not just** **​`int`​**​ **.**  `null`​ means "this pointer points at nothing." Value types like `int`​ don't have a pointer at all — they *are* the data — so `int`​ can never be `null`​ (in normal C#; there's a special `int?` nullable wrapper for that, but that's a separate topic).
- Arrays (`int[]`​), strings, and classes are reference types, so `null` is a valid, normal value for them.

‍

### 3.1.4 Why you'd use reference types

- **Big or complex data you don't want copied everywhere** — arrays, collections, objects with lots of fields. Copying the whole thing every assignment would be wasteful; copying just the pointer is cheap.
- **Shared mutable state** — if multiple parts of your program need to see and modify the *same* object (like a `Player` object being updated by different game systems), you want them all pointing at the same instance, not separate copies.
- **Custom objects (classes)**  — anything with identity and behavior, not just a raw value, is naturally a reference type.

‍

## 3.2 the STACK vs the HEAP

C# programs use two different regions of memory: the **stack** and the **heap**.

Where a piece of data lives depends on whether it's a value type or reference type.

‍

### 3.2.1 The stack

Fast, organized, temporary storage tied to the exact method call you're in.

csharp

```csharp
void DoSomething()
{
    int a = 5;   // 'a' lives on the stack
}  // when this method returns, 'a' is gone — stack space is reclaimed instantly
```

- **Step by step:**  the stack works like a stack of trays — memory gets added on top when a method starts, and removed off the top the instant that method finishes. It's predictable and cheap because it's just "add to top / remove from top," no searching required.
- Value types (`int`​, `float`​, `bool`​, `struct`) live here directly, right where the variable is declared — that's why copying them is cheap and fast.

‍

### 3.2.2 The heap

A big, more loosely-organized pool of memory that isn't tied to any one method call.

csharp

```csharp
int[] a = new int[] { 1, 2, 3 };
```

- **Step by step:**  the `new int[] {1, 2, 3}`​ part actually allocates the array's data *on the heap* — not inside the method's stack space. The variable `a`​ itself lives on the stack, but what it *holds* is just an address (a pointer) pointing over to where that array data actually sits on the heap.
- Heap memory isn't automatically cleared when a method ends. It sticks around as long as *something, somewhere* still has a reference (pointer) to it. Once nothing points to it anymore, C#'s garbage collector eventually cleans it up in the background.

‍

### 3.2.3 Why this split exists

- The heap lets multiple variables (even across different methods) point at and share the *same* underlying object — that's literally how reference types achieve their "shared state" behavior from the last question.
- The stack is faster but temporary and method-scoped; the heap is slower to allocate but flexible and long-lived, which is exactly what you want for big or shared objects like arrays and class instances.

‍

## 3.3 re: Method overloading

‍

### 3.3.1 core idea

- most literals in C# carry their own type with them, automatically, just by how they're written
- `null` does not.

  - it is a special literal that means "no object" and it can point at **any** reference type
  - on its own, it doesn't say *which* reference type it is

- unambiguous literals *force* a specific overload because their type is baked in; no ambiguity is possible.

‍

```cs
var a = 5;      // compiler knows this is an int, just by looking at it
var b = "hi";   // compiler knows this is a string, just by looking at it
var c = null;  // ❌ compiler error - "cannot infer type of null"
```

👣 step by step:

- line 1: `5`​ is unambiguously an `int` literal;
- line 2: `"hi"`​ is unambiguously a `string` literal;
- line 3: `null` isn't a value of any particular type; it is more like a placeholder meaning "nothing"

  - it is compatible with `int[]`​, `float[]`​, `string`, or literally any other reference type.
  - the compiler cannot guess which reference type is meant unless explicitly told / unless something else tells it

‍

### 3.3.2 application to overloading

```cs
int[] CrossProduct(int[] a, int[] b);
int[] CrossProduct(float[] a, float[] b);

CrossProduct(null, null);   // ❌ compiler error --> ambiguous!
```

👣 step by step:

- the compiler tries to match the call to a signature
- looks at arguments and asks "what type are these?" null cannot answer this question on its own, because it's agreeable to either answer (i.e. `null`​ could be either `int[]`​ or `float[]`
- since both overloads are equally valid candidates, the compiler cannot break the tie, throwing a compiler error (ambiguous call) rather than guessing.

‍

#### 3.3.2.1 solution: casting gives a `null` type

```cs
CrossProduct((int[])null, (int[])null);
```

👣 step by step:

- the cast `(int[])null`​ explicitly says "this `null`​ is specifically an `int[]`-typed null"
- now the argument *does* carry type information
- the compiler matches it against the `int[], int[]` overload with no ambiguity

‍

### <span id="20260709121757-02532je" style="display: none;"></span>3.3.3 Casting

- tells the compiler to treat a value as a specific type by either converting it, or, for reference types, relabeling what type it is being viewed as

```cs
(int[])null
```

👣 step by step:

- the syntax `(Type)value` is a cast expression:

  - `null`​ is wrapped in parentheses with `int[]` prefixed
- ·.༄࿔ 👄🗣️ 💬 => "I want the compiler to treat this `null`​ as having the type `int[]`​—not `float[]`​, not `string`​—`int[]`​ and `int[]` only!"
- without the cast, `null` is typeless; it is compatible with any reference type but doesn't commit to one.
- with the cast, the above expression now has a concrete, known type at compile time

‍

### 3.3.4 flavours of casting

```cs
// 1. Numeric conversion - actually changes the value/representation
double d = 6.9;
int i = (int)d;       // i = 3 (truncates, actual conversion happens)

// 2. Reference type cast - no conversion, only changes what type it will be treated as
(int[])null;          // null is null either way - just telling compiler "view this as int[]"
```

- (1) With numbers, a cast can genuinely transform data
- (2) With `null`/reference types, there is no data to transform, since null is just "nothing"; instead, the cast is purely a compile-time label so that the compiler's type-checker has something to work with

‍

# 4. Variables

‍

# 5. Loops

‍

## 5.1 `while` loop

‍

### 5.1.1 Structure

‍

1/ Initialise variables that will be used in the conditional expression before the `while` loop begins.

2/ Parts of a `while` loop:

  2.a/ Use the `while`​ keyword followed by the conditional expression in parentheses `(...)`;

  2.b/ Within curly braces `{...}` (i.e. a block), write code to be executed if the condition is evaluated as true.

  2.c/ To avoid an infinite loop, ensure that the variable being checked in the conditional expression is updated (e.g. incremented or incremented) in some operation within the block body.

‍

```cs
while (conditional_expression)
{
    // do this
    // update variable used in condition to avoid an infinite loop
}
```

‍

### 5.1.2 Notes:

‍

1/ The condition is (re)evaluated at the beginning of each iteration.

2/ If the conditional expression evaluates as true, the body code is executed at the end of the iteration; if false, the loop ends and execution resumes after the `while` loop.

‍

## 5.2 `for` loop

‍

### 5.2.1 Structure

‍

A `for` loop, in three parts:

1/ Initialiser: initialise local variables

2/ Condition: conditional expression that is checked at the beginning of every iteration; if the expression evaluates as true, the loop continues; if it evaluates as false, the loop ends.

3/ Update: this expression/operation is performed at the end of every iteration (where the condition evaluates as true), after the block code executes.

‍

### 5.2.2 Notes

‍

1/ Any of the three parts can be left blank/skipped if not required; C# just requires the semicolons as placeholders.

  1.a/ `for ( ; x > 10; x++)`

2/ The block body can also be left empty; this is valid in C#, though it may read a little awkwardly.

‍

```cs
for (
```

‍

## 5.3 `while`​/`for` loop equivalence

‍

- loops can be thought of as "counting" upwards or downwards until some condition is met
- a `for`​ loop with an empty body could be described as "keep going while this condition holds" --> literally what `while`​ means, so maybe you're better off using a `while` loop instead of trying to squish everything into a one-ish liner.

‍

## 5.4 Code examples

‍

### 5.4.1 Example: even or odd

‍

while loop:

```cs
```

‍

for loop:

‍

‍

# 6. Methods

‍

### 6.0.1 Overloading

‍

### <span id="20260708220153-u5nzlkb" style="display: none;"></span>6.0.2 Method signature

‍

from the **Yellow Book (p. 206):**

A given C# method has a particular signature which allows it to be uniquely identified in a program. The signature is the name of the method and the type and order of the parameters to that method:

- `void Silly(int a, int b)` – has the signature of the name Silly and two int parameters.

- `void Silly(float a, int b)` – has the signature of the name Silly and an float parameter followed by an integer parameter.

This means that the code:

- `Silly(1, 2) ;` - would call the first method, whereas:

- `Silly(1.0f, 2) ;` - would call the second.

Note that the type of the method has no effect on the signature.

‍

---

‍

# 7. Classes

‍

## 7.1 fields

‍

### <span id="20260709150120-27azof9" style="display: none;"></span>7.1.1 public vs private fields

- **​`public`​**​ — accessible from ​*anywhere*​: inside the class, from other classes, from your `Main`​ method, wherever. That's why in your earlier version, `Console.WriteLine($"{p1.name}, {p1.age}")` worked fine — outside code could reach in and read the fields directly via dot notation.
- **​`private`​**​ — only accessible from *inside the class itself*. No other class (including your `Main`​/top-level code where `p1`​, `p2`​, `p3`​ live) can touch `name`​ or `age` directly.

‍

### 7.1.2 how to read private fields from outside the class

- **How you'd actually fix it**: add a way for outside code to *read* the private fields without letting them be freely overwritten.
- Two common options:

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

‍

### 7.1.3 encapsulation

#cs_todo#​ #cs_adv#​ #notyet#

- **Why bother with** **​`private`​**​ **at all if it's more work?**  — This is ​**encapsulation**​, one of the core OOP principles. Keeping fields `private` and controlling access through properties/methods means:

  - You can validate data before it's set (e.g. reject a negative `age`).
  - You can change the internal implementation later without breaking code that depends on the class.
  - You prevent outside code from randomly mutating state in ways that leave the object in a broken/invalid condition.
- **Ruby comparison**​: this maps closely to Ruby's `attr_accessor`​ / `attr_reader`​. In Ruby, instance variables (`@name`​) are *always* private by default — you can never access `@name`​ directly from outside the object. You'd write `attr_reader :name` to auto-generate a getter, similar to a C# property. C# just makes the public/private choice explicit per field rather than defaulting everything to private.

‍

### 7.1.4 public

‍

---

‍

# 8. not yet!!!!!!!! (aka extra credit homework etc)

#notyet#​ #cs_adv#​ #extraCredit#

‍

{ terms i've come across but have not YET studied fully... }

‍

list of concepts (extra credit homework etc)

- base constructors
- inheritance chains

‍

### 8.0.1 base constructors

"super constructor" isn't actually C# terminology. It's a bit of vocabulary that leaked in from Java (where the parent class is called the "superclass" and you call `super(...)` to invoke its constructor).

- **In C#, the equivalent concept is called a base constructor**​, and you invoke it with `base(...)`​ instead of `super(...)`.
- **What it means**​: when class `B`​ inherits from class `A`​, `B`​'s constructor can explicitly call one of `A`'s constructors before running its own body. That's the "base constructor" call.
- **Syntax**​, following the same pattern as `this(...)`:

csharp

```csharp
public class Animal
{
    protected string name;

    public Animal(string name)
    {
        this.name = name;
    }
}

public class Dog : Animal
{
    private string breed;

    public Dog(string name, string breed) : base(name)
    {
        this.breed = breed;
    }
}
```

- **How it works step by step**:

  - `Dog`​'s constructor takes `name`​ and `breed`.
  - `: base(name)`​ forwards `name`​ up to `Animal`​'s constructor, which sets `this.name`.
  - Once `Animal`​'s constructor finishes, control returns to `Dog`​'s constructor body, which sets `breed`.
- **Why it matters**​: fields declared in the parent class (like `name`​, which is `protected`​ so `Dog`​ can see it but outside code can't) usually need parent-controlled initialization logic. `base(...)` lets you reuse that logic instead of duplicating it in every subclass.
- **If you don't write** **​`base(...)`​** ​ **explicitly**​: C# automatically inserts a call to the parent's *parameterless* constructor before your constructor body runs. If the parent doesn't have one (e.g. it only has a constructor that requires args), you'll get a compile error unless you specify `base(...)` yourself.
- **Ruby comparison**​: this is basically what `super`​ does in Ruby — `super(name)`​ in a subclass's `initialize`​ calls the parent's `initialize`​. Same concept, different keyword (`base`​ vs `super`​), and C#'s version is tied specifically to constructors via that `: base(...)`​ syntax rather than being callable anywhere in the method body like Ruby's `super`.
