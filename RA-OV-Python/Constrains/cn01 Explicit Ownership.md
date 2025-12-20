## Intent

Establish a strict rule for determining who owns data, who is allowed to mutate it,
and where responsibility for copying or transforming data lies.

### Explanation

This constraint exists to prevent:
- Accidental mutation across boundaries
- Hidden shared state
- Implicit coupling between functions
- Costly bugs caused by unclear responsibility

It encodes a single principle:

> **If ownership is not explicit, mutation is forbidden.**

This constraint is the foundation for:

- Predictable behavior
- Safe composition
- Scalable reasoning under complexity

---------

## The Rule

A function, closure, or module may mutate data **only if**:

1. It explicitly owns that data, OR
2. Ownership has been explicitly transferred to it, OR
3. The mutation is explicitly documented as part of the function’s contract

Otherwise, the data must be treated as read-only and transformed via copying.

### Explanation

This rule removes _interpretation_ from the equation.
- “Implicit permission” does not exist
- “Probably safe” does not exist
- “It works” is irrelevant

Ownership must be **provable from the call site** or from the function’s contract.

If you cannot answer _who owns this data_ in one sentence, mutation is illegal.

This applies equally to:
- Lists
- Dicts
- Sets
- Custom objects
- Captured variables in closures

--------

## Ownership Categories

Data falls into exactly one of the following categories:

### 1. Locally Created Data
- Created inside the function or closure
- Ownership is exclusive
- Mutation is always allowed

### 2. Explicitly Transferred Data
- Passed with a documented expectation of mutation
- Caller relinquishes ownership
- Mutation is allowed and expected

### 3. Borrowed Data
- Passed without transfer of ownership
- Must be treated as read-only
- Mutation is forbidden

### 4. Shared Global Data
- Accessible from multiple scopes or modules
- Mutation is forbidden by default

### Explanation

This classification eliminates ambiguity:

- **Locally created** → full control
- **Explicitly transferred** → controlled mutation
- **Borrowed** → transform, do not mutate
- **Shared global** → assume danger unless proven otherwise

Most bugs come from **misclassifying borrowed data as owned**.

This constraint forces you to make that classification explicit.

---------

## Practical Implications

- Function arguments are **borrowed by default**
- Mutation of arguments requires explicit documentation
- Copying is preferred over defensive guessing
- Closures must not mutate captured variables unless ownership is clear
- Public APIs must clearly state ownership expectations

### Explanation

This section answers the question:

> “What do I actually do when writing code?”

Key operational defaults:
- Assume **no ownership** unless proven otherwise
- Treat inputs as immutable views
- Make mutation visible, deliberate, and local
- Use transformation pipelines whenever possible

This aligns directly with:
- Functional-style thinking
- Safe list processing
- Defensive closures
- Predictable refactoring

This constraint also prevents the ORM-style illusion you described earlier:  
no boundary may hide mutation or cost.

-------

## Mental Checklist

Before mutating any data, ask:

1. Where was this data created?
2. Who currently owns it?
3. Was ownership explicitly transferred?
4. Would a caller be surprised by this mutation?
5. Would copying make the intent clearer?

If any answer is unclear, mutation is forbidden.

### Explanation

This checklist turns the constraint into a **habit**, not a guideline.

It forces:
- Cost awareness
- Boundary awareness
- Responsibility clarity

Over time, this becomes automatic:
- You will _feel_ when mutation is wrong
- Refactors become safer
- Code becomes easier to reason about months later

This constraint is the **keystone** for everything that follows:
- Patterns
- Concepts
- OOP (later)
- Performance-sensitive systems

---

