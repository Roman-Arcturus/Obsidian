## Intent

Clarify the separation between **what a function, closure, or module promises to do** (its interface) and **how it fulfills that promise** (its implementation), so that code remains composable, maintainable, and predictable.

### Explanation

- **Interface**: The contract or expectations a caller relies upon, including:
    - Input shape and meaning
    - Output shape and meaning
    - Side effects or mutation rules
    
- **Implementation**: The internal mechanics that achieve the contract, which may change without affecting callers as long as the interface is preserved.

> Mental model: _Interface is the “what”; implementation is the “how.”_

- Benefits:
    - Prevents accidental dependency on internal behavior
    - Encourages refactoring without breaking code
    - Clarifies boundaries, which reinforces:
        - [[cn01 Explicit Ownership]]
        - [[cn02 Functional Defaults]]
        - [[cn03 No Hidden Mutation]]
        - [[cn04 Evaluation Timing vs Lazy Mutation]]
    - Supports **pure pipelines** and **safe closures**

-------

## Why It’s Useful

- Prevents callers from relying on **implementation details**, reducing brittle code.  
- Supports **refactoring**: internal changes can occur without breaking the system.  
- Encourages **clear contracts** for inputs, outputs, and side effects.  
- Facilitates **composability**: functions and modules can be combined safely when interfaces are respected.  
- Helps enforce constraints:
  - [[cn01 Explicit Ownership]]  
  - [[cn02 Functional Defaults]]  
  - [[cn03 No Hidden Mutation]]  
  - [[cn04 Evaluation Timing vs Lazy Mutation]]  
- Clarifies **mental separation** between “what is promised” and “how it is achieved,” reducing cognitive load when reasoning about pipelines, closures, and data transformations.

### Explanation

- Interfaces define **the expectations you must satisfy**, not the mechanisms used.
- Implementation details can change as long as the interface remains consistent.
- This prevents subtle bugs when:
    - Pipelines rely on implicit ordering
    - Closures capture mutable state
    - Evaluation timing varies
    
- Reinforces your ability to reason about **data shape, meaning, and ownership** independently from the underlying computation.

------

## Unified Mental Model

- **Interface = Contract**
  - Defines expectations for inputs and outputs
  - Specifies permissible side effects and mutation
  - Should be **stable and predictable** for all callers

- **Implementation = Mechanics**
  - How the function/module achieves its interface
  - Can change internally as long as the interface remains valid

- **Reasoning Approach**
  1. Always ask: “What does the caller expect?”
  2. Ask separately: “How am I implementing this internally?”
  3. Avoid exposing internal mechanics to callers
  4. Refactor freely as long as the interface guarantees hold

- **Integration with other concepts**
  - Shape and meaning [[ct06 Data Shape vs Data Meaning]] define part of the interface
  - Ownership and mutation rules [[cn01–cn04]] constrain implementation choices
  - Patterns like [[pt02 Pure Data Pipelines]] are built around interface guarantees

### Explanation

- Interfaces and implementation are **orthogonal**: reasoning about one does not require knowing the other in detail.
- Mental shortcut: _“Promise vs Mechanics.”_
- Enforcing this separation increases **modularity, predictability, and safety**, especially when combined with functional defaults and ownership rules.

-------

## Practical Implications

- **Always document interfaces** clearly:
  - Input shape and meaning
  - Output shape and meaning
  - Side effects or mutation rules
- **Do not rely on caller code knowing your internals**:
  - Avoid exposing temporary variables or internal data structures
- **Encapsulate implementation details**:
  - Use closures, helper functions, or private functions to hide mechanics
- **Refactor confidently**:
  - Implementation can change without breaking other code as long as the interface is preserved
- **Integrate with functional pipelines and constraints**:
  - Ensure ownership and mutation rules are enforced at the interface boundary
  - Preserve shape and meaning invariants across transformations

### Explanation

- Provides **practical guidance** for writing functions, modules, and pipelines that are robust and maintainable.
- Reduces accidental dependencies on **internal behavior**, preventing subtle bugs.
- Supports **composability and reasoning** in larger systems, especially when working with mutable structures and deferred evaluation.

> Mental shortcut: _Caller sees the promise; internals can evolve freely as long as the promise holds._

--------

## Common Pitfalls / Notes

1. **Leaking implementation details**:
   - Returning internal data structures that callers modify
   - Exposing mutable objects unnecessarily

2. **Assuming caller behavior**:
   - Implementations that rely on caller not mutating inputs
   - Functions that silently depend on external state

3. **Ignoring shape vs meaning**:
   - Interface defines input/output shape, but meaning is left implicit
   - Leads to semantic bugs in pipelines or transformations

4. **Hidden side effects in implementation**:
   - Mutating objects captured by closures
   - Lazy evaluation that triggers mutation outside function scope

5. **Mitigation**:
   - Explicit documentation of input/output and side effects
   - Defensive copies or immutable returns
   - Clearly separate interface checks from implementation logic
   - Combine with constraints:
     - [[cn01 Explicit Ownership]]
     - [[cn02 Functional Defaults]]
     - [[cn03 No Hidden Mutation]]
     - [[cn04 Evaluation Timing vs Lazy Mutation]]

### Explanation

- These pitfalls are subtle but common in Python, especially in flexible, mutable, or lazy contexts.
- Awareness allows you to **audit interfaces**, enforce contracts, and maintain **predictable behavior** across pipelines, closures, and modules.
- This concept prepares you for **Pattern 4** and more advanced patterns where interface guarantees are critical.

--------

