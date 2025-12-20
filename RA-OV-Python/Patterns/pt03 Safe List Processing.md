## Intent

Provide a disciplined way to process lists that avoids unintended mutation, preserves ownership boundaries, and produces predictable results.

### Explanation

This pattern exists because:
- Lists are **mutable**, widely shared, and frequently passed across boundaries.
- Accidental list mutation is one of the most common sources of subtle bugs in Python.
- Python gives you _many_ ways to process lists, but not all are safe in shared contexts.

The intent is **not** to ban mutation, but to:
- Make ownership explicit
- Localize mutation when it is required
- Prefer transformation at boundaries

This pattern is a **direct application** of:
- [[ct05 Ownership and Boundaries]]
- [[ct02 Mutation vs Transformation]]
- [[ct04 Identity vs Equality]]
- [[ct01 Names and Binding]]

---
## Concepts involved

- [[ct01 Names and Binding]]  
- [[ct02 Mutation vs Transformation]]  
- [[ct04 Identity vs Equality]]  
- [[ct05 Ownership and Boundaries]]

### Explanation

This pattern is not about lists per se; it is about **managing shared mutable identity**.

How each concept participates:

- **[[ct01 Names and Binding]]**  
    Multiple names may reference the same list. Processing a list safely requires awareness of shared bindings.
    
- **[[ct02 Mutation vs Transformation]]**  
    List operations may mutate in place or produce new lists. Choosing between them defines safety at boundaries.
    
- **[[ct04 Identity vs Equality]]**  
    Two lists may be equal but not identical; mutating one only affects shared identities.
    
- **[[ct05 Ownership and Boundaries]]**  
    Safe processing depends on whether the current scope _owns_ the list or merely _borrows_ it.

By explicitly grounding the pattern in these concepts, we ensure it remains **predictable, explainable, and scalable**.

---

## Problem description

Lists are frequently passed between functions, stored in data structures, and shared across scopes.  

Because lists are mutable and identity-based, a single in-place modification can affect multiple parts of a program that appear unrelated.  

The problem is not mutation itself, but mutation occurring across unclear ownership boundaries, leading to action at a distance.


### Explanation

This problem manifests in several common ways:

- A function “just iterates” a list but accidentally mutates it.
- A helper function modifies a list that the caller assumes is unchanged.
- A list stored in a structure is mutated elsewhere, violating assumptions.
- Debugging becomes difficult because the mutation happens far from the observed effect.

Key insight:

> The danger arises when **mutation crosses a boundary without an explicit ownership contract**.

This pattern does **not** try to eliminate mutation.  
It tries to **contain it**, so that cause and effect remain close.

---

## Solution

1. Determine ownership:
   - If the function owns the list, in-place mutation is allowed.  
   - If the function only borrows the list, do not mutate it.

2. Use transformation at boundaries:
   - Return a new list with the desired modifications instead of mutating borrowed inputs.

3. Localize mutation:
   - Confine in-place changes to private copies or internal lists that will not escape the function or module boundary.

4. Document behavior explicitly:
   - Clearly state whether inputs may be mutated or if new objects are returned.

5. Use Pythonic idioms for safety:
   - List comprehensions and `map` to create new lists.
   - Slice notation (`list[:]`) for local copies when necessary.

### Explanation

- Step 1 enforces **[[ct05 Ownership and Boundaries]]**: mutation is allowed only if you are the owner.
    
- Step 2 applies **[[ct02 Mutation vs Transformation]]**: prefer transformation when ownership is ambiguous.
    
- Step 3 uses **[[ct01 Names and Binding]]** awareness: prevent shared references from escaping.
    
- Step 4 and 5 provide **predictable, maintainable, and Pythonic** practices.
    

Key takeaway:

> Safe list processing is **a discipline of ownership, mutation control, and explicit intent**, not a set of syntax tricks.

---

## When to Use

- When a function receives a list whose ownership is ambiguous.  
- When multiple parts of the program need to safely observe the same list without unintended side effects.  
- When writing library or API functions where predictable behavior is critical.  
- When you want to enforce explicit contracts about mutation and returned values.

## When Not to Use

- When a list is strictly local and owned by the function; in-place mutation is safe without extra precautions.  
- When performance considerations make creating new lists unnecessarily costly, and ownership is unambiguous.  
- When immutable sequences (tuples) suffice; transformation patterns are simpler and safer.


---
## Links

### Concepts
- [[ct01 Names and Binding]]
- [[ct02 Mutation vs Transformation]]
- [[ct04 Identity vs Equality]]
- [[ct05 Ownership and Boundaries]]

### Patterns
- [[pt01 Defensive Closures]]
- [[pt02 Pure Data Pipelines]]

### Examples
- [[ex02 Mutation vs Transformation]]
- Local Copies for Safety
- Function Contracts for Mutation

------
