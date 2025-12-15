# **Functions: Return Values, Data Flow, and Expression vs Statement Mindset**

This section completes the foundational understanding of how _data moves through functions_ and prepares you for writing clean, modern, Pythonic code.

As requested, each concept follows:
# 1. Return Values (What They Are)

A **return value** is the output produced by a function and handed back to the caller.

In Python:
```
def add(a, b):
    return a + b
```
- Execution jumps out of the function at `return`
- The expression after `return` becomes the function's result
- Functions without an explicit `return` automatically return `None`

This is similar to C-style return behavior, except Python functions can return **any object**—lists, dicts, functions, classes, generators, etc.

# 2. Why Return Values Are Useful

Return values:
- allow **data to flow** between functions
- enable **composition** (function calling function)
- isolate logic and make code **testable**
- eliminate reliance on global state
- make function calls predictable and pure
- express _what the computation is_, not how it mutates global memory

Modern Python heavily favors:
- **pure functions**
- explicit input/output
- minimal side effects

Returning values rather than modifying globals **is _the_ Pythonic approach**.

# 3. Unified Mental Model of Return Values

Think of a function call as:
input → processing → output

This is a **pipeline**:
- Inputs flow in through parameters
- Outputs flow out through `return`
- Everything inside the function is temporary, like a sealed container
- The only meaningful interaction with the outside world is via return values (or mutations on passed-in objects, if intentional)

Visually:
caller ----> [ function ] ----> result

When you master return values, you master:
- data pipelines
- transformations
- modular code
- testability

# Returning Multiple Values (Python Feature)

Python allows returning a tuple implicitly:
```
def compute(a, b):
    return a + b, a * b

sum_val, mult_val = compute(3, 4)
```
`compute` actually returns **one object: a tuple**, but Python’s unpacking makes it feel natural.

This is a powerful and clean way to return structured data without introducing classes prematurely.

# Expressions vs. Statements in Return Values

A **return statement** contains an **expression**.

Expression:
- evaluates to a value

Statement:
- performs an action

Python encourages using **expressions** to build results, for clarity:
return a + b   # good

Avoid using statements when expression-based designs are clearer:
result = a + b
return result

Both work—but the expression-based style is idiomatic when possible.

# Designing Functions: Inputs, Outputs, and Avoiding Hidden Mutability

This is where we begin thinking _like a modern Python developer_:

- pure functions
- predictable behavior
- refactoring strategies
- avoiding “surprise” side effects

## **1. What it is**

This topic covers how to design functions so that:
- their **inputs** are clear,
- their **outputs** are predictable,
- they avoid hidden or surprising side effects,
- and they follow modern best practices for real-world Python projects.

In Python, especially with mutable types (lists, dicts, sets), functions can easily mutate inputs unintentionally. Designing functions well means making such behavior explicit and controlled.

## **2. Why it’s useful**

Good function design leads to:

### **Predictability**

You always know what a function does and does not do.  
This reduces debugging and makes the system easier to reason about.

### **Reusability**

Pure, clean functions can be reused anywhere — including tests — because they do not depend on global state or hidden mutations.

### **Testability**

If a function:
- only reads inputs
- only returns outputs
- does not mutate anything outside itself  
    then writing tests becomes trivial.

### **Maintainability and teamwork**

Readable functions with clear contracts are easier for teammates (and future you) to work with.

## **3. Unified Mental Model**

**Think of a function as a black box with a contract.**

A contract has **three parts**:

### **A. Input contract**

- What types of parameters does the function expect?
- Are they mutable or immutable?
- Will the function mutate them?

### **B. Output contract**

- What exactly will the function return?
- Is the return value derived only from the inputs?
- Is the return value a new object or the same object mutated?

### **C. Side effects contract**

- Does the function change anything outside its body?
- Does it mutate a passed-in object?
- Does it access global or nonlocal state?

When in doubt, **prefer pure functions**:

- no side effects
- no reliance on globals
- return new objects instead of mutating old ones

This makes functions behave like mathematical functions: predictable and composable.


# Defensive Copying, Immutability, and Functional Safety Patterns

### What it is

Defensive copying is the practice of creating explicit copies of data structures—typically dictionaries, lists, or custom objects—before using or returning them. The goal is to prevent external code from mutating internal state in ways you cannot control.

In Python, because mutable containers are passed by reference, a function that receives a list or dict can mutate the caller’s data unless you explicitly copy it. Defensive copying mitigates this risk.

There are two principal forms:

1. **Shallow copy**  
    `dict(obj)`, `list(obj)`, or `copy.copy(obj)`  
    Copies the container but not nested objects.
    
2. **Deep copy**  
	 import copy
	 
    `copy.deepcopy(obj)`  
    Recursively copies all nested objects.

### **Immutability**  
A design approach where you avoid changing existing objects.  
Instead, you create _new_ objects when values change.

Python is not a purely functional language, but it supports immutable types (tuple, str, frozenset) and patterns that emulate immutability for safer design.

**Functional safety patterns**  

A set of practices that ensure functions:
- never mutate external state,
- do not rely on global variables,
- always behave predictably,
- make data-flow explicit.

This leads to code that is easy to reason about, debug, and test.

## **2. Why It’s Useful**

