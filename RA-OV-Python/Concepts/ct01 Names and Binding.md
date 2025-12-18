# Names and Binding

## What it is

In Python, names are identifiers that are *bound* to objects.

A name is not a container and does not store a value.
It is a reference that points to an object that exists independently.

Assignment in Python does not copy values.
It creates or updates a binding between a name and an object.

Multiple names can be bound to the same object at the same time.

### Explanation
- This is intentionally minimal.
- Every word here is load-bearing.
- We have not mentioned variables, memory addresses, or stack/heap — on purpose.

### Unified mental anchor

> Python operates on **bindings**, not boxes.


---
## Why it’s useful

Understanding names and binding explains behaviors in Python that otherwise appear inconsistent or surprising.

This model is required to correctly reason about:
- Assignment and reassignment
- Function argument passing
- Mutation of objects through multiple references
- Closure behavior and late binding
- Default argument evaluation
- Loop variable capture

Without this model, Python appears to have special cases.
With it, Python becomes internally consistent.

### Explanation

This section answers a very specific question:

> “What breaks if I _don’t_ understand this?”

Notice:
- We are not saying “helps with”
- We are saying “is required to reason about”

This frames the concept as **foundational**, not optional.

### Unified mental reinforcement

> Any confusion involving “why did this change over there?”  
> is almost always a binding question.

---
## Unified mental model

Think of a Python name as a label.

Objects exist independently.
Names are attached to objects.
Assignment moves the label; it does not alter the object.

If multiple labels point to the same object, a mutation through any label is visible through all others.

Nothing in Python “lives inside” a name.

### Explanation

This model intentionally avoids:
- memory diagrams
- stack vs heap
- implementation details

**Why?**
Because you are building a **portable reasoning model**, not a CPython internals map.

This model:
- works for lists and integers
- works for closures
- works for function arguments
- scales to every level of Python you will encounter

### Cross-check against your experience

This model should already explain:
- why rebinding inside a function doesn’t affect the caller
- why mutating a list argument does
- why closures “see the last value”

If it does not, stop and re-evaluate before moving on.

---
## Consequences

1. Rebinding a name never affects other names, even if they previously referred to the same object.

2. Mutating an object affects all names bound to that object.

3. Function arguments are new local names bound to the same objects passed by the caller.

4. Closures capture names, not values.

5. Loop variables are rebound on each iteration, not recreated.

6. Understanding these consequences eliminates the need for special rules.

### Explanation

This section does important work:
- It **derives behavior**, not lists quirks
- Each line is a direct consequence of the unified mental model
- There are no exceptions here—only misapplications

Pay special attention to these two lines:

- _“Function arguments are new local names bound to the same objects”_
- _“Closures capture names, not values”_

Together, they explain:

- argument passing semantics
- late binding
- mutable default traps

### Unified mental reinforcement

> If something “leaks” or “changes unexpectedly,”  
> you are either mutating a shared object or rebinding a name you didn’t mean to.

---
## Common misconceptions

1. Names are variables that store values.

2. Assignment copies data.

3. Function arguments are passed by value or by reference.

4. A closure freezes the value of a variable.

5. Each loop iteration creates a new variable.

6. Mutation and reassignment are the same operation.

### Explanation (do not paste this)

Every item here is a **false statement** that sounds reasonable—especially to someone with C, JS, or Pascal background.

This section exists so that:
- You can quickly detect faulty reasoning
- You can correct yourself without re-learning everything
- Future notes can link here instead of re-explaining

### Important discipline

Do **not** “fix” these statements in this section.  
They must remain wrong as written.

The correction lives in:
- the unified mental model
- the consequences section

This creates a **tension** that reinforces learning.

---
## Links

### Concepts
- [[ct02 Mutation vs Transformation]]
- [[ct03 Scope vs Lifetime]]
- [[ct04 Identity vs Equality]]
- [[ct05 Ownership and Boundaries]]

### Patterns
- [[pt01 Defensive Closures]]
- [[pt02 Pure Data Pipelines]]

### Examples
- [[ex10 Mutable Default Trap]]
- [[ex09 Late Binding Bug]]

### Explanation

- The **concepts** section points to other concepts this one interacts with.
- The **patterns** section points to notes where this concept is applied in reusable ways.
- The **examples** section points to illustrative edge cases or experiments.

This is the **graph-based thinking** Obsidian thrives on:  
concepts → patterns → examples → back to concepts.

-------
