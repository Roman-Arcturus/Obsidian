# Defensive Closures

## Intent

Create closures that capture the intended value of variables at the time of closure creation, avoiding late-binding surprises.

### Explanation
- This is **purpose-driven**: it states what the pattern solves, not how.
- Notice the focus: **capturing intended values**, not merely “fixing a bug.”

---

## Concepts Involved

- Names and Binding
- Scope vs Lifetime
- Mutation vs Transformation
- Ownership and Boundaries

### Explanation

- Each concept listed here is **directly leveraged** by the pattern.
- This section allows you to trace why the pattern works:
    - **Names and Binding** → explains why closures capture references, not values
    - **Scope vs Lifetime** → explains which variables are visible and how long they exist
    - **Mutation vs Transformation** → guides whether the captured object can change unexpectedly
    - **Ownership and Boundaries** → informs whether the closure can safely mutate its captured objects
- This reinforces the **concept-to-pattern mapping** principle in your vault.

---
## Problem Description

When defining a closure inside a loop or a function, the closure may capture a name rather than its current value.

As a result:
- All closures reference the same final value
- Unexpected mutations occur if the captured object is mutable
- Debugging is difficult because behavior appears inconsistent with intent

### Explanation
- This section is **about reasoning, not syntax**.
- It describes the **pain point** without giving the solution yet.
- Every bullet is a concrete manifestation of the **Names and Binding** concept:
    - “All closures reference the same final value” → late binding of names
    - “Unexpected mutations occur if the captured object is mutable” → mutation vs transformation issue
    - “Debugging is difficult…” → consequence of misapplied ownership and boundaries
- At this stage, the reader should be able to **predict the problem** in code they have already written.

---

## Solution

Bind the intended value to a new local name or immutable container at the time the closure is created.

Options include:
- Using default arguments to capture the current value
- Wrapping mutable objects in an immutable container (e.g., tuple) before capturing
- Explicitly passing the value as a parameter to an inner function

This ensures that each closure has an independent reference to the intended object, avoiding late-binding surprises.

### Explanation

- This section **translates concepts into strategies**:
    - **Names and Binding** → explains why capturing a new name works
    - **Scope vs Lifetime** → shows how default arguments or parameters extend the captured lifetime
    - **Mutation vs Transformation** → using immutables prevents shared mutation
    - **Ownership and Boundaries** → ensures each closure has a safe, independent reference
- Notice we **do not show actual code yet**. The mental model comes first.

---

## When to Use

- When creating closures inside loops or dynamically generated functions
- When the closure must capture the current value of a variable
- When mutable objects are involved and independent references are required

## When Not to Use

- When the closure does not depend on the loop or dynamic variable
- When the captured object is immutable and shared references are acceptable
- When performance overhead of creating new bindings is critical and unnecessary

### Explanation

- **When to Use** enforces the **intent alignment**: only apply the pattern if the problem exists.
- **When Not to Use** prevents unnecessary complexity, maintaining clarity.
- Each bullet **directly references your concepts**, reinforcing the mental model.
- Notice: this section defines the **decision boundary**, which is key for reusable patterns.

---

## Links

### Concepts
- [[ct01 Names and Binding]]
- [[ct03 Scope vs Lifetime]]
- [[ct02 Mutation vs Transformation]]
- [[ct05 Ownership and Boundaries]]

### Patterns
- [[pt02 Pure Data Pipelines]]

### Examples
- [[ex09 Late Binding Bug]]
- [[ex10 Mutable Default Trap]]

-------

