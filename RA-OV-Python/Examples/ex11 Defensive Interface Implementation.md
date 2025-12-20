## Intent

Demonstrate how to **apply Pattern 4 — Defensive Interface Implementation** in Python by:

- Explicitly validating inputs  
- Preserving ownership of mutable data  
- Preventing hidden side effects  
- Ensuring predictable outputs that respect the interface

### Explanation

- This example provides a **concrete scenario** for the pattern documented in pt04.
- Shows how to **protect function contracts** while performing common transformations on mutable data.
- Reinforces previous concepts and constraints:
    - [[ct06 Data Shape vs Data Meaning]]
    - [[ct07 Interfaces vs Implementation]]
    - [[cn01 Explicit Ownership]]
    - [[cn02 Functional Defaults]]
    - [[cn03 No Hidden Mutation]]
    - [[cn04 Evaluation Timing vs Lazy Mutation]]

----------

## Concept / Pattern Target

- **Pattern:**  
  - [[pt04 Defensive Interface Implementation]] — illustrates the application of the pattern in a real scenario  

- **Concepts:**  
  - [[ct06 Data Shape vs Data Meaning]] — ensuring input/output shape and meaning are preserved 
  - [[ct07 Interfaces vs Implementation]] — enforcing separation of promise vs mechanics  

- **Constraints:**  
  - [[cn01 Explicit Ownership]] — avoid mutating caller-owned data  
  - [[cn02 Functional Defaults]] — prevent hidden mutable state  
  - [[cn03 No Hidden Mutation]] — no implicit side effects  
  - [[cn04 Evaluation Timing vs Lazy Mutation]] — explicit evaluation and mutation control

--------

## Setup / Input

Scenario:

- A function receives a list of user records (dicts) containing:
  - `'name'` — string  
  - `'age'` — non-negative integer  

- Caller expectations (interface):
  - Input: list of user dicts  
  - Output: list of dicts representing adult users only  
  - No mutation of original data  

Input example:

```python
users = [
    {"name": "Alice", "age": 17},
    {"name": "Bob", "age": 23},
    {"name": "Charlie", "age": 19},
]
```

Constraints enforced:
- Ownership: function must not modify `users` directly
- Functional defaults: no hidden mutable state
- Lazy mutation: transformations are explicit and immediate

### Explanation

- Mirrors a **real-world scenario** where data flows between modules or pipelines.  
- Defines **shape and meaning** explicitly, reinforcing [[ct06 Data Shape vs Data Meaning]].  
- Provides a **controlled, safe setup** to demonstrate the pattern in Section 4.

---------

## Code / Execution

Applying Pattern 4 — Defensive Interface Implementation:

```python
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
    adults = [u for u in safe_users if u['age'] >= 18]
    return adults
```

- **Defensive copy**: prevents mutation of caller-owned `users`
- **Validation**: ensures input matches the expected interface (shape and meaning)
- **Transformation**: produces output consistent with interface contract, predictable and side-effect-free

### Explanation

- Shows **how the pattern is applied in a concrete scenario**.  
- Integrates constraints:
  - Ownership and mutation rules  
  - Functional defaults  
  - Lazy evaluation control  
- Ensures **pipeline and interface safety**, reinforcing mental models from Patterns pt04 and Concepts ct06–ct07.

-------

## Output / Behavior

```python
adults = filter_adults(users)
print(adults)
print(users)  # Original list remains unchanged
```

Expected output:
```python
[{'name': 'Bob', 'age': 23}, {'name': 'Charlie', 'age': 19}]
[{'name': 'Alice', 'age': 17}, {'name': 'Bob', 'age': 23}, {'name': 'Charlie', 'age': 19}]
```

Behavior explanation:
- `adults` contains only users with `age >= 18` — **interface contract fulfilled**
- Original `users` list remains unmodified — **ownership constraint honored**
- Transformation is explicit and immediate — **no hidden mutation**
- Shape and meaning of input and output are preserved — **pattern guarantees upheld**

### Explanation

- Confirms that **Defensive Interface Implementation** works as intended in practice.  
- Demonstrates integration of multiple constraints and concepts in a single function.  
- Provides a **predictable, safe function** that can be used reliably in pipelines or larger systems.

--------

## Lessons Learned / Notes

1. **Interface contracts matter**  
   - Functions must explicitly honor promised input/output shape, meaning, and mutation rules.

2. **Ownership enforcement prevents hidden bugs**  
   - Using defensive copies or immutables ensures callers’ data is safe.

3. **Validation clarifies shape and meaning**  
   - Explicit checks prevent silent semantic errors and enforce expectations.

4. **Transformation should be explicit and side-effect-free**  
   - Avoid hidden mutation or lazy evaluation that could violate the interface.

5. **Pattern applicability**  
   - Useful in pipelines, closures, or modules where functions operate on mutable or complex data.  
   - Integrates constraints and concepts previously learned:
     - [[cn01 Explicit Ownership]]  
     - [[cn02 Functional Defaults]]  
     - [[cn03 No Hidden Mutation]]  
     - [[cn04 Evaluation Timing vs Lazy Mutation]]  
     - [[ct06 Data Shape vs Data Meaning]]  
     - [[ct07 Interfaces vs Implementation]]

6. **Mental shortcut**: *“Promise → Mechanics → Safe Transformation”*
   - Check the promise (interface) first
   - Implement mechanics defensively
   - Transform safely without breaking the contract


### Explanation

- This section consolidates **practical takeaways** from applying the pattern.
- Reinforces your mental model for **predictable, safe, and composable code**. 
- Bridges **conceptual understanding and concrete implementation**, preparing for further patterns or pipelines.

----

