# Call Semantics in Python

## Pass-By-Object, Mutability, and How Parameters Really Work

# 1. What This Is

When you call a function in Python, **you do not pass values**, and **you do not pass references** in the C++ sense.

Python passes **object references by value**.

Meaning:
- The _reference_ (pointer) to an object is **copied** into the function parameter.
- Both caller and function variables point to **the same object**.
- Assigning a new object to a parameter does **not** affect the caller’s variable.
- But **mutating** a shared object _does_ affect the caller’s object.

### Analogy from other languages

- Not like C pass-by-value (copies raw data).
- Not like C++ pass-by-reference (aliasing variables).
- **Closest to Java:** object references are passed by value, primitives behave like immutable objects.

# 2. Why This Matters

This determines:
- When changes inside a function affect the caller.
- Why lists/dicts behave differently from ints/strings when passed around.
- Why some bugs involve “unexpected mutations.”
- How to design clean, predictable APIs.

Understanding this gives you 100% clarity on:
- Mutability
- Side effects
- Defensive copying
- Functional vs. imperative styles

This is essential for writing **correct and pythonic** code.

# 3. The Core Rule

### Python’s call semantics:

**“The function receives references to objects, not references to variables.”**

Let’s break this down.

### Example 1: Immutable objects (int, float, str, tuple)
```
def increment(x):
    x = x + 1
    return x

a = 10
increment(a)
print(a)  # still 10
```

Why?
- `x = a` means `x` refers to the _same_ integer object.
- `x = x + 1` creates a **new integer object** and rebinds `x`.
- `a` still points to the old integer.

### Example 2: Mutable objects (list, dict, set)
```
def add_item(lst):
    lst.append(99)

nums = [1, 2, 3]
add_item(nums)
print(nums)  # [1, 2, 3, 99]
```

Why?
- `lst = nums` means `lst` refers to the **same list object**.
- `lst.append()` **mutates** the list.
- Caller sees the mutation.

### Example 3: Rebinding vs. Mutating
```
def replace_list(lst):
    lst = [9, 9, 9]   # rebinding (no effect outside)

def mutate_list(lst):
    lst.append(4)     # mutation (affects caller)

items = [1, 2, 3]
replace_list(items)
print(items)  # [1, 2, 3]

mutate_list(items)
print(items)  # [1, 2, 3, 4]
```
**Rebinding never escapes the function.  
Mutation always does.**

# 4. Unified Mental Model

Use this simple mental model:

### 1. Objects live in memory.

Everything (int, list, dict, etc.) is an object.

### 2. Variables store **references** to objects.

Variables never contain values, only references.

### 3. When calling a function, Python copies the reference.

Caller’s variable → new reference in function parameter.

### 4. Rebinding a parameter creates a new object and breaks the link.

No effect on caller.

### 5. Mutating a shared mutable object affects the caller.

Mutability = shared state = side effects.

### Summary Table

|Object Type|Mutable?|If modified in function, caller sees change?|
|---|---|---|
|int, float, str, tuple|No|No|
|list, dict, set|Yes|Yes|
|custom class instance|Usually yes|Yes (if attributes mutated)|

This table alone eliminates 90% of confusion.

# 5. Best Practices

1. **Avoid mutating input arguments unless intentional.**
2. For safety, make defensive copies:
```
new_list = old_list.copy()
```
- Prefer returning new objects instead of modifying inputs.
- Use clear naming to signal intent:
    - `mutate_config(config)`
    - `with_config(config)` (returns modified copy)


When ready, say **“proceed to Scope, Namespaces, and Lifetime of Variables”**.