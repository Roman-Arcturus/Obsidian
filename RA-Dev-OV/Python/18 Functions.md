## Functions Deep Dive (Part 3)

### Topic: Default Parameters, Mutability Traps, and the Internal Model of Function Calls

We now extend function fundamentals into **how Python evaluates parameters**, **how defaults work under the hood**, and **the major pitfall that every Python developer must understand**: **mutable default arguments**.

# 1. Default Parameters

## What they are

Defaults allow a function to specify values that are used **when the caller does not supply them**.

```
def greet(name="Guest"):
    print(f"Hello, {name}")


greet()        # uses default "Guest"
greet("Alice") # overrides default
```
This is similar to optional parameters in C++ (`void f(int x = 10)`), except Python evaluates defaults at **function definition time**, not at call time.

# 2. Why defaults are useful

- They simplify APIs: callers provide only what they need.
- They allow adding parameters later _without breaking existing code_.
- They are fundamental to Pythonic design, where keyword arguments and optional settings are standard.

# 3. The _Mutability Trap_ (Critical Python Concept)

This is a notorious Python pitfall.  
**Never use a mutable object (list, dict, set) as a default.**

### The bad example
```
def add_item(item, items=[]):
    items.append(item)
    return items
    
# Call it twice:
print(add_item(1))  # [1]
print(add_item(2))  # [1, 2]  <-- unexpected!

```
Why?  
Because the list `[]` is created **once**, at function definition, and reused forever.

C, JS, and most languages do not behave this way.  
This is purely Python’s evaluation model.

# 4. The correct pattern

Use `None` as a sentinel and create the object inside the function:
```
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```
Now each call gets its own list.

# 5. Unified Mental Model

### How Python evaluates parameters

- When the interpreter sees `def ...`, it creates a Function Object.
- It _evaluates default arguments immediately_ and stores them inside the function object.
- On each call, Python does **not** re-create defaults.

**Immutable defaults (int, str, tuple)** are safe because they cannot change.  
**Mutable defaults (list, dict, set, custom objects)** persist and accumulate modifications.
### The mental model

Think of default parameters as **static storage** attached to the function.  
- Not recreated per call.  
- Stored once.  
- Shared across all calls.

Therefore:
- Immutable default = static constant (safe).
- Mutable default = static variable (dangerous).

This mental model will protect you in real-world code.

# 6. Best Practices

1. Only use immutable defaults:
    - numbers, strings, Booleans, `None`, tuples.
2. For any default that may need to change, use the `None → create inside` pattern.
3. Defaults must be declared before `*args` and `**kwargs`.
4. For clarity in real projects, always favor **keyword-only optional parameters**:

```
def process(file, *, retries=3, timeout=10):
    ...

The `*` forces callers to use keyword arguments → clearer APIs.
```

If this section is clear, we can proceed to the next major concept:  
**Return Values and Data Flow in Functions**, including multiple returns, early returns, and returning complex data structures.