## Functional Thinking in Python (Controlled)

We will continue leaning into functional style, but **Python-idiomatic**, not purely academic FP.

### Concept 1: Predicate vs Transformer (Very Important)

#### What it is
- A **predicate** returns `True` or `False` (used by `filter`, `any`, `all`)
- A **transformer** returns a _new value_ (used by `map`)

Examples:
- Predicate: `lambda x: x > 0`
- Transformer: `lambda x: x * 2`
#### Why it’s useful
- Mixing these roles causes confusion and bugs.
- Clean separation makes pipelines readable and predictable.

#### Unified Mental Model

> “Filtering answers _should this element exist?_  
> Mapping answers _what should this element become?_”

---
## Exercise 12 — Predicate Discipline

```
result = list(
    map(
        lambda z: f'{z} is even',
        filter(
            lambda y: y > 0 and y % 2 == 0,
            nums
        )
    )
)
```
Why this is good:
- `filter(...)` answers **“should this element exist?”**
- `map(...)` answers **“what should this element become?”**
- No logic leakage between stages
- Readable data flow
- Fully idiomatic functional Python

## Next Concept: Reducers — `any()` and `all()` as Final Gates

You already _used_ them correctly. Now we formalize the mental model.

### What they are
- `any(iterable)` → `True` if **at least one** element is truthy
- `all(iterable)` → `True` if **every** element is truthy

They **do not transform** data.  
They **collapse** a sequence into a single boolean.

### Why useful
- They replace flag variables and loops
- They short-circuit (stop early)
- They make intent explicit

### Unified Mental Model

> “Reducers answer _a question about the entire collection_.”

## Exercise 13 — Reducer Thinking
```
result: bool = all(
    z % 3 == 0 for z in filter(
        lambda y: y > 0,
        nums
    )
)
```
Why this is especially good:
- You avoided `list(...)` → preserves **short-circuiting**
- Predicate logic is cleanly separated
- Generator expression matches `all()`’s intent
- Data flow is easy to reason about

---

## Next Concept: When _Not_ to Use `map()` / `filter()`

This is important to avoid overengineering.

### What it is

Although Python supports functional pipelines, **list comprehensions are often clearer** when:
- Filtering and mapping are tightly coupled
- The transformation is simple
- Readability matters more than formal separation

### Why it’s useful
- Pythonic code prioritizes clarity over purity
- Comprehensions are optimized in CPython
- Fewer moving parts → fewer bugs

### Unified Mental Model

> “Use functional chains when stages are conceptually distinct.  
> Use comprehensions when logic is naturally local.”


## Exercise 14 — Choose the Better Tool

Produce a list of **squares of positive numbers**.

### **Part A**  
Write the solution using **filter + map**.
```
#Part A
result = list(
    map(
        lambda z: z * z,
        filter(
            lambda y: y > 0,
            nums
        )
    )
)
```
### What this communicates

- Two **distinct stages**:
    - _Selection_ (`y > 0`)
    - _Transformation_ (`z * z`)
- Strong alignment with a functional pipeline mindset

### When this is a good choice
- When stages are reusable
- When logic grows more complex
- When you want strict role separation

### **Part B**  
Write the solution using a **single list comprehension**.
```

#Part B
result = list(
    n * n
    for n in nums
    if n > 0
)
```

### What this communicates
- One local idea: _“square the positive numbers”_
- Minimal cognitive overhead
- Idiomatic Python

### When this is the better choice
- Simple transformation
- Simple condition
- No reuse of intermediate stages


## Unified Decision Model (Very Important)

> **Ask one question:**  
> “Do I think in _steps_ or in _one sentence_?”

- _Steps_ → `filter → sorted → map`
- _One sentence_ → comprehension

Python gives you both. Mastery is choosing **the simpler one that still tells the truth**.


## Where We Go Next

You now have:
- Functional pipelines
- Comprehensions
- Reducers (`any`, `all`)
- Nested data extraction
- Defensive thinking

**Next lessons (in order):**

1. **Functions as first-class values** (passing functions around)
2. **Closures (light, practical, not academic)**
3. **Immutability by convention**
4. **When mutation is acceptable and when it is dangerous**
5. **Bridging functional → object-oriented Python**

When you’re ready, say **“Continue”**, and we’ll move to _functions as values_.
