## Closures (Practical, Not Academic)

Closures are powerful, but they are also one of the easiest ways to confuse yourself if introduced incorrectly. We will approach them slowly.

## 1. What it is

A **closure** is a function that **remembers values from the scope in which it was created**, even after that scope has finished executing.

def make_multiplier(factor):
    def multiply(x):
        return x * factor
    return multiply


```
double = make_multiplier(2)
triple = make_multiplier(3)

double(5)   # 10
triple(5)   # 15
```
Key observation:
- `factor` is **not a parameter** of `multiply`
- Yet `multiply` can still access it

That is a closure.

## 2. Why it’s useful

Closures allow you to:
- Parameterize behavior _without_ classes
- Avoid global variables
- Create specialized functions cleanly
- Capture configuration once, use many times

They are commonly used in:
- Callbacks
- Factories
- Decorators (later)
- Functional pipelines

## 3. Unified Mental Model

> **A closure is a function + remembered environment.**

Think:
- The function carries a **backpack of values** with it.
- Those values are frozen at creation time.
## Exercise 18 — First Closure

**Example given**
```
def make_multiplier(factor):
    def multiply(x):
        return x * factor
    return multiply

double = make_multiplier(2)
triple = make_multiplier(3)

#print( double(5) )  # 10
#print( triple(5) )  # 15
```

**Exercise 18**
```
def make_adder(n):
    def add(x):
        return x + n
    return add
    
add_5 = make_adder(5)
add_10 = make_adder(10)

print( add_5(3) )
print( add_10(3) )
```

> [!NOTE] Prompt
> I spent ten minutes trying to figure out why my code wasn't working. — an exact copy of your closure example — didn't work. I finally realized that I needed to write: return add and not return add(n) this way when we do first assignment: add_5 = make_adder(5) add_5 will reference the function add() which will keep inner var equal to 5 and wait for the new parameter x the line print( add_5(3) ) makes the call add(3) and it return 3 + saved 5

## Why `return add` is correct and `return add(n)` is wrong

### What `return add` does

- Returns the **function object** itself    
- The function carries with it:
    - Its code
    - Its local scope
    - The captured variable `n`
- Nothing is executed yet

### What `return add(n)` does

This **immediately calls** `add`:
- `add(n)` means:
    - `x = n`
    - Returns a number
- The function is **gone**
- No closure survives

So `add_5` would become a number, not a function.

### Unified Mental Model (Critical)

> **Closures return behavior, not results.**

If you see parentheses `()`, the function is executed.  
If you omit them, the function is passed or returned.


## Why this mistake is extremely common

- It looks syntactically small
- The mental shift from “call now” to “call later” is subtle
- This exact mistake appears in:
    - Callbacks
    - Event handlers
    - Decorators
    - Async code

Catching this yourself is a **very strong signal** that the concept is landing.

## Closures — What Is Actually Remembered (This Is the Trap)

This lesson prevents **future bugs**. Many experienced developers get this wrong.

## 1. What is remembered

A closure does **not** store a snapshot value.  
It stores a **reference to a variable binding** in the enclosing scope.

That distinction matters.
### Example 1 — Immutable value (safe)
```
def make_adder(n):
    def add(x):
        return x + n
    return add
```
- `n` is an integer (immutable)
- Each call to `make_adder` creates a **new binding**
- Safe and predictable

This is why your example worked perfectly.

## 2. The classic closure pitfall (mutable + loop)

### Problematic example
```
funcs = []

for i in range(3):
    def f():
        return i
    funcs.append(f)

funcs[0]()  # ?
funcs[1]()  # ?
funcs[2]()  # ?

All return: 2
```

**Why this happens**
There is one variable i
All functions capture the same binding
The loop finishes → i == 2
Every closure reads the same final value

## 3. Unified Mental Model (Critical)

> **Closures capture variables, not values.**

Or more concretely:

> “The function looks up the variable _when it is called_, not when it is created.”

This is called **late binding**.


## 4. The correct pattern (freeze the value)

### Solution: introduce a new binding
```
funcs = []

for i in range(3):
    def make_f(n):
        def f():
            return n
        return f

    funcs.append(make_f(i))
    
funcs[0]()  # 0
funcs[1]()  # 1
funcs[2]()  # 2
```

## Why this matters professionally

This exact issue appears in:
- Callbacks
- GUI event handlers
- Async tasks
- Threaded code
- Decorators

Knowing this early puts you ahead.

## 1. Fixing late binding in closures (the correct Pythonic fix)

### What it is

In Python, closures capture **names**, not **values**.  
Late binding means the value of a captured name is looked up **when the inner function is called**, not when it is defined.

The canonical, modern fix is to **bind the value at function-definition time** using a **default argument**.

---

### Why it’s useful

This approach:
- Makes the binding **explicit**
- Preserves **immutability**
- Avoids mutation and hidden state
- Keeps closures readable and predictable
- Is idiomatic Python and widely relied upon in real codebases

It turns a dynamic lookup into a **fixed association**.

### The problem (briefly restated)

You likely had something conceptually equivalent to:
```
def make_multipliers(numbers: list[int]) -> list[callable]:
    funcs = []

    for n in numbers:
        def multiply(x: int) -> int:
            return x * n

        funcs.append(multiply)

    return funcs
    
# -> [30, 30, 30]
```

### The fix (best practice)

We bind `n` **at definition time** by copying it into a default argument:
