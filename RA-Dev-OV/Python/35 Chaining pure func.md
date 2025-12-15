Chaining multiple pure transformations while handling nested data safely

## Core idea: flatten your thinking before you flatten your data

### What it is

When dealing with nested data (lists of dicts, dicts containing lists, etc.), the primary risk is **accidental mutation** and **implicit assumptions** about structure.

The correct functional approach in Python is:
1. **Extract only what you need**
2. **Transform it into a simpler shape**
3. **Apply further transformations on the simplified data**

Each step is:
- A pure function
- Explicit in intent
- Easy to test independently

---

### Why it’s useful

Nested data:
- Increases cognitive load
- Encourages defensive mutation
- Makes pipelines fragile

By flattening _conceptually_ (not structurally) early:
- Each function does one thing
- Failures are localized
- Composition remains safe

---
### Unified mental model

> Treat nested data as a **foreign object**.  
> Extract → normalize → operate.

Do **not** carry complex structures deeper into the pipeline than necessary.

---
## Example: processing nested orders

> [!example]
> 35_chaining_puref.py

## Topic: Defensive extraction from nested structures (without mutation)

### What it is

Real-world nested data is often:
- Incomplete
- Inconsistent
- Partially trusted

Defensive functional style means:
- **Validate at the boundary**
- **Normalize early**
- **Propagate simple, safe values inward**

No mutation. No exceptions for control flow. No hidden defaults.

---
### Why it’s useful

Without defensive extraction:
- Pipelines become fragile
- Errors appear deep in unrelated code
- Debugging cost increases exponentially

With defensive extraction:
- Failures are localized
- Later stages can assume invariants
- Composition remains safe

---
### Unified mental model

> **Trust decreases with depth.**  
> The deeper the data, the earlier you normalize it.

---
## Example: Orders with optional discounts

### Input data (realistic, imperfect)

> [!Example]
> 35_chaining_puref.py

## Why this pattern works
- Missing keys are handled **once**
- Later stages do not branch defensively
- Each function has a single responsibility
- No mutation, no hidden coupling

---
## Design rule to internalize

> **Every function establishes and guarantees its own invariants.**

Downstream code should never wonder:
- “Is this key present?”
- “Is this value normalized?”

---
