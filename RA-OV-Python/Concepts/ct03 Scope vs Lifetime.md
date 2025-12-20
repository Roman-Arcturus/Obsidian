# Scope vs Lifetime

## What it is

Scope defines where a name is visible in the code; it determines which parts of a program can access a given name.

Lifetime defines how long an object exists in memory; it determines when the object is created and destroyed.

In Python, scope and lifetime are related but distinct: a name can go out of scope while the object it referenced continues to exist if other references remain.

### Explanation

- This section defines **two foundational terms** in Python: scope and lifetime.
- Distinguishing them is critical for reasoning about closures, function arguments, loops, and object references.
- Note the emphasis: scope is **visibility**, lifetime is **existence**.

---
## Why it’s useful

Distinguishing scope from lifetime allows you to:

- Predict when a variable name will be accessible.  
- Understand how closures retain access to objects even after their defining scope ends.  
- Reason about function arguments and returned objects.  
- Identify potential memory retention issues with lingering references.  
- Debug unexpected behaviors related to name visibility and object persistence.

Without this understanding, code involving closures, loops, or shared objects appears inconsistent and error-prone.

### Explanation
- Each bullet is **tied directly to Python behavior** that previously caused confusion.
- Emphasizes **practical reasoning** over memorizing rules.
- Sets the stage for patterns like **Defensive Closures** and safe pipeline design.

---
## Unified mental model

Scope controls **where a name can be used**; lifetime controls **how long an object exists**.

A name can go out of scope while its object remains alive if other references exist.

Closures capture **names**, not values, which allows objects to persist beyond their defining scope.

Mutable objects may be shared across scopes, so mutations can be observed even after the original scope ends.

Understanding the separation of scope and lifetime allows reasoning about visibility, object persistence, and side effects.

### Explanation

- Short, precise, and **directly actionable**.
- Highlights the key distinction: **visibility vs existence**.
- Connects directly to prior concepts:
    - **Names and Binding** → closures capture names
    - **Mutation vs Transformation** → shared mutables persist beyond scope
    - **Ownership and Boundaries** → explains when mutations can escape a scope
    
- This model allows predicting **when variables are accessible and when objects exist**, which is critical for safe functional-style code.

----
## Consequences

- A name is only accessible within its scope; using it outside raises a NameError.  
- An object continues to exist as long as at least one reference to it exists, even if its defining scope has ended.  
- Closures retain access to the objects referenced by captured names, extending their lifetime beyond the original scope.  
- Loop variables are rebound in each iteration, but captured references may persist if stored elsewhere.  
- Returning a mutable object from a function gives the caller access to the same object, potentially creating side effects.  

### Explanation

- Each bullet **derives directly** from the unified mental model.
- Translates theory into **predictable, actionable rules**.
- Directly supports safe patterns like **Defensive Closures** and **Pure Data Pipelines**:
    - You now know exactly why closures can access objects after the function exits.
    - You can reason about side effects when returning mutable objects.

---
## Common misconceptions

- A variable disappears immediately when its scope ends.  
- Objects always die when the function that created them returns.  
- Closures capture values, not names.  
- Loop variables create new objects automatically each iteration.  
- Returning a mutable object always creates a copy for the caller.  

### Explanation

- Each item is a **false intuition** that programmers often hold.
- Corrections to these misconceptions live in:
    - Unified mental model
    - Consequences section
    
- Reinforcement example:
    - “Closures capture values, not names” → corrected by remembering that closures capture **names**, which may reference mutable objects persisting beyond scope.


---
## Links

### Concepts
- [[ct01 Names and Binding]]
- [[ct02 Mutation vs Transformation]]
- [[ct05 Ownership and Boundaries]]
- [[ct04 Identity vs Equality]]

### Patterns
- [[pt01 Defensive Closures]]
- [[pt02 Pure Data Pipelines]]

### Examples
- Closure Retaining Loop Variables
- Returned Mutable Objects

----
