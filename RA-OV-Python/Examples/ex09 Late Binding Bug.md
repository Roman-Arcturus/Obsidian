## Intent

Demonstrate the common late-binding issue in closures, where loop variables
or mutable references are captured incorrectly, leading to unexpected behavior.

### Explanation

- Focuses on a **classic closure pitfall** in Python: closures capturing names rather than values.
    
- Highlights why **loop variables in list comprehensions or closures** often produce identical results.
    
- Reinforces mental models:
    - Names and Binding
    - Scope vs Lifetime
    - Defensive Closures

---

## Concept / Pattern Target

- Late Binding Bug  
- [[pt01 Defensive Closures]]  
- [[ct01 Names and Binding]]  
- [[ct03 Scope vs Lifetime]]

### Explanation

- **Late Binding Bug**: Core focus — explains why closures capture variable references rather than values.
- **Defensive Closures**: Shows how to prevent the bug with safe capture strategies.
- **Names and Binding**: Highlights the difference between name binding and object values.
- **Scope vs Lifetime**: Illustrates how closures extend variable lifetimes, affecting captured values.

This ensures the example teaches both the **problem** and **pattern for safe resolution**.

-----

## Setup / Input

- A list of integers: `numbers = [1, 2, 3]`  
- Goal: create a list of functions, each returning `numbers[i] * 2`  
- Intentional pitfall:
  - Using the loop variable `i` directly in the closure, leading to late binding  
- This setup allows comparison with a defensive closure solution

### Explanation

- `numbers` provides a simple, clear dataset for illustration.
    
- Using the loop variable `i` in the closure **demonstrates the problem directly**.
    
- Prepares the learner to see why all closures may return the same result without defensive measures.

---

## Code / Execution

```
## Code / Execution

numbers = [1, 2, 3]

# Late-binding bug: loop variable captured directly
funcs_bug = [lambda: numbers[i] * 2 for i in range(len(numbers))]

# Defensive closure: capture current value via default argument
funcs_safe = [lambda x=numbers[i]: x * 2 for i in range(len(numbers))]

# Execute functions
bug_results = [f() for f in funcs_bug]
safe_results = [f() for f in funcs_safe]

print("Late-binding bug results:", bug_results)
print("Defensive closure results:", safe_results)
```

### Explanation

- **Late-binding bug**:
    - All lambdas reference the same `i` variable, which ends at its final value (`2`).
    - Every function produces the same, unexpected result.
    
- **Defensive closure**:
    - Captures the current value of `numbers[i]` via the default argument `x=numbers[i]`.
    - Each lambda retains its intended value.
    
- Demonstrates **why defensive closures are necessary** when creating functions in loops.

---
## Output / Behavior

Late-binding bug results: [6, 6, 6]
Defensive closure results: [2, 4, 6]

### Explanation

- **Late-binding bug**:
    - All functions return `numbers[2] * 2 = 6` because `i` was captured by reference.
    - Illustrates why **closures capture names, not values**, leading to unexpected results in loops.
    
- **Defensive closure**:
    - Each function correctly returns its doubled value because `x` captures the current loop value.

**Key insight**:

> Loop variables in closures can lead to late-binding bugs; defensive strategies (like default arguments) prevent unintended sharing of references.  
> Reinforces concepts: **Defensive Closures**, **Names and Binding**, and **Scope vs Lifetime**.

----

## Lessons Learned / Notes

- Closures capture **names**, not values; loop variables in closures are a common source of late-binding bugs.  
- Defensive strategies include:
  - Using default arguments to capture the current value
  - Avoiding mutation of captured variables
- Understanding **scope vs lifetime** ensures closures retain intended objects safely.  
- This pattern reinforces:
  - [[pt01 Defensive Closures]]  
  - [[ct01 Names and Binding]]  
  - [[ct03 Scope vs Lifetime]]  
- Applying defensive closures is essential for predictable, side-effect-free higher-order functions.

### Explanation

- Converts a common **pitfall into a learning point and pattern**.
    
- Shows **practical application** for safely building closures in loops.
    
- Links directly to prior mental models around **binding, scope, and ownership**, reinforcing the broader knowledge structure.

---

