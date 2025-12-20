## Intent

Prevent subtle bugs and unpredictable behavior caused by deferred evaluation or delayed mutation.
Ensure that **when a value is computed or mutated** is explicit, predictable, and controlled.

### Explanation

- Python supports **lazy evaluation** through generators, iterators, comprehensions, and closure captures.
- Combined with **mutable data**, this can produce:
    - Hidden side effects
    - Order-dependent bugs
    - Ownership violations
- This constraint ensures that evaluation timing and mutation are **visible and deliberate**, not implicit.

> Mental model: _Compute or mutate only when you intend, never as a side effect of accessing a container or calling a function._

-------

## The Rule

1. No mutation or side effect may be deferred implicitly.  
2. All evaluation of data must be **explicit**:
   - Generators and iterators must not mutate underlying data silently.  
   - Lazy computations must be clearly documented and controlled.  
3. Any deferred computation or mutation must be:
   - Explicit in function signature or docstring, and  
   - Predictable in order and timing

### Explanation

- Covers common Python traps:
    - Deferred list or dict mutations via closures
    - Generator pipelines that mutate captured objects
    - Lazy evaluation that triggers unexpected side effects
    
- Integrates with:
    - [[cn01 Explicit Ownership]]
    - [[cn02 Functional Defaults]]
    - [[cn03 No Hidden Mutation]]
    
- Prevents subtle, time-dependent bugs that are difficult to debug or test.

-------

## Practical Implications

- Prefer **eager evaluation** for mutable data unless laziness is explicitly required.
- When using generators or iterators:
  - Avoid mutating the underlying collection while iterating
  - Clearly document what is computed lazily
- Avoid hidden mutations in comprehensions or chained pipelines
- Explicitly copy data before deferred transformations if mutation could occur
- Ensure closures do not mutate captured variables lazily
- Design APIs so that **evaluation order is predictable**

### Explanation

- These practices prevent **order-dependent and hidden side effects**.
- Ensures that both shape and meaning remain consistent throughout transformations (links to [[ct06 Data Shape vs Data Meaning]]).
- Supports **safe functional pipelines** and **defensive closures**.

> Mental shortcut: _If evaluation could surprise you or another caller, it must be made explicit._

-------

## Common Violations

1. Mutating a list, dict, or set inside a generator expression
2. Late-binding closures that modify captured variables only when invoked
3. Deferred side effects in comprehensions or map/filter pipelines
4. Relying on the order of evaluation in chained operations
5. Mutating shared or borrowed data during iteration over it
6. Lazy caching or memoization that modifies objects behind the scenes

### Explanation

- Each item represents a **frequent source of subtle, order-dependent bugs** in Python.
- Violations often arise even when the code “works” in small tests but fails in production.
- Awareness allows **defensive design**:
    - Explicit copying
    - Eager evaluation
    - Safe use of closures and generators
    
- Directly integrates with:
    - [[cn01 Explicit Ownership]]
    - [[cn02 Functional Defaults]]
    - [[cn03 No Hidden Mutation]]
    - [[ct06 Data Shape vs Data Meaning]]

--------

## Mental Checklist

Before using lazy evaluation or deferred computation, ask:

1. Will this evaluation mutate any existing data?  
2. Is the mutation explicit, visible, and documented?  
3. Could the evaluation order affect correctness or meaning?  
4. Are closures capturing mutable variables in a way that defers mutation?  
5. Would copying or eager evaluation make behavior clearer and safer?

If the answer is “unclear” or “yes” to any question above, refactor to make evaluation and mutation explicit.

### Explanation

- Converts the theoretical rules into **practical habits**.
- Ensures that lazy evaluation never **hides mutation or violates ownership/functional defaults**.
- Integrates with all previous constraints and concepts, especially:
    - [[cn01 Explicit Ownership]]
    - [[cn02 Functional Defaults]]
    - [[cn03 No Hidden Mutation]]
    - [[ct06 Data Shape vs Data Meaning]]

> Mental shortcut: _Evaluate or mutate only when you intend, visibly and safely._


--------
