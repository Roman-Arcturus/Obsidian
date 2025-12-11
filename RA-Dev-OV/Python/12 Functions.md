# 1. Functions in Python — Modern definition

A **function** in Python is a reusable block of code that:
1. Has a name. 
2. Receives input values (parameters).
3. Performs computations.
4. Returns a result.

Functions exist to:
- remove duplication,
- organize logic,
- improve testability,
- isolate complexity,
- make code readable.

In modern Python, functions should always use **type hints**, a **docstring**, and clear naming.

In older languages, a function often means:
- a block of code
- stored in a fixed memory address
- sometimes separate from data
- with fixed parameter types
- possibly passed by pointer (Pascal procedures, C function pointers)

In Python:
- **Functions are objects** (first-class citizens).
- You can store them in variables.
- You can pass them to other functions.
- You can return functions from functions.
- They have **no declared types** for arguments or return values.
- They capture surrounding variables (closures).

## 1.1 Basic Function Definition

```
def add(a: int, b: int) -> int:
    """Return the sum of two integers."""
    return a + b
```

- `def` — keyword that starts a function definition.
- `add` — function name (lowercase, descriptive).
- `(a: int, b: int)` — parameters with **type hints**.
- `-> int` — return type hint.
- Docstring — explains what the function does.
- `return` — sends the result back to the caller.

### Why type hints?

Because they enable:
- static analysis (mypy), 
- better editor support (VS Code),
- less ambiguity,
- cleaner large systems.

# 2. Function Parameters (modern concepts)

Python has flexible parameter types, but in production you use only the clean variants unless you need something advanced.

We focus on:
1. **Positional parameters**
2. **Keyword parameters**
3. **Default parameters**
4. **Return values**
5. **Multiple return values (tuple-based)**
6. **Pure functions vs. functions with side effects**

## 2.1 Positional Parameters
```
def area(width: float, height: float) -> float:
    return width * height
    
result = area(3.0, 5.0)
```
Order matters.

## 2.2 Keyword Parameters

```
area(width=3.0, height=5.0)
user = create_user(name="Roman", age=40)
```
Useful for readability.

## 2.3 Default Parameters

Use defaults when values are optional.
```
def greet(name: str, polite: bool = True) -> str:
    if polite:
        return f"Hello, {name}."
    else:
        return f"Hi, {name}"
```

This is clean and readable.
**Never use mutable defaults** (like lists or dicts).  
Reason: they persist between calls, causing bugs.

# 3. Return Values

A function can return:
- a single value
- multiple values (Python returns a tuple)
- nothing (`None`)

## 3.1 Single return
```
def double(x: int) -> int:
    return x * 2
```
## 3.2 Multiple returns (tuple)
```
def min_max(values: list[int]) -> tuple[int, int]:
    return min(values), max(values)
    
small, large = min_max([3, 7, 2, 9])
```
### Why tuples for multiple values?

Because:
- Tuples are immutable → safe 
- Lightweight → efficient
- Clear intention → structured return

We will dig deeper into tuples later.

# 4. Pure Functions vs Side-Effect Functions

Understanding this difference is critical for real projects.
## 4.1 Pure function

- No external state read/write    
- No I/O
- Same input → same output
```
def add(a: int, b: int) -> int:
    return a + b
```
Benefits:
- easy to test
- predictable
- composable

## 4.2 Function with side effects
```
def log_message(msg: str) -> None:
    print(msg)
```
Side effects include:
- printing
- file writes
- mutating data structures
- sending network requests

In real projects, you minimize side effects and isolate them.

# 5. Scope (Local vs Global Variables)

Understanding variable visibility:
## 5.1 Local scope (recommended)
```
def f():
    x = 10    # local
```
## 5.2 Global scope (avoid when possible)
```
counter = 0

def increment():
    global counter
    counter += 1
```
Why avoid globals?  
Because they break isolation, make testing harder, and introduce hidden dependencies.

# 6. Summary Before Moving On

At this point, you should clearly understand:
- how to define functions,
- how parameters and return values work,
- why we use type hints,
- why tuple-based returns are common,
- pure vs side-effect functions,
- scope rules.

These are prerequisites for lists, dicts, and sets, because all these structures are often **created, modified, and processed inside functions**.

We start **Lists** — including:
- what exactly a Python list object is, memory model,
- why lists are mutable and how that affects functions,
- iteration,
- list comprehensions (best modern practice),
- common real-project operations.