## 1. Conceptual Priority

- **Constraints 1–3** are already well-established: ownership, functional defaults, no hidden mutation.

- Before worrying about evaluation timing (which often interacts with lazy evaluation, generators, and deferred mutation), you need to **understand how data should be represented and interpreted**.

> “Shape vs meaning” is the foundation for reasoning about collections, transformations, and ownership. Without it, reasoning about lazy evaluation or deferred mutation becomes abstract and brittle.

---

## 2. Logical Dependency

- Evaluation timing is **more advanced**: it requires understanding how Python executes code, when objects are created, and how deferred computation works.
    
- Data shape and meaning is **immediately applicable**: every function, closure, pattern, and example already uses lists, dicts, and tuples. Solidifying this mental model will make **lazy mutation, pipelines, and defensive patterns easier to reason about later**.

---

## 3. Practical Leverage

- Clarifying shape vs meaning lets you:
    - Decide when to use tuples, dicts, or objects
    - Avoid misusing mutable containers
    - Enforce contracts in functional pipelines
    
- Once this is solid, evaluation timing is easier because you can **see how deferred execution interacts with specific shapes and semantics**.

------

### Recommendation

1. **Next:** `ct06 Data Shape vs Data Meaning` (concept)
2. Afterward: `cn04 Evaluation Timing vs Lazy Mutation` (constraint)

This preserves **your hierarchical structure**: solidify concepts first, then tackle evaluation timing as a constraint that enforces safe behavior.

--------

