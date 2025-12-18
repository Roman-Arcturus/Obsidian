# Identity vs Equality
## What it is

Equality (`==`) determines whether two objects have the same value.  

Identity (`is`) determines whether two names reference the exact same object in memory.  

In Python, two objects can be equal but not identical, or identical but not equal if custom equality methods are defined.

---
## Why it’s useful

Understanding identity and equality allows you to:

- Predict the behavior of comparisons and conditional logic.  
- Correctly handle mutable objects that may appear equal but are distinct.  
- Avoid unintended side effects when multiple names reference the same object.  
- Reason about caching, memoization, and object reuse.  
- Understand the behavior of built-in collections and custom equality methods.

Without this understanding, code involving comparisons, shared objects, or caching can produce subtle and confusing bugs.

---
## Unified mental model

Equality (`==`) answers the question: "Do these objects have the same value?"  

Identity (`is`) answers the question: "Do these names point to the exact same object?"  

Mutable objects can be equal without being identical; changes to one object may affect other references if they are identical.  

Immutable objects may be identical for optimization purposes (interning), but this is implementation-dependent and should not be relied upon for correctness.

---
## Consequences

- `==` compares values; `is` compares object identity.  
- Two objects can be equal (`==`) but not identical (`is`).  
- Two names can be identical (`is`) but have values that differ over time if one is mutable.  
- Comparison of mutable objects should consider both equality and potential shared references.  
- Immutable objects may sometimes be identical due to interning, but code should rely only on `==` for equality checks.  
- Using `is` for singleton checks (e.g., `None`) is safe and idiomatic; avoid `is` for general equality.

---
## Common misconceptions

- `==` and `is` are interchangeable.  
- Two objects that are equal are always identical.  
- Two identical objects cannot change independently.  
- Immutable objects cannot be shared across names.  
- Using `is` for value comparisons is safe in all cases.

---
## Links

### Concepts
- [[ct01 Names and Binding]]
- [[ct02 Mutation vs Transformation]]
- [[ct03 Scope vs Lifetime]]
- [[ct05 Ownership and Boundaries]]

### Patterns
- [[pt01 Defensive Closures]]
- [[pt02 Pure Data Pipelines]]

### Examples
- Mutable Object Comparison
- Singleton Checks with None

---