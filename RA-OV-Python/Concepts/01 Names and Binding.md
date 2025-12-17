# Names and Binding

## What it is

In Python, names are identifiers that are *bound* to objects.

A name is not a container and does not store a value.
It is a reference that points to an object that exists independently.

Assignment in Python does not copy values.
It creates or updates a binding between a name and an object.

Multiple names can be bound to the same object at the same time.

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

---
## Unified mental model

Think of a Python name as a label.

Objects exist independently.
Names are attached to objects.
Assignment moves the label; it does not alter the object.

If multiple labels point to the same object, a mutation through any label is visible through all others.

Nothing in Python “lives inside” a name.

---
## Consequences

```
## Consequences

Rebinding a name never affects other names, even if they previously referred to the same object.

Mutating an object affects all names bound to that object.

Function arguments are new local names bound to the same objects passed by the caller.

Closures capture names, not values.

Loop variables are rebound on each iteration, not recreated.

Understanding these consequences eliminates the need for special rules.
```

---
## Common misconceptions

---
## Links

