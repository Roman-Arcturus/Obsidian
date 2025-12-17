# Phase 3 — First Concept: Names and Binding (We Do This Together)
### Why this one first

Because it underpins:
- mutation
- closures
- default arguments
- argument passing
- loop behavior

Nothing else should be written before this.

---
## What I Need From You Now

Do **only** this before replying:
1. Create the vault
2. Create the folders
3. Create the five empty concept notes
4. Create the meta rules note

Then reply with:

> “Phase 0–2 complete. Ready to fill the first concept.”

From that point on, I will guide every note line-by-line until the system is stable.

---
# Concept 1 — Names and Binding

**File:**  
`/Concepts/Names and Binding.md`

We will fill the sections in this exact order:
1. What it is
2. Why it’s useful
3. Unified mental model
4. Consequences
5. Common misconceptions
6. Links

No examples yet. Examples come later.

---
## 1. What it is

Insert the following text **exactly**, then read it carefully:
```
## What it is

In Python, names are identifiers that are *bound* to objects.

A name is not a container and does not store a value.
It is a reference that points to an object that exists independently.

Assignment in Python does not copy values.
It creates or updates a binding between a name and an object.

Multiple names can be bound to the same object at the same time.
```

### Explanation (do not paste this)
- This is intentionally minimal.
- Every word here is load-bearing.
- We have not mentioned variables, memory addresses, or stack/heap — on purpose.
### Unified mental anchor (for you, not the note)

> Python operates on **bindings**, not boxes.

---
## Stop Here

Before we continue, do the following:
1. Read the section twice
2. Ask yourself:
    - “Is there _anything_ here that implies a name contains data?”
    - “Does this align with what I observed in closures and mutation?”

Do **not** add examples.  
Do **not** rephrase.

When you are ready, reply with:

> “Section 1 accepted. Continue.”

We will then proceed to **Why it’s useful**, which is where this concept starts paying dividends.

---
## 2. Why it’s useful

Insert the following text **exactly** under the next heading:
```
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
```
### Explanation (do not paste this)

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
## Stop Here

Before continuing:

1. Read each bullet and mentally connect it to a real bug or surprise you’ve already encountered.
2. Especially check:
    - closure late binding
    - mutation through shared references

If every bullet maps to a concrete memory, the concept is anchoring correctly.

When ready, reply with:

> “Section 2 accepted. Continue.”

Next we will write the **Unified mental model**, which is the most important section in the entire vault.

---

We now write the **most important section** of this note.

This section must be short, memorable, and powerful enough to replace dozens of fragmented rules.

---
## 3. Unified mental model

Insert the following text **exactly**:
```
## Unified mental model

Think of a Python name as a label.

Objects exist independently.
Names are attached to objects.
Assignment moves the label; it does not alter the object.

If multiple labels point to the same object, a mutation through any label is visible through all others.

Nothing in Python “lives inside” a name.
```

### Explanation (do not paste this)

This model intentionally avoids:
- memory diagrams
- stack vs heap
- implementation details

Why?

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
## Stop Here

Read this section once out loud.

If you can explain _three different Python behaviors_ using only this model, it is correct.

When ready, reply with:

> “Section 3 accepted. Continue.”

Next we will enumerate **Consequences**, where this abstract model becomes operational rules.

Excellent. The mental model is now anchored.  
We will convert it into **operational consequences**—rules you can reliably apply without memorization.

---

## 4. Consequences

Insert the following text **exactly**:
```
## Consequences

Rebinding a name never affects other names, even if they previously referred to the same object.

Mutating an object affects all names bound to that object.

Function arguments are new local names bound to the same objects passed by the caller.

Closures capture names, not values.

Loop variables are rebound on each iteration, not recreated.

Understanding these consequences eliminates the need for special rules.
```
