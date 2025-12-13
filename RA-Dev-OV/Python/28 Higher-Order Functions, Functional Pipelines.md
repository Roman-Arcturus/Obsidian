
## 1. What they are

**Higher-order functions** are functions that:
- **take other functions as input**, or
- **return functions**

Common higher-order functions in Python:
- `sorted(iterable, key=...)`
- `map(func, iterable)`
- `filter(func, iterable)`
- `any()`, `all()`
- custom functions you write yourself

These form the basis of **functional-style pipelines**, where data flows through transformations step by step without being mutated.

Example (just conceptual):
```
cleaned = sorted(
    filter(lambda u: u["active"], users),
    key=lambda u: u["meta"]["score"]
)
```

You already understand each piece.  
Now we start assembling them in systematic ways.

## 2. Why they’re useful

**Higher-order functions allow you to:**
- **compose logic** without writing loops
- **avoid mutation**, producing predictable and testable code
- express “what” should happen, not “how”
- reuse the same transformation in many places
- write clearer, more declarative data-processing pipelines

**In Python, you frequently see these patterns in:**
- data processing
- APIs that return nested JSON
- command pipelines
- sorting strategies
- filtering strategies

**This will be essential as we move toward:**
- generators
- advanced iteration patterns
- object-oriented design
- async pipelines

## 3. Unified Mental Model

Think of each higher-order function as a **machine that takes a dataset and a rule**, producing a new dataset.

- `filter(rule, data)` → keeps only elements where rule(element) is True
- `map(rule, data)` → transforms each element into something new
- `sorted(data, key=rule)` → orders data based on the rule
- `any(rule(element) for element in data)` → checks if any element matches
- `all(rule(element) for element in data)` → checks if all match

The **rule** is always a function.  
The **data** is always an iterable.

Data flows like this:
```
INPUT → filter → map → sorted → OUTPUT
```

This is the “functional pipeline” mental model.

----

/ higher_functions_pipeline.py

---


> [!NOTE] Prompt
>For now, please only use built-in Python types and do not import external modules, even standard ones, like "typing."
Alternatively, you could provide an overview of all the types defined in it so that I can learn how to perform full-blown type hinting. But later. Let's not drift away from our lesson.


> [!NOTE] Prompt
> Let's try a few more examples interactively, one by one.
Even though I understand how comprehensions, lambda, and higher-order functions work, I have trouble deciding which variant to use and its syntax. So, I first go through previous examples and exercises, find an appropriate one, and modify it.
This is not sustainable in the long run.

Understood.  
We will stay strictly with **built-in types**, no imports, no `typing`, no deviations.  
And we will proceed **one small interactive example at a time** until your intuition becomes automatic.

Before the first example, here is the plan for how I will guide you:

1. I give you a **very small, concrete task**.    
2. You attempt a solution using **any** pattern: comprehension, lambda, filter/map, or plain loop.
3. I check correctness, then show **two alternative correct variants**, so you see the trade-offs.
4. We repeat with slightly more complex shapes until choosing the right idiom becomes natural.

This eliminates guesswork and gets you to the point where your brain _automatically sees the right form_.