1. **Predictability**  
    You always know exactly what will happen.  
    No hidden modifications. No “where did this value change?” mysteries.
    
2. **Testability**  
    Pure, immutable-style functions are trivial to unit test:
    - same input → same output, every time.
    
3. **Debugging Efficiency**  
    When no function mutates external objects:
    - you don’t need to track subtle side effects,
    - state transitions are clear and intentional.
    
4. **Safer Composition**  
    Functions that do not mutate shared state can be freely combined, reused, parallelized, and reasoned about.
    
5. **Industry Best Practices**  
    Modern Python (2020s+) heavily favors:
    - explicit data flow,
    - immutability where practical,
    - minimal side effects,
    - clear boundaries of responsibility.

This aligns with modern engineering approaches used in:
- data engineering,
- backend services,
- ML pipelines,
- concurrency,
- distributed systems.

## **3. Unified Mental Model**

**Think about data as “values that flow forward.”**

A _pure_ function:
- receives data,
- transforms it,
- returns a new version of that data,
- never alters the past.

Visualizing data as **immutable states in a timeline** helps:
```
state0 --(function)--> state1 --(function)--> state2
```

Think of defensive copying as a “quarantine boundary.”  
Every time external data crosses into your function or object, you create a controlled replica. The outside world keeps its copy; you control yours. No unexpected cross-contamination occurs.

Shallow copies quarantine only the top-level container.  
Deep copies quarantine the entire structure recursively.

This mental model brings three big advantages:

### **A. No surprises**

Values never change behind your back.  
Every change is visible at the point of function return.

### **B. Traceability**

You can always follow where the data came from.

### **C. Data Flow is explicit**

Every dependency is obvious.

This is the same model used in:
- functional programming languages (Haskell, F#),
- React state management (if you have JS experience),
- distributed systems (immutable logs, event sourcing),
- modern pipeline architecture.

---

Exercise 4.
```
users = [
    {"name": "Alice", "score": 10},
    {"name": "Bob"},
    {"name": "Charlie", "score": 5}
]

def normalize_scores(users: list) -> list:
    """
    Return a new list of users dicts.
    Each new user will always contain "score", even if the input did not.
    """
    result:list[dict] = []
    
    for u in users:
        name = u["name"]
        score = u["score"] if "score" in u else 0
        
        new_u: dict = {
            "name" : name,
            "score" : score
        }
        result.append(new_u)

    return result
```

## **Evaluation**

### Correctness:
- Creates a new list → correct
- Creates a new dict for each user → correct
- No mutation of original → correct
- Proper fallback for missing `"score"` → correct
- Deterministic and pure → correct
- Style consistent with earlier lessons → correct (clean, explicit, readable)

### Conceptual correctness:
- You recognized that both container and contained values must be copied.
- You decomposed data (`name`, `score`) before building the new structure.
- You handled the optional field using explicit logic, no shortcuts.

This is professional-grade.


Below is **Exercise 5**, the capstone for the “Safe Transformations” module.  
It combines everything you have learned so far: immutability, shallow-vs-deep copying, structural reconstruction, and deterministic behavior.

The structure and difficulty are designed for your current skill level.

# **Exercise 5 — Deep Reconstruction of Nested Structures**

You now know how to safely transform:
- individual dictionaries
- lists of dictionaries
- optional fields
- fields that must be normalized
- without mutating inputs

This final exercise extends that to **nested inner dictionaries**, representing a realistic, multi-layer data transformation.

# **THE INPUT STRUCTURE**

You are given a list of users.  
Each user has this structure:

```
users = [
    {   "name": "Alice",
        "meta": { "score": 10, "level": 1 }
    },
    {   "name": "Bob",
        # no meta
    },
    {   "name": "Martha",
        "meta": {}
    },        
    {   "name": "Kevin",
        "meta": { "level": 3 }
    },    
    {   "name": "Roman",
        "meta": { "score": 9 }
    },    
]
```
Notes:
- `"meta"` may or may not exist.
- If `"meta"` exists, it is itself a dictionary.
- If `"meta"` does NOT exist, treat it as empty `{}`.

# **THE TASK**

Write a **pure, safe function**:
```
def normalize_meta(users: list) -> list:
    ...
```

The returned list must follow **this new normalized structure**:
- Always return a **new list**.   
- Every user must become a **new dict**.
- `"name"` must be preserved.
- `"meta"` must always exist and follow:
```
"meta": {
    "score": <int>,   # default 0 if missing
    "level": <int>,   # default 1 if missing
}
```
Rules:

- If `"meta"` is missing, you must synthesize it:
```
"meta": {"score": 0, "level": 1}
```
- If `"meta"` exists but is missing fields, fill them in.
- You **must not** mutate:
    - the original list
    - any original user dict
    - any original meta dict

```
def normalize_meta(users: list) -> list:
    result:list[dict] = []
    
    for u in users:
        name: str = u["name"]

        if "meta" in u:
            meta: dict = u["meta"].copy()
            if "score" not in meta:
                meta["score"] = 0
            if "level" not in meta:
                meta["level"] = 1
        else:
            meta: dict = {
                "score" : 0,
                "level" : 1
            }

        n_user: dict = {
            "name" : name,
            "meta" : meta
        }

        result.append(n_user)

    return result
```


