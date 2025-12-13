## Lesson: Functions as First-Class Values

This is a **foundational concept**. Many later Python features depend on it.

## 1. What it is

In Python, **functions are values**.

That means:
- A function can be assigned to a variable
- Passed as an argument
- Returned from another function
- Stored in a data structure

A function is **not special syntax** after it is defined.  
It is an **object** like an int, list, or dict.

```
def double(x):
    return x * 2

f = double

print( f(2) )

```

## 2. Why it’s useful

This enables:
- `map()`, `filter()`, `sorted(key=...)`
- Callbacks
- Custom behavior without `if`/`elif` chains
- Reusable logic

Without first-class functions:
- `lambda` would not exist
- Functional pipelines would not exist
- Much of Python’s expressiveness disappears

## 3. Unified Mental Model

> **A function is a value that happens to be callable.**

Think in C terms:
- Function pointer, but safe and managed
- No distinction between “code” and “data”

## Exercise 15 — Function as a Value
```
def squared(x: int | float) -> int | float:
    return x * x
fun = squared
print(fun(3))
```
## Next: Passing Functions as Arguments

### What it is

A function can receive **another function** as a parameter and decide _when_ and _how_ to call it.

This is the core idea behind:
- `map(function, iterable)`
- `sorted(key=function)`
- Custom processing pipelines

### Unified Mental Model

> “Behavior can be injected, not hardcoded.”


## Exercise 16 — Function as Parameter

```
def squared(x: int | float) -> int | float:
    return x * x

def triple(x: int | float) -> int | float:
    return x * 3

def apply(x: int | float, fn):
    return fn(x)

result_1 = apply(5, squared)
result_2 = apply(5, triple)
```

What this confirms:
- `fn` is just another value
- Calling `fn(x)` is no different from calling `squared(x)` or `triple(x)`
- Behavior is **parameterized**, not hardcoded

This is the exact mechanism behind:
- `map(fn, iterable)`
- `filter(fn, iterable)`
- `key=fn` in `sorted`

---

## Next Concept: Replacing Conditionals with Function Tables

This is a **professional-level pattern** that reduces complexity.

## 1. What it is
```
if mode == "square":
    result = x * x
elif mode == "triple":
    result = x * 3
else:
    raise ValueError

# --->>>

ops = {
    "square": squared,
    "triple": triple,
}

result = ops[mode](x)

```

## 2. Why it’s useful

- Removes branching logic
- Makes adding new behavior trivial
- Avoids deeply nested `if`/`elif`
- Aligns with open/closed principle

## 3. Unified Mental Model

> “Choose behavior by lookup, not by branching.”


### Exercise 17 — Function Dispatch Table
```
ops: dict = {
    "square": squared,
    "triple": triple,
}

def compute(x: int | float, mode: str) -> int | float | None:
    fn = ops.get(mode)
    if fn is None:
        return None
    
    return fn(x)

# advanced variant
def compute(x, mode):
    return ops.get(mode, lambda _: None)(x)

```

Next, we will move to:

### **Closures (Practical, Not Academic)**

- Functions that _remember_ values
- Why they exist
- When they are useful
- When they are dangerous

```
1. I misread the "no ifs" condition and got stuck with the .get() method 
   and stopped thinking after getting a working variant. This is a brain freeze 
   pattern that I hope you will notice and help me avoid in the future.

2. The advanced variant looks elegant, but I agree that it's better suited for later.
```

That is a very accurate self-diagnosis, and it shows real learning maturity.

Short feedback, as requested:

1. **You identified a genuine failure mode**  
    What you described is not a Python issue — it is a _cognitive stall_:
    
    - You satisfied the constraint (“no ifs”)
    - You got a working solution
    - You stopped reevaluating structure
    
    This happens frequently when learning expressive languages. I will actively watch for this pattern 
    and deliberately pause you at such points to re-evaluate intent vs. mechanics.
    
2. **Your instinct about the advanced variant is correct**  
    It is elegant but _compresses intent_.  
    Right now, clarity and explicitness are the correct priorities.

You have made substantial progress today:

- Functional pipelines
- Predicate vs transformer discipline
- Reducers
- First-class functions
- Dispatch tables

Pausing now is the right move.  
When you return, we will continue with **closures**, but in a strictly practical, restrained way.

