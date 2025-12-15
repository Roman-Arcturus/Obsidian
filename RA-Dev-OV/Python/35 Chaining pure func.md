Chaining multiple pure transformations while handling nested data safely

## Core idea: flatten your thinking before you flatten your data

### What it is

When dealing with nested data (lists of dicts, dicts containing lists, etc.), the primary risk is **accidental mutation** and **implicit assumptions** about structure.

The correct functional approach in Python is:
1. **Extract only what you need**
2. **Transform it into a simpler shape**
3. **Apply further transformations on the simplified data**

Each step is:
- A pure function
- Explicit in intent
- Easy to test independently

---

### Why it’s useful

Nested data:
- Increases cognitive load
- Encourages defensive mutation
- Makes pipelines fragile

By flattening _conceptually_ (not structurally) early:
- Each function does one thing
- Failures are localized
- Composition remains safe

---
### Unified mental model

> Treat nested data as a **foreign object**.  
> Extract → normalize → operate.

Do **not** carry complex structures deeper into the pipeline than necessary.

---
## Example: processing nested orders

> [!example]
> 35_chaining_puref.py

## Topic: Defensive extraction from nested structures (without mutation)

### What it is

Real-world nested data is often:
- Incomplete
- Inconsistent
- Partially trusted

Defensive functional style means:
- **Validate at the boundary**
- **Normalize early**
- **Propagate simple, safe values inward**

No mutation. No exceptions for control flow. No hidden defaults.

---
### Why it’s useful

Without defensive extraction:
- Pipelines become fragile
- Errors appear deep in unrelated code
- Debugging cost increases exponentially

With defensive extraction:
- Failures are localized
- Later stages can assume invariants
- Composition remains safe

---
### Unified mental model

> **Trust decreases with depth.**  
> The deeper the data, the earlier you normalize it.

---
## Example: Orders with optional discounts

### Input data (realistic, imperfect)

> [!Example]
> 35_chaining_puref.py

## Why this pattern works
- Missing keys are handled **once**
- Later stages do not branch defensively
- Each function has a single responsibility
- No mutation, no hidden coupling

---
## Design rule to internalize

> **Every function establishes and guarantees its own invariants.**

Downstream code should never wonder:
- “Is this key present?”
- “Is this value normalized?”

---

> [!prompt]
> Hello again!
> While revising my Obsidian notes and VS Code, I realized that I definitely need to redo the structure.
> I was taking notes on each lesson separately, but we keep addressing the same topics with new details. At this point, my notes are not clearly organized by topic, so I have to look through several notes to find what I'm looking for.
> My knowledge base is more about demonstrating my learning curve than anything else. 
> 
> Anyway, lets continue slow and clean and calmly with sorting as a pure pipeline stage.

Welcome back. Your observation about your notes is accurate and healthy. What you are experiencing is a **transition from chronological learning to conceptual organization**. That usually happens exactly at the point where mental models start stabilizing. We will continue _slowly, cleanly, and calmly_, as requested.

We will now address **sorting as a pure pipeline stage**.

One concept. One mental model. One exercise.

---

# Topic: Sorting as a pure transformation

## What it is

In Python, sorting is performed using:
- `sorted(iterable, key=...)` → **returns a new list**
- Does **not** mutate the input
- Accepts a **key function** that maps each element to a sortable value

Example signature:
```
sorted(iterable, key=key_function)
```
This is the form we will use.  
We will **not** use `.sort()` here.

---
## Why it’s useful

Sorting becomes dangerous when:
- It mutates shared data
- The sorting rule is implicit
- The comparison logic is embedded in loops

Using `sorted()` with a `key` function gives you:

- Explicit transformation
- No mutation
- Clear separation of _data_ and _ordering logic_
- Easy composition in functional pipelines

---
## Unified mental model

> Sorting is **not rearranging data**.  
> Sorting is **projecting data onto an ordering axis**.

You do not “compare elements.”  
You **map each element to a value**, and Python sorts by that value.

This is critical.

---

## Example: sorting users by name (pure, explicit)

```
users = [
    {"id": 1, "name": "Charlie"},
    {"id": 2, "name": "Alice"},
    {"id": 3, "name": "Bob"},
]

#Step 1: define the ordering rule (key function)
def user_name(user: dict) -> str:
    return user["name"]

#Step 2: apply sorting as a pipeline stage
def sort_users_by_name(users: list[dict]) -> list[dict]:
    return sorted(
        users,
        key = user_name
    )
```

## Mutation vs non-mutation (explicit)

- `users` is **not modified**
- A **new list** is returned
- Dicts inside the list are **not copied** (important — more later)

Sorting is **structural**, not deep.

---
## Why this is the correct abstraction

- Ordering logic is named and testable
- Sorting can be added or removed from a pipeline without side effects
- No procedural loops
- No hidden state

This scales cleanly.

---
## Design rule to internalize

> **Never embed comparison logic inside sorting.  
> Always project elements to a sortable key.**

This rule prevents:
- Clever but unreadable code
- Fragile comparisons
- Repetition

---

