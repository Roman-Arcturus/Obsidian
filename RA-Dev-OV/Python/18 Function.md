# Functions Deep Dive (Part 4)

## Return Values and Data Flow in Python Functions

This section explains **how values move into a function**, **how results move out**, and the key Python-specific patterns that differ from C/JS.

# 1. What Return Values Are

A `return` statement hands a value back to the caller:
```
def add(a, b):
    return a + b
```
Python functions always return _something_.  
If you omit `return`, Python returns:
```
None
```
This is equivalent to a void function in C, except Python formalizes it as a real object (`NoneType`).

### Multiple returns

You can return early:
```
def process(x):
    if x < 0:
        return "negative"
    return "non-negative"
```
Early returns are idiomatic and encouraged in Python for clarity.

# 2. Why Return Values Are Useful

Return values form the **data flow** of your program.

They allow you to:
- Build functions that transform data (functional style).
- Test functions easily (pure functions dominate test-driven design).
- Chain operations:
```
result = transform( clean( load() ) )
```
- Replace global variables with explicit data passing (more predictable, easier to debug).

For Pythonic design, functions should rarely mutate external state.  
They should **take data in → return new data out**.


# 3. Returning Multiple Values (Python-specific and powerful)

Python allows:
```
def stats(values):
    total = sum(values)
    count = len(values)
    return total, count
    
s, c = stats([10, 20, 30])
```
What other languages emulate using:

- output parameters (C)
- reference parameters (C++)
- objects (JS)

Python does naturally through tuples.

This leads to clean APIs that avoid temporary classes.

# 4. Returning Collections

Often, functions return:
- lists (ordered data)
- dicts (structured data)
- sets (unique items)
- tuples (lightweight structured data)
- custom objects

Example returning a dict (Pythonic):
```
def user_info(name, age):
    return {"name": name, "age": age}
```

This is very common in real-world code because dicts are lightweight data carriers.

# 5. Returning None Explicitly or Implicitly

**Explicit:**
```
def log(msg):
    print(msg)
    return None
```

**Implicit:**
```
def log(msg):
    print(msg)
```

Good Python code prefers the implicit version unless returning `None` is part of the API contract.

# 6. Unified Mental Model

### How to think about data flow in Python functions

1. **Arguments come in by assignment.**  
    Python does not “pass by value” or “pass by reference.”  
    It passes **object references by value** (object pointers are copied).
    
2. **The function body manipulates the same object the caller passed**,  
    **unless** you reassign the parameter.
    
3. **Return values are just objects sent back.**  
    They don’t behave differently from any other object.
    
4. **Returning multiple values returns one tuple object.**
    
5. **No reference/out parameters** like in C—everything is explicit and predictable.
    

The simplest correct model:

**A function receives references → does work → returns references.**

No hidden copying. No aliasing traps except with mutable objects that you choose to modify.

# 7. Best Practices

1. Prefer “pure” functions (input → output, no side effects) for most logic.
2. Use early returns for readability.
3. When returning structured data:
    - Tuples for small fixed data.
    - Dicts for flexible or named data.
    
4. Avoid returning complex nested structures unless necessary.
5. Document return types via type hints:

