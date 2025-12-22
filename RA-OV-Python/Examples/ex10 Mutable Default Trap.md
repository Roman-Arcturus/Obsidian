## Intent

Demonstrate the dangers of using mutable default arguments in Python functions,
and show safe alternatives to prevent unintended shared state.

### Explanation

- Focuses on a **common Python pitfall**: default argument values are evaluated once at function definition, not at call time.
    
- Highlights why **mutable defaults (like lists or dicts) can lead to unexpected behavior**.
    
- Reinforces mental models:
    - Names and Binding
    - Ownership and Boundaries
    - Mutation vs Transformation

---

## Concept / Pattern Target

- Mutable Default Trap  
- Defensive Closures  
- Names and Binding  
- Ownership and Boundaries

### Explanation

- **Mutable Default Trap**: Core focus — illustrates how default arguments can introduce shared mutable state across function calls.
    
- **Defensive Closures**: Shows strategies to safely initialize values, avoiding accidental mutation.
    
- **Names and Binding**: Explains why the default argument object is **shared across calls**.
    
- **Ownership and Boundaries**: Highlights how lack of ownership clarity can propagate unintended changes.

This ensures the example demonstrates both the **problem** and the **pattern for safe resolution**.

---

## Setup / Input

- Function intended to collect items in a list:

def add_item(item, collection=[]):
    collection.append(item)
    return collection

- Goal: observe behavior when calling `add_item` multiple times without explicitly passing `collection`.  
- Demonstrates shared mutable state due to default argument evaluation at definition time.

### Explanation

- The setup isolates **the mutable default trap** clearly and concisely.
- `collection=[]` is **evaluated once**, creating a single list object shared across all calls that don’t pass `collection`.
- This prepares for showing both the **problematic behavior** and **safe alternative**.

-------

## Code Execution

```python
## Code Execution

# Mutable Default Trap
def add_item_trap(item, collection=[]):
    collection.append(item)
    return collection

result1 = add_item_trap(1)
result2 = add_item_trap(2)
result3 = add_item_trap(3)

# Safe alternative using None as default
def add_item_safe(item, collection=None):
    if collection is None:
        collection = []
    collection.append(item)
    return collection

safe1 = add_item_safe(1)
safe2 = add_item_safe(2)
safe3 = add_item_safe(3)

print("Mutable default trap results:", result1, result2, result3)
print("Safe alternative results:", safe1, safe2, safe3)

```

### Explanation

- **Mutable Default Trap**:
    - `collection=[]` is created **once at function definition**.
    - All calls without an explicit argument share the same list.
    - Shows **unexpected accumulation of items**.
    
- **Safe Alternative**:
    - Using `None` as the default ensures **a new list is created on each call**.
    - Avoids shared mutable state, preserving **ownership boundaries**.
    
- Demonstrates a **common Python pitfall** and a **pattern for safe practice**.

---

## Output / Behavior

```
Mutable default trap results: [1, 2, 3] [1, 2, 3] [1, 2, 3]
Safe alternative results: [1] [2] [3]
```

### Explanation

- **Mutable default trap**:
    - All calls share the same list object created at function definition.
    - Each `append` modifies the same list, so all results reflect the **accumulated items**.
    - Demonstrates **why mutable defaults lead to unintended shared state**.
    
- **Safe alternative**:
    - Each call creates a new list if `collection` is `None`.
    - Results are **independent**, reflecting proper **ownership boundaries**.

**Key insight**:

> Mutable default arguments create **hidden shared state**. Using `None` and initializing inside the function is a safe pattern.  
> Reinforces concepts: **Ownership and Boundaries**, **Names and Binding**, **Defensive Closures**.

---

## Lessons Learned / Notes

- Default argument values are **evaluated once at function definition**, not at call time.  
- Mutable defaults (lists, dicts, sets) can produce **shared state across calls**, causing unintended side effects.  
- Safe pattern:
  - Use `None` as the default
  - Initialize a new object inside the function if needed
- Reinforces ownership and boundary principles:
  - Each function call receives its own data
  - No accidental mutation of shared objects
- Links to prior concepts/patterns:
  - [[pt01 Defensive Closures]]  
  - [[ct01 Names and Binding]]  
  - [[ct05 Ownership and Boundaries]]  
  - [[ct02 Mutation vs Transformation]]

### Explanation

- Converts a **common Python pitfall** into a concrete lesson and safe pattern.
    
- Reinforces mental models of **ownership, mutation, and safe data handling**.
    
- Prepares the learner to write **predictable, side-effect-free functions** in Python.

---
