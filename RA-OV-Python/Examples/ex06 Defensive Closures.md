## Intent

Demonstrate how to create closures that safely capture variables, avoiding common pitfalls such as late binding and unintentional shared mutation.

### Explanation

- Focuses on **preventing common closure mistakes** that occur when variables are captured by reference rather than by value.
    
- Shows **defensive strategies**, such as:
    
    - Using function arguments to capture current values
        
    - Avoiding mutable shared state within closures
        
- Reinforces mental models:
    
    - **Names and Binding**
        
    - **Scope vs Lifetime**
        
    - **Ownership and Boundaries**

---

## Concept / Pattern Target

- Defensive Closures  
- Names and Binding  
- Scope vs Lifetime  
- Ownership and Boundaries


### Explanation

- **Defensive Closures**: Primary focus — strategies to safely capture variables in closures.
    
- **Names and Binding**: Explains how closure variables reference objects, not values, and why rebinding matters.
    
- **Scope vs Lifetime**: Shows how closures preserve object lifetimes.
    
- **Ownership and Boundaries**: Highlights safe handling of mutable and shared objects within closures.
    

This ensures the example demonstrates **both the problem and the safe pattern**.

---

## Setup / Input

- A list of integers: `numbers = [1, 2, 3]`  
- Goal: create a list of functions that return `numbers[i] * 2`  
- Intentional pitfalls:
  - Using loop variables directly in closures (late binding)  
  - Potential shared references to mutable objects

### Explanation

- The setup isolates **typical closure pitfalls** in a simple, reproducible scenario.
    
- `numbers` is mutable, allowing demonstration of mutation propagation if captured unsafely.
    
- Loop variables will show **why direct reference in a closure can cause unexpected results**, preparing for defensive strategies.

---

## Code / Execution

```
## Code / Execution

numbers = [1, 2, 3]

# Unsafe closure (late-binding)
funcs_unsafe = [
    lambda: numbers[i] * 2
    for i in range(len(numbers))
]

# Defensive closure using default argument
funcs_defensive = [
    (lambda x=numbers[i]: x * 2)
    for i in range(len(numbers))
]

# Testing
unsafe_results = [f() for f in funcs_unsafe]
defensive_results = [f() for f in funcs_defensive]

print("Unsafe results:", unsafe_results)
print("Defensive results:", defensive_results)

```

### Explanation

- **Unsafe closure**:
    - Uses the loop variable `i` directly.
    - All functions reference the same variable `i` (late binding).
    - When executed, all functions may produce the same unexpected result.
        
- **Defensive closure**:
    - Captures the current value of `numbers[i]` via a default argument `x=numbers[i]`.
    - Each function now safely remembers its intended value.
    
- This demonstrates **defensive programming** with closures without introducing mutation.

---

## Output / Behavior

Unsafe results: [6, 6, 6]
Defensive results: [2, 4, 6]

### Explanation

- **Unsafe results**:
    
    - All lambdas use the **same final value of `i`**, producing `numbers[2] * 2 = 6`.
        
    - This illustrates the **late-binding problem**: closures capture names, not values.
        
- **Defensive results**:
    
    - Each lambda captures its **own value** via the default argument `x=numbers[i]`.
        
    - Produces the correct doubled values `[2, 4, 6]`.
        

**Key insight**:

> Properly capturing values in closures prevents accidental sharing of loop variables or mutable state.  
> This is a cornerstone of **Defensive Closures** and safe functional-style Python code.


---

## Lessons Learned / Notes

- Closures capture **names**, not values; loop variables in closures are a common source of late-binding bugs.  
- Defensive strategies include:
  - Using default arguments to capture the current value
  - Avoiding mutation of captured variables
- Understanding **scope vs lifetime** ensures closures retain intended objects safely.  
- Applying defensive closures reinforces:
  - Names and Binding  
  - Scope vs Lifetime  
  - Ownership and Boundaries  
- This pattern is essential for predictable functional-style pipelines and safe higher-order functions.


### Explanation

- This example transforms a common _pitfall_ into a **pattern** for safe programming.
    
- Reinforces why mental models around **name binding, scope, and ownership** are critical.
    
- Supports building **robust, composable closures** without hidden side effects.

---

