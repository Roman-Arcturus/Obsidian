**Late binding**
```
def make_multipliers(numbers: list[int]) -> list[callable]:
    funcs = []

    for n in numbers:
        def multiply(x: int) -> int:
            return x * n

        funcs.append(multiply)

    return funcs

multipliers = make_multipliers([2, 3, 4])
# late binding to the last value, n is equal 4 for all cases

result_1 = multipliers[0]
result_2 = multipliers[1]
result_3 = multipliers[2]

print(result_1(2), result_2(2), result_3(2) )
# ->  8 8 8
```

```
# bind the needed value **into the inner function at definition time**
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


## The defensive pattern: **Bind immutables at creation time**

### What it is

When a closure would otherwise capture a **mutable or late-bound name**, you explicitly bind the needed value **into the inner function at definition time**.

In Python, the canonical way is:
- **default argument binding**

Your fixed version does exactly this.

---

## Why this works (single reason)

> Default arguments are evaluated **once**, when the function object is created.

This converts:

- a **shared reference looked up later**

into:

- a **private, fixed value stored inside the function**

No mutation outside the function can affect it.

---

## Unified mental model (this is the key)

Use this sentence as your rule:

> **If a value must not change, bind it into the function when the function is created.**

In your example:
- `n` → loop variable (changes)
- `factor = n` → frozen value (does not change)

Each `multiply` carries its own constant.

---

## Why this is the _defensive_ pattern

Because it:
- Prevents late binding
- Prevents accidental mutation influence
- Makes the dependency **explicit**
- Requires no extra concepts
- Scales to real codebases

This is why it is relied on in production Python.

---

## Mutation vs non-mutation (explicit)

- `numbers` may be mutable, but it is irrelevant after iteration
- Each closure holds only an **immutable int**
- No shared state exists  
- Behavior is fixed and predictable


---

## Stop point (important)

Internal check — answer **yes or no**:
- Would mutating `numbers` later affect existing `multiply` functions? → **No**
- Does each `multiply` function fully describe its behavior? → **Yes**

If these are clear, you now own the **core closure safety rule** in Python.

As you already provided the short answers in your questions, I will make a longer answers.
Please check my reasoning for the first question. Comment and then wait for answer to question 2

Mutating numbers later _will not_ affect created functions in any way.
Furthermore, I tried to recall make_multipliers with a new list and it behaved as I expected:

```

# previous code
multipliers = make_multipliers([2, 3, 4])  # 3 functions created adressed with indicies 0, 1, 2
result_1 = multipliers[0] # -> 4  # creating a new reference for closure
result_2 = multipliers[1] # -> 6
result_3 = multipliers[2] # -> 8

print(result_1(2), result_2(2), result_3(2) )
# Output is correct ->  4 6 8

# now I call a closure creating function make_multipliers again
multipliers = make_multipliers([5]) # 1 new closure is created. multipliers contains only this one

result_4 = multipliers[0] # -> 4 # creating a new reference

#result_1, result_2, result_3 vars still contain references to the previously defined closures.
print(result_1(2), result_2(2), result_3(2) )
# Output is correct ->  4 6 8

print(result_4(2))
# Output is correct ->  10
```


Your **reasoning is correct**. The behavior you described follows directly from the closure model you have built, and you are applying it consistently.

I will comment **only on question 1**, as requested, and then stop.

---
## Comment on your reasoning (Question 1)

### Core claim

> _Mutating `numbers` later will not affect created functions in any way._

This is **correct**, and your follow-up experiment reinforces the right mental model.

### Why it is correct

In the fixed version:

`def multiply(x: int, factor: int = n) -> int:     return x * factor`

- `factor` is bound **at function creation time**
- Each closure stores its **own immutable integer**
- The original `numbers` list is no longer referenced by the closures

After the loop finishes:
- The list `numbers` is irrelevant to existing closures
- Even rebinding `numbers` to a _new list_ has no effect
- Each closure remains self-contained

Your observation that:
- `result_1`, `result_2`, `result_3` keep working unchanged
- A new call to `make_multipliers` creates **new, independent closures**

is exactly what should happen.

---
## Important nuance you implicitly got right

You also correctly distinguished between:

- **Rebinding a variable** (`multipliers = make_multipliers([5])`)
- **Existing references** (`result_1`, `result_2`, `result_3`)

Rebinding a name does **not** retroactively affect objects already created.

That is a foundational Python rule, and you applied it correctly.

---

## Unified mental model (validated)

> Each call to the outer function creates a **new closure environment**.  
> Existing closures are unaffected by later calls or rebinding.

Your experiment confirms this model empirically.


> [!prompt]
> Question 2: Does each `multiply` function fully describe its behavior? → **Yes** 
> 
> I began writing some pseudocode to explain my understanding but after a while I realized that native Python code is more expressive and cleared to me than my own, made up pseudocode. I beleive it's a good sign. So the short answer: Yes. Each multiply function definitions completely describe their behaviour.

## Confirmation

> **Does each `multiply` function fully describe its behavior? → Yes**

This is correct.

Each `multiply` function:
- Has all required inputs explicitly defined
    - `x` (call-time input)
    - `factor` (creation-time constant)
- Depends on no external names
- Reads no mutable state
- Produces its output solely from its arguments

In other words, each function is **self-contained**.

---

## Important refinement (tightening the mental model)

A more exact statement is:

> Each `multiply` function fully describes its behavior **after it is created**.

Before creation, `n` exists and varies.  
After creation, `factor` is fixed.

That creation moment is the boundary that matters.

---

## Why your pseudocode instinct matters

Your realization here is a strong signal:

> Native Python code is more expressive than invented pseudocode.

This means:
- Your mental model now aligns with Python’s execution model
- You are reasoning _with_ the language, not around it
- You no longer need a translation layer

That is exactly where clarity begins to compound.

---

## Unified mental model (finalized)

> A well-designed closure is a pure function plus frozen configuration.

Your `multiply` functions satisfy this completely.



## We now advance (one step only)

### Next lesson: **Defensive copying for mutable configuration**

We will cover:
- one technique
- one reason
- no alternatives

It builds directly on the name/object model you just solidified.

