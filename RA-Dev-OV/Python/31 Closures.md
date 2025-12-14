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
    
result_1 = multipliers[0]
result_2 = multipliers[1]
result_3 = multipliers[2]

print(result_1(2), result_2(2), result_3(2) )
# -> 8 8 8
```

### The fix (best practice)

We bind `n` **at definition time** by copying it into a default argument:
```
def make_multipliers(numbers: list[int]) -> list[callable]:
    funcs = []

    for n in numbers:
        def multiply(x: int, factor: int = n) -> int:
            return x * factor

        funcs.append(multiply)

    return funcs

multipliers = make_multipliers([2, 3, 4])

result_1 = multipliers[0]
result_2 = multipliers[1]
result_3 = multipliers[2]

print(result_1(2), result_2(2), result_3(2) )
# ->  4 6 8

```

When Python sees:
```
factor: int = n
```
it immediately evaluates `n` and stores the **value** inside the function object.

Each iteration creates a function with its **own constant**.

## Step 5 — Unified mental model (single sentence)

> **Closures capture names; default arguments capture values.**

That is the rule you use to reason about all closure behavior in Python.


## Step 6 — Sanity check (no code)

Answer this yes/no internally:
- Does each function now carry its own `factor`?
- Is any shared mutable state involved?
- Does calling the function require knowing anything about `n`?

```
- yes, each created function carry it's own factor 
- no, there is no shared mutable state in this example 
- no, to call the function I only provide multiplier.
```

## Next concept: **Closures as configuration (the first practical pattern)**

We will introduce **one pattern only**.

## What it is

A closure can be used to **configure behavior once**, and then reuse that behavior many times **without repeating parameters**.

This is sometimes called:
- function specialization
- partial application (informally, in Python)
- configuration via closure

## Why it’s useful

It allows you to:
- Separate **setup** from **execution**
- Avoid threading the same parameter through many calls
- Create small, purpose-built functions
- Keep call sites simple and explicit

This is common in:
- data processing pipelines
- validation logic
- callbacks
- transformation steps

## Minimal example (single step)

```
def make_threshold_checker(threshold: int):
    def is_above(value: int) -> bool:
        return value > threshold

    return is_above
```

## Unified mental model

> A closure is a function plus a **private constant environment**.

Here:
- `threshold` is fixed at creation time
- `value` is supplied at call time

Exactly the same mechanism you just mastered with `factor`.

## Stop point (important)

Do **not** generalize yet.

Ensure this sentence is fully internalized:

> “The outer function configures behavior; the inner function performs work.”

Confirm this understanding briefly, and we will look at the **first real pitfall** closures introduce in production code.

```
def make_threshold_checker(threshold: int) -> callable[[int], bool]:
    def is_above(value: int) -> bool:
        return value > threshold

    return is_above

# usage example: 
peak_limit = 42    # a constant that can be put in a config file 

# provide limit value once at the execution of the script:
check_level = make_threshold_checker(peak_limit)

# then, during the script run we test some changing value 
# towards the constant defined once
if check_level(current_value):
    ...
```

## Confirmation (no corrections)

- `make_threshold_checker` returns a **function**
- `threshold` is **bound once** at creation time
- `check_level` is a **configured predicate**
- `current_value` is supplied **at call time**
- No mutation
- No hidden state

Your comment about `peak_limit` belonging in configuration is exactly the intended use case.

## Unified mental model (restated, one sentence)

> The closure turns a runtime constant into a private parameter.

This is the same mechanism as the fixed `factor` earlier.

## Next step: **The first real pitfall**

We now introduce **one pitfall only**, tightly scoped.

### The pitfall

Closures can accidentally capture **mutable objects**, leading to behavior that changes _without the function being re-created_.

## Minimal illustration
```
def make_allowed_checker(allowed: set[int]):
    def is_allowed(value: int) -> bool:
        return value in allowed

    return is_allowed
```

## Why this is dangerous

- `allowed` is mutable
- The closure holds a **reference**, not a copy
- External mutation silently changes behavior

This violates predictability.


## Unified mental model (single rule)

> Closures are safest when they capture **immutable data**.

---

## Stop point

Before moving on, answer **yes or no** internally:
- Do you see how mutating `allowed` elsewhere would change `is_allowed`?
- Do you see why this differs from the `threshold` example?

```
# provided example:
def make_allowed_checker(allowed: set[int]):
    allowed.add(99)  # mutation
    def is_allowed(value: int) -> bool:
        return value in allowed

    return is_allowed

# usage example:
allowed_vals = {22, 33, 44} # defining a set in a config file for example
check_allowance = make_allowed_checker(allowed_vals) # creating a closure

# during the runtime we get unexpected result and obscure behaviour
print(check_allowance(22)) # True
print(check_allowance(99)) # should be False but is_allowed mutated global set

```

> [!NOTE] prompt
> .... shows the danger of sending a reference to a mutable to any function, even without closure.
>

This is **exactly correct**.

Closures do **not** introduce this risk.  
Closures merely make it **easier to forget** that the reference is shared.

## 3. The true rule (single, general rule)

> **Passing a mutable object passes the object, not a copy.  
> Any mutation affects all holders of that reference.**

This rule applies to:
- normal functions
- closures
- methods
- modules
- globals

Closures are not special here.

## 4. Why closures make this risk _worse_

Only one reason:
- The mutation is **far from the call site**
- The captured object is **invisible to the caller**
- The behavior change feels “magical”

That is why closures require discipline.

## 5. Unified mental model (final, compact)
- Mutability is the risk
- Closures increase the **distance** between cause and effect
- Immutability restores predictability

You are reasoning at the correct abstraction level.

### Next (one step only)

Next, we will cover **the single defensive pattern** used in real Python code to neutralize this risk — without banning closures and without overengineering.


> [!prompt]
> Your answer: "Set vs list — one real-life distinction only" was very helpful and exactly what I needed to see a clear difference. Please use this kind of analogies / comparision in future.
> 
> I need to pause again. Reading your lessons, capturing them in Obsidian, trying out examples in VS Code, saving my pieces of code to Obsidian as well, switching back to the chat becaue something I try doesnt work requires a lot of concentration.
> 
> Without pause and rest the reasoning becomes shallow and the intent shifts to "just make it work"
> 
> Thank you and see you later.

Understood. That is a **disciplined and correct decision**, not a retreat.

You are doing exactly what experienced engineers do when they want **durable understanding**, not surface fluency.

I will acknowledge three things explicitly, and then stop.

---

## 1. Your learning behavior is correct

What you described —  
reading, transcribing into Obsidian, testing in VS Code, reconciling discrepancies — is **deep learning**, not passive consumption.

The moment you notice your intent drifting toward _“just make it work”_, pausing is the right response.

That awareness is a strong signal of good engineering judgment.

---

## 2. The analogy feedback is noted

The “queue vs whitelist” distinction worked because:
- It maps to **intent**, not implementation
- It answers _“when would I reach for this?”_
- It reduces cognitive load

I will continue to introduce new concepts using **one sharp, real-world comparison** where appropriate, and avoid blending abstractions.

---

## 3. Where we will resume (no content now)

When you return, the **next single step** will be:

- The **defensive pattern** for closures capturing mutables  
    (one technique, one reason, no alternatives)

Nothing will be skipped or assumed.

---

