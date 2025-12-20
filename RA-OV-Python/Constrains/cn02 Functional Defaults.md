## Intent

Establish functional-style behavior as the default coding posture,
so that mutation, shared state, and side effects are opt-in rather than implicit.

### Explanation

This constraint exists to solve a very specific problem:

> Python makes mutation easy, cheap, and invisible.

Without a counterweight, this leads to:
- Accidental shared state
- Implicit coupling
- Time-dependent bugs
- Reasoning that degrades as code grows

**Functional Defaults** flips the baseline:

> Code should behave as if data were immutable unless explicitly stated otherwise.

This is not about ideology.  
It is about **predictability, composability, and reviewability**.

--------

## The Rule

All functions and closures must **treat inputs as immutable by default**:

1. Do not mutate arguments unless ownership is explicitly transferred (see Constraint 1).  
2. Return **new values** for any transformation.  
3. Avoid hidden state or side effects.  
4. Any mutation must be:
   - clearly documented, and  
   - explicitly justified as necessary.

### Explanation

- This rule enforces a **functional baseline**:
    - Inputs are “read-only views”
    - Outputs are new, independent values
    - Side effects are opt-in and visible
- Combines **Constraint 1 (Explicit Ownership)** with **functional reasoning**.
- Prepares you for **pure data pipelines**, **safe list processing**, and **defensive closures**.

It prevents subtle regressions where a function appears safe but mutates shared state behind the scenes.

-------

## Practical Implications

- All input arguments are treated as **read-only** unless ownership transfer is explicit.
- Transform data using:
  - comprehensions
  - `map` / `filter` / `sorted`
  - pure functions
- Avoid in-place methods like `.append()`, `.extend()`, `.update()`, `.pop()` on borrowed data.
- Use copying deliberately when mutation is unavoidable:
  - `new_list = old_list.copy()`
  - `new_dict = {**old_dict}`
- Side effects (logging, I/O, mutation) must be **obvious** at the call site.

### Explanation

- Ensures **predictable and reviewable code**.
- Enforces the mental model:
    - Inputs are stable
    - Outputs are predictable
    - Transformation is explicit
- Reduces cognitive load by **making the default safe**.

Functional defaults **complement Constraint 1** by preventing accidental mutation even when ownership is misunderstood.

-------

## Common Violations

1. Using mutable default arguments (lists, dicts, sets)
2. Modifying input arguments directly:
   - `list.append()` on a borrowed list
   - `dict.update()` on a shared dict
3. Side effects hidden inside helper functions:
   - Logging, caching, global mutation
4. Late-binding closure captures that mutate shared state
5. Implicit global state mutation

### Explanation

- Each violation demonstrates **how functional defaults can fail silently**.
- Violations often arise even in experienced teams because Python’s syntax _encourages_ mutation.
- Awareness of these patterns allows you to **audit code before subtle bugs appear**.
- This also bridges directly to the patterns you already documented:
    - [[pt01 Defensive Closures]]
    - [[pt02 Pure Data Pipelines]]
    - [[pt03 Safe List Processing]]

By systematically preventing these violations, you enforce **predictable, side-effect-free behavior**.

## Mental Checklist

Before modifying data or introducing side effects, ask:

1. Is this input owned by this function? (Constraint 1)  
2. Could this mutation be replaced by a transformation that returns a new value?  
3. Will this change affect code outside the function?  
4. Am I introducing hidden state or side effects?  
5. Could a closure capture this state and mutate it later?  

If the answer is “unclear” or “yes” to any question above, refactor to a pure functional style or explicitly document ownership/side effects.

### Explanation

- This checklist operationalizes **Functional Defaults** into daily coding habits.
- Encourages **automatic reasoning about state, mutation, and ownership**.
- Integrates with:
    - [[cn01 Explicit Ownership]]
    - Patterns like [[pt02 Pure Data Pipelines]] and [[pt03 Safe List Processing]]

Over time, these questions become reflexive, ensuring that **side effects and mutations are always deliberate** rather than accidental.

-------

