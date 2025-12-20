## Intent

Ensure that functions, closures, and modules **honor their interfaces** by:

- Explicitly validating inputs and outputs
- Preventing hidden mutation or side effects
- Preserving shape and meaning
- Making contracts predictable for all callers

### Explanation

- Interfaces define what a function **promises**; this pattern enforces that promise.
- Defensive implementation prevents subtle bugs from:
    - Mutable arguments being modified unexpectedly
    - Deferred evaluation triggering hidden side effects
    - Misinterpreted shapes or semantic meaning of data
    
- Reinforces constraints:
    - [[cn01 Explicit Ownership]]
    - [[cn02 Functional Defaults]]
    - [[cn03 No Hidden Mutation]]
    - [[cn04 Evaluation Timing vs Lazy Mutation]]
    
- Integrates with prior concepts:
    - [[ct06 Data Shape vs Data Meaning]]
    - [[ct07 Interfaces vs Implementation]]

> Mental shortcut: _A function must never promise one thing and deliver another, even accidentally._


----------

## Concept / Pattern Target

- **Concepts:**
  - [[ct06 Data Shape vs Data Meaning]] — preserve shape and meaning across interface boundaries 
  - [[ct07 Interfaces vs Implementation]] — enforce separation of promise vs mechanics  

- **Constraints:**
  - [[cn01 Explicit Ownership]] — ensure the function owns or copies mutable inputs  
  - [[cn02 Functional Defaults]] — avoid hidden state or default mutation  
  - [[cn03 No Hidden Mutation]] — no implicit side effects  
  - [[cn04 Evaluation Timing vs Lazy Mutation]] — control when computation and mutation occur  

- **Purpose:**
  - Prevent accidental semantic errors
  - Make function pipelines predictable
  - Enforce defensive coding at the interface layer

### Explanation

- This pattern **operationalizes the concepts and constraints**: it provides concrete steps to ensure that interfaces are honored in Python code.
    
- It applies to functions, closures, and modules that operate on complex or mutable data.
    
- Think of this as **a safety net for any code that exposes a contract to callers**.

-------

## Problem Description

- Functions or modules sometimes **violate their promises** to callers:
  - Mutating inputs unexpectedly  
  - Returning data with wrong shape or meaning  
  - Deferring computation that triggers hidden side effects  
- Pipelines and higher-order functions can silently break if interface contracts are not honored  
- Predictability and composability are compromised when callers must reason about implementation details

## Solution

- Validate inputs explicitly at the function boundary  
- Copy mutable inputs or work with immutables to maintain ownership  
- Document input/output shape and meaning clearly  
- Ensure any deferred computation or transformation is explicit  
- Return data that respects the promised interface without exposing internal state  

Example (Pythonic implementation):

```
from copy import deepcopy
from typing import List, Dict

def filter_adults(users: List[Dict[str, int]]) -> List[Dict[str, int]]:
    # Defensive copy to preserve ownership
    safe_users = deepcopy(users)
    
    # Validate shape and meaning
    for u in safe_users:
        if not isinstance(u, dict):
            raise TypeError(f"Expected dict, got {type(u)}")
        if 'name' not in u or 'age' not in u:
            raise ValueError("Missing required keys")
        if not isinstance(u['age'], int) or u['age'] < 0:
            raise ValueError("Invalid age value")
    
    # Transformation without mutating original
    return [u for u in safe_users if u['age'] >= 18]
```

### Explanation

- This solution enforces **interface guarantees** while allowing implementation changes.  
- Copying prevents caller data mutation.  
- Validation ensures **shape and meaning** correctness.  
- Transformation is explicit, predictable, and side-effect-free.

---

## When to Use

- Any public function, closure, or module that:
  - Receives mutable or complex inputs  
  - Returns data that will be consumed by other functions  
  - Is part of a pipeline or higher-order function chain  
- Scenarios where interface correctness is critical to prevent semantic bugs

## When Not to Use

- Internal helper functions where caller trust is guaranteed  
- Extremely performance-sensitive code where copying/validation overhead is unacceptable  
- Simple, one-off scripts where interface enforcement provides minimal benefit

----

## Links

- Related Concepts:
  - [[ct06 Data Shape vs Data Meaning]]  
  - [[ct07 Interfaces vs Implementation]]  

- Related Constraints:
  - [[cn01 Explicit Ownership]]  
  - [[cn02 Functional Defaults]]  
  - [[cn03 No Hidden Mutation]]  
  - [[cn04 Evaluation Timing vs Lazy Mutation]]  

- Related Patterns:
  - [[pt02 Pure Data Pipelines]]  
  - [[pt03 Safe List Processing]]  

-----