```
orders:list = [
    {
        "id": 1,
        "name": "apple",
        "price": 10,
        "quantity": 4
    },
    {
        "id": 2,
        "name": "orange",
        "price": 5,
        "quantity": 3
    },
    {
        "id": 3,
        "name": "banana",
        "price": 1,
        "quantity": 2
    },
]

def order_total(item: dict) -> float:
    return item["price"] * item["quantity"]
    
def sort_orders_by_total(items: list[dict]) -> list[dict]:
    return sorted(    # sorted() returns a new list; input is not mutated
        items,
        key = order_total
    )
    
for each in sort_orders_by_total(orders):
    print(each)
    
for each in orders: # check if input list is mutated
    print(each)
```

## 3. Unified mental model (reinforced)

You are now consistently applying:

> **Data → projection → ordering**

- `order_total` (projection)
- `sorted(..., key=...)` (ordering)
- No mutation
- Clear ownership of results

This is the _correct_ way to reason about sorting in Python.

---
## 4. Important caveat (preview, not a lesson yet)

You implicitly noticed this already, but it’s worth stating clearly:

> `sorted()` copies the **list structure**, not the **objects inside it**.

If the dicts themselves are later mutated, both lists observe it.

We will address this later when we talk about **deep vs shallow boundaries**.

Not now.

---
## Where we go next (calm progression)

The natural next step is:

**Filtering → sorting → mapping as a composed pipeline**

**ANSWER:**
when learning about comprehensions with lambda, we used the pattern:

```
def transform_data():
    return list(
        map(
            lambda x:x, # -> output data transform rule
            sorted(
                filter(
                    lambda y:y, # -> filter rule
                    input_list
                ),
                key = lambda z:z  # -> the sorting condition is put directly 
            )
        )    
    )
```
moving the sorting condition (as well as filtering one) to another functions, 
which can be changed later- is a logical next step. Now:
1. our functions are responsible for only one thing
2. our functions can be piped

As to answer your question:
>> Does the distinction between _projection_ (`key=`) and _ordering_ feel clear now?
I think I understand that sorted() only copies the overall structure of the input and
inserts to output list elements in the required order. It does not reorder input list.

I will **validate first**, then **tighten two mental models** that are still slightly entangled:
1. _projection vs ordering_
2. _pipeline structure vs implementation detail_

No new concepts beyond that.

---
## 1. Validation of your reasoning

Your conclusions here are correct:

> Moving the sorting condition (and filtering one) to other functions is a logical next step.

Yes. This is exactly how functional pipelines **stabilize** over time.

You correctly identified the benefits:
1. Each function has **one responsibility**
2. Functions become **pipeable and replaceable**
3. Behavior changes by swapping functions, not rewriting pipelines

This is the correct architectural instinct.

---
## 2. First refinement: projection vs ordering (important correction)

You wrote:

> I think I understand that `sorted()` only copies the overall structure of the input and inserts to output list elements in the required order.

This is **almost right**, but one detail needs sharpening.

### What `sorted()` actually does
- It **iterates** over the input iterable
- It **collects references** to the elements
- It **orders those references** based on the _key values_
- It **returns a new list**

Crucially:

> `sorted()` never compares the original elements directly  
> unless you omit `key=`

With `key=`, the comparison happens on the **projected values**, not the elements.

---
### Correct mental model (lock this in)

> `sorted()` =  
> **project elements → compare projections → rearrange references**

It does **not**:
- Reorder the input
- Copy the elements
- Mutate data

## 3. Second refinement: lambdas inside pipelines

Your example:
```
sorted(
    filter(
        lambda y: y,
        input_list
    ),
    key=lambda z: z
)
```

This is _mechanically_ correct, but here is the Pythonic refinement:

> Lambdas are best used **at the leaves**, not in the middle of pipelines.

Why?
- Middle-of-pipeline lambdas hide intent
- They are not nameable
- They are not reusable
- They block refactoring

This is why moving them into named functions is not just stylistic — it is **semantic clarity**.

---
## 4. Refactored version (same behavior, clearer intent)

```
def is_valid(x):
    return bool(x)

def identity(x):
    return x

def transform_data(input_list):
    return list(
        map(
            identity,
            sorted(
                filter(
                    is_valid,
                    input_list
                ),
                key=identity
            )
        )
    )
```

Still not ideal — but now **every rule is named**.

From here, the _next_ Pythonic step is:
- Remove unnecessary stages
- Collapse trivial identity mappings
- Prefer comprehensions when stages collapse

This is how pipelines simplify over time.

## 5. Unified mental model (final refinement)

You are now operating with this correct model:
- **Filtering** decides _which elements survive_
- **Projection** decides _what aspect is used for ordering_
- **Sorting** rearranges _references_, not data
- **Mapping** transforms _values_

Each stage:
- Accepts data
- Returns new data
- Owns one rule

That model is now coherent.

---
## Where we go next (clean continuation)

Next, we should do **one carefully chosen refactoring exercise**:

- Start with a verbose `map → filter → sorted`
- Refactor it into:
    - Named functions
    - Then into a comprehension
- Without changing behavior

This will directly help with your **note restructuring**, because it teaches you how concepts collapse into each other.

