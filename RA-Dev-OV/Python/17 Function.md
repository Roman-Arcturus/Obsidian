# **Functions — Deep Dive (Modern Python, Professional Practices)**

We already covered function basics earlier.  
Now we build a **complete, professional-level mental model** so you fully understand _how_ and _why_ functions work in Python.

Structure:
1. What functions are
2. Why they are useful
3. Unified mental model
4. Defining functions
5. Parameters (all 5 kinds — modern usage)
6. Return values (single, multiple, tuples)
7. Scope & closures
8. Docstrings
9. Type hints (modern standard)
10. Best practices

# **1. What Functions Are**

A **function** in Python is:
- A **first-class object** (can be stored in variables, passed around, returned).
- A **callable** (anything implementing `__call__`).
- A **closure-capable** execution unit (captures external variables).
- Created at runtime by the `def` statement (not a compile-time construct).
- Backed by a `function` object implemented in C inside CPython.

Analogy to other languages:
- Similar to JS functions (first-class, closures).
- More flexible than C functions (Python functions are runtime objects).
- Python also treats functions like data, similar to functional languages (Scheme, Haskell).

# **2. Why Functions Are Useful**

- Encapsulate logic.
- Avoid repetition (DRY).
- Improve readability.
- Enable composition.
- Enable modular architecture.
- Support higher-order functions (passing functions as arguments).
- Create closures for advanced behaviors (stateful callbacks, factories).

Professional Python code relies heavily on **clean, small, pure-ish functions**.


# **3. Unified Mental Model**

A function in Python is best understood as:

**“A named object that stores executable code + a private namespace + references to variables from its outer scope.”**

More concretely:

| Concept     | Meaning                                      |
| ----------- | -------------------------------------------- |
| First-class | Can be stored, passed, returned              |
| Callable    | Implements `__call__`                        |
| Closure     | Captures outer variables                     |
| Namespace   | Has its own local scope                      |
| Object      | Has attributes (`__name__`, `__doc__`, etc.) |
# **4. Defining Functions (Modern Syntax)**
```
def add(a: int, b: int) -> int:
    return a + b
```
You should always include:
- parentheses
- typed parameters
- typed return
- clear name
- docstring (for real projects)

This is the modern standard.

# **5. Parameters — The 5 Kinds (Python’s true complexity)**

Python has the most flexible function calling system of any mainstream language.

There are **5 types of parameters**:
### 1) Positional-only

(rare, but exists — uses `/`)
```
def f(a, b, /):
    ...
```

### 2) Positional-or-keyword (default)

Most common:
```
def f(a, b):
    ...
```

### 3) Keyword-only (after `*`)

Professional practice for clarity:
```
def connect(host: str, *, timeout: int = 30):
    ...
    
connect("https://", timeout=4242)
```

### 4) *args — variable positional parameters

Behaves like a tuple:
```
def add_all(*numbers):
    return sum(numbers)
    
add_all(1, 2, 3)
```

### 5) **kwargs — variable keyword parameters

`**kwargs` collects **all extra keyword arguments** passed to a function into a **dictionary**.  
Meaning: whatever arguments look like `key=value` but are **not explicitly listed** in the function signature will end up inside `kwargs`.

This is Python-specific syntactic sugar.  
In C you have `varargs` (`...`), but Python splits varargs into two categories:
- `*args` → extra positional arguments → tuple 
- `**kwargs` → extra keyword arguments → dict

This is extremely common in Pythonic libraries, decorators, class constructors, and wrappers around external APIs.
### Short Example
```
def create_user(name, **kwargs):
    print("Required:", name)
    print("Optional settings:", kwargs)

create_user(
    "Alice",
    age=30,
    is_admin=True,
    email="alice@example.com"
)
```
Explanation:
- `name` is required.
- Everything else (`age`, `is_admin`, `email`) was not declared in the signature, so Python packs them into a `dict` called `kwargs`.

### How to use the contents of `kwargs`

Because `kwargs` is a dictionary:
```
def create_user(name, **kwargs):
    age = kwargs.get("age", 0)
    is_admin = kwargs.get("is_admin", False)
    print(name, age, is_admin)
```
### Why it is useful

- It allows writing **functions that accept flexible configuration**.
- It allows writing **clean APIs** where callers may pass optional, named parameters without breaking existing code.

### Unified Mental Model

Think of `**kwargs` like a **bag of named optional settings**:

- The caller picks any keys they want.
- You decide how to use (or ignore) them.
- Internally you get a dictionary that you can inspect, validate, or forward.

It is Python’s way of making API surfaces **flexible, future-proof, and explicit**.

# **6. Return Values**

### Single value
```
return x
```
### Multiple values (really a tuple)
```
return x, y, status
```

Python automatically packs them into a tuple:
```
def f():
    return 1, 2

a, b = f()
```

# **7. Scope, LEGB, Closures (Important)**

Python resolves names using **LEGB**:
1. **L**ocal
2. **E**nclosing (outer functions)
3. **G**lobal (module)
4. **B**uiltins

Example closure:
```
def outer(x):
    def inner():
        return x    # captured from outer
    return inner
    
f = outer(10)
print(f())     # prints 10
```

# **8. Docstrings (Professional Standard)**

Every production function must have a docstring:
```
def circle_area(radius: float) -> float:
    """
    Compute area of a circle.

    Parameters
    ----------
    radius: float
        Radius in meters.

    Returns
    -------
    float
        Area in square meters.
    """
    return 3.14 * radius * radius
```
Docstrings are part of the function object and appear in:
```
help(circle_area)
```

# **9. Type Hints (Required in real projects)**

Modern Python uses static typing tools:
- `mypy`
- `pyright`
- `ruff` type checking
- IDE tooltips
    
Example:
```
def scale(value: float, factor: float) -> float:
    return value * factor
```

For advanced cases, you use:
- `typing.Optional`
- `typing.Union` (or newer `|` syntax)
- `typing.List`, `typing.Dict` (or native `list[]`)

# **10. Best Practices (Real Production Code)**

1. **Small functions** (<15–20 lines ideally).
2. **Pure when possible** (avoid hidden side effects).
3. **Use keyword-only args** for clarity.
4. **Use type hints consistently**.
5. **Always document behavior**.
6. **Use descriptive names** (no abbreviations).
7. **Return tuples for multiple results**.
8. **Never use mutable default parameters**:
```
def f(a, values=[]):  # wrong way

def f(a, values=None):  # right
    if values is None:
        values = []

```
