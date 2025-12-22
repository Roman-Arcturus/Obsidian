## Intent

Demonstrate the difference between mutating a data structure in place and producing a transformed copy, and how this difference affects ownership, predictability, and reuse.
### Explanation

- This example isolates **one of the most important distinctions in Python**.
- It shows how two functions with similar _results_ can have radically different _side effects_.
- It prepares the ground for **Pure Data Pipelines** and **Safe List Processing**.

---

## Concept / Pattern Target

- [[ct02 Mutation vs Transformation]]  
- [[ct05 Ownership and Boundaries]]  
- [[ct01 Names and Binding]]  
- [[pt02 Pure Data Pipelines]]

### Explanation

- **Mutation vs Transformation**: This is the primary focus—contrasting in-place change with value-producing operations.
    
- **Ownership and Boundaries**: Highlights how mutation crosses boundaries, while transformation preserves them.
    
- **Names and Binding**: Explains why mutating through one name affects all references.
    
- **Pure Data Pipelines**: Shows why transformations compose safely and predictably.

This ensures the example is interpreted as a **mental-model demonstration**, not just a coding trick.

---
## Setup / Input

- A mutable list of integers:
  - `data = [1, 2, 3, 4]`

- Two functions:
  - One will mutate the list in place.
  - One will return a transformed copy.

- A second name (`alias`) will reference the same list as `data` to expose shared identity effects.

### Explanation

- The list is intentionally **mutable** to make mutation observable.
- `alias` is introduced to demonstrate **shared bindings** and ownership implications.
- Using the _same input_ for both functions isolates the behavioral difference to **mutation vs transformation**, not data shape.

---

## Code / Execution

```python
def mutate_in_place(values: list[int]) -> None:
    """
    Mutates the input list by doubling each element.
    """
    for i, value in enumerate(values):
        values[i] = value * 2


def transform(values: list[int]) -> list[int]:
    """
    Returns a new list with each element doubled.
    Does not mutate the input list.
    """
    return [
        value * 2
        for value in values
    ]


data = [1, 2, 3, 4]
alias = data  # shared identity

mutate_in_place(data)
result_transformed = transform(data)
```

### Explanation

- `mutate_in_place`:
    - **Input type:** `list[int]`
    - **Output type:** `None`
    - Mutates the list **owned by the caller**
    - Side effects propagate to all names bound to the list (`data`, `alias`)
    
- `transform`:
    - **Input type:** `list[int]`
    - **Output type:** `list[int]`
    - Produces a **new list**
    - Preserves the original list’s identity and ownership
    
- `alias = data` ensures that mutation effects are observable beyond a single name.

This code intentionally performs the mutation **first**, so its consequences are visible when the transformation runs.

---

## Output / Behavior

data == [2, 4, 6, 8]
alias == [2, 4, 6, 8]

result_transformed == [4, 8, 12, 16]

alias is data == True

### Explanation

Step-by-step reasoning:

1. After `mutate_in_place(data)`
    - The list object referenced by `data` is mutated in place.
    - Because `alias` references the same object, it observes the same mutation.
    
2. After `transform(data)`
    - A **new list** is created by iterating over the _already mutated_ `data`.
    - No further mutation occurs.
    - `result_transformed` is independent from both `data` and `alias`.
    
3. Identity check
    - `alias is data` → `True`
    - Confirms shared identity and shared mutation history.


**Critical insight:**  
Transformation preserves boundaries _only relative to its input_. If the input has already been mutated upstream, the transformation will faithfully reflect that state.

---

## Lessons Learned / Notes

- Mutation changes an object in place and affects all names bound to that object.
- Transformation creates a new object and preserves the caller’s data.
- Functions that mutate require *ownership* of their inputs.
- Functions that transform can safely operate on *borrowed* data.
- Mutation leaks across boundaries; transformation composes cleanly.
- Pure transformations are the foundation of predictable data pipelines.

### Explanation

- This example cements **Mutation vs Transformation** as a _behavioral contract_, not a stylistic choice.
    
- It explains why Python codebases that favor transformation are:
    - easier to reason about
    - safer to compose
    - simpler to test
    
- It directly supports your existing patterns:
    - **[[pt02 Pure Data Pipelines]]**
    - **[[pt03 Safe List Processing]]**
    - **[[pt01 Defensive Closures]]**

---

