## Intent

Demonstrate the difference between a name’s scope (where it is visible)
and an object’s lifetime (how long it exists), and show why confusing the two
leads to incorrect reasoning about closures and functions.

### Explanation

- This example isolates a **frequent mental error**: assuming scope controls lifetime.
    
- It shows that:
    - names are resolved by scope rules
    - objects persist as long as they are referenced
    
- This example is foundational for understanding:
    - closures
    - returned functions
    - deferred execution

---

## Concept / Pattern Target

- Scope vs Lifetime  
- Names and Binding  
- Ownership and Boundaries  
- Defensive Closures

### Explanation

- **Scope vs Lifetime**: Core distinction — visibility of a name versus persistence of an object.
    
- **Names and Binding**: Explains why returned functions still “see” objects created in another scope.
    
- **Ownership and Boundaries**: Clarifies which function owns which objects after returning.
    
- **Defensive Closures**: Shows why closures are safe _when you understand what survives and why_.
    

This ensures the example is read as a **model correction**, not a trick.

---

## Setup / Input

- A function that:
  - Creates a local variable
  - Defines an inner function that uses that variable
  - Returns the inner function

- No global variables
- No mutation
- No external dependencies

### Explanation

- The variable created inside the outer function:
    - Has **local scope** (not visible outside)
    - Will outlive the function call if referenced by a closure
    
- Returning the inner function creates a clean experiment:
    - The outer function finishes execution
    - Its local scope is gone
    - Yet the object referenced by the closure may still exist
    

This setup forces us to confront the difference between:
- **Name visibility (scope)**
- **Object persistence (lifetime)**

---

## Code / Execution

```

def outer():
    value = 42  # local variable in outer scope

    def inner():
        return value  # references outer’s local variable

    return inner  # return the inner function

# Create closure
closure_func = outer()

# Call the inner function after outer has finished
result = closure_func()
print("Result:", result)
```

### Explanation

- `value` is **local to `outer`**, so its scope ends when `outer` returns.
    
- `inner` references `value`; returning `inner` creates a **closure** that keeps `value` alive.
    
- `closure_func()` demonstrates that the **object’s lifetime exceeds the scope** of the variable that created it.
    
- This example is fully **side-effect free**, focusing purely on scope vs lifetime.

---

## Output / Behavior

`Result: 42`

### Explanation

- The variable `value` still exists even though `outer()` has finished executing.
- The closure `inner` has **captured a reference** to the object `value`.
- Scope vs lifetime distinction:
    - **Scope**: `value` is not accessible directly outside `outer()`.
    - **Lifetime**: The object `42` persists because `inner` references it.

Key insight:

> Objects live as long as there are references to them; names may go out of scope without destroying the object.

This explains why closures in Python are safe **if you understand what survives and why**.

---

## Lessons Learned / Notes

- Scope determines **where names are visible**, lifetime determines **how long objects persist**.  
- A closure can extend the lifetime of objects beyond their original scope.  
- Returning a function that references local variables preserves those objects.  
- Understanding scope vs lifetime is essential for safe closures, functional composition, and avoiding accidental garbage collection.  
- This principle underpins patterns like **Defensive Closures** and **Pure Data Pipelines**.

### Explanation

- This example makes the abstract concepts **concrete and reproducible**.
- It trains your intuition to predict object persistence and understand closures safely.
- Connects directly to **ownership, mutation control, and functional patterns** in Python.

---

