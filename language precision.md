# language precision

##### <span id="20260709154958-z8dhuth" style="display: none;"></span>1. targeted constructor's body

-  **"Targeted constructor"**  just means *the specific constructor you're pointing to* with `this(...)`​ or `base(...)`​. When you write `: this("Jeff", DEFAULT_AGE)`​, you're explicitly telling C# "run the constructor whose signature matches `(string, int)`​" — that one is the ​*target*.
-  **"Body"**  \= the code inside the constructor's curly braces `{ }`. Every constructor has one, even if it's empty.
- **Concrete walkthrough** using your `Person` example:

```csharp
public Person() : this("Jeff", DEFAULT_AGE) { }

public Person(string name, int age)
{
    this.name = name;
    this.age = age;
}
```

- Here, `Person(string, int)`​ is the ​**targeted constructor**.
- Its **body** is `{ this.name = name; this.age = age; }` — that's the actual executable code.
- When you call `new Person()`:

  1. C# sees `: this("Jeff", DEFAULT_AGE)`​ and jumps to the 2-arg constructor ​**first**.
  2. It runs that constructor's body — `this.name = name`​ and `this.age = age`​ — using `"Jeff"`​ and `DEFAULT_AGE` as the values.
  3. Once that body finishes executing, control returns to `Person()`.
  4. *Then* `Person()`​'s own body runs — but in this case it's just `{ }`, empty, so nothing else happens.
- **Why this distinction matters**​: the chaining (`: this(...)`​) happens *before* the current constructor's body, no matter what. So if `Person()`​'s body wasn't empty — say it had `Console.WriteLine("Creating default person");`​ — that line would run **after** the 2-arg constructor's body finished, not before.
- **Order of execution summary**:

  1. `: this(...)`​ or `: base(...)` clause runs first (jumps to and fully executes the targeted constructor's body).
  2. Current constructor's own body runs second.
- **Ruby comparison**​: there's no direct equivalent since Ruby only has one `initialize` per class. The closest mental model is calling one method from inside another — e.g., a default-arg wrapper method calling a more specific one — except here it's baked into the language's constructor syntax rather than something you write manually inside the method body.
