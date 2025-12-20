## Intent

Prevent all forms of hidden or implicit mutation in functions, closures, and modules,
ensuring that any side effect is explicit, visible, and deliberate.

### Explanation

- Python allows mutation in many places without obvious syntax (`list.sort()`, `dict.setdefault()`, closure captures, global variables).
    
- Hidden mutation breaks reasoning, makes debugging costly, and invalidates mental models like **Functional Defaults** and **Explicit Ownership**.
    
- This constraint ensures **predictable code behavior** and enforces **strong boundaries**.

--------

## The Rule

No function, closure, or module may mutate data, state, or objects **implicitly**.  
All mutation must be:

1. Explicitly declared in the function signature or documentation, AND  
2. Obvious at the call site, AND  
3. Limited to data that the function explicitly owns or has been transferred.

Any mutation not meeting these criteria is forbidden.

### Explanation

- Covers all forms of hidden mutation:
    - In-place list, dict, or set operations
    - Captured variables in closures
    - Global variables or module-level state
    - Lazy evaluation side effects
    
- Integrates directly with:
    - [[cn01 Explicit Ownership]]
    - [[cn02 Functional Defaults]]
    
- Enforces **predictability, composability, and traceability** in code.

----------

## Practical Implications

- Avoid in-place methods on borrowed data:
  - `list.append()`, `list.sort()`, `dict.update()`, `set.add()`
- Avoid modifying closure-captured variables unless explicitly documented
- Avoid module-level or global state mutation without clear ownership
- Any caching, logging, or I/O with side effects must be explicit and visible
- Prefer transformation pipelines and returned new objects over mutation
- Document all mutations in function signatures or docstrings

### Explanation

- Forces all side effects to be **deliberate and visible**.
- Ensures that code behavior is **auditable and predictable**.
- Reinforces the mental models you already have:
    - [[ct02 Mutation vs Transformation]]
    - [[ct05 Ownership and Boundaries]]
    - [[pt02 Pure Data Pipelines]]

By following these practices, hidden bugs due to implicit mutation become **mechanically impossible**, rather than relying on memory or vigilance.

-------

## Common Violations

1. In-place operations on arguments passed by reference:
   - `list.append()`, `list.extend()`, `dict.update()`
2. Late-binding closures modifying captured variables
3. Hidden mutation inside helper functions
4. Module-level caching that mutates shared objects
5. Implicit state mutation via properties, descriptors, or dynamic attributes
6. Lazy evaluation that mutates objects when accessed

### Explanation

- Each item represents a **frequent Python trap** that violates both functional defaults and ownership rules.
- Awareness allows **preventive audits** instead of reactive debugging.
- Directly connects to previously documented patterns:
    - [[pt01 Defensive Closures]]
    - [[pt02 Pure Data Pipelines]]
    - [[pt03 Safe List Processing]]

Following these rules ensures **all mutation is explicit, controlled, and predictable**.

---
## Mental Checklist

Before mutating data or state, ask:

1. Is this mutation explicit in the function signature or documentation?  
2. Will the mutation be visible at the call site?  
3. Do I own the data, or has ownership been explicitly transferred?  
4. Could a closure or global state be affected implicitly?  
5. Can this transformation be done by returning a new object instead of mutating?

If any answer is “no” or “unclear,” mutation is forbidden.

### Explanation

- Converts **theoretical rules** into **practical habit**.
- Ensures every mutation is:
    - Explicit
    - Controlled
    - Predictable
    
- Reinforces constraints:
    - [[cn01 Explicit Ownership]]
    - [[cn02 Functional Defaults]]
    
- Acts as a **preemptive guard** against hidden bugs and subtle side effects.

------

