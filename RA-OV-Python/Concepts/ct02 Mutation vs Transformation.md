# Mutation vs Transformation

## What it is

Mutation is the process of changing an existing object in place.

Transformation is the process of creating a new object based on an existing one, leaving the original unchanged.

In Python, mutable objects (lists, dicts, sets) can be mutated.
Immutable objects (tuples, strings, numbers) cannot be mutated, so all operations on them are transformations.

Understanding the distinction helps reason about side effects, composability, and functional-style code.

### Explanation
- This section defines **core terms** and immediately differentiates them.
- Notice the connection to Python’s type system (mutable vs immutable).
- This sets the stage for reasoning about pure functions, pipelines, and defensive programming.


---
## Why it’s useful

Understanding mutation vs transformation allows you to reason about:

- Predictability: Pure transformations avoid hidden side effects
- Composability: Functions that transform data can be chained safely
- Defensive programming: Avoid accidental changes to shared objects
- Performance trade-offs: Knowing when mutation is acceptable versus when it introduces risk
- Debugging: Easier to trace changes when side effects are explicit or avoided

Without this understanding, code appears inconsistent or produces subtle, hard-to-diagnose bugs.

### Explanation

- Each bullet is **directly tied to the mental model**: mutation = side effect; transformation = independence.
- This section **justifies** the concept; it explains _why you must internalize it_.
- Note how it also sets up **patterns** like pure pipelines or defensive copies.

---
## Unified mental model

Think of every operation as either:

1. Changing an existing object (mutation)  
2. Producing a new object based on the original (transformation)

Mutable objects allow mutation; immutable objects only allow transformation.

Mutations affect all references to the same object.  
Transformations create independent objects, leaving the original intact.

Safe functional-style code favors transformations, especially when objects are shared.

### Explanation
- Short, precise, and **directly actionable**.
- This model answers **“what happens and why”** without referencing specific syntax.
- Key insights:
    - Mutation = shared history changes → side effects
    - Transformation = new history → composability
    - Reference behavior is consistent with **Names and Binding**
- This model forms the backbone for patterns like pure pipelines and defensive copies.

----
## Consequences

- Mutating a mutable object affects all names bound to it.
- Transforming an object leaves the original unchanged.
- Functions that mutate inputs are impure and have side effects.
- Functions that transform inputs are pure and can be safely composed.
- Passing mutable objects into multiple functions requires caution to avoid unintended changes.
- Immutable objects can be safely shared; mutable objects require ownership or defensive copying.

### Explanation

- Each bullet is **directly derived** from the unified mental model.
- This section translates theory into **predictable behavior** for code design and debugging.
- Highlights the **interaction with Names and Binding** and **Ownership and Boundaries**.
- Focus on recognizing patterns:
    - If a function modifies its argument → it is mutating.
    - If it returns a new object → it is transforming.

---
## Common misconceptions

- All operations create new objects.  
- Mutable objects are safe to share freely.  
- Functions that modify objects are always faster or better.  
- Immutable objects cannot be used in practical computations.  
- Transformation is always slower than mutation.  

### Explanation

- Each item is a **false intuition** that many programmers hold.
- This section exists as a **defensive tool**: you can quickly identify flawed reasoning.
- Corrections to these misconceptions live in:
    - Unified mental model
    - Consequences section

- Example of reinforcement:
    - “Mutable objects are safe to share” → violates Ownership and Boundaries; may produce unintended side effects.


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

### Explanation

- **Concepts**: Shows which mental models this concept interacts with.
- **Patterns**: Points to reusable strategies that rely on or reinforce mutation/transformation distinctions.
- **Examples**: Connects to concrete code snippets illustrating mutation or transformation behaviors.
- This finalizes the **Mutation vs Transformation** concept note, fully integrated into your vault.

---
