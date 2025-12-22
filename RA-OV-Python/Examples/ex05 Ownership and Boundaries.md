## Intent

Demonstrate how understanding which part of code “owns” a data structure affects safe mutation, predictable behavior, and proper use of copies versus references.

### Explanation

- Ownership defines **who is responsible for a data object** and who can safely mutate it.
- This example contrasts **owned vs borrowed** data and shows why explicit copying is necessary in some cases.
- It directly supports patterns like **Safe List Processing** and **Defensive Closures**.

---

## Concept / Pattern Target

- [[ct05 Ownership and Boundaries]]  
- [[ct02 Mutation vs Transformation]]  
- [[ct01 Names and Binding]]  
- [[pt03 Safe List Processing]]

### Explanation

- **Ownership and Boundaries**: Core focus—illustrates which scope “owns” the data and can mutate it safely.
    
- **Mutation vs Transformation**: Shows when to mutate in place versus return a transformed copy.
    
- **Names and Binding**: Multiple names may reference the same object, affecting ownership.
    
- **Safe List Processing**: Provides the practical context for applying these concepts safely.
    

This ensures the example reinforces **mental models**, not just code behavior.

---

## Setup / Input

- A mutable list:
  - `data = [1, 2, 3]`

- Functions:
  - `process_owned(lst: list[int])` → mutates its input, assumes ownership
  - `process_borrowed(lst: list[int])` → transforms input and returns a new list, does not mutate original

- A second name:
  - `alias = data` to demonstrate shared ownership effects

### Explanation

- `data` is a shared object with **multiple references**.
- `alias` allows observation of how mutation propagates when ownership is assumed.
- The distinction between **owned** and **borrowed** inputs is explicit, preparing for safe mutation versus transformation.
- No global variables are used, keeping side effects local to the example.

---

## Code / Execution

```python
## Code / Execution

def process_owned(lst: list[int]) -> None:
    """
    Mutates the input list in place, assuming ownership.
    """
    for i in range(len(lst)):
        lst[i] *= 2


def process_borrowed(lst: list[int]) -> list[int]:
    """
    Returns a new list with doubled values.
    Does not mutate the input list.
    """
    return [x * 2 for x in lst]


data = [1, 2, 3]
alias = data  # shared reference

# Owned mutation
process_owned(data)

# Borrowed transformation
result = process_borrowed(data)

```

### Explanation

- `process_owned`:
    - **Input type:** `list[int]`
    - **Output type:** `None`
    - Mutates its input, demonstrating _owned mutation_.
    
- `process_borrowed`:
    - **Input type:** `list[int]`
    - **Output type:** `list[int]`
    - Returns a new list, demonstrating _safe transformation on borrowed data_.
    
- `alias` demonstrates **side effects propagation** when ownership is assumed.

This code illustrates the **ownership principle** in a concrete, observable way.

---

## Output / Behavior

data: [2, 4, 6]
alias: [2, 4, 6]
result: [4, 8, 12]

### Explanation

- After `process_owned(data)`:
    - `data` is mutated in place because the function assumed ownership.
    - `alias` sees the same changes because it references the same object.
    
- After `process_borrowed(data)`:
    - A new list `result` is returned.
    - `data` remains `[2, 4, 6]`, showing that borrowed data is not mutated.
    
- This demonstrates that **ownership determines who may safely mutate** and who should transform.
    

**Key insight:**

> When ownership is clear, mutation is safe; when ownership is borrowed, transformation preserves boundaries.

---

## Lessons Learned / Notes

- Ownership determines who may safely mutate an object.  
- Borrowed data should generally not be mutated; instead, return transformed copies.  
- Multiple names referencing the same object share ownership implicitly; mutation propagates to all references.  
- Explicitly copying or transforming borrowed data preserves boundaries and avoids side effects.  
- Understanding ownership is critical for:
  - Safe List Processing  
  - Defensive Closures  
  - Predictable function composition

### Explanation
- This example reinforces **Ownership and Boundaries** as a central mental model.
- Clarifies the practical consequences of shared references and mutation.
- Directly supports safe functional-style patterns in Python, including **Pure Data Pipelines** and **Safe List Processing**.

---

