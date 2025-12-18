# Mutation vs Transformation

## What it is

Mutation is the process of changing an existing object in place.

Transformation is the process of creating a new object based on an existing one, leaving the original unchanged.

In Python, mutable objects (lists, dicts, sets) can be mutated.
Immutable objects (tuples, strings, numbers) cannot be mutated, so all operations on them are transformations.

Understanding the distinction helps reason about side effects, composability, and functional-style code.

---
## Why it’s useful

Understanding mutation vs transformation allows you to reason about:

- Predictability: Pure transformations avoid hidden side effects
- Composability: Functions that transform data can be chained safely
- Defensive programming: Avoid accidental changes to shared objects
- Performance trade-offs: Knowing when mutation is acceptable versus when it introduces risk
- Debugging: Easier to trace changes when side effects are explicit or avoided

Without this understanding, code appears inconsistent or produces subtle, hard-to-diagnose bugs.

---
## Unified mental model

Think of every operation as either:

1. Changing an existing object (mutation)  
2. Producing a new object based on the original (transformation)

Mutable objects allow mutation; immutable objects only allow transformation.

Mutations affect all references to the same object.  
Transformations create independent objects, leaving the original intact.

Safe functional-style code favors transformations, especially when objects are shared.

----
## Consequences

- Mutating a mutable object affects all names bound to it.
- Transforming an object leaves the original unchanged.
- Functions that mutate inputs are impure and have side effects.
- Functions that transform inputs are pure and can be safely composed.
- Passing mutable objects into multiple functions requires caution to avoid unintended changes.
- Immutable objects can be safely shared; mutable objects require ownership or defensive copying.

---
## Common misconceptions

- All operations create new objects.  
- Mutable objects are safe to share freely.  
- Functions that modify objects are always faster or better.  
- Immutable objects cannot be used in practical computations.  
- Transformation is always slower than mutation.  

---
## Links

### Concepts
- [[ct01 Names and Binding]]
- [[ct03 Scope vs Lifetime]]
- [[ct05 Ownership and Boundaries]]
- [[ct04 Identity vs Equality]]

### Patterns
- [[pt02 Pure Data Pipelines]]
- [[pt01 Defensive Closures]]

### Examples
- List Append vs New List Creation
- Immutable String Transformations