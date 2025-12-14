### What it is

A **pure function** is a function that:
- Depends _only_ on its inputs
- Produces an output
- Does **not** mutate external state
- Does **not** mutate its inputs

```
same input → same output
no observable side effects
```

### Why it’s useful

Pure functions give you:
- Local reasoning (no hidden dependencies)
- Testability without scaffolding
- Safe composition with `map`, `filter`, `sorted`
- Predictable behavior over time
---
### Unified mental model

> A pure function is a **mathematical mapping**, not a procedure.

Think in terms of:
`input data  →  transformation  →  output data`
Not:
`do this, then modify that, then remember something`

Time does not matter. Order does not matter. History does not matter.

## Example: Normalize user scores

### Problem statement

You are given a list of integer scores.  
You want a new list where each score is divided by the maximum score.
```
def normalize_scores(scores: list[int]) -> list[float]:
    max_score = max(scores)

    return [
        score / max_score
        for score in scores
    ]
```

### Mutation vs non-mutation

- `scores` is **not mutated**
- A **new list** is created
- `max_score` is a local binding, not state

### Why this is pure

- No external variables
- No mutation
- Deterministic output

You can call this function:

- In isolation
- In tests
- In pipelines

with zero surprises.

## Validation checkpoint (important)

Before we proceed, confirm the following are clear:
1. Why `normalize_scores` is pure
2. Why returning a new list is preferable here
3. Why this function is safe to compose with `map` or further transformations

---

> [!NOTE]
> This style of programming is my favorite. I realized that having completely independent functions that are unaware of the external state is efficient and easily debugged. It also allows for flexibility in combining all the little tools in the runtime code in any needed way. As the number of these functions grew, I separated them into logical groups/modules, which helped me stay organized even without OOP.
> 
> Answer to the 1st question in my own language.
> * The reference to the input mutable is copied to a local var so no mutation.
> * No dependancy on the external variables. 
> * Output is dependent only on input data by applying simple math to all the outpu elements in a cycle. We can use comprehensions here, or lambdas, it dosn't matter.

This is a strong and largely correct articulation. I will **validate what is right**, then make **two precise corrections** so your mental model remains exact rather than approximate. This is refinement, not a course change.

## Validation first — your core reasoning is correct

You correctly identified the three essential properties:

1. **No dependence on external variables**  
    Correct. All data needed for the computation enters through parameters.
2. **Output depends only on input data**  
    Correct. The transformation is deterministic and stateless.
3. **Element-wise transformation over a collection**  
    Correct. This is exactly the kind of work comprehensions are designed for, and the choice between comprehension vs `map` is secondary here.

Your broader architectural observation — organizing growing sets of such functions into modules without OOP — is also sound and historically proven. Many robust Python systems are built this way.

## Two important corrections (subtle but critical)

### 1. “The reference to the input mutable is copied to a local var so no mutation”

This sentence is **almost correct**, but the reason matters.
What actually happens:
- The **name** `scores` is bound to the same list object passed by the caller
- No copy is made
- Purity is preserved **because you never mutate the object**

So the correct model is:

> The function is pure **not because a copy is made**, but because the code refrains from mutating the object it received.

This distinction matters because:
- You _can_ mutate inputs in Python even when they are locally bound
- Purity is a **behavioral guarantee**, not a memory-level one

## Refined mental model (corrected)

Here is your answer, tightened but faithful to your intent:
- The function receives a reference to the input list but does not mutate it.
- It depends only on its parameters; no external state is observed.
- Each output value is independently derived from the corresponding input value via a deterministic transformation.
- The result is a new list, leaving the input untouched.

That model will not break later.

### 2. “Applying simple math to all the output elements in a cycle”

The word “cycle” is harmless colloquially, but mentally it can pull you toward a procedural model.

A better phrasing is:

> Each output element is the result of applying the same transformation rule independently to each input element.

This reinforces the **mapping** mental model rather than an imperative loop.


> [!prompt]
> I still sometimes mix rebounding:
> `new_ref = input_var` 
> with mutating, which is something like this:
> `input_var.add(element)'
> Also I slowly switch my mentality from the procedural style with cycles towards functional approach.
> Please correct me every time you notice any discrepancies.
> 
> Question 2. Why returning a new list is preferable here
> The first that comes into my mind is to allow piping. We can for example:
> filter the input list -> send the resulting list to normalize_scores -> new list can be sent to some other function.
> This way we have functions that do one thing and one thing only and we create new functionality by combining these functions, not by rewriting them.

## 1. Validation: your answer to Question 2 is correct

You wrote (paraphrased):

> Returning a new list allows piping: filter → normalize → further processing.  
> Functions stay single-purpose and are composed rather than rewritten.

This is **exactly correct**.

You identified the key reason:
- Returning new values enables **function composition**
- Composition is only safe when functions do not mutate shared inputs
- Each function becomes a reusable transformation

This is the _functional core_ of Python, even though Python is not a purely functional language.

## 2. Rebinding vs mutation — precise correction

You are right that this is a common source of confusion. Let’s make the distinction airtight.

### Rebinding (harmless by itself)

`new_ref = input_var`

What this means:
- `new_ref` and `input_var` now point to the **same object**
- No data is changed
- No side effects occur

Rebinding affects **names**, not objects.

### Mutation (observable side effect)

`input_var.add(element)`

What this means:
- The object referenced by `input_var` is modified in place
- All references to that object observe the change
- This breaks purity and composability

Mutation affects **objects**, not names.
### The critical rule (memorize this one)

> **Assignment rebinds names.  
> Method calls may mutate objects.**

Purity is violated only by the second category.

This rule predicts:
- Why closures behave the way they do
- Why list methods are dangerous in pipelines
- Why returning new objects is safer

## 3. Unified mental model (what you are already doing)

You are transitioning from:

> “Do steps in a loop that change things”

to:

> “Define transformations and connect them”

That shift is the entire point.

Your mental model is now:
```
data
  → filter
  → transform
  → normalize
  → aggregate
```
Each arrow:
- Accepts input
- Returns output
- Owns no state
- Mutates nothing

This is why your instinct to avoid in-place mutation is correct.

## One small precision improvement to your wording

You said:

> “new list can be sent to some other function”

More precisely:

> Each function returns a value that becomes the **sole input** to the next function.

This reinforces _ownership transfer_ and avoids thinking in terms of shared structures.

## Validation checkpoint (important)

Confirm that this distinction is now clear:
- Rebinding changes **which object a name refers to**
- Mutation changes **the object itself**
- Pure functions allow rebinding freely but forbid mutation

Once you confirm, we will proceed to **your first short exercise**, which will test this distinction explicitly without introducing new concepts.

Excellent. We will do **one focused exercise** that tests _exactly_ the rebinding-vs-mutation distinction and compositional thinking—nothing more.